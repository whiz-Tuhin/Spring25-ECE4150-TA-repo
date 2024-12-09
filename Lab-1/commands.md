### Create an S3 Bucket for hosting the static website for the application and add cors policy

```bash
- aws s3api create-bucket --bucket <bucketname> --region us-east-1 (bucketname - staticwebsite-choephel-2024-4150)
- aws s3api delete-public-access-block --bucket <bucket-name>
- aws s3api put-bucket-policy --bucket <bucket-name> --policy file://<path>/s3-bucketpolicy.json (make sure you include the file:// prefix)
- aws s3api put-bucket-cors --bucket my-example-bucket --cors-configuration file://cors-config.json
```

### Create an S3 Bucket for storing photos and add cors policy

```bash
- aws s3api create-bucket --bucket <bucketname> --region us-east-1 (bucketname - staticwebsite-choephel-2024-4150)
- aws s3api delete-public-access-block --bucket <bucket-name>
- aws s3api put-bucket-policy --bucket <bucket-name> --policy file://<path>/s3-bucketpolicy.json (make sure you include the file:// prefix)
- aws s3api put-bucket-cors --bucket my-example-bucket --cors-configuration file://cors-config.json
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

### Create a cognito user pool with name - PhotoGalleryUserPool

#### Step 1: Create the User Pool
```bash
aws cognito-idp create-user-pool \
    --pool-name PhotoGalleryUserPool \
    --policies PasswordPolicy="{MinimumLength=8,RequireUppercase=true,RequireLowercase=true,RequireNumbers=true,RequireSymbols=true}" \
    --auto-verified-attributes email \
    --admin-create-user-config AllowAdminCreateUserOnly=false \
    --account-recovery-setting '{"RecoveryMechanisms":[{"Priority":1,"Name":"verified_email"}]}'
```

Sample output
```json
{
    "UserPool": {
        "Id": "us-east-1_SbeSpjvX7",
        "Name": "PhotoGalleryUserPool",
        "Policies": {
            "PasswordPolicy": {
                "MinimumLength": 8,
                "RequireUppercase": true,
                "RequireLowercase": true,
                "RequireNumbers": true,
                "RequireSymbols": true,
                "TemporaryPasswordValidityDays": 7
            }
        },
        "DeletionProtection": "INACTIVE",
        "LambdaConfig": {},
        "LastModifiedDate": 1733691812.447,
        "CreationDate": 1733691812.447,
        "SchemaAttributes": [
            {
                "Name": "sub",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": false,
                "Required": true,
                "StringAttributeConstraints": {
                    "MinLength": "1",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "name",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "given_name",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "family_name",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "middle_name",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "nickname",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "preferred_username",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "profile",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "picture",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "website",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "email",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "email_verified",
                "AttributeDataType": "Boolean",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false
            },
            {
                "Name": "gender",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "birthdate",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "10",
                    "MaxLength": "10"
                }
            },
            {
                "Name": "zoneinfo",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "locale",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "phone_number",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "phone_number_verified",
                "AttributeDataType": "Boolean",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false
            },
            {
                "Name": "address",
                "AttributeDataType": "String",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "StringAttributeConstraints": {
                    "MinLength": "0",
                    "MaxLength": "2048"
                }
            },
            {
                "Name": "updated_at",
                "AttributeDataType": "Number",
                "DeveloperOnlyAttribute": false,
                "Mutable": true,
                "Required": false,
                "NumberAttributeConstraints": {
                    "MinValue": "0"
                }
            }
        ],
        "AutoVerifiedAttributes": [
            "email"
        ],
        "VerificationMessageTemplate": {
            "DefaultEmailOption": "CONFIRM_WITH_CODE"
        },
        "UserAttributeUpdateSettings": {
            "AttributesRequireVerificationBeforeUpdate": []
        },
        "MfaConfiguration": "OFF",
        "EstimatedNumberOfUsers": 0,
        "EmailConfiguration": {
            "EmailSendingAccount": "COGNITO_DEFAULT"
        },
        "AdminCreateUserConfig": {
            "AllowAdminCreateUserOnly": false,
            "UnusedAccountValidityDays": 7
        },
        "Arn": "arn:aws:cognito-idp:us-east-1:054037097987:userpool/us-east-1_SbeSpjvX7",
        "AccountRecoverySetting": {
            "RecoveryMechanisms": [
                {
                    "Priority": 1,
                    "Name": "verified_email"
                }
            ]
        }
    }
}
```

1.	--pool-name PhotoGalleryUserPool: Name of the User Pool.
2.	--policies: Configures the default Cognito password policy.
3.	--auto-verified-attributes email: Sets email verification.
4.	--admin-create-user-config: Enables self-registration.
5.	--account-recovery-setting: Allows self-service account recovery via verified email.

#### Step 2: Create the App Client
Create the app client MyCloudApp with the specified authentication flows and no secret

NOTE - take the user pool id from the output of the prevous step
```bash
aws cognito-idp create-user-pool-client \
    --user-pool-id <UserPoolId> \
    --client-name MyCloudApp \
    --no-generate-secret \
    --refresh-token-validity 30 \
    --explicit-auth-flows "ALLOW_REFRESH_TOKEN_AUTH" "ALLOW_USER_SRP_AUTH" "ALLOW_ADMIN_USER_PASSWORD_AUTH" "ALLOW_CUSTOM_AUTH" "ALLOW_USER_PASSWORD_AUTH" \
    --id-token-validity 3 \
    --access-token-validity 3
