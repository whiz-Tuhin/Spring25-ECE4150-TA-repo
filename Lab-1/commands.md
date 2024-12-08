### Create an S3 Bucket for hosting the static website for the application

```bash
- aws s3api create-bucket --bucket <bucketname> --region us-east-1 (bucketname - staticwebsite-choephel-2024-4150)
- aws s3api delete-public-access-block --bucket <bucket-name>
- aws s3api put-bucket-policy --bucket <bucket-name> --policy file://<path>/s3-bucketpolicy.json (make sure you include the file:// prefix)
```

### Create an S3 Bucket for storing photos

```bash
- aws s3api create-bucket --bucket <bucketname> --region us-east-1 (bucketname - staticwebsite-choephel-2024-4150)
- aws s3api delete-public-access-block --bucket <bucket-name>
- aws s3api put-bucket-policy --bucket <bucket-name> --policy file://<path>/s3-bucketpolicy.json (make sure you include the file:// prefix)
```

### Create a DynamoDB table named PhotoGallery with a partition key and a sort key named PhotoID and CreationTime

```bash
aws dynamodb create-table \
    --table-name PhotoGallery \
    --attribute-definitions \
        AttributeName=PhotoID,AttributeType=S \
        AttributeName=CreationTime,AttributeType=N \
    --key-schema \
        AttributeName=PhotoID,KeyType=HASH \
        AttributeName=CreationTime,KeyType=RANGE \
    --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5

Output - 
{
    "TableDescription": {
        "AttributeDefinitions": [
            {
                "AttributeName": "CreationTime",
                "AttributeType": "N"
            },
            {
                "AttributeName": "PhotoID",
                "AttributeType": "S"
            }
        ],
        "TableName": "PhotoGallery",
        "KeySchema": [
            {
                "AttributeName": "PhotoID",
                "KeyType": "HASH"
            },
            {
                "AttributeName": "CreationTime",
                "KeyType": "RANGE"
            }
        ],
        "TableStatus": "CREATING",
        "CreationDateTime": 1733685889.122,
        "ProvisionedThroughput": {
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 5,
            "WriteCapacityUnits": 5
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:us-east-1:054037097987:table/PhotoGallery",
        "TableId": "3191bf80-2594-4c10-a001-8d5ae5b4c7bf"
    }
}
```