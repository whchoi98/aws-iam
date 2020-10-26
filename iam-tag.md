# IAM 태그\(TAG\)기반 관리

## ABAC

ABAC\(속성 기반 액세스 제어\)는 속성을 기반으로 권한을 정의하는 권한 부여 전략입니다. AWS에서는 이러한 속성을 _태그_라고 합니다. IAM 보안 주체 \(사용자 또는 역할\) 및 AWS 리소스에 태그를 연결할 수 있습니다. 그런 다음 태그 조건 키를 사용하여 해당 태그를 기반으로 보안 주체에 권한을 부여하는 정책을 정의할 수 있습니다. 태그를 사용하여 AWS 리소스에 대한 액세스를 제어하면 AWS 정책에 대한 변경 사항이 줄어들면서 팀과 리소스가 성장할 수 있습니다. ABAC 정책은 각 개별 리소스를 나열해야 하는 기존 AWS 정책보다 유연합니다.

## 태그\(Tag\)rlqks 액세스 권한 정의



![](.gitbook/assets/image%20%28127%29.png)

### 1.EC2 인스턴스 생성

![](.gitbook/assets/image%20%28131%29.png)

4개의 EC2 인스턴스를 현재 계정에서 생성합니다. 

"AWS 관리 콘솔 - EC2" 메뉴에서 아래와 같이 4개의 EC2 인스턴스를 생성합니다.

