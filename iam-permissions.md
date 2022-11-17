# IAM Permissions for SageMaker Studio and SageMaker Notebook Instances

- From the AWS Console, search for IAM and navigate to the IAM Console
- Next Click Roles on the left hand side
- Then click your execution role for SageMaker Studio (AmazonSageMaker-ExecutionRole-XXXXXXX)
- Add the following policies to your execution role
    - AmazonTextractFullAccess
    - AmazonTranslateFullAccess
    - ComprehendFullAccess
    - ComprehendMedicalFullAccess
    
- Create a new Role using Custom Trust Policy and name it:  myDataAccessRole
  - Copy and paste the following JSON text
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": [
                    "comprehendmedical.amazonaws.com",
                    "comprehend.amazonaws.com"
                ]
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```
  - Attach the following Policies
    - AmazonS3FullAccess
    

- Add an inline policy and name it:  myPassThruPolicy
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "iam:PassRole",
            "Resource": "arn:aws:iam::*:role/*"
        }
    ]
}
```

