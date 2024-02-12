# Explore ways to run Serverless application on AWS Lambda with Java 21 based Docker Container Image 

## Architecture

<p align="center">
  <img src="pure-lambda-java-21-docker-image/src/main/resources/img/app_arch.png" alt="Application Architecture"/>
</p>

## Project Description
The code example include storing and retrieving product from the Amazon DynamoDB. I put Amazon API Gateway in front of my Lambdas.


# Installation and deployment

Install Java Coretto 21  (https://docs.aws.amazon.com/corretto/latest/corretto-21-ug/amazon-linux-install.html

sudo yum install java-21-amazon-corretto  

Install Maven  

sudo yum install -y maven  

or ..

wget https://mirrors.estointernet.in/apache/maven/maven-3/3.8.5/binaries/apache-maven-3.8.5-bin.tar.gz  
tar -xvf apache-maven-3.8.5-bin.tar.gz  
sudo mv apache-maven-3.8.5 /opt/  


M2_HOME='/opt/apache-maven-3.8.5'  
PATH="$M2_HOME/bin:$PATH"  
export PATH  




```bash

Clone git repository locally
git clone https://github.com/Vadym79/AWSLambdaJavaDockerImage.git

1) For aws-pure-lambda-java21-docker-image project with public.ecr.aws/lambda/java:21 base image  

Compile and package the Java application with Maven from the root (where pom.xml is located) of the project
mvn compile dependency:copy-dependencies -DincludeScope=runtime

docker build --no-cache -t aws-pure-lambda-java21-custom-docker-image:v1 .
docker save aws-pure-lambda-java21-custom-docker-image > aws-pure-lambda-java21-custom-docker-image

aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com  

aws ecr create-repository --repository-name aws-pure-lambda-java21-custom-docker-image --image-scanning-configuration scanOnPush=true --region eu-central-1  

docker tag aws-pure-lambda-java21-custom-docker-image:v1 {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com/aws-pure-lambda-java21-custom-docker-image:v1

docker push {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com/aws-pure-lambda-java21-custom-docker-image:v1 
2) For aws-pure-lambda-java21-docker-image-alpine-linux project with amazoncorretto:21-alpine base image  

Project don't need to be compiled (happens during Docker build)

docker build --no-cache -t aws-pure-lambda-java21-custom-docker-image-alpine:v1 .
docker save aws-pure-lambda-java21-custom-docker-image-alpine > aws-pure-lambda-java21-custom-docker-image-alpine

aws ecr get-login-password --region eu-central-1 | docker login --username AWS --password-stdin {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com  

aws ecr create-repository --repository-name aws-pure-lambda-java21-custom-docker-image-alpine --image-scanning-configuration scanOnPush=true --region eu-central-1  

docker tag aws-pure-lambda-java21-custom-docker-image-alpine:v1 {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com/aws-pure-lambda-java21-custom-docker-image-alpine:v1

docker push {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com/aws-pure-lambda-java21-custom-docker-image-alpine:v1 

3) For aws-spring-boot-3.2-java21-custom-docker-image project with public.ecr.aws/lambda/java:21 base image  

mvn compile dependency:copy-dependencies -DincludeScope=runtime
docker build --no-cache -t aws-spring-boot-3.2-java21-custom-docker-image:v1 .

aws ecr create-repository --repository-name aws-spring-boot-3.2-java21-custom-docker-image --image-scanning-configuration scanOnPush=true --region eu-central-1  

docker tag aws-spring-boot-3.2-java21-custom-docker-image:v1 {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com/aws-spring-boot-3.2-java21-custom-docker-image:v1

docker push {aws_account_id}.dkr.ecr.eu-central-1.amazonaws.com/aws-spring-boot-3.2-java21-custom-docker-image:v1 


Deploy your application with AWS SAM
sam deploy -g  
```

## Further Readings 

You can read my article series "AWS Lambda SnapStart" on https://dev.to/vkazulkin/measuring-java-11-lambda-cold-starts-with-snapstart-part-1-first-impressions-30a4
