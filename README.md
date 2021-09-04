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

In this task, you will create three Amazon S3 buckets:

- A website bucket which will host your front end website.
- An MP3 bucket which will store your MP3 files.
- A code bucket used to store your Lambda functions as they are uploaded from the AWS ToolKit in Eclipse.

You will only be creating the Amazon S3 buckets in this section and configuring them in a later step.

![lab-6-diagram3-2020](https://user-images.githubusercontent.com/22528198/131250867-061aa2f6-6bc9-4992-886f-5d0d3711f02c.png)

The diagram does not show the Code bucket since it is used to store your Lambda functions only.

294. From the browser tab logged into the **Amazon Management Console**, choose Services and select **Amazon S3**.

You can see that the region is Global. This is because Amazon S3 does not require region selection. However, Amazon S3 creates buckets in a region you specify. When creating your buckets, select the correct region based on what you noted before. If you are unsure of the region, verify the value with your instructor.

##### Task 5.1: (Java) - Create the Web bucket

295. Choose Create bucket
296. **Bucket name:** enter (all lowercase):
For example, if your name is John Smith, your bucket name would be polly-notes-web-john-smith.
297. Make sure that the Region field is set to the appropriate region that you have noted earlier.
298. Choose Create bucket

##### Task 5.2: (Java) - Create the MP3 bucket

In this task you will create another Amazon S3 bucket to hold the Amazon Polly audio files.

299. Choose Create bucket
300. **Bucket name:** (all lowercase):
For example, if your name is John Smith, your bucket name would be polly-notes-mp3-john-smith.
301. Make sure that the Region field is set to the appropriate region that you have noted earlier.
302. Choose Create bucket

##### Task 5.3: (Java) - Create the Code bucket

In this task you will create another Amazon S3 bucket to hold the java code.

303. Choose Create bucket
304. **Bucket name:** (all lowercase):
For example, if your name is John Smith, your bucket name would be **polly-notes-code-john-smith**.
305. Make sure the **Region** field is set to the appropriate region that you have noted earlier.
306. Choose Create bucket

### Task 6: Reviewing an IAM managed policy and creating an IAM role

In this task, you will review an IAM Managed Policy and create an IAM Role to allow your Lambda functions to:

- Perform CRUD operations on DynamoDB.
- Call the Amazon Polly API.
- Perform the PUT operation on Amazon S3 for the MP3.
- Log events to CloudWatch.

![lab-6-diagram3b-2020](https://user-images.githubusercontent.com/22528198/131251277-ea1b9025-cbf2-4967-9cb9-732a62f5a8e0.png)

307. From the browser tab logged into the **Amazon Management Console**, choose Services and select **IAM**.

You can see that the region is Global. IAM does not require region selection because it's a global service.

##### Task 6.1: (Java) - Review an IAM managed policy

308. To save time the **lambda_ddb_s3_polly** managed policy has already been created.
309.  The policy includes the following permissions:

``` 
    {
    "Version": "2012-10-17",
      "Statement": [
        {
          "Effect": "Allow",
          "Action": [
        "dynamodb:DeleteItem",
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:Query",
        "dynamodb:UpdateItem",
        "dynamodb:DescribeTable",
        "polly:SynthesizeSpeech",
        "s3:PutObject",
        "s3:GetObject",
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
    ],
                "Resource": ["*"]
            }
        ]
    }
```

#### Task 6.2: (Java) - Creating an IAM role
In this task, you will create an IAM Role called PollyNotesRole and attach the lambda_ddb_s3_polly policy to it. This Role will be used by the Lambda functions in the next sections. This IAM role allows you to delegate access with defined permissions in the above policy to trusted entities without having to hard code your credentials in your code.

310. In the navigation pane, choose **Roles**, and then choose Create role
311. Under **Select type of trusted entity**, choose **AWS service**.
312. Go to the **Choose a use case** section. Choose **Lambda**, and then, choose Next: Permissions
313. In the **Search** field, enter:
314. Select the **lambda_ddb_s3_polly** policy.
315. Choose Next: Tags
316. Choose Next: Review
317. For **Role name**, enter:
318. Choose Create role

You should see a message reading:

The role **PollyNotesRole** has been created

### Task 7: (Java) - Creating the Lambda ListFunction

In this task, you will create your Lambda List function. To do this, you edit the *ListFunction.java* file.

![lab-6-diagram4-2020](https://user-images.githubusercontent.com/22528198/131251622-6ec8c899-b773-4410-a693-d6e1110df2db.png)

#### Task 7.1: (Java) - Getting to the ListFunction code

319. Return to the **Windows Dev Instance**.
320. The PollyNotes project contains a few packages that are worth understanding for coding your ListFunction:

   - You will be editing the *ListFunction.java* file. In the Project Explorer, expand: and you will find the *ListFunction.java* file there. This is the file that you will do all of your development in.
   - Under , the **com.amazonaws.pollynotes.pojo** package contains POJO classes that are used to receive the data in the handler for Lambda. The Note will also be used to use the Object Persistence Model for DynamoDB so you can do simple operations on the Note POJO which already contains Java Annotations mapping each attributes to the object. It is worth taking a look at it to understand what was done.
   - Under , the **com.amazonaws.pollynotes.solution** package contains the Solution files to all of the four functions you will be creating. Note that the DictateFunction is already built hence why it doesn't have a solution. You can refer to the solution if you get stuck. Although the code of those functions looks fairly long, it is only due to the amount of logging done in the function. The actual code is only a few lines.
   - Under , the **com.amazonaws.pollynotes** package contains JUnit test cases that you can use to validate that the functions you create works. This will help you test your functions locally without having to upload them to Lambda.



### Task 7.2: (Java) - Coding the ListFunction

329. It is now time for you to code! You will need to edit the ListFunction.java. If you would rather not code and just use the Solution file, skip to ![Task 7.3: Upload Your ListFunction to Lambda]().

Your goal is to develop a Lambda function that will receive a userId as an input and return a list of Note objects.

Java is a strong type language which means that receiving JSON isn't as easy to parse as some other languages. Instead, Lambda and Java work together to deserialize the JSON payload into an object.

The input from Lambda will look similar to the following:

````
{
    "userId": "testuser"
}

````
   - It may look difficult to parse at first glance, however the Lambda Java core will do the work for you by serializing this JSON into a Note object that will have its userId property set and that you receive as an argument to your Lambda function as you can see in the function you need to develop below.

```
public List<Note> handleRequest(Note note, Context context) ...

```
   - The output that Lambda is an array of Note that should be similar to the following. This may sound like a challenge to return this kind of data. However, by returning a , the Lambda Java core will serialize the list of Note and output the above JSON data.
    
```
  [
    {
        "userId": "testuser",
        "noteId": "001",
        "note": "My note to myself"
    },
    {
        "userId": "testuser",
        "noteId": "002",
        "note": "My new note to myself"
    }
  ]
    
```
    
   - There are comments in the code to give you some help. However, it is your responsibility to make good use of those comments or to do it on your own. There are many ways to Query DynamoDB and the Solution Code makes use of the Object Persistence Model that was discussed in the class. You should take a look at the following:
    - Query and Scan
    - Query

   - Overall, the work you have to do is to query DynamoDB to find all of the notes of the userId based on the note argument and return a based on the response of your query. Good luck and remember that there is always a Solution file that can help you out! Once you think you have something working or would like to test things out, follow the next steps.

330. To test your code locally instead of having to upload your code to Lambda and invoke it from there, you will run a JUnit test. This will simulate a call to your Lambda handler with the userId . Even though this test runs locally, it's only your Lambda function that runs locally, your code is making an actual call to DynamoDB.


To test, complete the following steps:
    
- Open the file **ListFunctionTest.java** located in the package.
    
- Inspect the code of the test, which will make you realize that you are making a call to the CreateUpdateFunctionSolution function. Since this is a ListFunction, you need to make sure that there is data in the DynamoDB table. First thing to do is to create an item. Then, run your freshly coded ListFunction and make sure that the that is returned isn't empty.
    
- Run the test.
    - Open the context menu for the ListFunctionTest.java file.
    - Select **Debug As > JUnit Test**.
- You should see the output of the *CreateUpdateFunctionSolution* and then the beginning of your function in the Console View. If the Test successfully completes, the JUnit view should be all green. You can continue troubleshooting by using breakpoints and running the prior step again.

```
Initiating PollyNotes-CreateUpdateFunction...
Note received: {userId: "testuser", noteId: "001", note: "This is a test"}
Returning noteId: "001"
Initiating PollyNotes-ListFunction...
```

### Task 7.3: (Java) - Upload Your ListFunction to Lambda

In this task, you upload your Lambda function to Lambda. To do so, you will use the AWS ToolKit that has been installed in Eclipse. This simplifies the process instead of having to use the command line. What is done in the background is for you is to create a ZIP of your code, uploading it to your code Amazon S3 bucket, creating the Lambda function by using the ZIP, and associating the IAM Role you created earlier.

331. In the Project Explorer, open the context menu for the folder and select **Amazon Web Services >> Upload function to AWS Lambda...**

332. For **Select the Handler**, complete one of the following:

   - If you have coded your function, select .
   - If you want to use the Solution Code, select .

333. For **Select the AWS Region**, select the region for your lab as you noted it before.

334. Select **Create a new Lambda function** and replace the value of the field next to it (containing MyFunction) with .
335. Choose **Next**
336. For **IAM Role**, select .
337. For **S3 Bucket**, select your code Amazon S3 Bucket: 
338. In the **Memory (MB)** field, replace the content with **1024** so that your Lambda function can start faster on the first load.
339. Choose **Finish**

You have now uploaded your Lambda function!

340. The next step is to test the Lambda function from the Lambda UI.
341. From the **AWS Console**, choose Services and select **Lambda**.

This lab uses the **Updated Lambda Console**. If you are unsure which version you are using, simply choose the icon in the top-left corner of the Lambda console and it will indicate if the **Update Lambda Console option** is on or off. If it shows **off**, select the toggle option to turn it on.

342. Choose **PollyNotes-ListFunction**. If you don't see your function, make sure that you are in the correct Region.
343. Choose the **Test** tab and make the following selections under **Test event:**

   - .
   - **Template:**
   - **Event name**, enter:
   - Replace the JSON payload with the following:

    ```
    {
    "userId": "testuser"
    }
    
    ```
    
344. Choose Save Changes
345. Choose Test
346. There should be a green check mark just under the name of your Lambda function name with the message Execution result: succeeded (logs). Choose **Details** and you should see the output of your Lambda function listing a few notes that you have created in the DynamoDB section.

For example:

```
[
    {
        "userId": "testuser",
        "noteId": "001",
        "note": "My note to myself"
    },
    {
        "userId": "testuser",
        "noteId": "002",
        "note": "My new note to myself"
    }
]
```


### Task 8: (Java) - Creating the API Gateway

In this task, you will create a Restful API to front the back-end Lambda function. The API will use Amazon Cognito identities for authentication. For the application, you will be using the AWS API Gateway Service.

![lab-6-diagram5-2020](https://user-images.githubusercontent.com/22528198/131251981-5c13bee7-7f47-4bb0-a852-68c79a88e95c.png)

347. From the browser tab logged into the **Amazon Management Console**, choose Services and select **API Gateway**.

Verify that you are in the **correct region**. If you are unsure of the region, you can see it identified in the **Lab Information** section to the left of these instructions.

348. Under **Choose an API type**, select Build from the **REST API** card.

There is a similar option labeled as **REST API Private**. Please be mindful not to select this option.

349. In the **Create your first API** dialog box, choose OK
350. Under **Create new API**, select **New API**.
351. Enter the following formation: 

- **API name**, enter:
- **Description**, enter:
- **Endpoint Type**, select .

352. Choose Create API


#### Task 8.1: (Java) - Creating the Amazon Cognito Authorizer

In this task, you create the Amazon Cognito Authorizer. The Authorizer will allow you to control access to the API with the Amazon Cognito User Pool.

353. In the left navigation menu, choose **Authorizers**.
354. Choose + Create New Authorizer
355. Enter the following information:

   - **Name:**
   - **Type:**
   - **Cognito User Pool:**
   - **Token Source:**
   - **Token Validation**: Leave empty

356. Choose **Create**

#### Task 8.2: (Java) - Creating the resources

In this task, you create your API Resources.

![lab-6-diagram5-2020](https://user-images.githubusercontent.com/22528198/132088564-77268a69-c0d5-4fd8-bdc7-12dc510b77a6.png)

357. In the navigation pane, choose **Resources**.
358. Under **Resources**, select **/**.
359. Choose Actions , and then select **Create Resource**.
360. Under **Resource Name**, enter the following: 

The **Resource Path** will populate automatically with **notes**.

361. Choose Create Resource

Now that you have created your resource, you need to create your Methods.

#### Task 8.3: (Java) - Creating the API GET method

362. If not already selected, choose the **Resources** link and select **/notes**
363. Choose Actions , and then select **Create Method**.
364. Select **GET** and then choose the (check mark tick).
365. For **Integration type**, select **Lambda Function**.
366. Under **Lambda Function**, type an uppercase , then select your Lambda **PollyNotes-ListFunction**.
367. Choose Save
368. In the **Add Permission to Lambda Function** dialog box, choose OK
369. Choose **Method Request**
370. In the **Settings** section, next to **Authorization**, choose (Edit).
371. Select **PollyNotesPool** and then, choose (check mark tick).
372. In the breadcrumb, choose <- **Method Execution**
373. Choose **Integration Request**
374. Expand the **Mapping Templates** section.
375. Select **When there are no templates defined (recommended).**
376. Under **Content-Type**, choose **Add mapping template**
377. Enter and choose (check mark tick).
378. In the box that appeared below, enter the following:

```
{
    "userId": "testuser"
}
```

You are hard coding the value of userId here so that when you do your first test from the web interface, you can see the original notes you have created in DynamoDB. Since you won't be creating the CreateUpdate Function until after doing your test, you would have no way to create a note for your user. The first step once you are done fully testing the List function, will be to change this mapping template to be dynamic based on the Amazon Cognito ID of the user login.

379. Choose Save
380. Choose the **/notes** resource.
381. Choose Actions and select **Enable CORS**.
382. Under **Gateway Responses for PollyNotes API**, check option for **DEFAULT 4XX** and **DEFAULT 5XX**.
383. Choose Enable CORS and replace existing CORS headers
384. In the **Confirm method changes** dialog box, choose Yes, replace existing values
385. Choose Actions , then select **Deploy API**. Make the following updates:

- **Deployment stage:**
- **Stage name: **

386. Choose Deploy
387. Copy the **Invoke URL** and paste it into a file. You will need it later in the lab.
388. In the **prod Stage Editor**, choose the **Logs/Tracing** tab and make the following updates:

- **CloudWatch Settings**
    - **Enable CloudWatch Logs**
    - **Log level**
    - **Log full requests/responses data**

389. Choose Save Changes

### Task 9: (Java) - Creating the front-end website

In this task, you create the front-end website.

390. Download and extract the following zip file:

**Source file: ![ZIP FILE]()

391. Modify the *main.bundle.js* file (lines 2-4) and change the following values:

```
// Change the following 3 variable's value
var API_GATEWAY_INVOKE_URL = "https://your-api-url"
var COGNITO_POOL_ID = 'us-east-1-xxxxxxxxxxx'
var COGNITO_APP_CLIENT_ID = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
```
You should have noted the needed values from prior lab steps. If you do not have details, you can get them by using the Console and the following paths:

- **Services -> API Gateway -> PollyNotesAPI -> Stages -> prod (API Gateway Invoke URL)**
- **Services -> Cognito -> Manage User Pools -> PollyNotesPool (Cognito Pool ID)**
- **Services -> Cognito -> Manage User Pools -> PollyNotesPool -> App clients (Cognito App client ID)**

392. Save the changes to the file.
393. Next, you need to enable CORS on the MP3 Bucket. When users access your website, they will be loading the website from the static Website bucket. Then, they will use javascript in their browser to connect to two different endpoints; API Gateway and the MP3 bucket. You already enabled CORS on the API Gateway, but you now need to enable CORS on your MP3 bucket, so that it can set response headers.
394. From the browser tab logged into the **Amazon Management Console**, choose Services and select **Amazon S3**.
395. Choose your bucket.
396. Choose the **Permissions** tab.
397. Navigate down to the **Cross-origin resource sharing (CORS)** section, then click Edit
398. Copy and paste the following JSON code into the text box, then choose Save changes

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "GET"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": []
    }
]

```

399. Next, go to your Website bucket. In the breadcrumb trail, choose **Amazon S3**, and then, choose your bucket.
400. Choose Upload
401. Upload all the files and folders in the folder named (not the folder itself) by dragging them onto the upload window.

![drag](https://user-images.githubusercontent.com/22528198/132087733-28e74f35-8d8e-477c-bead-03fb0f773704.png)

402. Move to the bottom of the screen, and choose Upload
403. Once the files and folders have uploaded successfully (32 total), choose Close

![files-example](https://user-images.githubusercontent.com/22528198/132087763-d2c27a53-97b1-4ad4-8ab5-8e4e53c9c7ca.png)

There should be a combined total of **10 files and folders** in the **root** of the Amazon S3 bucket.

404. Choose the **Permissions** tab.
405. In the **Block public access (bucket settings)** card choose Edit
406. Unselect **Block** *all* **public access**.
407. Choose Save changes
408. A confirmation box will appear; type and choose Confirm
409. Move down to the **Bucket policy** card and choose Edit
410. Copy and paste the following bucket policy into the **Bucket policy** field.

Ensure that you replace your bucket name.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::polly-notes-web-<firstname>-<lastname>/*"
        }
    ]
}

```

411. Choose Save changes
412. The description of your bucket should now be flagged with Publicly accessible and the **Permissions overview** card will read **Access** as
Public
413. Choose the **Properties** tab.
414. In the **Static website hosting** card, choose Edit

Configure the following settings:

- **Static website hosting:** Enable
- **Index document: **
- **Error document:**

415. Choose Save changes
416. Move to the **Static website hosting** card and copy your *Bucket website endpoint* URL to a text editor.

You will use this again in the lab.

### Task 10: (Java) - Testing the web application

In this task, you test the web application by loading your Amazon S3 Hosted URL.

You can obtain your Amazon S3 Hosted URL by going into the Amazon S3 Console. Select your web hosting bucket. Choose the **Properties** tab, and then choose, **Static website hosting**.

417. In a browser, enter your Static website hosting URL.
418. You should be presented with a login page.

<img width="947" alt="pollynotes-loginpage" src="https://user-images.githubusercontent.com/22528198/131252081-16d9abe0-d29f-48a0-aa4f-9db65857c037.png">

419. Login with the following:
- For **UserName**, enter: 
- For **Password**, enter: 

420. You should be presented with a page that looks like the following:

<img width="947" alt="pollynotes-loginpage" src="https://user-images.githubusercontent.com/22528198/131252121-b08d611e-a1b3-4789-8320-877600be8100.png">

You should expect to see the notes you created earlier. All other functionality, such as adding notes, updating, deleting, or playing will not work. You need to create the API methods and Lambda functions to enable this functionality.

### Task 11: (Java) - Creating the remaining Lambda functions

Now that the ListFunction is working end-to-end, you need to create the rest of the application. Having a list of the notes is great, but if you can't Create, Update, Delete and Dictate, the application isn't doing much.

![lab-6-diagram6-2020](https://user-images.githubusercontent.com/22528198/131252165-72a5a6a8-002c-4ad8-81af-41e5ac552bbe.png)

Download the following ZIP file to your laptop: ![PollyNotes-JavaSolutionFunctions]().

#### Task 11.1: (Java) - Creating the Lambda DictateFunction

In this task, you work with the Lambda DictateFunction. This function is already completely coded for you due to the length of this lab. This is why you will not find Solution Code for Dictate.

This lab uses the **Updated Lambda Console**.

422. From the browser tab logged into the **Amazon Management Console**, choose Services and select **Lambda**.

436. Replace the JSON payload with the following: (492)
```
{
    "voiceId": "Russell",
    "note": {
        "userId": "testuser",
        "noteId": "001"
    }
}
```
439. Choose the **Details** link under that text. You should see the output of your Lambda function providing a very long URL to your MP3 bucket which represents a pre-signed URL.

For example:
```
"https://polly-notes-mp3-<firstname>-<lastname>.s3.amazonaws.com/testuser/001.mp3?X-Amz-Security-Token=..."
```

440. Copy and paste that URL in your browser. It will have you download an mp3. Open the file to listen to Russell dictating your note!

Congratulations! You have now created your Lambda Dictate function.


#### Task 11.2: (Java) - Creating the Lambda CreateUpdateFunction

455. Replace the JSON payload with the following: (492)

```
{
    "userId": "testuser",
    "noteId": "001",
    "note": "My very new note to myself"
}
```

458. Choose the **Details** link under that text. You should see the output of your Lambda function providing the noteId that you just uploaded. For example, 

Congratulations! You have now created your Lambda CreateUpdate function.

#### Task 11.3: (Java) - Creating the Lambda SearchFunction

473. Replace the JSON payload with the following: (492)

```
{
    "userId": "testuser",
    "note": "very"
}
```

476. Choose the **Details** link under that text. (495)

You should see the output of your Lambda function listing a the note you just searched for using the keyword that was just created in your previous CreateUpdateFunction.

```
[
    {
        "userId": "testuser",
        "noteId": "001",
        "note": "My very new note to myself"
    }
]
```
Congratulations! You have now created your LambdaSearch function.


#### Task 11.4: (Java) - Creating the Lambda DeleteFunction

477. Choose the Functions bread crumb at the top of the page.
478. Choose **Create function** and make the following updates:

- **Author from scratch**
- **Function name:**
- **Runtime:**
- **Permissions:**
    - Expand **Change default execution role.**
    - **Execution role:**
    - **Existing role:**

479. Choose **Create function**

*The lambda function page will be displayed with your function configuration.*

480. In the **Code source** section, choose Upload from , select **Upload a .zip or .jar file**, and then choose the Upload button. Locate the *PollyNotes-JavaSolutionFunctions.zip* file that you downloaded in the previous section and choose Save
481. Scroll down to the **Runtime settings** card, choose Edit and enter the following details:
- **Handler:**
482. Choose Save
483. Choose the **Configuration** tab.
484. Choose **Environment variables** from the options on the left and then choose Edit
485. Choose Add environment variable
486. Add the following values:

  - **Key:**
  - **Value:** (Update with your bucket name.)
 
487. Choose Save
488. Choose **General configuration** from the list on the left, then choose Edit and make the following update:
 - **Memory (MB):** (so that your Lambda function can start faster on the first load)
489. Choose Save
490. Now test the function. Choose the **Test** tab and configure the following fields under **Test event**:

- X
- **Template**:
- **Name**:

492. Replace the JSON payload with the following:

```
{
    "userId": "testuser",
    "noteId": "001"
}
```
493. Choose Save changes
494. Choose Test

*It is normal for it to take a little longer as this is the first time you are running your Lambda function.*

*There should be a green check mark just under the name of your Lambda function name on the left side of the screen with text mentioning: Execution result: succeeded (logs).*

495. Choose the **Details** link under that text. 

You should see the output of your Lambda function providing the noteId that you just deleted. For example, .
 

### Task 12: (Java) - Creating the remaining Restful API

In this task, you create the remaining Restful API.

![lab-6-diagram-2020](https://user-images.githubusercontent.com/22528198/131252279-3ea1752f-a912-4ecb-8b33-0da869868245.png)

496. From the browser tab logged into the **Amazon Management Console**, choose Services and select **API Gateway**.
497. Choose **PollyNotesAPI.**
498. Choose the **/notes/GET** method.
499. Choose **Integration Request**
500. Navigate to the bottom and expand **Mapping Templates**.
501. Choose **application/json**.
502. Move down and replace the mapping template with the following:
```
{
    "userId": "$context.authorizer.claims.sub"
}
```
*You are now removing the hard coded value of testuser to a variable that is dynamically populated based on your authorizer. In this case, the authorizer is Amazon Cognito. This means, userId will now be set to the value of the Amazon Cognito ID of the user login.*

503. Choose Save
504. Download the following Swagger file (API-Gateway-for-students-swagger.yaml):

 Source file: ![SWAGGER]()
 
505. Modify **line 11** of the file to replace it with your **Amazon Cognito User Pool ARN** (in between the quotes "").
506. **Save** the file.
507. Choose Actions and select **Import API**.
508. Navigate to the bottom of the page and change the **Import mode** to **Merge**.

Be mindful not to select .

509. Choose Select Swagger File and then select your saved Swagger file.
510. Choose Import

*The Swagger file created the additional resources and methods for your API. You still need to integrate the Methods with your Lambda functions.*

511. Choose the /notes/POST method.

*If you encounter an error reading, You do not have permission to perform this action. you can ignore it.*

*The reason for this error is due to the swagger file specifying a mock function from another region. API Gateway tries to pull it up and returns the error because you do not have permissions to that region.*

512. Choose **Integration Request**
513. For **Lambda Region**, choose (edit).
514. Select your **Lambda Region** based on what you have noted before and then, choose (check mark tick).
515. For **Lambda Function**, type P and then select **PollyNotes-CreateUpdateFunction**. Choose (check mark tick).
516. In the **Add Permission to Lambda Function** dialog box, make sure the Lambda function reflects the correct name.

- If the name is correct, choose **OK** to accept and allow invocation permissions to be created and your trigger to be setup.
- If it reads as the **Mock** function name, we have to do a couple additional steps to workaround this UI issue.
    - Choose **OK**.
    - Choose **Resources** link on the left to leave that section.
    - Next choose the appropriate method and select **Integration Request** and repeat the instructions as normal.
    - You should see that the region is set correctly but the **Lambda Function** is pointing to **arn:aws:lambda:us-west-2:012345678901:function:Mock** and needs to be updated.
    - There appears to be a bug with the API Gateway UI that causes this issue. It should be resolved soon. Until then, this is the workaround.

517. Repeat these steps for the remaining methods with the following mappings:

- **/search/GET** PollyNotes-SearchFunction
- **/{id}/DELETE** PollyNotes-DeleteFunction
- **/{id}/POST** PollyNotes-DictateFunction

Once you have completed the steps for each of the above methods, complete the following:

- Choose Actions and select **Deploy API**.
- Select the **prod** stage, and then choose **Deploy**.

Your API should now be fully functional.


### Task 13: (Java) - Testing the web application

In this task, you test the web application by loading your Amazon S3 Hosted URL.

You can obtain your Amazon S3 Hosted URL by going into the Amazon S3 Console. Select your web hosting bucket. Choose the **Properties** tab, and then, choose **Static website hosting**.

518. Test the application by exploring the functionality of the site. Test the add, delete, update, and search functions. Test that the voice options work correctly.

*Should you receive a backend server error, revisit the method integration steps and ensure the Lambda Function value is set correctly.*

- If you find any that read as, **arn:aws:lambda:us-west-2:012345678901:function:Mock/invocation**, go back and update those methods to the correct Lambda Function name.
- In case of errors, correct the code and run the test again.

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
