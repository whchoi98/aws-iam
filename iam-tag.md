# IAM 태그\(TAG\)기반 관리

## ABAC





![](.gitbook/assets/image%20%28127%29.png)

4개의 EC2 인스턴스를 생성합니다. 

"AWS 관리 콘솔 - EC2"

* dev1-crm-01
* dev1-crm-02
* dev2-msa-01
* dev2-msa-02

![](.gitbook/assets/image%20%28144%29.png)

아래와 같이 만들어진 EC2 인스턴스의 Tag를 구성합니다.

![](.gitbook/assets/image%20%28137%29.png)

| EC2 Instance | Tag Key | Tag Value |
| :--- | :--- | :--- |
| dev1-crm-01,02 | name | dev1-crm-01,02 |
|  | team | dev1 |
|  | project | crm |
| dev2-msa-01,02 | name | dev2-msa-01,02 |
|  | team | dev2 |
|  | project | msa |

위임 정책 구성

각 그룹 또는 사용자에게  위임 역할을 구성합니다. dev1,dev2 2개의 위임정책을 구성하며, 이 때 사용되는 Resource의  ARN은 다음과 같습니다.

```text
arn:aws:iam::ACCOUNT-ID:role/ROLE-NAME
```



![](.gitbook/assets/image%20%28148%29.png)

dev1-sts-assume 정책

![](.gitbook/assets/image%20%28146%29.png)

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
                    "iam:ResourceTag/team": "dev1"
                }
            }
        }
    ]
}
```

![](.gitbook/assets/image%20%28140%29.png)

dev2-sts-assume 정책

```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::841260513405:role/dev2-role",
            "Condition": {
                "StringEquals": {
                    "iam:ResourceTag/team": "dev2"
                }
            }
        }
    ]
}
```

![](.gitbook/assets/image%20%28135%29.png)

EC2에 접근하기 위한 역할\(Role\)에 대한 정책을 구성합니다.



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
                    "ec2:ResourceTag/team": "dev1",
                    "ec2:ResourceTag/project": "crm"
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

![](.gitbook/assets/image%20%28141%29.png)

![](.gitbook/assets/image%20%28143%29.png)

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
                    "ec2:ResourceTag/team": "dev2",
                    "ec2:ResourceTag/project": "msa"
                    
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

![](.gitbook/assets/image%20%28129%29.png)

 역할 정의

![](.gitbook/assets/image%20%28130%29.png)

![](.gitbook/assets/image%20%28142%29.png)

![](.gitbook/assets/image%20%28125%29.png)

![](.gitbook/assets/image%20%28133%29.png)

![](.gitbook/assets/image%20%28149%29.png)

![](.gitbook/assets/image%20%28150%29.png)

![](.gitbook/assets/image%20%28138%29.png)

![](.gitbook/assets/image%20%28128%29.png)

![](.gitbook/assets/image%20%28152%29.png)

![](.gitbook/assets/image%20%28134%29.png)

![](.gitbook/assets/image%20%28145%29.png)

![](.gitbook/assets/image%20%28139%29.png)



