# Circle Ci process

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
  - install Front-End Dependencies
  - install API Dependencies
 4. setting up elastic benstalk
 5. install aws-cli
 6. configue aws access key
 7. deploy code

