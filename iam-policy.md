# IAM 정책(Policy)과 권한(Permission)

## IAM 정책 소개

IAM에서 자격 증명에 대한 권한을 설정해야 할 경우 AWS 관리형 정책, 고객 관리형 정책 또는 인라인 정책 중 어느 것을 사용할지를 결정해야 합니다.

정책 타입은 아래와 같이 6가지로 분류됩니다.(참조 - [https://docs.aws.amazon.com/ko\_kr/IAM/latest/UserGuide/access\_policies.html](https://docs.aws.amazon.com/ko\_kr/IAM/latest/UserGuide/access\_policies.html))

* **자격 증명 기반 (Identity-based policies)**
  * **AWS 관리형 정책 - AWS에서 제공하는 글로벌 적용**
  * **AWS 고객 관리형 정책 - 고객의 계정에서 생성해서 사용관리.**
  * **AWS 인라인 정책 - 단일 사용자, 그룹, 역할(Role)에 직접 추가하는 방식.**
* 리소스 정책 기반 (Resource-based policies)
* **권한 경계 기반 정책 (Permissions boundaries)**
* 조직 SCP 기반 정책 (Organizations SCPs)
* 액세스 제어 리스트 (Access control lists -ACLs)
* 세션 정책 (Session policies)

## Identity-Based Policy&#x20;

ID 기반 정책은 ID(사용자, 사용자 그룹 및 역할)가 수행할 수 있는 작업, 리소스 및 조건을 제어하는 JSON 권한 정책 문서입니다. 자격 증명 기반 정책을 추가로 분류할 수 있습니다.

### 1. AWS 관리형 정책 (AWS Managed Policy)

**AWS 관리형 정책**은 AWS에서 생성 및 관리하는 독립적인 정책입니다. 여기에서 독립적인 정책이란 정책 스스로 정책 이름이 포함된 Amazon 리소스 이름(ARN)을 갖고 있다는 것을 의미합니다. AWS 에서 대부분 제공되는 정책입니다. AWS에서 제공되는 정책이므로 글로벌하게 적용됩니다.

예를 들어 `arn:aws:iam::aws:policy/IAMReadOnlyAccess`는 AWS 관리형 정책입니다.

![\[AWS 관리형 정책\]](<.gitbook/assets/image (92).png>)

![](<.gitbook/assets/image (159).png>)



### 2.AWS 고객 관리형 정책 (AWS Customer Managed Policy)

독립적인 정책은 사용자 자신의 AWS 계정에서 관리하도록 생성할 수도 있습니다. 이러한 정책을 고객 관리형 정책이라고 합니다. 이렇게 생성된 정책은 AWS 계정에 속한 다수의 보안 주체 엔터티에 추가할 수 있습니다. 정책을 보안 주체 엔터티에 추가할 경우 정책에서 정의한 권한까지 엔터티에게 부여하게 됩니다.

고객이 관리하는 정책을 생성하는 좋은 방법은 AWS에서 관리하는 기존의 정책을 복사하여 시작하는 것입니다. 이렇게 하면 시작 시 올바른 정책으로 시작하므로 해당 환경에 맞게 사용자 지정만 하면 됩니다. 고객 관리형 정책은 만들어진 Account에서만 사용할 수 있습니다.

![\[AWS 고객 관리형 정책\]](<.gitbook/assets/image (9).png>)

![](<.gitbook/assets/image (79).png>)

### 3. AWS 인라인 정책

AWS 인라인 정책은 1 to 1 정책으로 명시적으로 할당되는 정책입니다.따라서 IAM 자격 증명(사용자, 그룹 또는 역할)에 포함되는 정책입니다. 즉, 정책은 자격 증명의 고유한 부분입니다. Lab에서 명시적 접근 거부로 확인합니다.

![](<.gitbook/assets/image (63).png>)

## **사용자 권한 추가**

### 1.사용자 선택

AWS 관리 콘솔에서 IAM을 선택하고, 대시보드에서 사용자를 선택합니다.&#x20;

### 2.사용자 권한 추가 선택

Captain 사용자를 선택하고, 사용자 **권한 추가**합니다.

![](<.gitbook/assets/image (65).png>)

### 3. 기존 정책 직접연결 선택

captain 사용자에 기존 정책 직접 연결을 선택합니다. captain에 **"AdministratorAccess"** 권한을 부여합니다.

![](<.gitbook/assets/image (52).png>)

### 4. 정책 JSON 확인

정책을 연결하고 사용자에서 Captain을 선택하고, JSON 파일을 확인합니다.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "*"
        }
    ]
}
```

### 5. 정책 시뮬레이션 확인

정책 시뮬레이션을 선택합니다.

![](<.gitbook/assets/image (10).png>)

IAM 정책 시뮬레이션은 현재 적용된 정책이 어떤 권한을 가지고, 자원을 사용할 수 있는지를 평가해 주는 도구입니다.

![](<.gitbook/assets/image (17).png>)

### 7. 정책이 할당된 사용자로 로그인

이제 로그아웃을 하고, captain으로 Login을 합니다.&#x20;

### 8. 다수 사용자들에 대해 권한을 할당

아래 표에 여러개의 사용자에 정책 권한을 할당하고, 시뮬레이션을 실행해 봅니다.&#x20;

예를 들어 아래와 같이 각 사용자들에 대해서 정책과 권한을 할당합니다. 사용자 doctor 와 spiderman, captain과 thanos은 동일한 permission Policy를 가지게 되므로, 구성시에 "기존 사용자에서 권한 복사"를 활용해 봅니다.

![](<.gitbook/assets/image (126).png>)

| User         | Permission Policies                                          |
| ------------ | ------------------------------------------------------------ |
| captain      | AdministartorAccess                                          |
| hulk         | AmazonEC2ReadOnlyAccess                                      |
| hawkeye      | AmazonEC2FullAccess                                          |
| ironman      | AmazonS3FullAccess                                           |
| blackpanther | AmazonS3ReadOnlyAccess                                       |
| doctor       | AmazonEC2FullAccess,AmazonS3FullAccessk,CloudWatchFullAccess |
| spiderman    | AmazonEC2FullAccess,AmazonS3FullAccessk,CloudWatchFullAccess |
| thor         | AmazonEC2ReadOnlyAccess,AmazonS3ReadOnlyAccess               |
| thanos       | AdministartorAccess                                          |

### 9. 사용자 권한 확인

이제 각 사용자들로 로그인을 통해 권한이 정상적으로 구성되고 동작하는지 확인해 봅니다.

* hulk - EC2 인스턴스를 생성해 봅니다. 아래와 같은 에러메세지를 출력합니다.

![](<.gitbook/assets/image (116).png>)

* hawkeye - EC2 인스턴스를 생성해 봅니다. 정상적으로 EC2 인스턴스를 생성할 수 있습니다. 하지만 S3 버킷을 생성할 수 없습니다.

![](<.gitbook/assets/image (48).png>)

* ironman - S3 버킷을 생성할 수 있습니다. 하지만 EC2에 대한 정책 권한이 없으므로 아래와 같이 에러가 발생합니다.

![](<.gitbook/assets/image (164).png>)

* blackpanther - S3를 조회할 수는 있지만, S3를 생성할 수 있는 정책 권한이 없으므로 아래와 같이 에러가 발생합니다.

![](<.gitbook/assets/image (15).png>)

![](<.gitbook/assets/image (113).png>)

* doctor, spiderman - 같은 정책권한을 소유하고 있습니다. EC2,S3 전체 정책 권한과 Cloudwatch에 대한 권한을 가지고 있으므로 접근이 가능합니다.
* thor - EC2,S3에 대한 접근권한과 Read권한을 가지고 있으므로 , 생성할 수는 없습니다.
* thanos - Admin권한을 가지고 있으므로 captain과 동일하게 모두 접근 가능합니다.

## 인라인 정책

앞서 사용자 "hawkeye"는 관리형 정책을 통해 "AmazonEC2FullAccess" 정책에 대한 권한을 적용하였습니다. 관리형 정책에 부여된 정책 가운데, 필요에 따라서 거부하는 것을 구성해 봅니다. 이때 인라인 정책을 구성해서 확인해 봅니다.

### 1.사용자 선택

#### hawkey 사용자를 선택하고 , 현재 정책을 확인합니다.

AWS 관리 콘솔 - IAM - 대쉬보드 - 사용자 - hawkeye 선택

![](<.gitbook/assets/image (95).png>)

{% hint style="info" %}
AWS 관리형 정책은 사용자들이 AWS 정책을 손쉽게 만들수 있도록 사전에 잘 만들어진 정책입니다. 해당 정책에는 단순히 해당 자원을 접근하기 위한 정책과 기타 연동성을 고려한 정책이 구성되어 있습니다. 따라서 필요에 따라 고객 관리형 정책을 사용할 때는 복사해서 사용하는 것이 좋습니다.
{% endhint %}

AmazonEC2FullAccess 정책에는 EC2 이외에도 ELB, Cloudwatch와 같은 접근 권한이 허용되어 있습니다.

### 2. 인라인 정책 추가 선택

#### hawkeye 사용자에 인라인 정책을 추가합니다.

IAM - 대시보드 - 사용자 - hawkeye - 인라인 정책 추가 선택

![](<.gitbook/assets/image (14).png>)

### 3. 거부 정책 추가

hawkeye 사용자에게 Cloudwatch 에 대한 접근을 거부하려고 합니다. 아래와 같이 추가합니다.

![](<.gitbook/assets/image (38).png>)

또는 JSON을 입력합니다.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Deny",
            "Action": [
                "cloudwatch:DescribeInsightRules",
                "cloudwatch:PutMetricData",
                "cloudwatch:GetMetricData",
                "cloudwatch:DescribeAlarmsForMetric",
                "cloudwatch:ListDashboards",
                "cloudwatch:PutAnomalyDetector",
                "cloudwatch:GetMetricStatistics",
                "cloudwatch:GetMetricWidgetImage",
                "cloudwatch:DeleteAnomalyDetector",
                "cloudwatch:ListMetrics",
                "cloudwatch:DescribeAnomalyDetectors"
            ],
            "Resource": "*"
        },
        {
            "Sid": "VisualEditor1",
            "Effect": "Deny",
            "Action": "cloudwatch:*",
            "Resource": [
                "arn:aws:cloudwatch::841260513405:dashboard/*",
                "arn:aws:cloudwatch:*:841260513405:alarm:*",
                "arn:aws:cloudwatch:*:841260513405:insight-rule/*"
            ]
        }
    ]
}
```

