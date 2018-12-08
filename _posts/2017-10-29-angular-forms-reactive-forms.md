---
layout: post
title: "Angular Forms: Reactive Forms"
author: "Lasse Schultebraucks"
---

In last weeks blog post I have talked about the template-driven form approach in Angular. Beside why using forms I have also explained how to use template driven forms in Angular, how to use validation in template driven forms and what the advantages and disadvantages are. In this weeks blog post I want to show you how to use reactive forms in Angular.

### Concept of Reactive Forms

The concept of Reactive Forms differs from the concept of template driven forms. In the template driven form approach you are building in the HTML template form controls like input or select and connecting these with ngModel to a model.

With reactive forms, there will be created a tree style data structure of form control object including validation etc. outside of the HTML template. This enabled control and manipulation of the form inside the component class.

Before we dive deeper into the different concepts and feature of reactive forms lets start to implement reactive forms.

### Implementation of Reactive Forms

To use reactive forms we first have to import the ReactiveFormsModule from @angular/forms.

<script src="https://gist.github.com/LSchultebraucks/1718c1fff6ede222310fac324b314b9d.js"></script>

Notice that we have imported another module than in the template driven form approach. In the template driven approach we have imported the FormsModule from @angular/forms.

After setting everything up, we can start to build our template. To compare the template driven forms approach and reactive forms I will build a similar form like in last blog post.

<script src="https://gist.github.com/LSchultebraucks/469d1a2214b7c6c5b4d8375c7629cb22.js"></script>

So let's start with the template of our reactive contact form.

As we can see, there are some changes we have made he compared to the template driven contact form.

First we have added an extra email validator, but we will talk about validation of reactive forms later.

On line 3 we declare the FormGroup of our reactive form which is called contactForm. It also has a ngSubmit event method like our template driven contact form. But instead of binding a model to our input and textares we use formControlName to access the FormControls of our FormGroup and direct them to our input and textarea elements. The FormControls in the FormGroup makes it possible not to use ngModel.

Notice that we have to made a few changes in our contact.component.ts file too:

<script src="https://gist.github.com/LSchultebraucks/4c3348742f480b5998429a45dc510d9a.js"></script>

Here we declare the contact FormGroup and build it with the FormBuilder which we also have to import from @angular/forms and declare it in the constructor.

In the ngOnInit() method we assign the FormControls to the FormGroup. The FormControls exist out of the value and the Validators we assign them. Here we assign name and message the required Validators and email the email Validator which checks if the typed in email address is a email address.

The isContactFormDisabled() method returns true if one of the three named validating FormControls (name, email and message) are invalid. If the isContactFormDisabled() is true, then the "Submit" button on the template will be disabled so the user can not submit his form.

The concept of validation here is working similar like in the template driven form approach, but there are some differences to it. First we define the validators in the FormControls and not in the HTML template. This enables us to test our form with classic unit testing. Remember that we only could test our template driven form with e2e tests! Second, we use the validators from the @angular/forms package (like required and email) and we can also build own validators to check the typed in input from the user. But this is a topic for another blog post.

### Advantages and Disadvantages of Reactive Forms

So why would you use reactive forms?

- Build for large scaled forms, FormGroup is being outsourced out of the template
- Validation from @angular/forms and custom validation possible
- Modification of the form outside of the template enabled unit testing

Why you should not use reactive forms?

- Probably "too complex" for easy forms

### Conclusion

So should you use reactive forms? Yes, if your application uses a lot of forms it is ideal. You can test it easily the outsourcing logic out of the template makes your application much cleaner.

But in some cases it is also recommending to use template driven forms. Anyway what forms you use, you should use also one kind of forms in your application. You could use both, but for a matter of consistency you should only use on type of forms in your Angular application.