# An example of schema only resource

A minimal resource type package needs:
 - .rpdk-config
 - schema.json
 - A handler file end with .jar or .zip

The handler file must present even it only contains dummy file.


### Commands to register this type
```
# Package schema only package
zip example-test-schemaonly.zip schema.json .rpdk-config placeholder.zip

# Upload the artifact to S3 
# Make sure you have a S3 bucket and grant cloudformation permission (https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/registry.html#registry-register)
# And configure like `export S3_BUCKET_NAME=<YOUR S3 BUCKET NAME>`
aws s3 cp example-test-schemaonly.zip s3://$S3_BUCKET_NAME

# Register type to CloudFormation
aws cloudformation register-type \
--type-name Example::Test::SchemaOnly \
--schema-handler-package s3://$S3_BUCKET_NAME/example-test-schemaonly.zip \
--type RESOURCE  

# Describe the registration token and wait it to be ready
aws cloudformation describe-type-registration --registration-token <REGISTRATION_TOKEN_RETURN_ABOVE_COMMAND>

# Describe the type
aws cloudformation describe-type --type "RESOURCE"  --type-name "Example::Test::SchemaOnly"

```