### 4. 인라인 정책 이름 생성

인라인 정책 이름을 생성합니다.

인라인 정책 이름을 정의하고 정책을 설정합니다. 인라인 정책은 해당 Account -  사용자(User)에서만 정의 됩니다.

![](<.gitbook/assets/image (44).png>)

### 5. 사용자 정책 권한 확인

정책을 확인합니다.

새로운 정책이 정상적으로 추가되었는지 확인합니다.

![](<.gitbook/assets/image (46).png>)

hawkeye 사용자는 AWS 관리형 정책 - AmazonEC2FullAccess 권한을 통해 CloudWatch의 권한을 허용 받았지만, 인라인 정책에서 CloudWatch에 대한 명시적 접근 거부가 설정되어, 거부 권한이 동작하게 됩니다.

### 6. 명시적 접근 거부 확인

hawkeye 사용자로 로그인 하고 , Cloudwatch에서 메뉴들을 확인합니다.

Cloudwatch 의 메뉴들을 정상적으로 볼 수 없게 됩니다.

![](<.gitbook/assets/image (78).png>)

{% hint style="info" %}
명시적 거부가 있는 경우, 허용된 정책 권한이 있더라도 우선하게 됩니다. hawkeye는 EC2FullAccess 권한으로 Cloudwatch에 대한 권한을 가지고 있지만, 사용자에 인라인정책을 적용하여 명시적 접근 거부 정책으로 Cloudwatch에 대한 접근을 제어할 수 있습니다.
{% endhint %}

