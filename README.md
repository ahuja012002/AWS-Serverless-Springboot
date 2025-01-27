# AWS Serverless Services using Spring boot

In this Project, we will implement a full serverless solution using AWS Services and Java Spring boot as the framework for the processing.
We will implement the below solution : 

User Invoking API Gateway Rest API and upload file on S3.

S3 event notification triggers Lambda.

Lambda processes the records in the file and pushes them to SQS.

Another Lambda function consumes the records from SQS and saves it into Dynamo DB.

Here is the architecture diagram for the same :

![diagram-export-1-27-2025-1_59_22-PM](https://github.com/user-attachments/assets/1fed67ee-fd26-4aa2-aaf0-52646273c290)

## Step 1 : Create a S3 bucket.

Navigate to AWS Console and search for S3. Click Create Bucket.

<img width="1648" alt="Screenshot 2025-01-27 at 2 02 29 PM" src="https://github.com/user-attachments/assets/0f93d258-c868-404a-89af-16624d884dcc" />

Give name to S3 bucket and uncheck block all public access. Keep all other settings as default .

<img width="1655" alt="Screenshot 2025-01-27 at 2 03 32 PM" src="https://github.com/user-attachments/assets/29c08ee5-632b-4c78-9701-2de392edc890" />

This will create our S3 bucket.

## Step 2 : Create API Gateway Rest API

In the AWS Console , search for API Gateway and Select Rest API and Build.

<img width="1694" alt="Screenshot 2025-01-27 at 2 06 28 PM" src="https://github.com/user-attachments/assets/ffc11ee7-90a8-4d47-8a41-eb3b4dff1956" />

Give the name and description and Click Create API

<img width="1701" alt="Screenshot 2025-01-27 at 2 07 10 PM" src="https://github.com/user-attachments/assets/258ecb2c-e806-4e3e-89ed-0f8cdaf8bbcb" />

Once API is created, Click Create Resource :

Resource path should be left as it is and Resource-name should be {bucket}. This means we will pass our bucket name as path parameter.

Click Create Resource again and enter resource-name as {filename}

<img width="285" alt="Screenshot 2025-01-27 at 2 08 13 PM" src="https://github.com/user-attachments/assets/2bf7ef7e-a785-4f87-aee0-922fef8e1632" />


Lets now create API Gateway role to access s3

Navigate to API Gateway and select Create Role and select AWS Service as API Gateway

<img width="1469" alt="Screenshot 2025-01-27 at 2 15 53 PM" src="https://github.com/user-attachments/assets/0fd6e466-56cf-4898-b9aa-b28d4186714a" />

Clickk, next, next and Enter role name and hit submit.

Navigate to the newly created role and attach permissions tab . click Add permissions and add the below json :

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "s3:*",
            "Resource": "*"
        }
    ]
}

Now, Let us go back to API Gateway and Click Create Method.

Select method as PUT and AWS Service as S3 as shown in the screenshot. Rest all details should be default.

<img width="1645" alt="Screenshot 2025-01-27 at 2 22 26 PM" src="https://github.com/user-attachments/assets/79f5f870-081a-4ccd-8f78-f67ff4f0eb23" />

Now go to integration request add edit path variables.

<img width="1645" alt="Screenshot 2025-01-27 at 2 22 26 PM" src="https://github.com/user-attachments/assets/fafd62ad-de7b-4299-b4cb-648f9159bbb6" />

One last thing, Go to settings , we need to accept binary file types and enter */*

<img width="1462" alt="Screenshot 2025-01-27 at 2 26 26 PM" src="https://github.com/user-attachments/assets/ea4e66ee-ee56-42ad-8786-f88e30e2ca2f" />

Now click on deploy and enter stage name as dev.

Once it is deployed successdfully, we have not integrated API Gateway and S3.

## Step 3 : Create Lambda Function which will trigger when file gets uploaded to S3 and send the data to Amazon SQS.










