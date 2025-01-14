# GAME-DAY NOTIFICATION SOLUTION
## Project Overview

![Gameday Notification Solution (1)](https://github.com/user-attachments/assets/3b9c38d0-c9b5-45cb-a26d-f88ec0de5cc7)


This project is a serverless application that fetches sports game data from an external API and sends notifications to subscribers via Amazon Simple Notification Service (SNS). It uses AWS Lambda, Amazon EventBridge, and the SportsData.io API to deliver game updates in real time.

## Project Structure

```game-day-notifications/
   ├── src/
   │   ├── game-day-notifications.py
   ├── iam policies/
   │   ├── sns_policy.json 
   ├── .gitignore
   └── README.md`                  

## Prerequisites
Before you commence this project, you must have the following:

**1. Sportdata.io account**: 
Create a free tier account on [SportData ](https://sportsdata.io/)
Follow the steps to get your NBA API Key.

**2. AWS Credentials:**
If you dont already have an AWS account you can create a free tier account using this [link](https://signin.aws.amazon.com/signup?request_type=register).

## Project Walkthrough

**SNS**

*Creating an SNS Topic*
1. Navigate to SNS on your AWS Management Console.
2. Click on create topic and select the standard as the topic type.
3. Give your topic a unique name copy your SNS ARN (you will need it later) and click create topic.

*Adding subcribers to the SNS Topics*
1. Click the topic you previously created.
2. Navigate to the subscriptions tab and create subscription.
3. Set protocol to email and input the email address you want to receive notifications.
4. Click create subscription.
5. Go to your email and confirm your subscription.

**IAM**

*You will need to create an IAM Role and policy for both the SNS and Lambda.*

*SNS Policy*
1. Navigate to IAM on your AWS Management Console.
2. Click on Roles → Create Role
3. Click JSON and paste the JSON policy from sns_policy.json file
4. Replace REGION and ACCOUNT_ID with your AWS region and account ID.
5. Click Next: Review.
6. Enter a unique name for the policy (e.g., gameday_sns_policy).
7. Review and click Create Policy.

*Lambda Role*
1. Open the IAM service in the AWS Management Console.
2. Click Roles → Create Role.
3. Select Lambda as the AWS service.
4. Attach the LambdaBasicExecutionRole and the gameday_sns_policy you previously created.
5. Enter a unique name for the policy (e.g., gameday_lambda_role).

**Lambda Function**

*To create a lambda function;*
1. Navigate to Lambda Service on your AWS Management Console.
2. Click create function.
3. Select Author from scratch
4. Give your function a unique name (game-day-notifications) and select the Python 3.x from the dropdown as the runtiime.
5. Assign the IAM role you previously created to the function.

6. Under the Function Code section:
   
    a.Copy the content of the src/game-day-notifications.py file from the repository.
    b. Paste it into the inline code editor.
7. Under the Environment Variables section, add the following:
   
    a. NBA_API_KEY: your NBA API key.
    b.  SNS_TOPIC_ARN: the ARN of the SNS topic created earlier.
10. Click Create Function

**EventBridge**

*To setup eventbridge schedule to trigger lambda;*
1. Navigate to EventBridge on your AWS Management Console.
2. Click Rules → Create Rules.
3. Select Event Source: Schedule.
4. Set the cron schedule for when you want updates (e.g., hourly).
5. Under Targets, select the Lambda function (game-day-notifications) and save the rule.

**Test**
1. Open the Lambda function in the AWS Management Console.
2. Create a test event to simulate execution.
3. Run the function and check CloudWatch Logs for errors.
4. Verify you received a mail.
