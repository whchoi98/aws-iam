# IAM 정책\(Policy\)과 권한\(Permission\)

IAM 정책 소개

IAM에서 자격 증명에 대한 권한을 설정해야 할 경우 AWS 관리형 정책, 고객 관리형 정책 또는 인라인 정책 중 어느 것을 사용할지를 결정해야 합니다.

_AWS 관리형 정책_은 AWS에서 생성 및 관리하는 독립적인 정책입니다. 여기에서 _독립적인 정책_이란 정책 스스로 정책 이름이 포함된 Amazon 리소스 이름\(ARN\)을 갖고 있다는 것을 의미합니다. 

예를 들어 `arn:aws:iam::aws:policy/IAMReadOnlyAccess`는 AWS 관리형 정책입니다.

