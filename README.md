# DMS-migration-lab #

###A lab to demonstrate how Database Migration Services work###


This lab will provide you with basic understanding of using AWS Database Migration Service. We will use Cloudformation to create a registration webpage on one side using Cloudformation with VPC, Bastion Host, Web server and RDS. On the other side, we will create a single Server LAMP stack. Assume the LAMP stack is your existing production event registration site and you want to migrate it to AWS with minimum downtime.

For this workshop, we support 6 different region:

    N. Viginia(us-east-1)
    N. California(us-west-1)
    Tokyo(ap-northeast-1)
    Sydney(ap-southeast-2)
    Frankfurt(eu-central-1)
    London(eu-west-2)


We pick these region because later we will deploy the stack with the pre-selected AMI

### Step 0:
In AWS console, select the Region for our lab.
For example, we pick **N. Viginia(us-east-1)** for our lab practice.

### Step 1:
* Start up a AWS Cloud9 and install mysql.
* To set up AWS Cloud9 go to the AWS console, in the top right click Services and search/click Cloud9.
* Click Create Environment.
* Give it this environment a name and change nothing else.

### Step 2:
* Check if you already have a EC2 Key pair in your selected region.
* If not, create one through **AWS Console > EC2 > Key Pairs > Create Key Pair**.
* Remember to download the private key(.pem) and well saved.
* We will copy this PEM file into our AWS Cloud9 IDE created in step 1. In your AWS Cloud9 IDE top menu bar go to `File > Upload Local Files...`
* Once uploaded, in the AWS Cloud9 IDE console (bottom) find this PEM file and run ``` chmod 0400 XXXXX.pem ```

### Step 3:
* create the LAMP stack in the default VPC

* Create cloudformation stack: **AWS Console > Cloudformation > Create Stack > from S3 template >
https://s3-ap-northeast-1.amazonaws.com/workshop-data-public/cloudformation-workshop-20180731-vpc-bastion-rds.cfn.yml**
* For the stack configuration:
- Stack Name: Whatever you want to name
- Environment: dev
- Availability Zone 1: Pick 1 AZ
- Availability Zone 2: Pick a different AZ
- EC2 Key Pair: Select the Key Pair Name you setup in Step 2.
- DB Engine: mySql
- DB User/DB Password/DB Name: **Please remember what you input or use the default**
* Wait till the stack creation ready, the status will change to `CREATE_COMPLETE`
