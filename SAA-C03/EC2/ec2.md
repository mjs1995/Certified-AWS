- EC2 예산 설정
  - root 계정 > 계정 설정 > 결제 정보에 대한 IAM 사용자 및 역할 액세스 > IAM 액세스 활성화 
  - <img width="955" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/183bb65c-7457-4c44-bb49-2f62e45b1c79">
  - iam 사용자 계정 > Budgets > 예산 생성 
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/0c93a42a-b630-40a8-8703-82f2b090ee03)
- EC2 기초 
  - EC2 = Elastic Compute Cloud = Infrastructure as a Service
  - 주요 구성 요소 
    - Renting virtual machines (EC2)
    - Storing data on virtual drives (EBS)
    - Distributing load across machines (ELB)
    - Scaling the services using an auto-scaling group (ASG)
  - EC2 사용자 데이터
    - EC2 사용자 데이터 스크립트를 사용하여 인스턴스를 부트스트랩할 수 있습니다.
    - 부트스트랩은 인스턴스가 시작될 때 명령을 실행하는 것을 의미합니다.
    - 해당 스크립트는 인스턴스가 처음 시작될 때에만 실행됩니다.
    - EC2 사용자 데이터는 다음과 같은 부트 작업을 자동화하는 데 사용됩니다:
      - 업데이트 설치
      - 소프트웨어 설치
      - 인터넷에서 일반 파일 다운로드
      - 생각할 수 있는 모든 작업
    - EC2 사용자 데이터 스크립트는 root 사용자로 실행됩니다.
  - 인스턴스 타입
    - <img width="1117" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/5f3f6902-8320-4024-bb98-b33a51629fec">
- EC2 생성
  - EC2 > 인스턴스 > 인스턴스 시작 > 인스턴스 생성
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/740b4b88-4893-4802-a304-e04ec5f6f427)
  - 키 페어(로그인) > 키 페어 생성
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/b209dd22-5472-49e4-aeb4-c879277da58a)
  - 네트워크 설정 > 인터넷에서 HTTP 트래픽 허용 체크 
  - 고급 세부 정보 > 사용자 데이터 > 명령어 입력 (몇 가지 업데이트하고 HTTP 웹 서버를 머신에 설치)
  - ```bash
    #!/bin/bash
    # Use this for your user data (script from top to bottom)
    # install httpd (Linux 2 version)
    yum update -y
    yum install -y httpd
    systemctl start httpd
    systemctl enable httpd
    echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
    ```
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/7920997b-54ae-43f5-a177-af68f40136f4)
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/c7103303-4d13-47d5-bfaf-f122c856816e)
- EC2 인스턴스 유형 기본 사항
  - m5.2xlarge
    - m: instance class
    - 5: generation (AWS improves them over time)
    - 2xlarge: size within the instance class
  - EC2 인스턴스 타입 
    - Compute Optimized
      - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/90e023be-fd67-440f-b2bc-29edc71e82a3)
      - 고성능 프로세서가 필요한 계산 집약적인 작업에 적합합니다.
        - 일괄 처리 작업 부하
        - 미디어 변환 작업
        - 고성능 웹 서버
        - 고성능 컴퓨팅 (HPC)
        - 과학 모델링 및 머신 러닝
        - 전용 게임 서버
    - Memory Optimized
      - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/b913fd28-9aa8-4121-b210-6efc36c1c4c5)
      - 대용량 데이터 세트를 메모리에서 처리하는 작업에 빠른 성능을 제공합니다.
      - 사용 사례
        - 고성능 관계형/비관계형 데이터베이스
        - 분산 웹 규모의 캐시 저장소
        - 비즈니스 인텔리전스(BI)에 최적화된 인메모리 데이터베이스
        - 대용량 비구조화 데이터의 실시간 처리를 수행하는 애플리케이션
    - Storage Optimized
      - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/43a8f730-9c0b-49df-9213-027c9a6ee358)
      - 로컬 스토리지에서 대용량 데이터 세트에 대한 고도의 순차적인 읽기 및 쓰기 액세스가 필요한 저장 과제에 적합합니다.
      - 사용 사례
        - 고주파 온라인 트랜잭션 처리(OLTP) 시스템
        - 관계형 및 NoSQL 데이터베이스
        - 인메모리 데이터베이스의 캐시 (예: Redis)
        - 데이터 웨어하우징 애플리케이션
        - 분산 파일 시스템
    - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/4d7552ec-8995-499e-b28f-3ce4e63723dc)
