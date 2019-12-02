# reinvent-2019-hotel-recommender

Train and deploy Hotel Recommender Systems using the public Expedia data from Kaggle competition https://www.kaggle.com/c/expedia-hotel-recommendations/overview

**The goal is to predict which hotel type an Expedia customer will book!**

## Setup

- Python 3.6+ is required
- `make create_environment`, then `conda activate reinvent-2019-hotel-recommender` and, finally, `make requirements` in order to create a virtualenv
- AWS CLI is installed and `~/.aws/credentials` is configured with the right profile
- Download data (expedia-hotel-recommendations.zip) from https://www.kaggle.com/c/expedia-hotel-recommendations/data and unzip the downloaded file in the root directory of the project.
- Add (and execute) the following to your Jupyter notebook to create a Personalize Role:
    ```
    import boto3 
    
    session = boto3.Session(profile_name='<your-aws-profile>')
    iam = session.client("iam")
    
    role_name = "PersonalizeRole"
    assume_role_policy_document = {
        "Version": "2012-10-17",
        "Statement": [
            {
              "Effect": "Allow",
              "Principal": {
                "Service": "personalize.amazonaws.com"
              },
              "Action": "sts:AssumeRole"
            }
        ]
    }
    
    create_role_response = iam.create_role(
        RoleName = role_name,
        AssumeRolePolicyDocument = json.dumps(assume_role_policy_document)
    )
    
    # AmazonPersonalizeFullAccess provides access to any S3 bucket with a name that includes "personalize" or "Personalize" 
    # if you would like to use a bucket with a different name, please consider creating and attaching a new policy
    # that provides read access to your bucket or attaching the AmazonS3ReadOnlyAccess policy to the role
    policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonPersonalizeFullAccess"
    iam.attach_role_policy(
        RoleName = role_name,
        PolicyArn = policy_arn
    )
    
    time.sleep(60) # wait for a minute to allow IAM role policy attachment to propagate
    
    role_arn = create_role_response["Role"]["Arn"]
    print(role_arn)
    
    ```

## How To

`jupyter notebook`
