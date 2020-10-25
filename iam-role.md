# IAM 역할\(Role\)과 Switch

## 역할 \(Role\)

IAM 역할\(Role\)은 신뢰하는 개체에 권한\(Permission\)을 부여하는 안전한 방법입니다.특정 권한을 가진 계정에 생성할 수 있는 IAM 자격 증명, IAM 역할은 IAM 사용자와 몇 가지 점에서 유사합니다. 역할과 사용자 모두 AWS에서 자격 증명으로 할 수 있는 것과 할 수 없는 것을 결정하는 권한 정책을 포함하는 AWS 자격 증명입니다. 그러나 역할은 한 사람과만 연관되지 않고 해당 역할이 필요한 사람이라면 누구든지 맡을 수 있어야 합니다. 또한 역할에는 그와 연관된 암호 또는 액세스 키와 같은 표준 장기 자격 증명이 없습니다. 그 대신, 역할을 수임하면 역할 세션을 위한 임시 보안 자격 증명을 제공합니다.

역할\(Role\)은 하나 이상의 정책\(Policy\)을 기반으로 구성됩니다. 앞서 소개한 데로 정책\(Policy\)는 서비스 별로 할 수 있는 작업을 정의하고 있는 규칙들입니다.

역할은 다음의 주체들이 사용할 수 있습니다.

* 동일한 AWS 계정의 IAM 사용자
* 역할과 다른 AWS 계정의 IAM 사용자
* Amazon Elastic Compute Cloud\(Amazon EC2\)와 같은 AWS가 제공하는 웹 서비스
* SAML 2.0, OpenID Connect 또는 사용자 지정 구축 자격 증명 브로커와 호환되는 외부 자격 증명 공급자\(IdP\) 서비스에 의해 인증된 외부 사용자

## Role Switch

### 1.RBAC\(Role-Based Access Control\)

 RBAC을 사용하게 되면 그룹 또는 사용자에게 직접 권한을 주지 않고, 여러 권한의 논리적인 집합들을 역할\(Role\)로 만들고 그룹 또는 사용자에게 연결 할 수 있습니다. 이렇게 되면 필요에 따라 역할을 부여할 수 있으므로 복잡하고, 경직되게 권한을 관리하지 않아도 됩니다. 

### 2. Role Switch 구성

사용자 "thor"는 그룹 "Test"에 속해 있으며, 그룹에는 정책이 구성되어 있지 않습니다. 또한 사용자에는 "EC2ReadOnly", "S3ReadOnly" 정책이 할당되어 있습니다. 

사용자 "thor"는 해당 계정에서 , 시험 목적으로 EC2FullAccess, S3FullAccess 권한이 필요합니다. 이러한 경우 정책을 할당하고, 제거하는 방식은 번거로울 수 있습니다. 

Role Switch는 역할을 이용해서 간단하게 , 필요한 역할을 그룹이나 사용자에게 할당 할 수 있습니다.

![](.gitbook/assets/image%20%2858%29.png)

 먼저 현재 계정을 복사 해 둡니다.

![](.gitbook/assets/image%20%2875%29.png)

### 3.새로운 역할\(role\) 구성

새로운 역할을 만들기 위해서 "AWS 관리 콘솔 - IAM - 역할 - 역할만들기"를 선택합니다.

![](.gitbook/assets/image%20%28109%29.png)

다른 AWS 계정을 선택합니다. 계정 ID는 앞서 복사 해둔 현재 계정을 입력합니다.

![](.gitbook/assets/image%20%2892%29.png)

### 4. 역할 만들기-권한정책 연결

EC2FullAccess,S3FullAccess 정책을 선택합니다.

![](.gitbook/assets/image%20%28118%29.png)

### 5.역할 이름 구성

역할\(Role\)의 이름을 구성합니다.

![](.gitbook/assets/image%20%2879%29.png)

### 6.생성된 역할 확인

생성된 역할을 확인하고, 역할 ARN을 복사해 둡니다.

![](.gitbook/assets/image%20%2885%29.png)

### 7.위임을 위한 새로운 정책 생성