```

1.	--user-pool-id <UserPoolId>: Replace <UserPoolId> with the ID of the User Pool created in Step 1.
2.	--client-name MyCloudApp: Sets the name of the app client.
3.	--no-generate-secret: Specifies that the app client does not require a secret.
4.	--refresh-token-validity 30: Sets refresh token expiration to 30 days.
5.	--explicit-auth-flows: Allows the specified authentication flows.
6.	--id-token-validity and --access-token-validity: Set token expiration to 3 minutes.

Sample output

```json
{
    "UserPoolClient": {
        "UserPoolId": "us-east-1_SbeSpjvX7",
        "ClientName": "MyCloudApp",
        "ClientId": "4bq5qotn1hmrlil7k760cmcqll",
        "LastModifiedDate": 1733691997.612,
        "CreationDate": 1733691997.612,
        "RefreshTokenValidity": 30,
        "AccessTokenValidity": 3,
        "IdTokenValidity": 3,
        "TokenValidityUnits": {},
        "ExplicitAuthFlows": [
            "ALLOW_CUSTOM_AUTH",
            "ALLOW_USER_PASSWORD_AUTH",
            "ALLOW_ADMIN_USER_PASSWORD_AUTH",
            "ALLOW_USER_SRP_AUTH",
            "ALLOW_REFRESH_TOKEN_AUTH"
        ],
        "AllowedOAuthFlowsUserPoolClient": false,
        "EnableTokenRevocation": true,
        "EnablePropagateAdditionalUserContextData": false,
        "AuthSessionValidity": 3
    }
}
```

#### Step 3: Verification
```bash
aws cognito-idp list-user-pools --max-results 10
aws cognito-idp describe-user-pool --user-pool-id <UserPoolId> (NOTE -take this id from previous command)
aws cognito-idp describe-user-pool-client --user-pool-id <UserPoolId> --client-id <AppClientId> (NOTE - take this id from previous command)
```

### Create IAM Roles

#### Step 1: Create a new IAM role called ‘lambda_photogallery_role’ for Lambda service, as shown below. Attach policy called AmazonDynamoDBFullAccess to this role. Then attach policy called AmazonCognitoPowerUser to this role.

```bash
aws iam create-role \
    --role-name lambda_photogallery_role \
    --assume-role-policy-document '{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "lambda.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            }
        ]
    }'
```

Sample output
```json
{
    "Role": {
        "Path": "/",
        "RoleName": "lambda_photogallery_role",
        "RoleId": "AROAQZFG4VYBQHAK653MA",
        "Arn": "arn:aws:iam::054037097987:role/lambda_photogallery_role",
        "CreateDate": "2024-12-08T21:15:42Z",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "lambda.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
    }
}
```
#### Step 2:  Attach the AmazonDynamoDBFullAccess Policy
```bash
aws iam attach-role-policy \
    --role-name lambda_photogallery_role \
    --policy-arn arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess
