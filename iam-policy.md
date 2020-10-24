# IAM 정책\(Policy\)과 권한\(Permission\)

## IAM 정책 소개

IAM에서 자격 증명에 대한 권한을 설정해야 할 경우 AWS 관리형 정책, 고객 관리형 정책 또는 인라인 정책 중 어느 것을 사용할지를 결정해야 합니다.

_AWS 관리형 정책_은 AWS에서 생성 및 관리하는 독립적인 정책입니다. 여기에서 _독립적인 정책_이란 정책 스스로 정책 이름이 포함된 Amazon 리소스 이름\(ARN\)을 갖고 있다는 것을 의미합니다. 

예를 들어 `arn:aws:iam::aws:policy/IAMReadOnlyAccess`는 AWS 관리형 정책입니다.



## **사용자 권한 추가**

### 사용자 권한 추가

AWS 관리 콘솔에서 IAM을 선택하고, 대시보드에서 사용자를 선택합니다. Captain 사용자를 선택하고, 사용자 **권한 추가**합니다.

![](.gitbook/assets/image.png)

captain 사용자에 기존 정책 직접 연결을 선택합니다. captain에 "AdministratorAccess" 권한을 부여합니다.

![](.gitbook/assets/image%20%282%29.png)

정책을 연결하고 사용자에서 Captain을 선택하고, JSON 파일을 확인합니다.

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

정책 시뮬레이션을 선택합니다.

![](.gitbook/assets/image%20%2823%29.png)

IAM 정책 시뮬레이션은 현재 적용된 정책이 어떤 권한을 가지고, 자원을 사용할 수 있는지를 평가해 주는 도구입니다.

![](.gitbook/assets/image%20%2816%29.png)

이제 로그아웃을 하고, captain으로 Login을 합니다. 

앞서와 동일하게 사용자들에 대해 권한을 할당하고, 시뮬레이션을 실행해 봅니다. 예를 들어 아래와 같이 각 사용자들에 대해서 정책과 권한을 할당합니다. 사용자 doctor 와 spiderman, captain과 thanos은 동일한 permission Policy를 가지게 되므로, 구성시에 "기존 사용자에서 권한 복사"를 활용해 봅니다.

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

이제 각 사용자들로 로그인을 통해 권한이 정상적으로 구성되고 동작하는지 확인해 봅니다.

* hulk - EC2 인스턴스를 생성해 봅니다. 아래와 같은 에러메세지를 출력합니다.

![](.gitbook/assets/image%20%2831%29.png)

* hawkeye - EC2 인스턴스를 생성해 봅니다. 정상적으로 EC2 인스턴스를 생성할 수 있습니다. 하지만 S3 버킷을 생성할 수 없습니다.

![](.gitbook/assets/image%20%2820%29.png)

* ironman - S3 버킷을 생성할 수 있습니다. 하지만 EC2

