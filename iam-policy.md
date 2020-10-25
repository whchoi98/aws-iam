# IAM 정책\(Policy\)과 권한\(Permission\)

## IAM 정책 소개

IAM에서 자격 증명에 대한 권한을 설정해야 할 경우 AWS 관리형 정책, 고객 관리형 정책 또는 인라인 정책 중 어느 것을 사용할지를 결정해야 합니다.

### AWS 관리형 정책 \(AWS Managed Policy\)

**AWS 관리형 정책**은 AWS에서 생성 및 관리하는 독립적인 정책입니다. 여기에서 독립적인 정책이란 정책 스스로 정책 이름이 포함된 Amazon 리소스 이름\(ARN\)을 갖고 있다는 것을 의미합니다. AWS 에서 대부분 제공되는 정책입니다. AWS에서 제공되는 정책이므로 글로벌하게 적용됩니다.

예를 들어 `arn:aws:iam::aws:policy/IAMReadOnlyAccess`는 AWS 관리형 정책입니다.

![\[AWS &#xAD00;&#xB9AC;&#xD615; &#xC815;&#xCC45;\]](.gitbook/assets/image%20%2816%29.png)

![](.gitbook/assets/image%20%283%29.png)



### AWS 고객 관리형 정책 \(AWS Customer Managed Policy\)

독립적인 정책은 사용자 자신의 AWS 계정에서 관리하도록 생성할 수도 있습니다. 이러한 정책을 고객 관리형 정책이라고 합니다. 이렇게 생성된 정책은 AWS 계정에 속한 다수의 보안 주체 엔터티에 추가할 수 있습니다. 정책을 보안 주체 엔터티에 추가할 경우 정책에서 정의한 권한까지 엔터티에게 부여하게 됩니다.

고객이 관리하는 정책을 생성하는 좋은 방법은 AWS에서 관리하는 기존의 정책을 복사하여 시작하는 것입니다. 이렇게 하면 시작 시 올바른 정책으로 시작하므로 해당 환경에 맞게 사용자 지정만 하면 됩니다. 고객 관리형 정책은 만들어진 Account에서만 사용할 수 있습니다.

![\[AWS &#xACE0;&#xAC1D; &#xAD00;&#xB9AC;&#xD615; &#xC815;&#xCC45;\]](.gitbook/assets/image%20%2835%29.png)

![](.gitbook/assets/image%20%2823%29.png)

AWS 인라인 정책

AWS 인라인 정책은 1 to 1 정책으로 명시적으로 할당되는 정책입니다.따라서 IAM 자격 증명\(사용자, 그룹 또는 역할\)에 포함되는 정책입니다. 즉, 정책은 자격 증명의 고유한 부분입니다. Lab에서 명시적 접근 거부로 확인합니다.

![](.gitbook/assets/image%20%2850%29.png)

## **사용자 권한 추가**

### 사용자 권한 추가

1.AWS 관리 콘솔에서 IAM을 선택하고, 대시보드에서 사용자를 선택합니다. 

2.Captain 사용자를 선택하고, 사용자 **권한 추가**합니다.

![](.gitbook/assets/image.png)

3. captain 사용자에 기존 정책 직접 연결을 선택합니다. captain에 "AdministratorAccess" 권한을 부여합니다.

![](.gitbook/assets/image%20%289%29.png)

4. 정책을 연결하고 사용자에서 Captain을 선택하고, JSON 파일을 확인합니다.

```text
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

5. 정책 시뮬레이션을 선택합니다.

![](.gitbook/assets/image%20%2846%29.png)

IAM 정책 시뮬레이션은 현재 적용된 정책이 어떤 권한을 가지고, 자원을 사용할 수 있는지를 평가해 주는 도구입니다.

![](.gitbook/assets/image%20%2831%29.png)

7. 이제 로그아웃을 하고, captain으로 Login을 합니다. 

8. 앞서와 동일하게 사용자들에 대해 권한을 할당하고, 시뮬레이션을 실행해 봅니다. 

예를 들어 아래와 같이 각 사용자들에 대해서 정책과 권한을 할당합니다. 사용자 doctor 와 spiderman, captain과 thanos은 동일한 permission Policy를 가지게 되므로, 구성시에 "기존 사용자에서 권한 복사"를 활용해 봅니다.

| User | Permission Policies |
| :--- | :--- |
| captain | AdministartorAccess |
| hulk | AmazonEC2ReadOnlyAccess |
| hawkeye | AmazonEC2FullAccess |
| ironman | AmazonS3FullAccess |
| blackpanther | AmazonS3ReadOnlyAccess |
| doctor | AmazonEC2FullAccess,AmazonS3FullAccessk,CloudWatchFullAccess |
| spiderman | AmazonEC2FullAccess,AmazonS3FullAccessk,CloudWatchFullAccess |
| thor | AmazonEC2ReadOnlyAccess,AmazonS3ReadOnlyAccess |
| thanos | AdministartorAccess |

### 사용자 권한 확인

1.이제 각 사용자들로 로그인을 통해 권한이 정상적으로 구성되고 동작하는지 확인해 봅니다.

* hulk - EC2 인스턴스를 생성해 봅니다. 아래와 같은 에러메세지를 출력합니다.

![](.gitbook/assets/image%20%2855%29.png)

* hawkeye - EC2 인스턴스를 생성해 봅니다. 정상적으로 EC2 인스턴스를 생성할 수 있습니다. 하지만 S3 버킷을 생성할 수 없습니다.

![](.gitbook/assets/image%20%2841%29.png)

* ironman - S3 버킷을 생성할 수 있습니다. 하지만 EC2에 대한 정책 권한이 없으므로 아래와 같이 에러가 발생합니다.

![](.gitbook/assets/image%20%285%29.png)

* blackpanther - S3를 조회할 수는 있지만, S3를 생성할 수 있는 정책 권한이 없으므로 아래와 같이 에러가 발생합니다.

![](.gitbook/assets/image%20%2860%29.png)

![](.gitbook/assets/image%20%2828%29.png)

* doctor, spiderman - 같은 정책권한을 소유하고 있습니다. EC2,S3 전체 정책 권한과 Cloudwatch에 대한 권한을 가지고 있으므로 접근이 가능합니다.
* thor - EC2,S3에 대한 접근권한과 Read권한을 가지고 있으므로 , 생성할 수는 없습니다.
* thanos - Admin권한을 가지고 있으므로 captain과 동일하게 모두 접근 가능합니다.

### 명시적 접근 거부

앞서 사용자 "hawkeye"는 관리형 정책을 통해 "AmazonEC2FullAccess" 정책에 대한 권한을 적용하였습니다. 관리형 정책에 부여된 정책 가운데, 필요에 따라서 거부하는 것을 구성해 봅니다. 이때 인라인 정책을 구성해서 확인해 봅니다.

#### 1.hawkey 사용자를 선택하고 , 현재 정책을 확인합니다.

AWS 관리 콘솔 - IAM - 대쉬보드 - 사용자 - hawkeye 선택

![](.gitbook/assets/image%20%2827%29.png)

{% hint style="info" %}
AWS 관리형 정책은 사용자들이 AWS 정책을 손쉽게 만들수 있도록 사전에 잘 만들어진 정책입니다. 해당 정책에는 단순히 해당 자원을 접근하기 위한 정책과 기타 연동성을 고려한 정책이 구성되어 있습니다. 따라서 필요에 따라 고객 관리형 정책을 사용할 때는 복사해서 사용하는 것이 좋습니다.
{% endhint %}

AmazonEC2FullAccess 정책에는 EC2 이외에도 ELB, Cloudwatch와 같은 접근 권한이 허용되어 있습니다.

#### 2. hawkeye 사용자에 인라인 정책을 추가합니다.

IAM - 대시보드 - 사용자 - hawkeye - 인라인 정책 추가 선택

![](.gitbook/assets/image%20%2861%29.png)

hawkeye 사용자에게 Cloudwatch 에 대한 접근을 거부하려고 합니다. 아래와 같이 추가합니다.

![](.gitbook/assets/image%20%287%29.png)

또는 JSON을 입력합니다.

```text
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

3. 인라인 정책 이름을 생성합니다.

인라인 정책 이름을 정의하고 정책을 설정합니다. 인라인 정책은 해당 Account -  사용자\(User\)에서만 정의 됩니다.

![](.gitbook/assets/image%20%2837%29.png)

4. 정책을 확인합니다.

새로운 정책이 정상적으로 추가되었는지 확인합니다.

![](.gitbook/assets/image%20%2814%29.png)

hawkeye 사용자는 AWS 관리형 정책 - AmazonEC2FullAccess 권한을 통해 CloudWatch의 권한을 허용 받았지만, 인라인 정책에서 CloudWatch에 대한 명시적 접근 거부가 설정되어, 거부 권한이 동작하게 됩니다.

5. hawkeye 사용자로 로그인 하고 , Cloudwatch에서 메뉴들을 확인합니다.

Cloudwatch 의 메뉴들을 정상적으로 볼 수 없게 됩니다.

![](.gitbook/assets/image%20%2842%29.png)

## 그룹 정책

### 그룹에 사용자 추가

| Group | Policy | User | Policy |
| :--- | :--- | :--- | :--- |
| SuperUser | AdministratorAccess | captain | AdministartorAccess |
|  |  | thanos | AdministartorAccess |
| Dev1 | AmazonEC2FullAccess | hawkeye | AmazonEC2FullAccess |
|  |  | hulk | AmazonEC2ReadOnlyAccess |
| Dev2 | AmazonS3FullAccess | ironman | AmazonS3FullAccess |
|  |  | blackpanther | AmazonS3ReadOnlyAccess |
| PRD | AmazonEC2FullAccess AmazonS3FullAccess | doctor | AmazonEC2FullAccess AmazonS3FullAccessk CloudWatchFullAccess |
|  |  | spiderman | AmazonEC2FullAccess AmazonS3FullAccessk CloudWatchFullAccess |
| Test |  | thor | AmazonEC2ReadOnlyAccess AmazonS3ReadOnlyAccess |



![](.gitbook/assets/image%20%2829%29.png)

![](.gitbook/assets/image%20%288%29.png)

![](.gitbook/assets/image%20%2833%29.png)

### 그룹 정책과 사용자 정책 관계 확인

![](.gitbook/assets/image%20%2843%29.png)

![](.gitbook/assets/image%20%2834%29.png)

## Permissions Boundary

### Permission Boundary란?



### Permission Boundary 설정



![](.gitbook/assets/image%20%281%29.png)

![](.gitbook/assets/image%20%282%29.png)

![](.gitbook/assets/image%20%2856%29.png)

![](.gitbook/assets/image%20%2839%29.png)

