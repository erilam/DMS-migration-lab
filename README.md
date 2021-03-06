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
* create an all-in-one LAMP stack in the default VPC as the source database.
* Create an all-in-one LAMP stack in an EC2 using this cloudformation template: https://raw.githubusercontent.com/erilam/DMS-migration-lab/master/source/LAMP-Stack-all-in-one.yaml
- DB User/DB Password/DB Name: **Please remember what you input or use the default**
* Wait till the stack creation ready, the status will change to `CREATE_COMPLETE`
* Setup mysql with username/password, create a database and table.
- ssh to the ec2: `ssh -i xxxx.pem ec2-user@your_ec2_ip`
- `sudo mysql -u root`
- `create database YOURE_DBName;`
- `create user 'YOUR_DBUser'@'localhost' identified by 'YOUR_DBPassword';`
- `grant all on YOURE_DBName.* to 'YOUR_DBUser' identified by 'YOUR_DBPassword';`
- `use YOURE_DBName;`
- `create table users (name VARCHAR(25), email VARCHAR(25), date DATE);
exit`

### Step 4:
* create an LAMP stack using VPC, EC2 and RDS as the destination database
* create VPC using this cloudformation template: https://raw.githubusercontent.com/erilam/DMS-migration-lab/master/source/vpc.cfn.yml
* ![vpc](https://github.com/aws-samples/startup-kit-templates/blob/master/images/vpc.png)
* (Optional) create bastion host using this cloudformation template: https://raw.githubusercontent.com/aws-samples/startup-kit-templates/master/vpc-bastion.cfn.yml
* ![bastion host](https://raw.githubusercontent.com/aws-samples/startup-kit-templates/master/images/vpc_bastion.png)
* create Website Server and RDS using this cloudformation template: https://raw.githubusercontent.com/erilam/DMS-migration-lab/master/source/EC2-Website-RDS-Database.yaml
- DB User/DB Password/DB Name: **Please remember what you input or use the default**
* Wait until Cloudformation CREATE_COMPLETE before move forward

### Step 9:
* Create a table in MySQL of RDS
* From Cloud9, SSH to the Website EC2 (or Bastion Host:) ssh -i XXXX.pem ec2-user@YOUR_WEBEC2_Public_IP
* Access the RDS MySQL: `mysql -h <RDS ENDPOINT> -P 3306 -u YOUR_DBUSER -p`
* Select the DB that your have created: `mysql> use YOUR_DBNAME`
* create the user table `mysql> CREATE TABLE users (name VARCHAR(25), email VARCHAR(25), date DATE);`
