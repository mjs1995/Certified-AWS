# Disaster Recovery & Migrations
- 재해 복구 개요
  - 회사의 비즈니스 연속성 또는 재정에 부정적인 영향을 미치는 모든 사건은 재해로 간주됩니다.
  - 재해 복구 (DR)는 재해에 대비하고 재해로부터 회복하는 것을 목표로 합니다.
  - 어떤 종류의 재해 복구가 필요한가요?
    - 온프레미스 => 온프레미스: 전통적인 재해 복구 방식으로 매우 비용이 많이 들 수 있습니다.
    - 온프레미스 => AWS 클라우드: 하이브리드 복구
    - AWS 클라우드 리전 A => AWS 클라우드 리전 B
  - 두 가지 용어를 정의해야 합니다
    - RPO: 복구 지점 목표 (Recovery Point Objective)
    - RTO: 복구 시간 목표 (Recovery Time Objective)
- 재해 복구 전략
  - 백업 및 복원 (Backup and Restore) - High RPO
  - 파일럿 라이트 (Pilot Light)
    - 클라우드에서 항상 작동 중인 앱의 작은 버전
    - 핵심 기능에 유용함 (파일럿 라이트)
    - 백업 및 복원과 매우 유사함
    - 핵심 시스템이 이미 가동 중이므로 복원보다 빠름
  - 웜 스탠바이 (Warm Standby)
    - 전체 시스템이 작동 중이지만 최소 크기로 유지됨
    - 재해 발생 시, 운영 부하에 맞게 확장할 수 있음
  - 핫 사이트 / 멀티 사이트 접근 방식 (Hot Site / Multi Site Approach)
    - 매우 낮은 RTO(복구 시간 목표) (분 또는 초 단위) - 매우 비용이 많이 듦
    - 전체 제품 규모가 AWS와 온프레미스에서 운영 중
- 재해 복구 팁
  - 백업
    - EBS 스냅샷, RDS 자동 백업/스냅샷 등
    - S3/S3 IA/Glacier로 정기적으로 데이터 전송, 수명 주기 정책, 교차 리전 복제
    - 온프레미스에서의 경우: Snowball 또는 Storage Gateway 사용
  - 고가용성
    - Route53을 사용하여 DNS를 리전 간에 마이그레이션
    - RDS Multi-AZ, ElastiCache Multi-AZ, EFS, S3
    - Direct Connect에서의 복구로 Site-to-Site VPN 사용
  - 복제
    - RDS 복제 (교차 리전), AWS Aurora + Global Databases
    - 온프레미스에서 RDS로의 데이터베이스 복제
    - Storage Gateway
  - 자동화
    - CloudFormation/Elastic Beanstalk을 사용하여 전체 새로운 환경 재생성
    - CloudWatch에서 알람 실패 시 EC2 인스턴스 복구/재부팅
    - 사용자 정의 자동화를 위한 AWS Lambda 함수
  - 혼돈
    - Netflix는 "simian-army"를 사용하여 무작위로 EC2를 종료함
- DMS(Database Migration Service) - 데이터베이스 마이그레이션 서비스
  - 빠르고 안전하게 데이터베이스를 AWS로 마이그레이션할 수 있으며, 견고하며 자체 복구 기능을 가지고 있음
  - 마이그레이션 중에도 소스 데이터베이스는 사용 가능함
  - 다음을 지원함
    - 동질적인 마이그레이션: 예를 들어 Oracle에서 Oracle로
    - 이질적인 마이그레이션: 예를 들어 Microsoft SQL Server에서 Aurora로
  - CDC(Continuous Data Replication)를 사용한 지속적인 데이터 복제
  - 복제 작업을 수행하기 위해 EC2 인스턴스를 생성해야 함
  - DMS(Source) - 데이터베이스 마이그레이션 서비스 소스
    - 온프레미스 및 EC2 인스턴스 데이터베이스: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, MongoDB, SAP, DB2
    - Azure: Azure SQL Database
    - Amazon RDS: Aurora를 포함한 모든 RDS
    - Amazon S3
    - DocumentDB
  - DMS(Target) - 데이터베이스 마이그레이션 서비스 대상
    - 온프레미스 및 EC2 인스턴스 데이터베이스: Oracle, MS SQL Server, MySQL, MariaDB, PostgreSQL, SAP
    - Amazon RDS
    - Redshift, DynamoDB, S3
    - OpenSearch Service
    - Kinesis Data Streams
    - Apache Kafka
    - DocumentDB 및 Amazon Neptune
    - Redis 및 Babelfish
