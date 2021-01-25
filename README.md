# advanced-api-gateway-resources
Cloud Academy - Advanced API Gateway
https://cloudacademy.com/course/advanced-use-of-amazon-api-gateway/creating-dynamodb-table-and-lambda-functions/

Description
Amazon API Gateway allows you to design RESTful interfaces and connect them to your favorite backend. You can design your own resources structure, add dynamic routing parameters, and develop custom authorizations logic. Each API resource can be configured independently, while each stage can have specific cache, throttling, and logging configurations.

This approach is particularly useful when you consider that each request and response can be attached to a custom mapping template, in order to perform custom data manipulation or improve API backward compatibility.

Description
In this course, we will create a multi-tier serverless architecture on AWS using Amazon API Gateway, AWS Lambda, AWS Step Functions, and Amazon Polly text to speech. This is a hands-on course where you will learn how to create serverless functions, data access, business logic, and integration layers on AWS. Also, you will learn how to create a presentation layer for your application using the client SDK generated by Amazon API Gateway. We will then host the application on Amazon S3.

Learning Objectives
By the end of this course, you will be able to recognize and implement an end to end workflow built in the Amazon Cloud using Serverless components.

Intended Audience
This course is intended for developers or DevOps engineers who want to create serverless applications on AWS, or who may be considering migrating their existing web applications to AWS.

Prerequisites
A basic familiarity with Amazon API Gateway, Lambda, DynamoDb, S3, and AWS Step functions is required for this course. Some knowledge of building web applications using HTML and JavaScript will also be helpful.

Resources
The GitHub repository for this course is available here: https://github.com/cloudacademy/advanced-api-gateway-resources.

Transcript
Hello and welcome back. In this lecture, we'll create the data and data access layers for our application, which will consist of DynamoDB and Lambda functions. Let's look at our application workflow before we head to create the Lambda functions. In our application, the user will select Menu Items, Delivery Options and a Payment Type, and then submit the order. Once the order is submitted, the payment will be processed, and either approved or rejected. Based on that, the order will be further processed or canceled, and the user will be notified accordingly. This workflow can be easily translated into Step Functions or a State Machine, and we'll see that in the next video. For now, we can just think of each step in the workflow as a Lambda function which does a discreet task, and passes the results to the next function. To start, we'll first need to create a table in DynamoDB to which our Lambda functions will write to and re-prompt. I'll go to our DynamoDB console and create a table, let's call it Orders. I'll choose Order ID as a primary key. We don't need to do anything else here since it's a NoSQL Database, and we can insert our records later through the Lambda function in the format we like. We'll create our Lambda functions using a CloudFormation template. But before that, I would need to place my code on S3 so that the CloudFormation template can access it. Let's create a bucket in S3 and call it gopizza-lamba-functions. All of the Lambda functions are in .js files, and I'll zip them all up as Lambda Functions.zip and upload to the bucket. From here, the CloudFormation template can access the code. Okay, now here is my CloudFormation template that I've already created using YAML. This template creates a role which our Lambda functions will use. It has a AssumeRolePolicyDocument property, which is a trust policy associated with this role, which will allow Lambda functions to assume its role. It also has these ManagePolicyArns that give it full access to S3, DynamoDB and Amazon Polly, as required by our Lambda functions. Then we create our five Lambda functions through the template. If you look at the first Lambda function, PlaceOrder, it has the type of Lambda function, and properties including description, function name, a handler, which is the function Lambda will invoke, and since we've saved our Node.js function as PlaceOrder.js, the handler will be PlaceOrder.handler. For role, we'll get the ARN of the role created earlier in the template. For code, we'll specify the S3 Bucket where our code is located, and the name of the ZIP file which contains all the functions. So, I'll go to CloudFormation Console and execute the template to create our functions in us-east-2 region. If you plan to use Step Functions, make sure you choose a region where it support it. The template also creates an IAM role, so I'll need to acknowledge that, and then click on create. Our stack is successfully created, now let's look at each Lambda function to understand it's functionality. The first function to be executed is PlaceOrder. This function simply puts all the data from the input event, including Order Type, Menu Items, and Payment Details to the Orders table in DynamoDB and passes over the information to the next Lambda function in the workflow which is ProcessPayment. ProcessPayment is just a stub method here, with some hard-coded logic for demo. It only passes the payment if credit card number is all ones, all the others are rejected. In the real world, of course, this method would be much sophisticated to correctly process the payment details. This function returns two objects, one is a paymentSuccess boolean value, and the other is the Params from the event. The paymentSuccess variable allows us to determine whether the order was successfully placed or canceled, and will be used in the choice state later in the State Machine. After that we have two more functions, ConfirmOrder and CancelOrder. Both update the order status of the record, where they confirm or cancel the value, based on the paymentSuccess value. After that we have NotifyUser function, which just returns the order status in text format. And the last function, NotifyWithPolly, converts the text order status to speech in the form of MP3 file, and stores it on S3 for the front-end page to playback so the user can hear the order status in live-like speech format from our front-end app. Okay, now we have all our Lambda functions in place, so it's time to put them in the correct order of execution. We'll do that using the step functions in our next video.

