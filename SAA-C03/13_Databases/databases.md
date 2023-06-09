# Databases
- Database Types
  - RDBMS (= SQL / OLTP): RDS, Aurora – 조인에 적합
  - NoSQL 데이터베이스 - 조인이 없고 SQL을 사용하지 않음: DynamoDB (JSON 형식), ElastiCache (키/값 쌍), Neptune (그래프), DocumentDB (MongoDB용), Keyspaces (Apache Cassandra용)
  - 객체 저장소: S3 (대용량 객체용) / Glacier (백업/아카이브용)
  - 데이터 웨어하우스 (= SQL 분석 / BI): Redshift (OLAP), Athena, EMR
  - 검색: OpenSearch (JSON) - 자유 텍스트, 비구조적인 검색
  - 그래프: Amazon Neptune - 데이터 간의 관계를 표시
  - 원장: Amazon Quantum Ledger Database
  - 시계열: Amazon Timestream
- Amazon RDS
  - 관리형 PostgreSQL / MySQL / Oracle / SQL Server / MariaDB / 사용자 정의
  - 프로비저닝된 RDS 인스턴스 크기와 EBS 볼륨 유형 및 크기
  - 저장소에 대한 자동 스케일링 기능
  - 읽기 복제본 및 멀티 AZ 지원
  - IAM, 보안 그룹, KMS, 전송 중 SSL을 통한 보안
  - 최대 35일까지 지원하는 시점 복원 기능을 갖춘 자동 백업
  - 장기 복구를 위한 수동 DB 스냅샷
  - 관리 및 예약된 유지보수 (다운타임 포함)
  - IAM 인증, Secrets Manager 통합을 지원
  - Oracle 및 SQL Server를 위한 RDS Custom으로 기본 인스턴스에 액세스하고 사용자 정의 가능
  - 사용 사례: 관계형 데이터셋 (RDBMS / OLTP) 저장, SQL 쿼리 수행, 트랜잭션
- Amazon Aurora
  - PostgreSQL / MySQL와 호환되는 API, 저장소와 컴퓨트의 분리
  - 저장소: 데이터는 3개의 가용 영역(AZ)에 걸쳐 6개의 복제본에 저장됩니다. 고가용성, 자가 치유, 자동 스케일링을 지원합니다.
  - 컴퓨트: 여러 가용 영역에 분산된 DB 인스턴스 클러스터, 읽기 복제본의 자동 스케일링
  - 클러스터: 작성자와 리더 DB 인스턴스를 위한 사용자 정의 엔드포인트
  - RDS와 동일한 보안/모니터링/유지보수 기능
  - Aurora의 백업 및 복원 옵션 이해
  - Aurora Serverless: 예측 불가능한/간헐적인 워크로드에 적합, 용량 계획이 필요 없음
  - Aurora Multi-Master: 연속적인 쓰기 장애 조치를 위한 다중 마스터(고쓰라이트 가능성)
  - Aurora Global: 각 지역에 최대 16개의 DB 읽기 인스턴스, 1초 미만의 저장소 복제
  - Aurora Machine Learning: Aurora에서 SageMaker 및 Comprehend를 사용하여 ML 수행
  - Aurora 데이터베이스 클론: 기존 클러스터에서 새로운 클러스터 생성, 스냅샷 복원보다 빠름
  - 사용 사례: RDS와 동일하지만 유지보수가 적고 유연성과 성능이 더 뛰어난 기능이 필요한 경우
- Amazon ElastiCache
  - Managed Redis / Memcached (RDS와 유사한 제공)
  - 인메모리 데이터 저장소, 서브밀리초의 지연 시간
  - ElastiCache 인스턴스 유형 선택 (예: cache.m6g.large)
  - 클러스터링(Redis) 및 멀티 AZ, 리드 복제(샤딩) 지원
  - IAM, 보안 그룹, KMS, Redis 인증을 통한 보안
  - 백업/스냅샷/시간 복원 기능
  - 관리 및 예약된 유지보수
  - 활용을 위해 일부 애플리케이션 코드 변경이 필요
  - 사용 사례: 키/값 저장소, 빈번한 읽기, 적은 쓰기, DB 쿼리 결과를 캐시로 저장, 웹 사이트의 세션 데이터 저장, SQL을 사용할 수 없음.
