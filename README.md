## Lab 6 - Developing an end to end Application with AWS

### Overview

In this lab, you will learn how to use the AWS SDK and several AWS services to build an end to end serverless web application instead of the little pieces you have been building in the previous labs.

The idea of this web application is to provide a way for users to create and manage text notes and play them back audibly in different accents and voices by using Amazon Polly.

The web app will have the following:

- A frontend based on Angular whose files will be hosted on a static website in Amazon S3.
- A backend based on Amazon Cognito User Pools, Amazon API Gateway, Amazon Lambda, Amazon DynamoDB, and Amazon Polly.

There is a need to develop one Lambda function as part of this lab and use four Lambda functions that have already been coded for you. Once you complete the lab and you still have time remaining, take the challenge by coding three of those four Lambda functions yourself.

![lab-6-diagram-2020](https://user-images.githubusercontent.com/22528198/131243198-276c7a4f-612e-401e-9a0b-49579364a2cd.png)

This lab makes use of the following AWS services:

#### Amazon API Gateway

- Create, maintain, and secure APIs at any scale. Run multiple versions of the same API simultaneously with quick iteration, testing, and release. Throttle traffic and authorize API calls to withstand traffic spikes. Monitor performance metrics and information on API calls, data latency, and error rates.
- [Learn more about Amazon API Gateway](https://aws.amazon.com/api-gateway)

#### Amazon Cognito

- Amazon Cognito lets you add user sign-up, sign-in, and access control to your web and mobile apps quickly and easily. Amazon Cognito scales to millions of users and supports sign-in with social identity providers, such as Apple, Facebook, Google, and Amazon, and enterprise identity providers via SAML 2.0 and OpenID Connect.
- [Learn more about Amazon Cognito](https://aws.amazon.com/cognito)

#### Amazon DynamoDB

- Amazon DynamoDB is a key-value and document database that delivers single-digit millisecond performance at any scale. It's a fully managed, multi-region, multi-active, durable database with built-in security, backup and restore, and in-memory caching for internet-scale applications.
- [Learn more about Amazon DynamoDB](https://aws.amazon.com/dynamodb)

#### Amazon Elastic Compute Cloud (Amazon EC2)

- Amazon EC2 is a web service that provides resizable compute capacity in the cloud. It reduces the time required to obtain and boot a new server instance to minutes, allowing you to quickly scale capacity, both up-and-down, as your computing requirements change.
- [Learn more about Amazon EC2](https://aws.amazon.com/ec2)

#### Amazon Lambda

- AWS Lambda is a serverless compute service that lets you run code without provisioning or managing servers, creating workload-aware cluster scaling logic, maintaining event integrations, or managing runtimes. With Lambda, you can run code for virtually any type of application or backend service - all with zero administration.
- [Learn more about AWS Lambda](https://aws.amazon.com/lambda/)

#### Amazon Polly

- Amazon Polly is a service that turns text into lifelike speech, allowing you to create applications that talk, and build entirely new categories of speech-enabled products. Polly's Text-to-Speech (TTS) service uses advanced deep learning technologies to synthesize natural sounding human speech. With dozens of lifelike voices across a broad set of languages, you can build speech-enabled applications that work in many different countries.
- [Learn more about Amazon Polly](https://aws.amazon.com/polly)

#### AWS Identity and Access Management (IAM)

- AWS Identity and Access Management (IAM) enables you to manage access to AWS services and resources securely. Using IAM, you can create and manage AWS users and groups, and use permissions to allow and deny their access to AWS resources.
- [Learn more about IAM](https://aws.amazon.com/iam)

#### Simple Storage Service (Amazon S3)

- Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance.
- [Learn more about Amazon S3](https://aws.amazon.com/s3)


### Objectives

After completing this lab, you will be able to:

- Use Remote Desktop (RDP) to connect to an Amazon EC2 instance running Windows Server as your development environment.
- Use Amazon EC2 Instance Connect to connect to an Amazon EC2 Instance running Linux.
- Host a Static Website in an Amazon S3 bucket.
- Create an IAM Policy and Role to give specific access to Lambda.
- Create an Amazon Cognito User Pool to authenticate users and control access to API Gateway.
- Create Lambda functions to perform CRUD operations on a DynamoDB table.
- Create a Restful API on API Gateway to front Lambda functions.
- Access the Amazon Polly service through API.

### Start Lab

#### Task 1: Connect to your development environment

This lab is to be completed using the Windows Dev Instance for the .NET and the Java versions of the lab.

The Python version of the lab will be completed using the Linux Dev Instance. You will be instructed to use EC2 Instance Connect at the appropriate times.

For now, connect to the Windows Dev Instance using one of the following options if you are going to use the .NET or Java version of the lab. Otherwise move ahead to Task 1.2:


You can use the [editor on GitHub](https://github.com/keerthik26/AmazonPollyNotes/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/keerthik26/AmazonPollyNotes/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
