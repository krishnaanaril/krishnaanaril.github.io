---
layout: post
title: "Google sign-in in Angular application"
date: 2018-06-02 17:26:04
image: '/assets/img/alvaro-reyes-500037-unsplash.jpg'
description: Implementation of a minimal angular 6 application that performs Google authentication.
category: ''
tags:
- Tech
- Angular
twitter_text: "Google sign-in in Angular application"
introduction: Implementation of a minimal angular 6 application that performs Google authentication.
---

User authentication is an integral part of any web application. Most of the sites provide different authentication mechanisms which include Google and Facebook login. Here we are trying to implement Google signin in an angular application. 

## Creating a Google Cloud project

Steps for creating cloud project is available [here](https://cloud.google.com/resource-manager/docs/creating-managing-projects)

## Creating angular 6 application

Steps for creating angular application is available [here](https://angular.io/guide/quickstart#create-proj)

## Load Google platform library

First, we need to load Google platform library in index.html. We may need to remove **async** and **defer** from the script to remove script errors as view will be loaded before loading **platform.js**.

~~~ html
<script src="https://apis.google.com/js/platform.js" async defer></script>
~~~

## Creating Google Sign-in component

Next we need to create an angular compnent that renders google sign-in button. So that we casn use this component in various pages if need. Maked use of the follwing angular-cli commmand.

~~~
ng g component google-siginin
~~~

Add the following **renderButton()** function in the **GoogleSigninComponent** which we just created.

~~~ javascript
renderButton() {
    //Sets the client id of the application
    gapi.load('auth2', () => {
      gapi.auth2.init({
        client_id: 'YOUR_CLIENTID' //TODO: Update it
      });
    });
    //Render google button
    gapi.signin2.render('googleBtn', {
      'scope': 'profile email',
      'width': 240,
      'height': 50,
      'longtitle': true,
      'theme': 'dark',
      'onsuccess': this.onSuccess,
      'onfailure': this.onFailure
    });
}
~~~

Here, we have declared two functions for success and failure respectively which needs to be defined. Add the following code to the same component.

~~~ javascript
onSuccess(googleUser) {
    let profile = googleUser.getBasicProfile();
    console.log('Token || ' + googleUser.getAuthResponse().id_token);
    console.log('ID: ' + profile.getId());
    console.log('Name: ' + profile.getName());
    console.log('Image URL: ' + profile.getImageUrl());
    console.log('Email: ' + profile.getEmail());
    //YOUR CODE HERE
}

onFailure(error) {
    console.log(error);
}  
~~~ 

Now we need to add the html code for button. Button will be embedded in the div which we've mentioned in the **renderButton()**

~~~ html
<div id="googleBtn"></div>
~~~

We are almost done with our coding. Now we need to render the button on page load. For that include **OnInit** from **@angular/core** 

~~~ javascript
import { Component, OnInit } from '@angular/core';
declare const gapi: any;
~~~

Call the **renderButton()** method from the **ngOnInit** lifecycle hook and we are done.

~~~ javascript
ngOnInit() {        
    this.renderButton();
}  
~~~

Run the following cli command to see your applictation in action. If **http://localhost:5000** is not whitelisted, please make it in the cloud project console.

~~~
ng serve --port 5000
~~~

Photo credits: <a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@alvaroreyes?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="_blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Alvaro Reyes"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Alvaro Reyes</span></a>