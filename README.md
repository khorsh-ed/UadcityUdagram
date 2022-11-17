# Hosting a Full-Stack Application

## Description
 this project is an application of hosting full stack application using 
 aws services like rds for managing databases, s3 for hosting the frontend
 and elastic benstalk for interaction between rds and s3
 also we applied ci/cd using circle ci

s3 live `http://abdallah-bucket-1.s3-website-us-east-1.amazonaws.com`

## dependencie
- aws-cli

## AWS

### IAM Service Setup

- Create IAM user Service with `Administrator Access`

- Configure the aws cli user with your terminal via `aws configure`

### S3 Service Setup

- open terminal  and run the following to create s3 bucket

```bash
aws s3api create-bucket --bucket abdallah-bucket-1 --region us-east-1
```
- Set Bucket Policy for S3 Bucket

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::abdallah-bucket-1/*"
            ]
        }
    ]
}

```
- in your s3 bucket properties go to static website hosing and enable it

- you should have a url of s3 live `http://abdallah-bucket-1.s3-website-us-east-1.amazonaws.com`

- upload you static files by run deploy script

```bash
aws s3 sync build/ s3://abdallah-bucket-1
```

### elastic benstalk
- add .elasticbeanstalk/ to .gitignore file
- Create an Elastic Beanstalk environment
 1. Intialize the eb with the eb init command.
 2. Create an environment running a sample application with the eb create command.
 3. set the environment variables using this command
    ```bash
       eb setenv AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID AWS_BUCKET=$AWS_BUCKET AWS_DEFAULT_REGION=$AWS_DEFAULT_REGION AWS_PROFILE=$AWS_PROFILE AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY DB_PORT=$DB_PORT JWT_SECRET=$JWT_SECRET PORT=$PORT POSTGRES_DB=$POSTGRES_DB POSTGRES_HOST=$POSTGRES_HOST POSTGRES_PASSWORD=$POSTGRES_PASSWORD POSTGRES_USERNAME=$POSTGRES_USERNAME URL=$URL
    ```
 4. Open config.yml in .elasticbeanstalk folder and add this

```yml
deploy:
  artifact: ./www/Archive.zip
```
5. upload backend api using eb deploy.

# Steps to configure CircleCI with AWS S3, RDS and Elastic Beanstalk

1. Create Circle CI Project
2. Setup Environment Variables

| Name                  |                         Value                         |
| --------------------- | :---------------------------------------------------: |
| AWS_REGION            |  The AWS region you used to provision RDS, S3 and EB  |
| AWS_ACCESS_KEY_ID      |                 Your AWS Access key ID               |
| AWS_SECRET_ACCESS_KEY |              Your AWS secret Access key               |
| AWS_S3_ENDPOINT       |             The url of the S3 hosted app.             |
| AWS_REGION            |  The AWS region you used to provision RDS, S3 and EB  |
| AWS_PROFILE           |                   Your AWS profile                    |
| AWS_BUCKET            | The name of the S3 bucket used to host the front end  |
| ----------------------|-------------------------------------------------------|
| POSTGRES_HOST         |         The url of the RDS database instance          |
| POSTGRES_DB           |                       postgres                        |
| POSTGRES_USERNAME     | The username specified when creating the RDS instance |
| POSTGRES_PASSWORD     | The password specified when creating the RDS instance |
| DB_PORT               |  The port of the RDS db instance (5432 for postgres)  |

3. configure the .circleci/config.yml by adding version, orbs, jobs, workflows
4. add root level package.json
5. create deploy.sh for api for deploying files to elastic benstalk
    ```bash
        eb init udagram-backend --region us-east-1 --platform node.js
        eb use udagram-backend
        eb setenv AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID AWS_BUCKET=$AWS_BUCKET AWS_DEFAULT_REGION=$AWS_DEFAULT_REGION AWS_PROFILE=$AWS_PROFILE AWS_SECRET_ACCESS_KEY=$AWS_SECRET_ACCESS_KEY DB_PORT=$DB_PORT JWT_SECRET=$JWT_SECRET PORT=$PORT POSTGRES_DB=$POSTGRES_DB POSTGRES_HOST=$POSTGRES_HOST POSTGRES_PASSWORD=$POSTGRES_PASSWORD POSTGRES_USERNAME=$POSTGRES_USERNAME URL=$URL
        eb deploy
    ```
6. create deploy.sh for frontend for deploying files to s3
    ```bash
    aws s3 cp --recursive --acl public-read ./www s3://abdallah-bucket-1
    aws s3 cp --acl public-read --cache-control="max-age=0, no-cache, no-store, must-revalidate" ./www/index.html s3://abdallah-bucket-1
    ```



## pipeline

 1. create environment
 2. prepare environment variable
 3. install dependencies
 4. setting up elastic benstalk
 5. install aws-cli
 6. configue aws access key
 7. checkout code
 8. deploy code
