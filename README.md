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

- [Connect to Your Windows Dev Instance from a Windows Machine using RDP]()
- [Connect to Your Windows Dev Instance from a macOS Machine using RDP]()

##### Task 1.2: Command window options

When lab instructions in subsequent sections require a command window, use a PowerShell session.

#### Task 2: Choose a programming language

4. Choose the programming language you would like to use for this lab. You can use the .NET, Java, or Python.

#### Task 3: (Java) - Amazon Cognito Authentication

The AWS SDK for Java simpliﬁes use of AWS Services by providing a set of libraries that are consistent and familiar for Java developers.

- [Learn more about the AWS SDK for Java](https://aws.amazon.com/sdk-for-java)

In this task, you will:

- Create an Amazon Cognito user pool, which you are going to use to authenticate your end users and control access through API Gateway, making sure that your endpoint is secure.
- Create and confirm an Amazon Cognito user using the Linux Dev Instance. The Amazon Cognito user is used to test the end to end application.

![lab-6-diagram1-2020](https://user-images.githubusercontent.com/22528198/131244291-8bce3292-9d91-44a2-aa7d-ba879d4a8a86.png)

For more information on Amazon Cognito User Pools, see: Amazon Cognito User Pools: [Amazon Cognito User Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-user-identity-pools.html)

##### Task 3.1: (Java) - Creating an Amazon Cognito user pool

247. From the browser tab logged into the Amazon Management Console, choose Services and select Cognito.

Verify that you are in the correct region. If you are unsure of the region, you can see it identified in the Lab Information section to the left of these instructions. Every service that is used in the lab (Amazon Cognito, API Gateway, Lambda, and DynamoDB) must be in the same region. Take a note of this region and every time the instructions prompt you to verify your region, make sure it is the same.

248. Choose Manage User Pools
249. Choose Create a user pool
250. For **Pool name**, enter: 
251. For **How do you want to create your user pool?**, choose **Step through settings**
252. Go to the **Which standard attributes do you want to require?** section.

This step can't be modified once the User Pool is created. To correct this, you would need to delete the current User Pool and create a new one.

253. Remove the checkbox next to **email** and choose Next step
254. In the **What password strength do you want to require?** section, you set the specifics required for the password. The password used is for testing purposes and you will be entering it many times so it has limited security settings.

Make the following configuration:

- **Minimum length**

Unselect the following options:

- Require numbers
- Require special character
- Require uppercase letters
- Require lowercase letters 

255. Choose Next step
In the navigation menu on the left, you will find a list of all of the main steps for the creation of the Amazon Cognito User Pool. As many settings can stay as the defaults, you will skip ahead and go to the ones that need to be modified.

256. Choose **App clients**.
257. Choose **Add an app client**
Make sure that you follow the next steps exactly as this configuration can't be undone. To correct, you would have to delete the current App client and create a new one.

- **App client name:**
- Unselect **Generate client secret.**

258. Choose Create app client
259. In the navigation menu on the left, choose **Review** and review all of the settings.
260. Choose Create pool
261. Save the **Pool Id** and the **Pool ARN** from your Amazon Cognito Pool information into a separate file for use later in this lab.

Make sure to note that it is for **Amazon Cognito Pool Id** and **Amazon Cognito Pool ARN** as well.

For example, in the Oregon region, the **Pool Id** would look like **us-west-2_XXXXXXXXX** and the **Pool ARN** would look like **arn:aws:cognito-idp:us-west-2:012345678901:userpool/us-west-2_XXXXXXXXX.

262. In the navigation pane on the left, under **General settings**, choose **App clients**.
263. Save the **App client id** in the form of xxxxxxxxxxxxxxxxxxxxxxxxxx information into the same file. Make sure to note that it is for the **Amazon Cognito App client id**.

##### Task 3.2: (Java) - Creating a user for the Amazon Cognito user pool

In this task, you will create and confirm an Amazon Cognito user by using the Linux Dev Instance. The Amazon Cognito user is used to test the end to end application.

You could connect to your Windows Dev Instance and open a PowerShell session to complete this step. However, to show another option, you will connect to the Linux Dev Instance using EC2 Instance Connect. It will connect you to a web-based ssh session.

264. From the browser tab logged into the **Amazon Management Console**, choose Services and select **EC2**.
265. Choose **Instances**.
266. Select **Linux Instance Dev** and then choose Connect
267. Select the **EC2 Instance Connect** tab.
268. Ensure the **User name field** is set to .
269. Choose **Connect**
270. Enter the following command, but make sure to change **change-me_app-client-id** with the value of the **App client id** that you copied to a file earlier:

``` markdown
aws cognito-idp sign-up --client-id change-me_app-client-id --username student --password student

**Expected output:**

{
    "UserConfirmed": false,
    "UserSub": "abc70c2f-52v7-4abc-c195-012abc8560ab"
}
```
The values in the output should show your specific configuration.

You have now created a user in Amazon Cognito with the username and a password of . Even though this isn't secure, you are using a simple username and password for simplicity during testing.

271. Next, confirm the user that you created. You will need to change **change-me_pool-id** with the value of the **Pool Id** that you copied to a file earlier.

To confirm the user, enter the following command:

`
aws cognito-idp admin-confirm-sign-up --user-pool-id change-me_pool-id --username student
`
If you don't see any output, it means that the command is successful and you can continue.

If the EC2 Instance Connect SSH session stops responding, hit the browser refresh button to re-initialize it.

272. Go back to the **Amazon Cognito** console, choose Manage User Pools
273. Under User Pools choose **PollyNotesPool**.
274. Under **General settings**, choose **Users and groups**.

You should see the username student and the status is **CONFIRMED**. If you do not see it, choose to refresh the browser.

Congratulations! You have successfully created an Amazon Cognito User Pool, manually added a user, and confirmed that user.

Normally, the sign-up process would be done via the application. However, the application that you are using doesn't provide such a function. So instead, it is manually created.

#### Task 4: (Java) - Creating a DynamoDB table

In this task, you will create a table in DynamoDB called pollynotes. This table is going to be used to store your user id, along with your notes.

![lab-6-diagram2-2020](https://user-images.githubusercontent.com/22528198/131245070-38e453d2-8718-4bc3-9dd0-10557a3e3223.png)

275. From the browser tab logged into the **Amazon Management Console**, choose Services and select **DynamoDB**.
276. Choose the **Tables** link on the navigation menu on the left side.
277. Choose **Create table**
278. For **Table name**, enter:
279. For **Primary key**, enter: (String)
280. Select **Add sort key**.
281. For **Add sort key**, enter: (String)
282. Choose **Create**
283. You should be placed on the pollynotes table pane. If not, choose the **pollynotes** table.
284. Choose the **Items** tab.
285. Choose **Create item**
286. In the **VALUE** field next to **userId**, enter:
287. In the **VALUE** field next to **noteId**, enter:
288. Choose the (plus circle icon) to the left of **noteId**.
289. Select **Append** , and then choose **String**.
290. In the **FIELD**, field enter:
291. In the **VALUE** field next to **note**, enter: 
292. Choose Save
293.  Create a few more items by going through the Create item process again. For example, your next noteId should be , then , and more. The userId should always be . The note can be anything that you would like, but be aware that it will be recited back by Amazon Polly at some point.

#### Task 5: (Java) - Creating Amazon S3 buckets
          
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
