# IAM
- 사용자 추가 
  - <img width="1274" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/c73fd25b-b7be-47cc-a657-424f64f8901d">
  - 권한 설정에서 사용자 그룹 생성을 통해 그룹 이름을 admin으로 등록해줍니다. 권한 정책은 AdministratorAccess로 지정합니다.
  - <img width="720" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/af9042c0-8150-4acb-97f4-ef867faa4ce9">
  - 사용자 생성을 완료해줍니다.
- 대시보드에서 AWS 계정의 별칭을 지정해줍니다. 그 후에 AWS 콘솔에서 생성된 IAM 유저로 로그인합니다.
  - <img width="1003" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/700d8fd4-ef07-4ab0-b58f-b230be230840">

# IAM Policies Structure
- <img width="418" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/4597d279-6fb1-4626-af18-ceb2eae8ba1c">
  - Version : 정책 언어 버전, 항상 "2012-10-17"을 포함해야 함
  - ID: 정책 식별자 (선택 사항)
  - Statement: 하나 이상의 개별 문장(필수)으로 구성됨
    - Sid: 문장 식별자 (선택 사항)
    - Effect: 문장이 액세스를 허용하거나 거부하는지 여부 (Allow, Deny)
    - Principal: 이 정책이 적용되는 계정/사용자/역할
    - Action: 이 정책이 허용하거나 거부하는 작업 목록
    - Resource: 작업이 적용되는 리소스 목록
    - Condition: 이 정책이 적용되는 조건(선택 사항)
- 암호 정책
  - 계정설정에서 암호 정책에 대해 설정할 수 있습니다.
  - <img width="1050" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/d15b7073-0532-4787-80ff-5c72f1efa2d9">
- MFA (Multi Factor Authentication - 다요소 인증) 
  - MFA = password you know + security device you own
  - MFA devices options in AWS
    - <img width="957" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/8d4edd77-ffce-454d-aad6-34452d245f33">
    - <img width="911" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/983a21cd-12dc-4bf5-95db-fac263626845">
