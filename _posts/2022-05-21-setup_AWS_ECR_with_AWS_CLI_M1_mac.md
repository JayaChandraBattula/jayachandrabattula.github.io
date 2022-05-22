---
layout: post
title:  "Configure AWS ECR with AWS CLI in Mac M1"
date:   2022-05-21 23:11:56 +0000
categories: jekyll update
---

- Authenticate your current AWS profile either by exporting AWS_ACCESS_KEY_ID and AWS_SECRET_ACCESS_KEY or by using `aws configure`
```
    aws_access_key_id=<aws_access_key_id>
    aws_secret_access_key=<aws_secret_access_key>
```

- Authenticate docker cli to  AWS ECR (private repository) so that docker can push and pull images

    ```
    aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
    ```
    - if you encounter the following error. `Error response from daemon: login attempt to https://public.ecr.aws/v2/ failed with status: 400 Bad Request  add --no-cli-auto-prompt` as shown below.

    ```
    aws --no-cli-auto-prompt ecr get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
    ```
    (--no-cli-auto-prompt flag disables automatically prompt for CLI input parameters)

- Authenticate docker cli to  AWS ECR (public repository) so that docker can push and pull images

    ```
    aws --no-cli-auto-prompt ecr-public get-login-password --region <region> | docker login --username AWS --password-stdin <account-id>.dkr.ecr.<region>.amazonaws.com
    ```

- create a repository to hold the images.

    ```
    aws ecr create-repository \
        --repository-name <repository-name> \
        --image-scanning-configuration scanOnPush=true \
        --image-tag-mutability IMMUTABLE \
        --region us-east-1
    ```

- Build you own docker image
    ```
    docker build -t <image-name> .
    ```

-Update the tag of the image which resembles the repository folder structure in AWS ECR.
    ```
    docker tag <current_image>:<tag> <account-id>.dkr.ecr.<region>.amazonaws.com/<repo-name>:latest
    ```

- Push the image to the ECR repo
    ```
    docker push <account-id>.dkr.ecr.<region>.amazonaws.com/<repo-name>:latest
    ```

- pull image from AWS ECR repo
    ```
    docker pull <account-id>.dkr.ecr.<region>.amazonaws.com/<repo-name>:latest
    ```

- Tag immutability: This is a key configuration while creating the repository. If tag immutability is enabled, ECR prevents image tags from being overwritten by subsequent images pushes with the same tag. Disabling this tag allows image tags to be overwritten.
    - commands to add mutable/immutable tags for an existing repository
        ```
        aws ecr put-image-tag-mutability --repository-name <name> --image-tag-mutability <MUTABLE/IMMUTABLE> --region <region>
        ```

https://docs.aws.amazon.com/AmazonECR/latest/userguide/getting-started-cli.html#cli-create-repository