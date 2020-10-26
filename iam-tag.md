# IAM 태그\(TAG\)기반 관리

## ABAC





![](.gitbook/assets/image%20%28124%29.png)



```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::841260513405:role/dev1-role",
            "Condition": {
                "StringEquals": {
                    "iam:ResourceTag/team": "${aws:PrincipalTag/team}"
                }
            }
        }
    ]
}
```



```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "ec2-instance-connect:SendSSHPublicKey",
            "Resource": "*",
            "Condition": {
                "StringEquals": {
                    "ec2:ResourceTag/team": "dev1"
                }
            }
        },
        {
            "Effect": "Allow",
            "Action": "ec2:Describe*",
            "Resource": "*"
        },
        {
            "Effect": "Deny",
            "Action": [
                "ec2:CreateTags",
                "ec2:DeleteTags"
            ],
            "Resource": "*"
        }
    ]
}
```