- Amazon DynamoDB
  - AWS 독점 기술, 관리형 서버리스 NoSQL 데이터베이스, 밀리초 지연 시간
  - 용량 모드: 선택적으로 자동 확장이 가능한 예약 용량 또는 온디맨드 용량
  - ElastiCache를 대체할 수 있는 키/값 저장소(세션 데이터 저장 등, TTL 기능 활용)
  - 기본적으로 고가용성, 멀티 AZ, 읽기와 쓰기는 결합되지 않으며 트랜잭션 기능
  - 읽기 캐시를 위한 DAX 클러스터, 마이크로초 읽기 지연 시간
  - IAM을 통한 보안, 인증 및 권한 부여
  - 이벤트 처리: AWS Lambda와 통합하기 위한 DynamoDB Streams 또는 Kinesis Data Streams
  - Global Table 기능: 액티브-액티브 설정
  - 최대 35일 동안의 자동 백업과 PITR(새 테이블로 복원) 또는 온디맨드 백업
  - PITR 창 내에서 RCU를 사용하지 않고 S3로 내보내기, WCU를 사용하지 않고 S3에서 가져오기
  - 스키마를 신속하게 진화시키는 데 좋음
  - 사용 사례: 서버리스 애플리케이션 개발(작은 문서, 수백 KB), 분산 서버리스 캐시
- Amazon S3
  - S3는 객체의 키/값 저장소입니다.
  - 큰 객체에 적합하며 작은 객체에는 적합하지 않습니다.
  - 서버리스로 작동하며 무한하게 확장 가능하며, 최대 객체 크기는 5TB이며 버전 관리 기능을 제공합니다.
  - 티어: S3 Standard, S3 Infrequent Access, S3 Intelligent, S3 Glacier + 라이프사이클 정책
  - 기능: 버전 관리, 암호화, 복제, MFA-Delete, 액세스 로그 등
  - 보안: IAM, 버킷 정책, ACL, 액세스 포인트, 객체/보관소 잠금, CORS, 객체/보관소 잠금
  - 암호화: SSE-S3, SSE-KMS, SSE-C, 클라이언트 측, 전송 중 TLS, 기본 암호화
  - S3 Batch를 사용한 일괄 작업, S3 Inventory를 사용한 파일 목록 작성
  - 성능: 멀티파트 업로드, S3 Transfer Acceleration, S3 Select
  - 자동화: S3 이벤트 알림 (SNS, SQS, Lambda, EventBridge)
  - 사용 사례: 정적 파일, 큰 파일을 위한 키/값 저장소, 웹사이트 호스팅
- DocumentDB
  - Aurora는 PostgreSQL 및 MySQL의 "AWS 구현"입니다.
  - DocumentDB는 MongoDB의 "AWS 구현"입니다 (NoSQL 데이터베이스).
  - MongoDB는 JSON 데이터를 저장, 쿼리 및 인덱싱하는 데 사용됩니다.
  - Aurora와 유사한 "배포 개념"을 갖고 있습니다.
  - 완전히 관리되며, 3개의 가용 영역 간 복제를 통해 고가용성을 제공합니다.
  - DocumentDB 스토리지는 자동으로 10GB 단위로 증가하여 최대 64TB까지 확장됩니다.
  - 수백만 개의 요청을 처리하는 워크로드에 자동으로 스케일링됩니다.