```

#### Step 3: Attach the AmazonCognitoPowerUser Policy
```bash
aws iam attach-role-policy \
    --role-name lambda_photogallery_role \
    --policy-arn arn:aws:iam::aws:policy/AmazonCognitoPowerUser
```

#### Step 4: Verify the Role and Policies
```bash
aws iam get-role --role-name lambda_photogallery_role (role details)
aws iam list-attached-role-policies --role-name lambda_photogallery_role (attached policies)
```


#### Step 5: Create another IAM role called ‘apiS3PutGet-Role’ to upload the photos to the S3 Bucket service, as shown below. Attach policy called AmazonAPIGatewayPushToCloudWatchLogs to this role. Then attach policy called AmazonS3FullAccess to this role.

```bash
aws iam create-role \
    --role-name apiS3PutGet-Role \
    --assume-role-policy-document '{
        "Version": "2012-10-17",
        "Statement": [
            {
                "Effect": "Allow",
                "Principal": {
                    "Service": "apigateway.amazonaws.com"
                },
                "Action": "sts:AssumeRole"
            }
        ]
    }'
```
Sample output
```json
{
    "Role": {
        "Path": "/",
        "RoleName": "apiS3PutGet-Role",
        "RoleId": "AROAQZFG4VYBRNDVKAIVL",
        "Arn": "arn:aws:iam::054037097987:role/apiS3PutGet-Role",
        "CreateDate": "2024-12-08T21:22:01Z",
        "AssumeRolePolicyDocument": {
            "Version": "2012-10-17",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Principal": {
                        "Service": "apigateway.amazonaws.com"
                    },
                    "Action": "sts:AssumeRole"
                }
            ]
        }
    }
}
```

#### Step 6: Attach the AmazonAPIGatewayPushToCloudWatchLogs Policy
```bash
aws iam attach-role-policy \
    --role-name apiS3PutGet-Role \
    --policy-arn arn:aws:iam::aws:policy/service-role/AmazonAPIGatewayPushToCloudWatchLogs
```

#### Step 7: Attach the AmazonS3FullAccess Policy
```bash
aws iam attach-role-policy \
    --role-name apiS3PutGet-Role \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess
```

#### Step 8: Verify Creation
```bash
aws iam get-role --role-name apiS3PutGet-Role
aws iam list-attached-role-policies --role-name apiS3PutGet-Role
```

### Create API and Resources using API Gateway

#### Step 1: Create API Gateway: PhotoGalleryAPI
```bash
aws apigateway create-rest-api --name "PhotoGalleryAPI" --description "REST API for Photo Gallery App" --region us-east-1
```

Sample output
```json
{
"id": "r3jtubrv5f",
"name": "PhotoGalleryAPI",
"description": "REST API for Photo Gallery App",
"createdDate": "2024-12-09T07:04:31-05:00",
"apiKeySource": "HEADER",
"endpointConfiguration": {
"types": [
"EDGE"
]
},
"disableExecuteApiEndpoint": false,
"rootResourceId": "cuixa2oc76"
}
```

Moving forward, replace these values in the command with the values obtained above.
<api-id-generated-from-step-1> = id (eg, =r3jtubrv5f)
<root-id-generated-from-step-1> = rootResourceId (eg, =cuixa2oc76)

#### Step 2: Create resource and method for /signup

##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <root-id-generated-from-step-1> --path-part signup --region us-east-1 
```

This will return a <signup-resource-id-created>

Sample output
```json
{
    "id": "8dlxbk",
    "parentId": "cuixa2oc76",
    "pathPart": "signup",
    "path": "/signup"
}
```