- AWS Schema Conversion Tool (SCT) - AWS 스키마 변환 도구
  - 데이터베이스의 스키마를 한 엔진에서 다른 엔진으로 변환합니다.
  - OLTP 예시: (SQL Server 또는 Oracle)에서 MySQL, PostgreSQL, Aurora로 변환
  - OLAP 예시: (Teradata 또는 Oracle)에서 Amazon Redshift로 변환
  - 데이터 변환을 최적화하기 위해 컴퓨팅 집약적인 인스턴스를 선호합니다.
  - 동일한 DB 엔진으로 마이그레이션하는 경우에는 SCT를 사용할 필요가 없습니다.
    - 예: 온프레미스 PostgreSQL => RDS PostgreSQL
    - DB 엔진은 여전히 PostgreSQL입니다. (RDS는 플랫폼 역할을 합니다)
- RDS 및 Aurora MySQL 마이그레이션
  - RDS MySQL에서 Aurora MySQL로의 마이그레이션
    - 옵션 1: RDS MySQL의 DB 스냅샷을 복원하여 MySQL Aurora DB로
    - 옵션 2: RDS MySQL에서 Aurora Read Replica를 생성하고 복제 지연이 0이 될 때까지 기다린 후 독립된 DB 클러스터로 프로모션 (시간이 소요되며 비용이 발생할 수 있음)
  - 외부 MySQL에서 Aurora MySQL로의 마이그레이션
    - 옵션 1:
      - Percona XtraBackup을 사용하여 Amazon S3에 파일 백업 생성
      - Amazon S3에서 Aurora MySQL DB 생성
    - 옵션 2
      - Aurora MySQL DB 생성
      - mysqldump 유틸리티를 사용하여 MySQL을 Aurora로 마이그레이션 (S3 방법보다 느릴 수 있음)
  - 두 데이터베이스가 모두 작동 중인 경우 DMS 사용
- RDS 및 Aurora PostgreSQL 마이그레이션
  - RDS PostgreSQL에서 Aurora PostgreSQL로의 마이그레이션
    - 옵션 1: RDS PostgreSQL의 DB 스냅샷을 복원하여 PostgreSQL Aurora DB로
    - 옵션 2: RDS PostgreSQL에서 Aurora Read Replica를 생성하고 복제 지연이 0이 될 때까지 기다린 후 독립된 DB 클러스터로 프로모션 (시간이 소요되며 비용이 발생할 수 있음)
  - 외부 PostgreSQL에서 Aurora PostgreSQL로의 마이그레이션
    - 백업을 생성하고 Amazon S3에 저장
    - aws_s3 Aurora 확장을 사용하여 가져옴
  - 두 데이터베이스가 모두 작동 중인 경우 DMS 사용
- AWS와 함께하는 온프레미스 전략
  - Amazon Linux 2 AMI를 VM 형식(.iso)으로 다운로드 가능
    - VMWare, KVM, VirtualBox(Oracle VM), Microsoft Hyper-V와 같은 가상화 플랫폼에서 사용 가능
  - VM Import / Export
    - 기존 애플리케이션을 EC2로 마이그레이션
    - 온프레미스 VM을 위한 재해 복구 저장소 전략 구축
    - EC2에서 VM을 온프레미스로 내보낼 수 있음
  - AWS Application Discovery Service
    - 온프레미스 서버에 대한 정보 수집하여 마이그레이션 계획 수립
    - 서버 활용 및 종속성 매핑
    - AWS Migration Hub로 추적
  - AWS Database Migration Service (DMS)
    - 온프레미스 => AWS, AWS => AWS, AWS => 온프레미스로 데이터베이스 복제
    - Oracle, MySQL, DynamoDB 등 다양한 데이터베이스 기술과 호환
  - AWS Server Migration Service (SMS)
    - 온프레미스 실시간 서버의 증분 복제를 AWS로 수행
