# IAM 기본 구성

## IAM User 구성

AWS Identity and Access Management\(IAM\) 사용자는 AWS에서 생성하는 엔터티로서 AWS와 상호 작용하기 위해 그 엔터티를 사용하는 사람 또는 애플리케이션을 나타냅니다. AWS에서 사용자는 이름과 자격 증명으로 구성됩니다.

AWS 관리 콘솔에서 "**서비스 - IAM - 사용자"** 를 선택합니다.

![](.gitbook/assets/image%20%2823%29.png)

**사용자 추가**를 선택합니다.

![](.gitbook/assets/image%20%2821%29.png)

첫번째 단계로, 9명의 새로운 사용자를 추가합니다.

* 사용자 이름 - captain, hulk, thor, blackpanther,ironman,doctor,spiderman,hawkeye,thanos
* 액세스 유형 - 프로그래밍 액세스, AWS Management Console 액세스 선택
* 콘솔비밀번호 - 사용자 지정 비밀번호 선택 및 비밀번호 입력

![](.gitbook/assets/image%20%2842%29.png)

두번째 단계에서, 기존 그룹에 사용자를 추가하거나 , 기존 사용자에서 권한\(permission\) 복사, 기존 정책\(Policy\)에 직접 연결할 수 있습니다. 또한 권한 경계 설정을 할 수 있습니다.이 단계는 별도 설정하지 않습니다.

![](.gitbook/assets/image%20%2844%29.png)

세번째 단계에서는 태그를 추가할 수 있습니다. 이 단계에서는 별도 설정하지 않습니다.

![](.gitbook/assets/image%20%2843%29.png)

네번째 단계에서는 앞서 구성된 단계들의 설정 내용을 검토합니다. 앞서 사용자 생성 이외에는 별도의 생성을 하지 않았습니다. 사용자 만들기를 최종 선택합니다.

![](.gitbook/assets/image%20%2831%29.png)

최종적으로 생성된 사용자\(User\)들의 구성 내역을 살펴봅니다.

![](.gitbook/assets/image%20%288%29.png)

{% hint style="info" %}
생성된 각 사용자\(user\)는 Amazon 리소스 이름\(ARN\)입니다. 모든 AWS 전반에 사용자를 특별하게 식별할 필요가 있는 경우 ARN을 사용합니다. 예를 들어, ARN을 사용하여 사용자를 Amazon S3 버킷에 대한 IAM 정책에서 **`Principal`**로서 지정할 수 있습니다. IAM 사용자의 ARN은 다음과 같은 모습입니다.사용자의 ARN은 AWS 관리 콘솔 내 보안 자격 증명 또는 권한이 있는 사용자의 사용자요약에서 확인 할 수 있습니다.

arn:aws:iam::xxxxxxxxxxxx:user/whchoi
{% endhint %}

## IAM Group 구성

AWS 관리 콘솔의 IAM Side Bar에서 그룹을 선택하고 **새로운 그룹 생성**을 선택합니다.

![](.gitbook/assets/image%20%284%29.png)

첫번째 단계에서 그룹이름을 설정합니다.

![](.gitbook/assets/image%20%2840%29.png)

두번째 단계에서 정책\(Policy\)연결을 설정합니다. 

**필터 - 정책유형**에서 **EC2**를 입력하고, **"AmazonEC2FullAccess"**를 선택합니다.

![](.gitbook/assets/image%20%2811%29.png)

세번째 단계에서 검토를 확인하고, **그룹 생성**을 선택합니다.

![](.gitbook/assets/image%20%2817%29.png)

그룹 생성 마법사를 반복해서 Dev2, PRD, SuperUser, Test 그룹을 생성합니다.

Dev2 그룹은 정책\(Policy\)연결 단계에서 "AmazonS3FullAccess"를 선택합니다. PRD 그룹은 정책\(Policy\)연결단계에서 "AmazonEC2FullAccess", "AmazonS3FullAccess"를 선택합니다. SuperUser 그룹은 정책\(Policy\)연결단계에서 "AdministratorAccess"를 선택합니다. Test 그룹은 정책연결단계에서 선택하지 않습니다.

| **그룹\(Group\)** | **정책\(Policy\)** |
| :--- | :--- |
| Dev1 | AmazonEC2FullAccess |
| Dev2 | AmazonS3FullAccess |
| PRD | AmazonEC2FullAccess, AmazonS3FullAccess |
| SuperUser | AdministratorAccess |
| Test |  |

모든 단계를 완료하게 되면, 아래와 같이 구성됩니다.

![](.gitbook/assets/image%20%2818%29.png)

## User 인증강화를 위한 MFA 구성

### MFA 소개

보안 강화를 위해 멀티 팩터 인증\(MFA\)을 구성하여 AWS 리소스를 보호하는 것이 좋습니다. IAM 사용자 또는 AWS 계정 루트 사용자에 대해 MFA를 활성화할 수 있습니다. 루트 사용자에 대해 MFA를 활성화하면 해당 루트 사용자 자격 증명에만 적용됩니다. 이 계정의 IAM 사용자들은 자신의 자격 증명에 더하여 별도로 자격 증명을 갖게 되며, 이 별도의 자격 증명에 고유의 MFA가 구성됩니다.

