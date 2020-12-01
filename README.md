# Making AWS RestAPI private and secure
![alt text](https://github.com/plygenom/aws_RestApi_Priv/blob/main/RestApi_Private.jpg?raw=true)
# Cloudformation tempalte 

The cfn template will created below resources 

1) DynamoDD table with (key: UserId sort-key: Department)
2) 3 NodeJS Lambda functions(select,upsert,delete) which does dynamoDB SDK operations and Lambda layer to send the access log to custom log-stream. 
3) rest-api with 3 resource method 

      1) select(GET) -> Integerated with Lambda  -> DynamoDB table (does `select` Item)
      2) upsert(POST) -> Integerated with Lambda proxy -> DynamoDB table (does `Insert and Update` Item)
      3) delete(POST) -> Integerated with Lambda proxy -> DynamoDB table (does `delete` Item)

2) IAM role with a limited policy to allow access only to dynamoDB table and cloudwatch log group  

## Pre-Req

1) Create gateway endpoint for AWS dynamoDB services and attach a policy 
2) Create interface endpoint for AWS API Gateway services and attach security group 
3) `aws ec2 describe-prefix-lists` get  prefixlist and private IP range of your VPC and update cfn mappings
4) Updated S3 details of lambda function code in cfn mappings section  

# Deploy the cloudformation template

1) download the template and update mappings accordingly
```powershell
git clone <<repo>>
cd << repo>>/cfn
copy `PrivateApi.yaml` to local 
```
2) Choose Parameters 

![alt text](https://github.com/plygenom/aws_RestApi_Priv/blob/main/cfn.png?raw=true)

3) Once 

```powershell
*** pre Req***

```

###  Testing Private API**

```powershell

```
