# IAM 역할\(Role\)

## 역할 \(Role\)

IAM 역할은 신뢰하는 개체에 권한을 부여하는 안전한 방법입니다.특정 권한을 가진 계정에 생성할 수 있는 IAM 자격 증명, IAM 역할은 IAM 사용자와 몇 가지 점에서 유사합니다. 역할과 사용자 모두 AWS에서 자격 증명으로 할 수 있는 것과 할 수 없는 것을 결정하는 권한 정책을 포함하는 AWS 자격 증명입니다. 그러나 역할은 한 사람과만 연관되지 않고 해당 역할이 필요한 사람이라면 누구든지 맡을 수 있어야 합니다. 또한 역할에는 그와 연관된 암호 또는 액세스 키와 같은 표준 장기 자격 증명이 없습니다. 그 대신, 역할을 수임하면 역할 세션을 위한 임시 보안 자격 증명을 제공합니다.

역할은 다음의 주체들이 사용할 수 있습니다.

* 동일한 AWS 계정의 IAM 사용자
* 역할과 다른 AWS 계정의 IAM 사용자
* Amazon Elastic Compute Cloud\(Amazon EC2\)와 같은 AWS가 제공하는 웹 서비스
* SAML 2.0, OpenID Connect 또는 사용자 지정 구축 자격 증명 브로커와 호환되는 외부 자격 증명 공급자\(IdP\) 서비스에 의해 인증된 외부 사용자

## Role Switch

계정번호 복사

![](.gitbook/assets/image%20%2858%29.png)

새로운 역할\(role\) 구성

![](.gitbook/assets/image%20%2887%29.png)

역할만들기

![](.gitbook/assets/image%20%2877%29.png)

역할 이름 구성

![](.gitbook/assets/image%20%2868%29.png)

생성된 역할 확인

![](.gitbook/assets/image%20%2871%29.png)

위임을 위한 새로운 정책 생성



![](.gitbook/assets/image%20%2869%29.png)



![](.gitbook/assets/image%20%2870%29.png)

```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::ACCOUNT-NUMBER:role/ROLE-NAME"
        }
    ]
}
```

![](.gitbook/assets/image%20%2885%29.png)

사용자에 권한 추가

![](.gitbook/assets/image%20%2886%29.png)

![](.gitbook/assets/image%20%2849%29.png)

![](.gitbook/assets/image%20%2872%29.png)

![](.gitbook/assets/image%20%2888%29.png)

![](.gitbook/assets/image%20%2873%29.png)

![](.gitbook/assets/image%20%2882%29.png)

![](.gitbook/assets/image%20%2883%29.png)

![](.gitbook/assets/image%20%2874%29.png)



## 교차계정 Role Switch

