---
layout: post
date: 2017-03-25 11:00
title: Information Overload - Building Our Data
categories: [programming clojure]
---

For our project, our data is the foundation.
If we design it well, we'll be set up for a smooth development process.
If we don't, we're going to be spending time trying to fix it.

If you haven't, I strongly encourage you to read the 
[first post](https://admay.github.io/the-information-overload-plan/) 
in the series, just so you have an idea of what we're doing here.

## Modeling Data

#### Step 1: Account registration

For a user to create and store notes, they will need to register an account.
This will consist of creating a username and password.
We're also going to request an email address from the user for critical notifications.
One the home page, we're going to display a 'create an account' button for new users.

#### Step 2: Login and logout

After creating an account, user's will need to be able to login and logout.
We'll also need to secure our application, we can't just store passwords as plaintext in a database.
On the home page, we want to display a 'log in' button for existing users next to the 'register' button.


#### Step 3: Create notes

With user registration and authentication out of the way, users need to be able to create notes.
This should be a simple text area for users to type in.
We won't be supporting any specific formatting.

#### Step 4: Update notes



#### Step 5: Download notes



#### Step 6: Delete notes



#### Step 7: Delete account



