# 01-csv-mail-to-db

---

## Schema

```txt
Application > mail + csv > WorkMail > SES > S3 Bucket #1 > Lambda > S3 Bucket #2 > Athena > Quicksight
```

---

## Sources

[Extract Email Attachment using AWS](https://towardsdatascience.com/extract-email-attachment-using-aws-624614a2429b)  
[Résolution des problèmes liés aux e-mails entrants Amazon SES](https://aws.amazon.com/fr/premiumsupport/knowledge-center/ses-troubleshoot-inbound-emails/)  
[S3 trigger to invoke a Lambda function](https://docs.aws.amazon.com/lambda/latest/dg/with-s3-example.html)  
[Configuration is ambiguously defined](https://aws.amazon.com/fr/premiumsupport/knowledge-center/lambda-s3-event-configuration-error/)  
[Create external table skipping first row](https://stackoverflow.com/questions/46452010/aws-athena-create-external-table-skipping-first-row)  
[Insufficient permissions](https://docs.aws.amazon.com/quicksight/latest/user/troubleshoot-athena-insufficient-permissions.html)

---

## WorkMail

Configure a dedicated email server using Amazon WorkMail:

- Create an organization  
- Select an email domain for the new email server  
- Create a user

---

## SES

Configure Amazon SES to send a copy of incoming email in raw format to Amazon S3:

- An active rule set called ‘INBOUND_MAIL’ is automatically created.  
- Create Rule under Amazon SES.  
- Add a recipient to personalize the rule.  
- Add an action for ‘S3’ by choosing a destination S3 bucket.

---

## Lambda

When create new lambda, select `Use a blueprint` type `s3` in filter and choose `Get S3 object` (Name = s3-get-object-python).

AWS lambda function in python to extract email attachment:

```python
import json
import boto3
import email
import os
from datetime import datetime
import re

def get_timestamp():
    current = datetime.now()
    return(str(current.year) + '-' + str(current.month) + '-' + str(current.day) + '-' + str(current.hour) + '-' + str(current.minute) + '-' + str(current.second))
    
def lambda_handler(event, context):
    # Get current timestamp
    timestamp = get_timestamp()
    
    # Initiate boto3 client
    s3 = boto3.client('s3')
    
    # Get s3 object contents based on bucket name and object key; in bytes and convert to string
    data = s3.get_object(Bucket=event['Records'][0]['s3']['bucket']['name'], Key=event['Records'][0]['s3']['object']['key'])
    contents = data['Body'].read().decode("utf-8")
    
    # Given the s3 object content is the ses email, get the message content and attachment using email package
    msg = email.message_from_string(contents)
    attachment = msg.get_payload()[1]
    fromAddress = msg['from']
    regex = "\\<(.*?)\\>"
    fromAddress = re.findall(regex, fromAddress)[0]

    # Write the attachment to a temp location
    open('/tmp/attach.csv', 'wb').write(attachment.get_payload(decode=True))
    
    # Upload the file at the temp location to destination s3 bucket and append timestamp to the filename
    # Destination S3 bucket is hard coded to 'legacy-applications-email-attachment'. This can be configured as a parameter
    # Extracted attachment is temporarily saved as attach.csv and then uploaded to attach-upload-<timestamp>.csv
    try:
        s3.upload_file('/tmp/attach.csv', '<destination bucket name>', fromAddress + '/attach-upload-' + timestamp + '.csv')
        print("Upload Successful")
    except FileNotFoundError:
        print("The file was not found")
    
    # Clean up the file from temp location
    os.remove('/tmp/attach.csv')
    
    return {
        'statusCode': 200,
        'body': json.dumps('SES Email received and processed!')
    }
```

Through the blueprint configure process role for source bucket is set.

For destination bucket add an inline policy to the role as follow:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket name>"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::<bucket name>/*"
            ]
        }
    ]
}
```

---

## S3 Bucket

In S3 Bucket, `Properties` tab, add an `Event notifications` to trigger lambda (the one above) execution on `Put` and `Post` event types.

---

## Glue Crawler

Configure a crawler to browse bucket which contain `csv` files to automatically discover data schema, create database and table.

---

## Athena

Use Athena to get csv data from bucket to db.

Sample SQL, in `Query editor` with a column containing seconds and name of it with parentheses (special characters):

```sql
SELECT * FROM "<db name>"."<table name>" ORDER BY "(s)" DESC limit 20;
SELECT COUNT("(s)") FROM "<db name>"."<table name>";
```

---

## Quicksight

Use Quicksight to get data from db (Athena) to analytic dashboard.

---
