---
layout: post
title: "Angular Forms: The template-driven form approach"
author: "Lasse Schultebraucks"
---

In Angular there existing two ways to create and use forms. To clear things up, I want to talk about both ways. In this post I want to talk about the Template-driven Form approach. In the next week blog post I will talk about Reactive Forms.

### Why using forms?

You may ask yourself why should I use forms?

If you are developing a Fronted application it may be the case that you have to implement some kind of form, e.g. to sign in, to fill out some kind of user information etc. There are also application who are very form intensive and existing nearly only out of forms. And in some kind of application you even have to develop a form which spans about multiple pages aka a dialogs. In this case you have to deal with following problems:

- Keeping track of the global state of your form
- Dynamical changing the structure of forms during the filling out process of the user
- Validation of user input and displaying error to the user
- You need easy and quick access on the form

Luckily Angular can help here and give us as a developer some support to face this problems.

There exist the template driven form and reactive forms. In this blog post I will talk about the Template Driven Forms approach.

### How it works

Since AngularJS (Angular 1.x) there existing the template driven approach. In the template driven you are using ngModel (in AngularJS ng-model) to keep your form synchronized with your model. When you are using the approach your only possibility to test your form is an End to End test (e2e), because you are need the access to the DOM to test your form.

### Implementation

To use the template driven approach you first have to import the FormsModule form @angular/forms into your app.module.ts

<script src="https://gist.github.com/LSchultebraucks/003129ec78e3ddbaae358b35f47cceb7.js"></script>

Assuming we have a typescript model for a contact form with name, e-mail, title and message. All of the attributes of this model should be strings.

<script src="https://gist.github.com/LSchultebraucks/c323f08265edbd7df155afbf042e7cfa.js"></script>

Then we can build a form with the template-driven approach.

<script src="https://gist.github.com/LSchultebraucks/3f1dd0fd79f92762dab8a096c6b880c5.js"></script>

In the form element there are label and input/textarea field pairs. The labels functionality should be clear. In the input and textarea fields are just passing with ngModel fitting attributes from our contact model which we have declared in our contact.component.ts file. If we type in now in the input field the contact model change with it because of the two way data binding ability of ngModel.

We are even building some kind of required validation for name and message here, because the user should at least write his name and a message in a contact form. We will concentrate on validation in the next paragraph more intensive. We are validating the model on the submit button on line 34 so that form with a empty name and message can not be submitted. But if the user fills out the name and message field Angular enables the submit button and the user can submit his contact form and with it the contact model.

We have not implemented onSubmitContact() here, but we could simply implement it in the contact.component.ts so that we send the form to our backend and redirect the use in the client to a success page, which show him that he have send the contact form with success.

### Validation

We have build in some logic validation in our contact form above. We can do this because Angular is tracking the form field and saves his states as CSS state classes. A form field is either touched or untouched, valid or invalid and pristine or dirty. The valid and invalid states should be clear. They describe if the value in a field is valid or invalid. There are much more native HTML form validation beside required like minLength or maxLength.

But what is about pristine/touched and touched/untouched?

The default state of a form field is pristine and untouched. If the user blur the form field, the form field will be set to touched and if the user change the value of a form field the form field will be set to dirty. This enable the application to only show an error if the user has actually edited the form field. Otherwise it would be very annoying for the user and therefore not very good for the user experience.

### Advantages and Disadvantages of the Template-driven Form approach

So why would you use the template driven form approach?

- Simplicity: quick implementation with an existing model.
- Validation with native html validation.

Why you should not use the template driven form approach?

- Larger forms are difficult to implement with the template driven form approach and will become complex.
- Validation can only be tested with E2E (end to end) tests.

### Conclusion

So should you use template driven forms? Yes, in some cases definitely. For little forms with a couple of fields (like contact form or login in/out form) they are perfect except you want to test you validation beside E2E tests. For more complex forms reactive forms could be the smarter choice. I will talk about reactive forms in next weeks blog posts.