- AWS Backup
  - 완전히 관리되는 서비스로, AWS 서비스 전반에 걸쳐 백업을 중앙에서 관리하고 자동화할 수 있습니다.
  - 사용자는 사용자 정의 스크립트나 수동 프로세스를 만들 필요가 없습니다.
  - AWS Backup은 다음과 같은 서비스를 지원합니다
    - Amazon EC2 / Amazon EBS
    - Amazon S3
    - Amazon RDS (모든 DB 엔진) / Amazon Aurora / Amazon DynamoDB
    - Amazon DocumentDB / Amazon Neptune
    - Amazon EFS / Amazon FSx (Lustre 및 Windows File Server)
    - AWS Storage Gateway (Volume Gateway)
  - 또한, AWS Backup은 지역 간 백업과 계정 간 백업을 지원합니다.
  - AWS Backup은 지원되는 서비스에 대해 Point-In-Time Recovery (PITR)를 지원합니다.
  - 온디맨드 및 예약된 백업을 지원하며, 태그 기반의 백업 정책을 생성할 수 있습니다. 이러한 백업 정책은 Backup Plans라고도 불립니다.
  - Backup Plans을 설정할 때 다음과 같은 옵션을 설정할 수 있습니다
    - 백업 빈도 (매 12시간, 매일, 매주, 매월, cron 표현식)
    - 백업 창 (Backup window)
    - Cold Storage로의 이전 시기 (Never, Days, Weeks, Months, Years)
    - 보존 기간 (Always, Days, Weeks, Months, Years)
  - AWS Backup Vault Lock
    - AWS Backup Vault에 저장된 모든 백업에 대해 WORM (Write Once Read Many) 상태를 적용합니다.
    - 이는 다음과 같은 추가적인 방어층을 제공하여 백업을 보호합니다
      - 실수로나 악의적으로 삭제 작업을 방지합니다.
      - 보존 기간을 단축하거나 변경하는 업데이트로부터 보호합니다.
    - 루트 사용자라도 활성화된 경우 백업을 삭제할 수 없습니다.
- AWS Application Discovery Service
  - 온프레미스 데이터 센터에 대한 정보를 수집하여 마이그레이션 프로젝트를 계획하는 데 도움을 줍니다.
  - 마이그레이션을 위해 서버 활용도 데이터와 의존성 매핑은 중요한 정보입니다.
  - Agentless Discovery (AWS Agentless Discovery Connector)
    - VM 인벤토리, 구성 및 CPU, 메모리, 디스크 사용량과 같은 성능 이력과 같은 정보를 수집합니다.
  - Agent-based Discovery (AWS Application Discovery Agent)
    - 시스템 구성, 시스템 성능, 실행 중인 프로세스 및 시스템 간의 네트워크 연결 세부 정보 등을 수집합니다.
  - 수집된 데이터는 AWS Migration Hub 내에서 확인할 수 있습니다.
- AWS Application Migration Service (MGN)
  - CloudEndure Migration의 "AWS 진화"로서 AWS Server Migration Service (SMS)를 대체하는 서비스입니다.
  - 이 서비스는 AWS로의 애플리케이션 마이그레이션을 간소화하기 위한 "리프트 앤 쉬프트(rehost)" 솔루션을 제공합니다.
  - MGN은 물리적, 가상 및 클라우드 기반 서버를 AWS에서 네이티브하게 실행할 수 있도록 변환해 줍니다. 다양한 플랫폼, 운영 체제 및 데이터베이스를 지원하며, 최소한의 다운타임과 비용으로 마이그레이션을 수행할 수 있습니다.
- VMware Cloud on AWS(VMware Cloud에서 AWS로 진입)
  - 일부 고객은 VMware Cloud를 사용하여 온프레미스 데이터 센터를 관리합니다.
  - 그들은 데이터 센터 용량을 AWS로 확장하고 VMware Cloud 소프트웨어를 계속 사용하려고 합니다.
  - 사용 사례
    - VMware vSphere 기반 워크로드를 AWS로 마이그레이션
    - VMware vSphere 기반의 프로덕션 워크로드를 개인, 공용 및 하이브리드 클라우드 환경에서 실행
    - 재해 복구 전략 구축
- 대량의 데이터를 AWS로 전송하는 방법
  - 예시: 200 TB의 데이터를 클라우드로 전송합니다. 우리는 100 Mbps의 인터넷 연결을 가지고 있습니다.
  - 인터넷 또는 사이트 간 VPN을 통해
    - 즉시 설정할 수 있습니다.
    - 200(TB)*1000(GB)*1000(MB)*8(Mb)/100 Mbps = 16,000,000초 = 185일이 소요됩니다.
  - Direct Connect 1Gbps를 통해
    - 한 번의 설정에는 시간이 오래 걸립니다(약 한 달).
    - 200(TB)*1000(GB)*8(Gb)/1 Gbps = 1,600,000초 = 18.5일이 소요됩니다.
  - Snowball을 통해
    - 2~3개의 스노우볼을 병렬로 사용해야 합니다.
    - 전체 전송에는 약 1주일이 소요됩니다.
  - DMS와 함께 사용할 수 있습니다
    - 지속적인 복제 또는 전송을 위해: 사이트 간 VPN 또는 DMS 또는 DataSync와 함께 DX를 사용합니다.
