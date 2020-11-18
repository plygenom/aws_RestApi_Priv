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

```powershell
git clone <<repo>>
cd << repo>>
docker --version
docker build -t sample_node:latest .
docker images --filter=reference='sample*'
```

## Pre-Req

1) Create gateway endpoint for AWS dynamoDB services and attach a policy 
2) Create interface endpoint for AWS API Gateway services and attach security group 

# Publish the image to **private registry**

Example: To publish the builded image to AWS ECR repo.

```powershell
*** pre Req***
Import-Module -Name AWSPowerShell
Get-Module -Name AWSPowerShell

*** 1.login to AWS poweshell Module ***
Set-AWSCredential 
--verify with `Get-AWSCredential`

*** 2. create new ECR repo **
$RepositoryName="sample_node"
New-ECRRepository -RepositoryName $RepositoryName -ImageTagMutability IMMUTABLE -ImageScanningConfiguration_ScanOnPush $true

*** 3. Tag the docker image **
$ECRRepository=(Get-ECRRepository -RepositoryName $RepositoryName).RepositoryUri
docker tag sample_node:latest ${ECRRepository}:latest

*** 4. push the image to AWS ECR repo **
(Get-ECRLoginCommand).Password | docker login --username AWS --password-stdin (Get-ECRLoginCommand).Endpoint
docker push ${ECRRepository}:latest

```

###  To Remove Image & ECR repo**

```powershell
$ImageId=(Get-ECRImage -RepositoryName $RepositoryName)
Remove-ECRImageBatch -RepositoryName $RepositoryName -ImageId $ImageId

***To Remove ECR repo ***

Remove-ECRRepository -RepositoryName $RepositoryName
```