이제 사용자가 새로 만든 역할\(Role\)을 사용하기 위해, STS 기반의 위임\(Assume\) 정책을 생성합니다. 이 정책은 고객관리형 정책으로 해당 계정에서만 존재하게 됩니다.

**"AWS 관리 콘솔 - IAM - 정책 - 정책생성"**을 선택합니다.

![](.gitbook/assets/image%20%2882%29.png)

정책 생성에서 시각적 편집기 또는 JSON으로 정책을 생성합니다.

* **서비스 - STS**
* **작업 - STS에서 허용되는 작업 지정 - AssumeRole**
* **리소스 - 앞서 복사해둔 Role의 ARN을 입력합니다. 아래와 같은 구성입니다.**

  ```text
  Resource": "arn:aws:iam::ACCOUNT-NUMBER:role/ROLE-NAME
  ```

![](.gitbook/assets/image%20%2884%29.png)

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

정책이름을 정의합니다.

![](.gitbook/assets/image%20%28106%29.png)

### 8.사용자에 권한 추가

앞서  생성한 정책 권한을 추가합니다.

**"AWS 관리 콘솔 - IAM -사용자 - "thor" - 요약 - 권한 추가"**

![](.gitbook/assets/image%20%28107%29.png)

앞서 생성한 고객관리형 기반 정책으로 만들어진, 위임을 위한 정책을 선택합니다.

![](.gitbook/assets/image%20%2873%29.png)

### 9. 사용자에 관리형정책 확인

사용자에 정상적으로 관리형 정책이 할당되었는지 확인합니다.

![](.gitbook/assets/image%20%2886%29.png)

### 10. 사용자 재로그인과 역할 전환

위임정책이 할당된 사용자 "thor"로 로그인을 합니다. 로그인 후에 메뉴 상단의 사용자를 선택하고 **역할 전환\(Role Switch\)**를 선택합니다.

![](.gitbook/assets/image%20%28111%29.png)

![](.gitbook/assets/image%20%2877%29.png)

현재 계정번호와 새롭게 생성한 역할\(Role\)을 입력합니다.

* **계정 - 현재 계정번호**
* **역할 - 새롭게 생성한 역할\(Role\)이름**
* **색상 - 원하는 색상을 선택.**

![](.gitbook/assets/image%20%28101%29.png)

### 11. 역할 전환 확인

이제 역할이 정상적으로 전환되었는지 메뉴 상단을 확인합니다. 역할이름과 계정번호로 전환된 것을 확인 할 수 있습니다.

![](.gitbook/assets/image%20%28102%29.png)

사용자 "thor"에게는 S3 버킷을 생성할 수 있는 권한이 없었습니다. 역할 전환을 통해 정상적으로 새로운 역할을 부여 받았는지 확인합니다. S3 버킷을 생성해 봅니다. 정상적으로 생성된 것을 확인할 수 있습니다.

![](.gitbook/assets/image%20%2887%29.png)

{% hint style="info" %}
아래는 역할 전환에 대한 소개 그림입니다. 역할 전환은 AWS 자원, 사용자 계정, 그룹등에 다양하게 사용되며, STS assumerole을 통해 안전하게 단기 임시 보안 Token을 사용할 수 있습니다. 이러한 방식은 자원등에 보안정보를 바인딩하지 않아도 되므로 보안도 강화할 수 있습니다.
{% endhint %}

![](.gitbook/assets/image%20%28114%29.png)

## 교차계정 Role Switch

### 1.교차계정 역할전환 소개

역할전환은 단일 계정에서만 사용가능한 것이 아니라 교차계정에서도 가능합니다. 예를 들어 Account-Dev의 그룹 Test는 개발이 완료된 이후 , Account-Dev에서의 사용자를 통해서 Account-PRD에 자원을 직접 생성하거나 조회할 수 있습니다. 대부분의 조직과 Account는 개발, 시험, 스테이징, Production 등으로 세분화 되므로, 이러한 경우에 교차계정 역할전환을 사용하면 편리하고 보안을 강화할 수 있습니다.

![](.gitbook/assets/image%20%2880%29.png)

### 2. 교차계정에서 역할\(Role\)만들기

