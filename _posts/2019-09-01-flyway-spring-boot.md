---
layout: post
title: "Migrations with Spring Boot and Flyway" 
author: "Lasse Schultebraucks"
categories:  [Spring Boot, Java, Hibernate, Flyway, Database]
comments: true
---

In the last month I worked a lot with migrations in a Spring Boot application. I used [Flyway](https://flywaydb.org/) as a version control tool to manage migration scripts. Flyway is a great and easy to use tool to deal with migrations in a Java project with a SQL database.

In the following I will show you in a demo application how to set up flyway in a spring boot application with PostgreSQL. You can check out the whole code on my [GitHub](https://github.com/LSchultebraucks/FlywayMigrationDemoSpringBoot).

First we need to create a fresh spring boot application. You can also add Flyway to your spring boot application if you are already in development or in production and want to begin with database migrations. 

You can create a fresh spring boot application with the [spring initializer](https://start.spring.io/). As dependencies choose Web, JPA, Flyway and PostgreSQL. That is all you need in the beginning. I started added a Book entity.

```java
package com.lasseschultebraucks.flyway.demo.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Book {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;

    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

```

As you can see it already has JPA annotations on the class and on one attribute to mark the whole class as an database entity and the determine the primary key.

Next we can use PostgreSQL to set up a local database and to connect it to our application. Therefore we first need to [download and install PostgreSQL](https://www.postgresql.org/download/). After the installation you can start pgAdmin, add a user (with username/password `admin`) and create a new database named `flyway-core` and owner `admin`.

Next we can create the configure the connection to the database in the `application.yaml`.

```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/flyway-demo
    username: ${db.appUser:admin}
    password: ${db.password:admin}
  jpa:
    hibernate:
      ddl-auto: none
    database-platform: org.hibernate.dialect.PostgreSQLDialect
    properties:
      hibernate:
        temp:
          use_jdbc_metadata_defaults: false
  flyway:
    baseline-on-migrate: true
```

In the first lines we have configured our PostgreSQL database connection. Then we told hibernate to disable the automatic schema generation, because we will deal with it yourself now. Last but not least we enabled the flyway migration on the application start.

Next we can create our first migration script. We can either create migration scripts in Java or SQL with flyway. In this tutorial I will just create migration scripts in SQL. To do this, we need to create a new file in the `resources/db/migration` package. Flyway executes the migration script in folder based on their version numbers. So we will start with a file named `V0__InitialBook.sql`

```sql
CREATE TABLE public.book
(
    id   BIGINT NOT NULL,
    name CHARACTER varying(255),
    CONSTRAINT book_pkey PRIMARY KEY (id)
);
ALTER TABLE public.book
    OWNER TO admin;
```

As you can see the script will create a new table in our database for the book entity which we created previously.

Now we can start our Spring Boot Application for the first time. You can open pgAdmin and see that the there will be now a table named `book`. There is also a table named flyway_schema_history, which holds and manage the current version and state of the database. If there is a new migration script which is not marked in the table, then flyway will execute the script and bring the database to the newest state.

Next I added a new entity named `Author`.

```java
package com.lasseschultebraucks.flyway.demo.model;

import javax.persistence.*;
import java.util.ArrayList;
import java.util.Collection;

@Entity
public class Author {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Column(name = "author_name")
    private String name;

    @OneToMany(mappedBy = "author", cascade = CascadeType.DETACH)
    private Collection<Book> bookCollection = new ArrayList<>();

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Collection<Book> getBookCollection() {
        return bookCollection;
    }

    public void setBookCollection(Collection<Book> bookCollection) {
        this.bookCollection = bookCollection;
    }
}

```

Also the book entity gets an extra line:

````java
   // ...
    @ManyToOne
    private Author author;
    //...
````
As you can see, there is now a Many to One relationship between Author and book. 

Because we have a new entity and also a new relationship between our entities we have to update our database schema with a second migration script:

```sql
CREATE TABLE public.author(
    id BIGINT NOT NULL,
    author_name CHARACTER varying(255),
    CONSTRAINT author_pkey PRIMARY KEY (id)
);
ALTER TABLE public.author
    OWNER TO admin;

ALTER TABLE public.book
    ADD author_id bigint,
    ADD CONSTRAINT author_id FOREIGN KEY (author_id)
        REFERENCES public.author (id) MATCH SIMPLE;

CREATE SEQUENCE public.hibernate_sequence
    INCREMENT 1
    MINVALUE 1
    MAXVALUE 9223372036854775807
    START 1
    CACHE 1;
ALTER TABLE public.hibernate_sequence
    OWNER TO admin;
```

The first two queries will create a author table and create a foregin key constraint in book for the author id.

The last query will create a hibernate sequence. A hibernate sequence manage the id numbers in our database, so that no entity has equals ids. We need this because in our next step we will insert data in our database:

````java
package com.lasseschultebraucks.flyway.demo;

import com.lasseschultebraucks.flyway.demo.model.Author;
import com.lasseschultebraucks.flyway.demo.model.Book;
import com.lasseschultebraucks.flyway.demo.repository.AuthorRepository;
import com.lasseschultebraucks.flyway.demo.repository.BookRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import java.util.Arrays;

@SpringBootApplication
public class DemoApplication implements CommandLineRunner {

    private final AuthorRepository authorRepository;
    private final BookRepository bookRepository;

    @Autowired
    public DemoApplication(AuthorRepository authorRepository, BookRepository bookRepository) {
        this.authorRepository = authorRepository;
        this.bookRepository = bookRepository;
    }

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Override
    public void run(String... args) throws Exception {
        clearTables();

        Author author = new Author();
        author.setName("J.K. Rowling");
        Book book = new Book();
        book.setName("Harry Potter and the Philosopher’s Stone");
        book.setAuthor(author);
        Book book1 = new Book();
        book1.setName("Harry Potter and the Chamber of Secrets");
        book1.setAuthor(author);
        author.setBookCollection(Arrays.asList(
                book, book1
        ));
        authorRepository.save(author);
        bookRepository.save(book);
        bookRepository.save(book1);
    }

    private void clearTables() {
        authorRepository.deleteAll();
        bookRepository.deleteAll();
    }
}

````

After executing once again our Spring Boot application we will now have a new state of our database and also initial data in our database. You can check you state of your database once again in pgAdmin.

As mentioned earlier, you can find the whole code on my [GitHub](https://github.com/LSchultebraucks/FlywayMigrationDemoSpringBoot).