- Amazon Neptune
  - 완전히 관리되는 그래프 데이터베이스
  - 인기있는 그래프 데이터셋은 소셜 네트워크일 것입니다.
    - 사용자는 친구를 갖고 있습니다.
    - 게시물에는 댓글이 있습니다.
    - 댓글에는 사용자의 좋아요가 있습니다.
    - 사용자는 게시물을 공유하고 좋아합니다.
  - 3개의 가용 영역에 걸쳐 고가용성을 제공하며, 최대 15개의 읽기 레플리카를 사용할 수 있습니다.
  - 고도로 연결된 데이터셋을 사용하는 애플리케이션을 빌드하고 실행할 수 있습니다.이러한 복잡하고 어려운 쿼리에 최적화되어 있습니다.
  - 최대 수십억 개의 관계를 저장하고 밀리초의 지연 시간으로 그래프를 쿼리할 수 있습니다.
  - 여러 가용 영역에 걸쳐 복제되어 고가용성을 제공합니다.
  - 지식 그래프 (위키피디아), 사기 탐지, 추천 엔진, 소셜 네트워킹에 적합합니다.
- Amazon Keyspaces (for Apache Cassandra)
  - Apache Cassandra는 오픈 소스 NoSQL 분산 데이터베이스입니다.
  - AWS에서 제공하는 관리형 Apache Cassandra 호환 데이터베이스 서비스입니다.
  - 서버리스, 확장 가능하며 고가용성을 제공하며 AWS가 완전히 관리합니다.
  - 애플리케이션의 트래픽에 따라 테이블을 자동으로 확장/축소합니다.
  - 테이블은 여러 가용 영역에 3번 복제됩니다.
  - Cassandra Query Language (CQL)을 사용합니다.
  - 어떤 규모에서도 한 자릿수 밀리초의 지연 시간, 초당 수천 건의 요청 처리 가능합니다.
  - 용량: 온디맨드 모드 또는 자동 스케일링을 지원하는 프로비저닝 모드로 운영 가능합니다.
  - 암호화, 백업, 최대 35일까지의 PITR(Point-In-Time Recovery) 기능을 제공합니다.
  - 사용 사례: IoT 장치 정보 저장, 시계열 데이터 등
- Amazon QLDB
  - QLDB는 "Quantum Ledger Database"의 약자입니다.
  - 원장(ledger)은 금융 거래를 기록하는 책입니다.
  - 완전히 관리되며 서버리스하며 고가용성을 제공하며 3개의 가용 영역에 복제됩니다.
  - 애플리케이션 데이터에 대한 모든 변경 내역을 시간에 따라 검토하는 데 사용됩니다.
  - 변경된 항목을 삭제하거나 수정할 수 없으며 암호학적으로 검증할 수 있는 불변 시스템입니다.
  - 일반적인 원장 블록체인 프레임워크보다 2-3배 더 나은 성능을 제공하며 SQL을 사용하여 데이터를 조작할 수 있습니다.
  - Amazon Managed Blockchain과의 차이점: 탈중앙화 구성 요소가 없으며 금융 규정 규칙에 따릅니다.
- Amazon Timestream
  - 완전히 관리되는, 빠르고 확장 가능하며 서버리스한 시계열 데이터베이스입니다.
  - 용량에 따라 자동으로 스케일 업/다운합니다.
  - 하루에 수조 개의 이벤트를 저장하고 분석할 수 있습니다.
  - 관계형 데이터베이스의 1/10 비용으로 1000배 빠릅니다.
  - 예약된 쿼리, 다중 측정 레코드, SQL 호환성을 제공합니다.
  - 데이터 저장 계층 구분: 최근 데이터는 메모리에 유지하고, 과거 데이터는 비용을 최적화한 저장소에 유지합니다.
  - 시계열 분석 기능이 내장되어 있어 데이터의 패턴을 거의 실시간으로 식별하는 데 도움을 줍니다.
  - 데이터 전송 및 저장 암호화를 지원합니다.
  - 사용 사례: IoT 애플리케이션, 운영 애플리케이션, 실시간 분석 등