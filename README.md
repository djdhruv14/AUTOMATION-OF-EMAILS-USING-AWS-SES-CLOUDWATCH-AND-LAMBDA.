# AUTOMATION-OF-EMAILS-USING-AWS-SES-CLOUDWATCH-AND-LAMBDA.
The project focuses on automating scheduled emails using AWS services.

Technologies Utilized
1. Amazon Simple Email Service (SES):

Used for sending emails programmatically.

Ensures reliable and scalable email delivery.

2. AWS Lambda:

Facilitates serverless execution of code in response to events.

Handles the logic for composing and sending emails.

3. Amazon CloudWatch:

Monitors AWS resources and applications.

Triggers Lambda functions based on predefined schedules.


Implementation Overview
Email Automation Workflow:

CloudWatch Events are configured to trigger Lambda functions at specified intervals.

Upon invocation, the Lambda function executes code that utilizes SES to send emails to designated recipients.


Scheduling Mechanism:

CloudWatch Events (also known as EventBridge) are set up with cron expressions to define the timing of email dispatches.

This setup ensures emails are sent automatically without manual intervention.


Serverless Architecture:

By leveraging AWS Lambda, the project avoids the need for managing servers, leading to reduced operational overhead and cost.

The serverless model also allows for automatic scaling based on demand.


HERE IS THE LINK TO THE PROJECT VIDEO:

https://video.syr.edu/media/t/1_4s20gxcw





HERE IS THE CODE:

import boto3
import os
from botocore.exceptions import ClientError

def lambda_handler(event, context):
    # Configuration
    SENDER = "your-verified-sender@example.com"
    RECIPIENT = "recipient@example.com"
    AWS_REGION = "us-east-1"
    SUBJECT = "Automated Notification from AWS Lambda"
    # Email body
    BODY_TEXT = ("Hello,\r\n"
                 "This is a test email sent from AWS Lambda using Amazon SES.")
    BODY_HTML = f"""<html>
    <head></head>
    <body>
      <h1>Automated Email</h1>
      <p>This email was sent using <b>AWS Lambda</b> and <b>Amazon SES</b>.</p>
    </body>
    </html>"""
    CHARSET = "UTF-8"
    # Create a new SES client
    client = boto3.client('ses', region_name=AWS_REGION)
    # Try to send the email
    try:
        response = client.send_email(
            Destination={
                'ToAddresses': [RECIPIENT],
            },
            Message={
                'Body': {
                    'Html': {
                        'Charset': CHARSET,
                        'Data': BODY_HTML,
                    },
                    'Text': {
                        'Charset': CHARSET,
                        'Data': BODY_TEXT,
                    },
                },
                'Subject': {
                    'Charset': CHARSET,
                    'Data': SUBJECT,
                },
            },
            Source=SENDER,
        )
    except ClientError as e:
        print(f"Error: {e.response['Error']['Message']}")
    else:
        print(f"Email sent! Message ID: {response['MessageId']}")