교차계정에서 역할만들기를 선택합니다.

**"AWS 관리 콘솔 - IAM - 역할 - 역할만들기"**

![](.gitbook/assets/image%20%2872%29.png)

### 3. 교차계정에서 다른 계정 선택

교차계정의 역할 만들기 - 신뢰할 수 있는 유형의 개체 선택에서 "다른 AWS 계정"을 선택합니다. 이때 계정 ID는 사용자 "thor"가 있는 계정의 ID를 입력합니다.

![](.gitbook/assets/image%20%2868%29.png)

### 4. 교차계정 역할에 정책 연결

교차계정의 역할에서 권한 정책을 연결합니다. "EC2FullAcess", "S3FullAccess" 를 선택해서, EC2,S3 자원에 대한 권한을 부여합니다.

![](.gitbook/assets/image%20%28108%29.png)

### 5. 교차계정 역할 이름 입력

교차계정 역할에 대한 이름을 입력하고 , 완료합니다.

![](.gitbook/assets/image%20%28121%29.png)

### 6. 교차계정 역할 정보 확인

교차계정 역할을 완료한 후 , 정책연결 내용 및 역할 ARN을 확인합니다. **ARN은 상대방 계정에서 사용되므로 복사 해 둡니다.**

![](.gitbook/assets/image%20%28100%29.png)

### 7. 위임을 위한 새로운 정책 만들기

사용자 "thor"가 존재하는 계정에서 정책을 설정합니다.

**"AWS 관리 콘솔 - IAM - 정책 - 정책생성"**

![](.gitbook/assets/image%20%2883%29.png)

정책 생성에서 시각적 편집기 또는 JSON으로 정책을 생성합니다.

* **서비스 - STS**
* **작업 - STS에서 허용되는 작업 지정 - AssumeRole**
* **리소스 - 앞서 복사해둔 교차계정 Role의 ARN을 입력합니다. 아래와 같은 구성입니다.**

![](.gitbook/assets/image%20%2814%29.png)

```text
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": "sts:AssumeRole",
            "Resource": "arn:aws:iam::133802754686:role/cross-switch-role"
        }
    ]
}
```

ARN 편집을 통해 교차계정의 역할 ARN을 입력합니다.

![](.gitbook/assets/image%20%2881%29.png)

새로운 정책이 정상적으로 생성되었는지 확인합니다.

![](.gitbook/assets/image%20%2895%29.png)

### 8. 그룹에 권한 추가

사용자 "thor"가 속해 있는 , 그룹 "Test"에 생성된 정책 권한을 추가합니다.

**"AWS 관리 콘솔 - IAM -  그룹 - Test - 정책연결"**

![](.gitbook/assets/image%20%28122%29.png)

앞서 생성한 위임을 위한 정책을 연결합니다.

![](.gitbook/assets/image%20%28103%29.png)

### 9. 사용자 재로그인과 교차계정 역할 전환

사용자 "thor" 로 로그인하고, 사용자 정보 메뉴에서 역할 전환을 선택합니다.

![](.gitbook/assets/image%20%2889%29.png)

역할 전환 창에서 교차계정번호와 교차계정에서 선언한 역할 이름을 정의해 줍니다.

* **계정 - 교차계정번호**
* **역할 - 교차계정에서 선언한 역할 이름.**

![](.gitbook/assets/image%20%2898%29.png)

10. 역할전환 확인

이제 정상적으로 역할전환이 이뤄집니다. 아래와 같이 교차계정에서 선언한 역할 이름으로 교차계정 역할전환이 이뤄진 것을 확인 할 수 있습니다.

![](.gitbook/assets/image%20%28119%29.png)

이제 교차계정의 자원을 정상적으로 생성할 수 있는 지 확인하기 위해, S3 버킷을 생성해 봅니다. 정상적으로 생성되는 것을 확인합니다.

![](.gitbook/assets/image%20%28110%29.png)

교차계정 AWS 관리콘솔에서 S3 서비스로 접근해서 , 실제 S3 버킷이 만들어졌는지 확인합니다.

![](.gitbook/assets/image%20%2893%29.png)