About the Author

Tehreem Siddiqui

Sr. Software Engineer

Tehreem is a Sr. Software Engineer with passion in Cloud Technologies, Big Data analytics, Software Testing and Automation. She has over 10 years of work experience comprising of her tenure at ServiceNow, Microsoft and Harmonic Inc. Most recently she has been developing learning content in-line with the emergence of Public Clouds and XaaS platforms with focus on AWS, Microsoft Azure and GCP. Tehreem resides in BayArea, CA with her family and when not working she enjoys nature/outdoors, movies and fine dining.

![after image](https://assets.cloudacademy.com/bakery/media/uploads/laboratories/environment_start_images/before_L6y0zJY.png)
![after iamge](https://assets.cloudacademy.com/bakery/media/uploads/laboratories/environment_end_images/After_kkGvc81.png)

Amazon API Gateway allows you to design RESTful interfaces and connect them to your favorite backend. You can design your own resources structure, add dynamic routing parameters, and develop custom authorizations logic. Each API resource can be configured independently, while each stage can have specific cache, throttling, and logging configurations.

This approach is particularly useful when you consider that each request and response can be attached to a custom mapping template, in order to perform custom data manipulation or improve API backward compatibility.

In this Lab, you will see how to define a simple API and how to connect it to AWS Lambda. This provides a nice way to obtain a scalable backend for modern web applications or mobile apps. You will configure custom stages, protect resources with an API key, and explain how to best connect API Gateway stages with AWS Lambda versions and aliases. You will learn about AWS Lambda's basic configuration, monitoring, and versioning as you progress through the Lab.

Lab Objectives
Upon completion of this Lab, you will be able to:

Understand the basics of RESTful APIs
Implement REST APIs using Amazon API Gateway
Enable desirable API features in API Gateway including caching, throttling, CORS, usage plans, and API key access
Create serverless API backends using AWS Lambda functions
Implement best practices for integrating Lambda backends in API Gateway
Lab Prerequisites
You should be familiar with:

Serverless computing fundamentals, particularly AWS Lambda, is beneficial
Prior experience using or implementing REST APIs is beneficial, but not required
Updates
January 22nd, 2021 - Updated AWS Lambda lab step to reflect latest user interface changes

July 21st, 2020 - Updated lab security policy to prevent the appearance of a DescribeVpcEndpoints error pop-up while creating the REST API

January 10th, 2019 - Added a validation Lab Step to check the work you perform in the Lab

September 5, 2018 - Improved Lab instructions format and clarity. Updated instructions and screenshots to match the new API Gateway interface

https://cloudacademy.com/lab/restful-microservices-aws-lambda-api-gateway/?context_resource=lp&context_id=241


