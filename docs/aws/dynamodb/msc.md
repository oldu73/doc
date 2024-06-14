# msc

---

## Insert data from csv

With AWS SDK for Python (Boto3)

[boto3 dynamo](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/dynamodb.html)

[How to set Region using Python and boto3 library](https://medium.com/@luiscelismx/how-to-set-region-using-python-and-boto3-library-807220a8a168)

[own github project boto3-001](https://github.com/oldu73/boto3-001)

main.py:

```py
import boto3
import uuid
from datetime import datetime, timezone
import csv

boto3.setup_default_session(region_name='us-east-1')

# Get the service resource.
dynamodb = boto3.resource('dynamodb')

# Instantiate a table resource object without actually
# creating a DynamoDB table. Note that the attributes of this table
# are lazy-loaded: a request is not made nor are the attribute
# values populated until the attributes
# on the table resource are accessed or its load() method is called.
table = dynamodb.Table('tableName')

# Print out some data about the table.
# This will cause a request to be made to DynamoDB and its attribute
# values will be set based on the response.
print(table.creation_date_time)

# Get the current time in UTC
now_utc = datetime.now(timezone.utc)

# Format the datetime object to a Zulu timestamp string
zulu_timestamp = now_utc.strftime("%Y-%m-%dT%H:%M:%SZ")

print("Current Zulu timestamp:", zulu_timestamp)

# Open the CSV file
with open('file.csv', mode='r', newline='') as file:
    # Create a DictReader object
    csv_dict_reader = csv.DictReader(file)

    # Iterate through the rows
    for row in csv_dict_reader:
        # Access the values using the column headers
        fileName = row['fileName']

        # Print the values (or do any processing you need)
        print(f"fileName: {fileName}")

        table.put_item(
            Item={
                'id': str(uuid.uuid1()),
                'createdAt': zulu_timestamp,
                'fileName': fileName,
                'updatedAt': zulu_timestamp,
                '__typename': 'Configuration',
            }
        )
```

---