<signup-resource-id-created> = id (eg, =8dlxbk )

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <signup-resource-id-created> --http-method POST --authorization-type "NONE" --region us-east-1
```

Sample output
```json
{
    "httpMethod": "POST",
    "authorizationType": "NONE",
    "apiKeyRequired": false
}
```

##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <signup-resource-id-created> --http-method POST --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_signup/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <signup-resource-id-created> with their respective values.

Sample output
```json
{
    "type": "AWS_PROXY",
    "httpMethod": "POST",
    "uri": "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:054037097987:function:photogallery_signup/invocations",
    "passthroughBehavior": "WHEN_NO_MATCH",
    "timeoutInMillis": 29000,
    "cacheNamespace": "ixgs6g",
    "cacheKeyParameters": []
}
```

#### Step 3: Create resource and method for /confirmemail

##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <root-id-generated-from-step-1> --path-part confirmemail --region us-east-1 
```

This will return a <confirmemail-resource-id-created>

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <confirmemail-resource-id-created> --http-method POST --authorization-type "NONE" --region us-east-1
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <confirmemail-resource-id-created> --http-method POST --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_confirmemail/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <confirmemail-resource-id-created> with their respective values.

#### Step 4: Create resource and method for /login

##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <root-id-generated-from-step-1> --path-part login --region us-east-1 
```

This will return a <login-resource-id-created>

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <login-resource-id-created> --http-method POST --authorization-type "NONE" --region us-east-1
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <login-resource-id-created> --http-method POST --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_login/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <login-resource-id-created> with their respective values.


#### Step 5: Create resource and method for /photos

##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <root-id-generated-from-step-1> --path-part photos --region us-east-1 
```

This will return a <photos-resource-id-created>

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <photos-resource-id-created> --http-method POST --authorization-type "NONE" --region us-east-1
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <photos-resource-id-created> --http-method POST --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_addphoto/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <photos-resource-id-created> with their respective values.



#### Step 6: Create resource and method for /photos (GET)

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <photos-resource-id-created> --http-method GET --authorization-type "NONE" --region us-east-1
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <photos-resource-id-created> --http-method GET --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_getphoto/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <photos-resource-id-created> with their respective values.
Use <photos-resource-id-created> created in Step 5

#### Step 7: Create resource and method for /photos/{id} 

##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <photos-resource-id-created> --path-part "{id}" --region us-east-1 --query 'id'
```

This will return a <id-photo-resource-id-created>

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <id-photo-resource-id-created> --http-method GET --authorization-type "NONE" --region us-east-1
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <id-photo-resource-id-created> --http-method GET --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_getphoto/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1>, <photos-resource-id-created> (Step 5) and <id-photo-resource-id-created> with their respective values.


#### Step 8: Create resource and method for /uploadphoto


##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <root-id-generated-from-step-1> --path-part uploadphoto --region us-east-1 
```

This will return a <upload-photo-resource-id-created>

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <upload-photo-resource-id-created> --http-method PUT --authorization-type "NONE" --region us-east-1 
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <upload-photo-resource-id-created> --http-method PUT --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:s3:path/photobucket-corporan-2021-4813/photos/{object}" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <upload-photo-resource-id-created> with their respective values.
-- {bucket-name} : photobucket-corporan-2021-4813
-- {object} : name of the file to be uploaded - updated dynamically from url




#### Step 9: Create resource and method for /search

##### Create Resource
```bash
aws apigateway create-resource --rest-api-id <api-id-generated-from-step-1> --parent-id <root-id-generated-from-step-1> --path-part search --region us-east-1 
```

This will return a <search-resource-id-created>

##### Create HTTP Method on the resource
```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-step-1> --resource-id <search-resource-id-created> --http-method POST --authorization-type "NONE" --region us-east-1
```


##### Integrate Lambda function with the API endpoint
```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-step-1> --resource-id <search-resource-id-created> --http-method POST --type AWS_PROXY --integration-http-method POST --uri "arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:<account-number>:function:photogallery_search/invocations" --region us-east-1
```

Replace <account-number>, <api-id-generated-from-step-1> and <search-resource-id-created> with their respective values.


#### Step 10: Allow only certain types of images (png, jpeg, jpg) to be uploaded

