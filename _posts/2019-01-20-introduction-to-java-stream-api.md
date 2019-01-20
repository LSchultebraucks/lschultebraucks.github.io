---
layout: post
title: "Introduction to Java Stream API"
author: "Lasse Schultebraucks"
categories: [Java]
comments: true
---

Java Stream API is there since Java 8. It is used to express computation on data is a short and elegant way. In the following post I will introduce you to the most common methods to give you an idea what you can achieve with the Java Stream API.

# Streams vs Collections

Before jumping right into the code I want to explain the difference between Streams and Collections. It is clear that both have properties in common, they can both be iterated for example. You can iterate through Collection external with e.g. for each loops. Instead you don't loop explicit through streams. You express your computations in a functional way and the Stream API deals for iterations internally. Also the Stream API is lazy, its elements are computed or fetched via network by demand. Collections are a in memory data store which means that every element in your Collection has to be computed and stored in your RAM before you can access it. But this does not mean that you can't use streams for already computed data. It rather makes Streams more flexible than Collections in specific situations. You also can create Streams out of Collection based data structures as I will do in the following examples.

# Examples

In the following I will show you some examples of the usage of Streams and I will also compare my solution to the classical programming approach.

## `filter`

If you want to filter a list, you can just use `filter`.

Let's assume we have a list of words.

```java
List<String> words = Arrays.asList("Abra", "Kadrabra", "Aloha");
```

Now we want to filter all words which starts with a capital A and print them.

```java
words.stream()
        .filter(word -> word.startsWith("A"))
        .forEach(System.out::println);
```

We actually use two stream methods here. First we transform the list to a stream and filter and words which starts with a capital A. On the resulting stream we are printing out every word with `System.out.println`. 

You probably have seen similar lambda expressions like the on in the `filter` before, but maye you have not seen many which I used in the `forEach` method. `System.out::println` is just syntactic sugar for following lambda expression: `x -> System.out.println(x)`.

We can also write the code by not using streams by simply using a for loop:

```java
for (String word : words) {
        if (word.startsWith("A")) {
            System.out.println(word);
    }
}
```

This solution is in my opinion less elegant and can be written more expressive as I showed in the upper code snippet.

We can even improve our Stream example by creating a method for the `startsWith` condition in a external method.

```java
words.stream()
        .filter(StreamExamples::startsWithA)
        .forEach(System.out::println);
```

with

```java
private static boolean startsWithA(String word) {
    return word.startsWith("A");
}
```

## `sum`

With the Stream API you also never have to write code like this again

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

int sum = 0;
for (Integer number : numbers) {
    sum += number;
}
System.out.println(sum);
```

Instead you can write

```java
System.out.println(numbers.stream().mapToInt(Integer::intValue).sum());
```

which does exactly the same as the upper code snippet.

## `map`

`map` projects every element of a stream into another form. As I used `mapToInt` to project the element to Int's, I can use map to project every element of a stream in to a new element.

```java
numbers.stream()
        .map(number -> number * number)
        .forEach(System.out::println);
```

A traditional approach of writing this code would be following snippet:

```java
for (Integer number : numbers) {
    int squaredNumber = number * number;
    System.out.println(squaredNumber);
}
```

## `flatMap`

Next lets assume we have following data source:

```java
List<List<Integer>> matrix = Arrays.asList(Arrays.asList(1, 2), Arrays.asList(3, 4), Arrays.asList(5, 6));
```

Our task is it now to sum up all element in the matrix.

To flat the data structure we can use `flatMap`. It concatenates streams and generates a single stream as a result. So to compute the sum, we can use first `flatMap` to concatenate multiple streams into one and then use `mapToInt` and `sum` as shown in the upper example of `sum`.

```java
System.out.println(matrix.stream().flatMap(Collection::stream).mapToInt(Integer::intValue).sum());
```

`flatMap` allows use to abstain from for-loops in for-loops and write elegant code in one line.

Your colleagues will thank you if they pull your code.

## `collect`

Last but not least you can transform a stream into a traditional data structure by using `collect`.

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

List<Integer> evenNumbers = numbers.stream().filter(x -> x % 2 == 0).collect(Collectors.toList());
```

# Conclusion

I have uploaded all examples to my [Github](https://github.com/LSchultebraucks/StreamAPIExamples/blob/master/src/StreamExamples.java)

Java 8 introduced Java Streams which allows us to express data processing queries in a short functional and elegant way. There are many more operations to explore, this post should just gave you an introduction to the Stream API. I hope you are motivated now to use the Stream API next time when you are using Java.