{% hint style="warning" %}
특정 자원 접근을 막기 위해 , 인라인 정책을 적용할 때 정책 담당자는 리소스간의 연관 관계를 상세하게 알아야 하기 때문에 적용하는 데 불편이 따를 수 있습니다. " Permission Boundary" 설정으로 자원 허용범위에 대한 접근을 쉽게 할 수 있습니다.
{% endhint %}

## 그룹 정책

### 1. 그룹에 사용자 추가

아래와 같이 생성된 사용자들을 그룹에 할당합니다.

![](<.gitbook/assets/image (39).png>)

| Group     | Policy                                           | User         | Policy                                                                    |
| --------- | ------------------------------------------------ | ------------ | ------------------------------------------------------------------------- |
| SuperUser | AdministratorAccess                              | captain      | AdministartorAccess                                                       |
|           |                                                  | thanos       | AdministartorAccess                                                       |
| Dev1      | AmazonEC2FullAccess                              | hawkeye      | AmazonEC2FullAccess                                                       |
|           |                                                  | hulk         | AmazonEC2ReadOnlyAccess                                                   |
| Dev2      | AmazonS3FullAccess                               | ironman      | AmazonS3FullAccess                                                        |
|           |                                                  | blackpanther | AmazonS3ReadOnlyAccess                                                    |
| PRD       | <p>AmazonEC2FullAccess<br>AmazonS3FullAccess</p> | doctor       | <p>AmazonEC2FullAccess<br>AmazonS3FullAccessk<br>CloudWatchFullAccess</p> |
|           |                                                  | spiderman    | <p>AmazonEC2FullAccess<br>AmazonS3FullAccessk<br>CloudWatchFullAccess</p> |
| Test      |                                                  | thor         | <p>AmazonEC2ReadOnlyAccess<br>AmazonS3ReadOnlyAccess</p>                  |

