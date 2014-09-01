---
layout: post
title: "Learning MVC"
category : architecture
tagline: ""
tags: [mvc]

---
{% include JB/setup %}

#My understandings on MVC#

##MODEL##

##VIEW##

##CONTROLLER##


##Others##
###Data Validation###
Ideally, you want 3 layers of validation:

1. View: Client side (javascript, html5 validation, etc.). This catches obvious errors and omissions before the data hits the controller, wasting the user's time and invoking an unnecessary page load if there are errors. Also, **Inputs from user can't be trusted**
2. Controller: This is your Form validation layer. Controllers usually are meant to handle input directly, and send it over to the model. It is very rare that every field in your form has a directly related column in your DB, you usually need to alter the data in some way before passing it to the model. Just because you have a field you need to validate called "confirm email", doesn't mean that your model will be dealing with a "confirm email" value. Sometimes, this will be the final validation step.But Controller does NOT CONTAIN validation rules and it does not know how to validate data presented by user! Controller only knows which action is requested and action creates decent model which knows how to validate data.
3. Model: This is your last line of defense for validation, and possibly your only validation in the case of sending data to the model without it coming directly from a form post. There are many times when you need to send data to the DB from a controller call, or with data that is not user input. We don't want to see DB errors, we want to see errors thrown by the app itself. Models typically should not be dealing with $_POST data or user input directly, they should be receiving data from the controller. You don't want to be dealing with useless data here like the email confirmation.

1. Data verification (not form validation) should be performed in model layer, as only bean knows how your data should look like.
/*2.  controller contains input (processing) rules. Your model contains business rules. You want your business rules to always be enforced, no matter what happens. Assuming you had business rules in the controller, then you'd have to duplicate them, should you ever create a different controller.*/

###Encoding###