MFA는 사용자가 AWS 웹 사이트 또는 서비스에 액세스할 때 사용자의 정규 로그인 자격 증명 외에도 AWS가 지원되는 MFA 메커니즘의 고유 인증을 제출하라고 요청함으로써 보안을 더욱 강화합니다.

현재 아래와 같이 4가지를 제공합니다.

**가상 MFA 디바이스** 스마트폰 또는 기타 디바이스에서 실행되며 물리적 디바이스를 에뮬레이션하는 소프트웨어 애플리케이션입니다. 디바이스가 동기화된 1회 암호 알고리즘에 따라 여섯 자리 숫자 코드를 생성합니다. 사용자는 로그인할 때 두 번째 웹페이지에서 디바이스의 유효 코드를 입력해야 합니다. 사용자에게 할당된 각 가상 MFA 디바이스는 고유해야 합니다. 사용자는 다른 사용자의 가상 MFA 디바이스의 코드를 입력하여 인증할 수 없습니다. 가상 MFA는 안전하지 않은 모바일 디바이스에서 실행될 수 있으므로 U2F 디바이스 또는 하드웨어 MFA 디바이스와 동일한 수준의 보안을 제공하지 않을 수 있습니다. \(참조 멀티팩터 인증 앱 - [https://aws.amazon.com/ko/iam/features/mfa/?audit=2019q1](https://aws.amazon.com/ko/iam/features/mfa/?audit=2019q1)\)

**U2F 보안 키**. 컴퓨터의 USB 포트에 연결하는 디바이스입니다. U2F는 [FIDO Alliance](https://fidoalliance.org) 에서 호스팅하는 공개 인증 표준입니다. U2F 보안 키를 활성화하려면, 코드를 수동으로 입력하는 대신, 본인의 자격 증명을 입력한 다음 디바이스를 터치하여 로그인합니다. 

**하드웨어 MFA 디바이스** 동기화된 1회 암호 알고리즘에 따라 여섯 자리 숫자 코드를 생성하는 하드웨어 디바이스입니다. 사용자는 로그인할 때 두 번째 웹페이지에서 디바이스의 유효 코드를 입력해야 합니다. 사용자에게 할당된 각 MFA 디바이스는 고유해야 합니다. 사용자는 다른 사용자의 디바이스의 코드를 입력하여 인증받을 수 없습니다.  

**SMS 문자 메시지 기반 MFA**. IAM 사용자 설정이 해당 사용자의 SMS 호환 모바일 디바이스의 전화번호를 포함하는 MFA 유형입니다. 사용자가 로그인하면 AWS가 SMS 문자 메시지로 여섯 자리 숫자 코드를 사용자의 모바일 디바이스로 전송합니다. 사용자는 로그인 시 두 번째 웹 페이지에서 이 코드를 입력해야 합니다. SMS 기반 MFA는 IAM 사용자만 사용할 수 있습니다. AWS 계정 루트 사용자에서는 이러한 유형의 MFA를 사용할 수 없습니다. 

### MFA 구성

이 랩에서는 captain 사용자\(User\)에 MFA 인증을 구성해 봅니다. 먼저 사용자가 소유한 스마트폰에서 OTP 인증 앱을 설치합니다. 안드로이드, iOS사용자는 Google의 OTP를 설치합니다.

![](.gitbook/assets/image%20%2835%29.png)

![](.gitbook/assets/image%20%2813%29.png)

AWS 관리 콘솔에서 IAM - 대시보드 - 사용자를 선택하고, 사용자 "captain"을 선택합니다.

![](.gitbook/assets/image%20%2830%29.png)

사용자 요약 화면에서 **"보안자격증명"**을 선택하고, **할당된 MFA 디바이스 - 관리**를 선택합니다.

![](.gitbook/assets/image%20%2828%29.png)

MFA 디바이스 관리화면에서 **"가상 MFA 디바이스"**를 선택합니다.

![](.gitbook/assets/image%20%289%29.png)

가상 MFA 디바이스 설정에서 **QR 코드 표시**를 선택합니다.

![](.gitbook/assets/image%20%2812%29.png)

QR 코드 표시를 선택하면, QR 코드가 나타납니다. 앞서 스마트폰에 설치한 Google Authenticator를 이용해서 QR Code를 촬영하고, 가상 OTP를 추가합니다.

![](.gitbook/assets/image%20%2839%29.png)

가상 OTP를 추가하게 되면 아래와 같이 OTP가 생성되고, 2개의 MFA OTP  번호를 입력합니다.

![](.gitbook/assets/image%20%2838%29.png)

AWS 관리 콘솔에서 아래와 같이 가상 MFA 디바이스 설정을 완료합니다.

![](.gitbook/assets/image%20%2834%29.png)

이제 관리 콘솔에서 로그아웃을 하고 "captain" 사용자로 로그인합니다.

![](.gitbook/assets/image%20%2819%29.png)

스마트폰에 설치된 가상 OTP를 이용해서 값을 입력하고 로그인합니다.

{% hint style="success" %}
필요에 따라서 몇개의 사용자를 선택해서 OTP 인증을 추가해 봅니다.
{% endhint %}



