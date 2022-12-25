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

---

## Athena

Use Athena to get csv data from bucket to db.

---

## Quicksight

Use Quicksight to get data from db (Athena) to analytic dashboard.

---
