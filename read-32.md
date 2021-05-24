##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 32 - Serverless and Amplify

<hr>

# Intro to Serverless

## What is Serverless Architecture? What are its Pros and Cons?

>***“Focus on your application, not the infrastructure”***

***Serverless applications are event-driven cloud-based systems where application development rely solely on a combination of third-party services, client-side logic and cloud-hosted remote procedure calls (Functions as a Service).***

## Traditional vs. Serverless Architecture:

<img src = "https://hackernoon.com/hn-images/1*x_v5NRC3TTMt1MaYl1gMUg.jpeg">

<hr>

## Functions as a Service (FaaS)

`FaaS` is an implementation of Serverless architectures where engineers can deploy an individual function or a piece of business logic. They start within milliseconds (~100ms for AWS Lambda) and process individual requests within a 300-second timeout imposed by most cloud vendors.

**Principles of FaaS:**

* Complete management of servers
* Invocation based billing
* Event-driven and instantaneously scalable



## The Serverless App:

A Serverless solution consists of a web server, Lambda functions (FaaS), security token service (STS), user authentication and database.

<img src = "https://hackernoon.com/hn-images/1*TIrjN7EjLUVJmJ6YvHR7Dg.png">

* Client Application — The UI of your application is rendered client side in Modern Frontend Javascript App which allows us to use a simple, static web server.
* Web Server — Amazon S3 provides a robust and simple web server. All of the static HTML, CSS and JS files for our application can be served from S3.
* Lambda functions (FaaS) — They are the key enablers in Serverless architecture. Some popular examples of FaaS are AWS Lambda, Google Cloud Functions and Microsoft Azure Functions. AWS Lambda is used in this framework. The application services for logging in and accessing data will be built as Lambda functions. These functions will read and write from your database and provide JSON responses.
* Security Token Service (STS) — generates temporary AWS credentials (API key and secret key) for users of the application. These temporary credentials are used by the client application to invoke the AWS API (and thus invoke Lambda).
* User Authentication — AWS Cognito is an identity service which is integrated with AWS Lambda. With Amazon Cognito, you can easily add user sign-up and sign-in to your mobile and web apps. It also has the options to authenticate users through social identity providers such as Facebook, Twitter or Amazon, with SAML identity solutions, or using your own identity system.
* Database — AWS DynamoDB provides a fully managed NoSQL database. DynamoDB is not essential for a serverless application but is used as an example here.


## Summary

Serverless architecture is certainly very exciting, but it comes with a bunch of limitations. As the validity and success of architectures depend on the business requirements and by no means solely on technology. In the same way, Serverless can shine when used in proper place.

<hr>

## AWS Amplify:
 
is a set of tools and services that can be used together or on their own, to help front-end web and mobile developers build scalable full stack applications, powered by AWS. With Amplify, you can configure app backends and connect your app in minutes, deploy static web apps in a few clicks, and easily manage app content outside the AWS console.

Amplify supports popular web frameworks including JavaScript, React, Angular, Vue, Next.js, and mobile platforms including Android, iOS, React Native, Ionic, Flutter.


<hr>

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.

###### [resource1](https://hackernoon.com/what-is-serverless-architecture-what-are-its-pros-and-cons-cc4b804022e9)
###### [resource2(AWS Amplify)](https://aws.amazon.com/amplify/)
###### [resource3 (GraphQL)](https://docs.amplify.aws/cli/graphql-transformer/overview)
<!-- ###### [resource4]() -->