### 2. 그룹에 사용자 추가

생성된 5개 그룹에 각 사용자를 추가합니다. **"그룹에 사용자 추가"**를 선택하고 사용자를 추가합니다.

![](<.gitbook/assets/image (89).png>)

![](<.gitbook/assets/image (130).png>)

모두 추가하면 아래와 같이 그룹에 추가된 사용자의 수를 확인할 수 있습니다.

![](<.gitbook/assets/image (137).png>)

### 3. 그룹 정책과 사용자 정책 관계 확인

앞서 이미 사용자에 정책 권한이 적용되어 있습니다. 또한 그룹에도 권한이 할당되어 있습니다. "blackpather"는 사용자 정책권한에 "S3ReadOnly"만 할당되어 있습니다.&#x20;

"blackpather"로 사용자 로그인을 하고, S3 버킷을 만들어 봅니다. 아래와 같이 버킷이 만들어집니다.

![](<.gitbook/assets/image (120).png>)

"thor" 사용자는 정책 권한이 "EC2ReadOnly", "S3ReadOnly"가 할당되어 있습니다. 하지만 "thor"가 속해 있는 "Group - Test"에는 어떠한 권한도 할당되어 있지 않습니다.&#x20;

"thor" 사용자 로그인을 시도합니다. "thor" 사용자에 할당된 정책권한으로 S3 버킷을 조회할 수 있는 확인합니다.

![](<.gitbook/assets/image (110).png>)

{% hint style="warning" %}
그룹과 사용자의 정책이 적용되어 있을 경우, 한 군데라도 명시적 접근 허용이 있으면 자원에 접근할 수 있습니다. 하지만 명시적 접근 거부가 있는 경우 거부 정책을 우선합니다.
{% endhint %}

## 권한 경계 (Permissions Boundary)

### 권한 경계 (Permission Boundary)란?

권한 경계는 자격 증명 기반 정책을 통해 IAM 엔터티에 부여할 수 있는 최대 권한을 설정하는 고급 기능입니다. 엔터티에 대한 권한 경계를 설정할 경우 해당 엔터티는 자격 증명 기반 정책 및 관련 권한 경계 모두에서 허용되는 작업만 수행할 수 있습니다. 사용자나 역할을 보안 주체로 지정하는 리소스 기반 정책은 권한 경계에 제한을 받지 않습니다.

이미 명시적으로 허용된 넓은 범위의 권한을 특정 사용자 또는 그룹을 대상으로 허용범위를 제약하는 방법으로 매우 효과적입니다.&#x20;

### 1. 권한 경계 설정

![](<.gitbook/assets/image (32).png>)

"thanos"는 앞서 "SuperUser" 그룹에 속해 있었습니다. 특정한 목적으로 "SuperUser"그룹에서 "thanos"의 정책권한을 축소하려고 합니다. 이때 권한경계 정책을 통해 적용해 봅니다.

"AWS 관리 콘솔 - IAM - 사용자" 를 선택하고, "경계설정"을 선택합니다.

![](<.gitbook/assets/image (83).png>)

### 2. 권한 경계 정책 생성

"thanos" 사용자의 권한 경계에 대한  **"AWS 관리형 정책 - EC2FullAccess"**를 선택합니다.

![](<.gitbook/assets/image (12).png>)

### 3. 권한 경계 설정 확인

"thanos" 사용자의 권한(Permission)에 정상적으로 , **Permission Boundary**가 추가되었는지 확인합니다.

![](<.gitbook/assets/image (36).png>)

### 4. 사용자 자원 제한 확인

이제 "thanos" 사용자로 로그인한 이후 S3서비스에 접근해서 , 서비스가 정상적으로 제공되는 지 확인합니다. 권한 경계 설정이 정상적으로 이뤄 졌다면 , S3에 아래와 같은 접근 제어 오류를 확인할 수 있습니다.

![](<.gitbook/assets/image (142).png>)