{% hint style="info" %}
EC2 인스턴스의 생성방법은 [https://awskocaptain.gitbook.io/imd-general/ec2/ec2-linux](https://awskocaptain.gitbook.io/imd-general/ec2/ec2-linux) 를 참조합니다.
{% endhint %}

아래와 같은 Tag:Name과 Value를 갖는 4개의 인스턴스를 구성하며 , 추가적으로 Tag를 생성합니다.

* dev1-crm-01
* dev1-crm-02
* dev2-msa-01
* dev2-msa-02

![](.gitbook/assets/image%20%28148%29.png)

아래와 같이 만들어진 EC2 인스턴스의 추가적인 Tag를 구성합니다.

![](.gitbook/assets/image%20%28139%29.png)

EC2 인스턴스 Tag Key와 Value

| EC2 Instance | Tag Key | Tag Value |
| :--- | :--- | :--- |
| dev1-crm-01,02 | name | dev1-crm-01,02 |
|  | team | dev1 |
|  | project | crm |
| dev2-msa-01,02 | name | dev2-msa-01,02 |
|  | team | dev2 |
|  | project | msa |

### 2. 위임 정책 구성

![](.gitbook/assets/image%20%28138%29.png)

각 그룹 또는 사용자에게  위임 역할을 구성합니다. dev1,dev2 2개의 위임정책을 구성하며, 이 때 사용되는 Resource의  ARN은 다음과 같습니다.

```text
arn:aws:iam::ACCOUNT-ID:role/ROLE-NAME
```

"AWS 관리 콘솔 - IAM - 정책 - 정책생성"을 선택하여, 각 목적에 맞는 위임정책을 생성합니다.

![](.gitbook/assets/image%20%28152%29.png)

dev1-sts-assume 정책을 아래와 같이 JSON 파일로 직접 생성합니다. 

![](.gitbook/assets/image%20%28150%29.png)

아래 코드를 복사해서 JSON 코드에 입력합니다.

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

위임정책의 이름은 "dev1-sts-assume" 이라고 정의합니다.

![](.gitbook/assets/image%20%28142%29.png)

앞서 생성한 위임정책과 동일하게 , dev2 그룹을 위한 dev2-sts-assume 정책을 아래 JSON 코드를 복사해서 생성합니다.

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

모두 생성이 완료되면 위임정책을 확인합니다. 

![](.gitbook/assets/image%20%28136%29.png)

### 3. 역할\(Role\)을 위한 정책 구성

![](.gitbook/assets/image%20%28146%29.png)

EC2에 접근하기 위한 역할\(Role\)에 대한 정책을 구성합니다. 미리 생성된 4개의 인스턴스는 이미 2개의 그룹으로 나뉘어져 있습니다. 1개는 Dev1 그룹 개발자들이 인스턴스에 접속할 수 있도록 정책을 구성하고, 1개는 Dev2 그룹 개발자들이 인스턴스에 접속할 수 있도록 정책을 구성합니다.

아래 JSON Code는 Dev1 그룹이 위임정책을 통해 Role에 접근할 때 , 연결될 정책입니다.  EC2 인스턴스의 "tag key -team, value - dev1", "tag key - project, value - crm" 태그가 있는 경우 접속을 허용합니다.

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

JSON 편집기를 통해 위의 코드를 복사해서 붙여넣습니다.

![](.gitbook/assets/image%20%28143%29.png)

정책의 이름을 "dev1-role-policy"라고 정의합니다.

![](.gitbook/assets/image%20%28147%29.png)

아래 JSON Code는 Dev2 그룹이 위임정책을 통해 Role에 접근할 때 , 연결될 정책입니다.  EC2 인스턴스의 "tag key -team, value - dev2", "tag key - project, value - msa" 태그가 있는 경우 접속을 허용합니다.

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

2개의 정책생성이 완료되면, 아래와 같이 모든 정책이 구성완료 됩니다.

![](.gitbook/assets/image%20%28129%29.png)

### 4. 역할 만들기

![](.gitbook/assets/image%20%28145%29.png)

 이제 역할을 정의합니다. 

**"AWS 관리 콘솔 - IAM - 역할 - 역할 만들기" 를 선택합니다.**

![](.gitbook/assets/image%20%28130%29.png)

다른 AWS 계정을 선택하고, 현재 계정  ID를 입력합니다.

![](.gitbook/assets/image%20%28144%29.png)

앞서 [역할을 위한 정책 구성](iam-tag.md#3-role) 단계에서 이미 정책을 생성하였습니다. 모든 단계를 건너 뛰고, 검토 단계에서 역할 이름을 정의합니다. 역할 이름을 dev1-role, dev2-role 을 각각 생성합니다. 

![](.gitbook/assets/image%20%28134%29.png)

![](.gitbook/assets/image%20%28153%29.png)

이제 생성된 각 dev1-role, dev2-role에 정책을 연결해 줍니다.

* dev1-role : dev1-role-policy
* dev2-role : dev2-role-policy

![](.gitbook/assets/image%20%28154%29.png)

![](.gitbook/assets/image%20%28140%29.png)

dev1-role,dev2-role에 태그를 설정합니다.

* dev1-role : tagkey-team, value-dev1 / tagkey-project, value-crm
* dev2-role : tagkey-team, value-dev2 / tagkey-project, value-msa

![](.gitbook/assets/image%20%28128%29.png)

### 5. 그룹에 정책 연결

각 그룹에 [위임정책구성](iam-tag.md#2)에서 생성한 정책을 연결해 줍니다.

* **Dev1 - dev1-sts-assume**
* **Dev2 - dev2-sts-assume**
* **Test - dev1-sts-assume**

그룹을 선택합니다.

![](.gitbook/assets/image%20%28156%29.png)

해당그룹의 **"권한-정책연결"**을 선택합니다.

![](.gitbook/assets/image%20%28135%29.png)

각 그룹에 적절한 정책을 선택합니다. 

![](.gitbook/assets/image%20%28149%29.png)

정상적으로 정책이 연결되었는지 확인합니다.

![](.gitbook/assets/image%20%28141%29.png)

### 6. 태그기반의 정책 확인

사용자 "hawkeye", "hulk","thor"가 EC2 인스턴스 태그 dev1-crm을 포함하는 인스턴스에 정상적으로 접속되는지 확인합니다. 또한 EC2 인스턴스 태그 dev2-msa 에 접속되는지 확인합니다.

사용자 "ironman","blackpanther"가 EC2 인스턴스 태그 dev2-msa를 포함하는 인스턴스에 정상적으로 접속되는지 확인합니다. 또한 EC2 인스턴스 태그 dev1-crm에 접속되는지 확인합니다.