```bash
aws apigateway update-rest-api --rest-api-id <api-id-generated-from-command-1> --patch-operations "op=add,path=/binaryMediaTypes/image~1jpg"
```
```bash
aws apigateway update-rest-api --rest-api-id <api-id-generated-from-command-1> --patch-operations "op=add,path=/binaryMediaTypes/image~1jpeg"
```
```bash
aws apigateway update-rest-api --rest-api-id <api-id-generated-from-command-1> --patch-operations "op=add,path=/binaryMediaTypes/image~1png"
```

Sample output:
```json
{
    "id": "r3jtubrv5f",
    "name": "PhotoGalleryAPI",
    "description": "REST API for Photo Gallery App",
    "createdDate": "2024-12-09T07:04:31-05:00",
    "binaryMediaTypes": [
        "image/jpg",
        "image/png",
        "image/jpeg"
    ],
    "apiKeySource": "HEADER",
    "endpointConfiguration": {
        "types": [
            "EDGE"
        ]
    },
    "tags": {},
    "disableExecuteApiEndpoint": false,
    "rootResourceId": "cuixa2oc76"
}
```

#### Step 11: Allow CORS for each resource endpoint (Steps given for /signup)

Repeat the below steps for every resource endpoints created above:

##### A: Add OPTIONS Method

```bash
aws apigateway put-method --rest-api-id <api-id-generated-from-command-1> --resource-id <signup-resource-id-created> --http-method OPTIONS --authorization-type NONE --region us-east-1
```

Sample output:
```json
{
    "httpMethod": "OPTIONS",
    "authorizationType": "NONE",
    "apiKeyRequired": false
}
```

##### B: Link OPTIONS to MOCK integration:

```bash
aws apigateway put-integration --rest-api-id <api-id-generated-from-command-1> --resource-id <signup-resource-id-created> --http-method OPTIONS --integration-http-method MOCK --type MOCK --region us-east-1
```

Sample output:
```json
{
    "type": "MOCK",
    "passthroughBehavior": "WHEN_NO_MATCH",
    "timeoutInMillis": 29000,
    "cacheNamespace": "ixgs6g",
    "cacheKeyParameters": []
}
```

##### C: Define the Method response:

```bash
aws apigateway put-method-response --rest-api-id r3jtubrv5f --resource-id ixgs6g --http-method OPTIONS --status-code 200 --response-models '{\"application/json\": \"Empty\"}' --response-parameters '{\"method.response.header.Access-Control-Allow-Origin\": true, \"method.response.header.Access-Control-Allow-Headers\": true, \"method.response.header.Access-Control-Allow-Methods\": true}'--region us-east-1
```

Sample output:
```json
{
    "statusCode": "200",
    "responseParameters": {
        "method.response.header.Access-Control-Allow-Headers": true,
        "method.response.header.Access-Control-Allow-Methods": true,
        "method.response.header.Access-Control-Allow-Origin": true
    },
    "responseModels": {
        "application/json": "Empty"
    }
}
```



##### D: Define the Integration response:

<to-do>

Note: Replace <api-id-generated-from-step-1> and <sign-resource-id-created> with their respective values, and repeat for every endpoint.

#### Step 12: Create a new stage and deploy

##### Create deployment:
```bash
aws apigateway create-deployment \
    --rest-api-id <api-id-generated-from-command-1> \
    --stage-name dev \
    --region us-east-1
```

This will return a <deployment-id>

Sample output:

```json
{
    "id": "v4nxcd",
    "createdDate": "2024-12-09T08:43:13-05:00"
}
```

<deployment-id> = id (eg, =v4nxcd)

##### Create Stage and add the deployment:

```bash
aws apigateway create-stage \
    --rest-api-id <api-id-generated-from-command-1> \
    --stage-name dev \
    --deployment-id <deployment-id> \
    --region us-east-1
```

Sample output:
```json
{
    "deploymentId": "v4nxcd",
    "stageName": "dev",
    "cacheClusterEnabled": false,
    "cacheClusterStatus": "NOT_AVAILABLE",
    "methodSettings": {},
    "tracingEnabled": false,
    "createdDate": "2024-12-09T08:44:28-05:00",
    "lastUpdatedDate": "2024-12-09T08:44:28-05:00"
}
```