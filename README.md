NBA Game Day Notifications and Alerts System

Project Overview:

This project is an alert system that sends real-time NBA game day score notifications to subscribed users via SMS/Email. 
It leverages Amazon SNS, AWS Lambda and Python, Amazon EvenBridge and NBA APIs to provide sports fans with up-to-date game information. The project demonstrates cloud computing principles and efficient notification mechanisms.

Features
Fetches live NBA game scores using an external API.
Sends formatted score updates to subscribers via SMS/Email using Amazon SNS.
Scheduled automation for regular updates using Amazon EventBridge.
Designed with security in mind, following the principle of least privilege for IAM roles.
Prerequisites
Free account with subscription and API Key at sportsdata.io
Personal AWS account with basic understanding of AWS and Python

ARCHITECTURE:

![image](https://github.com/user-attachments/assets/718bf05e-8c71-478f-9b83-cab30b65366c)


Technologies:
Cloud Provider: AWS
Services: SNS, Lambda, EventBridge
External API: NBA Game API (SportsData.io)
Programming Language: Python 3.x

IAM Security:
Least privilege policies for Lambda, SNS, and EventBridge.

SetUp Details:
- Open the AWS Management Console.
- Navigate to SNS service.
- Click Create Topic and select Standard as the topic type.
- Name the topic (e.g., NBA_topic) and note the ARN.
- Click Create Topic.

  ![image](https://github.com/user-attachments/assets/4ede6704-be66-47ec-bdf7-029a04ea0c67)


Add Subscriptions to the SNS Topic:
- After creating the topic, click on the topic name from the list.
- Navigate to the Subscriptions tab and click Create subscription.

![image](https://github.com/user-attachments/assets/f552c4af-3545-4a5b-bbe3-e19d862c31f8)


Select a Protocol:
For Email:
 > Choose Email.
 > Enter a valid email address.

![image](https://github.com/user-attachments/assets/b4dae61b-1412-4ec3-8e8f-51c55b58af61)


Click Create Subscription.
If you added an Email subscription:
Check the inbox of the provided email address.
Confirm the subscription by clicking the confirmation link in the email.

![image](https://github.com/user-attachments/assets/df71c0ab-64f6-4c97-b6b1-f63c7360debe)


Create the SNS Publish Policy:
 - Open the IAM service in the AWS Management Console.
 - Navigate to Policies → Create Policy.

![image](https://github.com/user-attachments/assets/c4976614-9ffb-445a-bd97-6ed18adf35bd)

 - Click JSON and paste the JSON policy from gd_sns_policy.json file
 - Replace REGION and ACCOUNT_ID with your AWS region and account ID.
 - Click Next: Tags (you can skip adding tags).

![image](https://github.com/user-attachments/assets/916f5ca7-5636-47bb-8ba6-b9e6c48bbdab)

 - Click Next: Review.
 - Enter a name for the policy (e.g., NBA_sns_policy).
 - Review and click Create Policy.

   ![image](https://github.com/user-attachments/assets/43589c85-892f-4aad-ac05-fe201c88aa10)


Create an IAM Role for Lambda:
Open the IAM service in the AWS Management Console.
 - Click Roles → Create Role.

![image](https://github.com/user-attachments/assets/4a3e1857-c147-4d27-a3a8-8fbfa3135e74)

 - Select AWS Service and choose Lambda.

![image](https://github.com/user-attachments/assets/d73d6502-3d97-4139-99a0-6b5d9e85f171)


![image](https://github.com/user-attachments/assets/cd71bf2a-4636-447e-9aaa-8201bdf44291)


Attach the below policies:
 - SNS Publish Policy (NBA_sns_policy) (created in the previous step).

![image](https://github.com/user-attachments/assets/e045c638-b185-431c-9d15-55b942b29ced)

 - Lambda Basic Execution Role (AWSLambdaBasicExecutionRole) (an AWS managed policy).

![image](https://github.com/user-attachments/assets/22e4f026-36fd-48df-a160-288349f36a76)


> Click Next: Tags (you can skip adding tags).
> Click Next: Review.
> Enter a name for the role (e.g., NBA_role).

![image](https://github.com/user-attachments/assets/c2d69c42-ce5d-4130-8176-0d32c7f8371d)

> Review and click Create Role.
> Copy and save the ARN of the role for use in the Lambda function.

Deploy the Lambda Function:
> Open the AWS Management Console and navigate to the Lambda service.
> Click Create Function.

![image](https://github.com/user-attachments/assets/801c2bdb-8af0-45e2-8c4e-ff08c2da0505)

> Select Author from Scratch.
> Enter a function name (e.g., NBA_notifications).
> Choose Python 3.x as the runtime.
> Assign the IAM role created earlier (NBA_role) to the function.

![image](https://github.com/user-attachments/assets/9d31ef63-8ad7-497e-bf09-cfc3ead3e35a)

Under the Function Code section:
Copy the content of the src/NBA_notifications.py file from the repository.
Paste it into the inline code editor.

![image](https://github.com/user-attachments/assets/0c6c84a4-fafc-46bb-98c8-5fcd8b94727f)

Under the Environment Variables section, add the following:
 - NBA_API_KEY: your NBA API key.
 - SNS_TOPIC_ARN: the ARN of the SNS topic created earlier.
Click Create Function.

![image](https://github.com/user-attachments/assets/a41015e9-eee0-42be-ba50-aee71ab651dd)

Set Up Automation with Eventbridge:
 - Navigate to the Eventbridge service in the AWS Management Console.
 - Go to Rules → Create Rule.
 - Select Event Source: Schedule.

![image](https://github.com/user-attachments/assets/93650eca-5765-4510-851f-902d3c09dc17)

 - Set the cron schedule for when you want updates (e.g., hourly).

![image](https://github.com/user-attachments/assets/e5f0a619-8b33-45f2-aa53-f98bd2aec0a1)

 - Under Targets, select the Lambda function (gd_notifications) and save the rule.

![image](https://github.com/user-attachments/assets/932c08b6-cf42-4ff5-804d-4ee4d3e0624c)


Test the platform:
1. Open the Lambda function in the AWS Management Console.
2. Create a test event to simulate execution.

![image](https://github.com/user-attachments/assets/0ce0cc14-1a30-4706-b225-21afa54acc3f)

4. Run the function and check CloudWatch Logs for errors.

![image](https://github.com/user-attachments/assets/b15aa455-9d2e-42a4-be30-80ac8c60b18b)

6. Verify that SMS notifications are sent to the subscribed users.

![image](https://github.com/user-attachments/assets/a676bee6-3eb4-4b0e-98db-023d6d6ca859)

What We Learned:
> Designing a notification system with AWS SNS and Lambda.
> Securing AWS services with least privilege IAM policies.
> Automating workflows using EventBridge.
> Integrating external APIs into cloud-based workflows.

Future Modification:
1. Add NFL score alerts for extended functionality.
2. Store user preferences (teams, game types) in DynamoDB for personalized alerts.
3. Implement a web UI


