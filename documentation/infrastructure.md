# Hosting a Full-Stack Application

## Description
 this project is an application of hosting full stack application using 
 aws services like rds for managing databases, s3 for hosting the frontend
 and elastic benstalk for interaction between rds and s3
 also we applied ci/cd using circle ci

s3 live `http://abdallah-bucket-1.s3-website-us-east-1.amazonaws.com`

## S3
- S3 Standard offers high durability, availability, and performance object storage for frequently accessed data. Because it delivers low latency and high throughput. It is perfect for a wide variety of use cases including cloud applications, dynamic websites, content distribution, mobile and gaming applications, and Big Data analytics.In our application we used S3 for deploying our frontend.


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

## elastic benstalk

- Elastic Beanstalk is an orchestration service provided by Amazon AWS to create web applications easily and quickly we used it to deploy our backend


### elastic benstalk Service Setup
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



## RDS
- is a service provided by amazon to manage app database.

1. Choose your RDS database from the list of instances.
2. Scroll to the Connectivity & Security section then find the “VPC Security groups” and click on the  active security group link. This will directly redirect you to the security group you need to whitelist the IP address at.
3. Make sure the security group that belongs to your RDS database is selected/highlighted.
4. Click on Inbound Rules at the bottom. Then click Edit Inbound Rules.
5. In this last step you will just need to whitelist IP.





