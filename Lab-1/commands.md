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