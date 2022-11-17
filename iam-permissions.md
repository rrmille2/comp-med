# IAM Permissions for running sm-docker in SageMaker Studio or Notebook

- From the AWS Console, type IAM and go to the IAM Console
- Next Click Roles on the left hand side
- Then click your execution role for SageMaker Studio (AmazonSageMaker-ExecutionRole-XXXXXXX)
- Add the following policies to your execution role
    - AWSCodeBuildAdminAccess
    - CloudWatchFullAccess
    - EC2InstanceProfileForImageBuilderECRContainerBuilds
- Add an inline policy and name it "codebuild-passthru"
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::*:role/*",
            "Condition": {
                "StringLikeIfExists": {
                    "iam:PassedToService": "codebuild.amazonaws.com"
                }
            }
        }
    ]
}
```
- Lastly modify the Trust Relationship to look like the following
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "codebuild.amazonaws.com",
                    "sagemaker.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
