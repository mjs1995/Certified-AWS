# DNS
- DNS
  - DNS는 "Domain Name System"의 약어로, 인간이 읽기 쉬운 호스트네임을 컴퓨터의 IP 주소로 변환하는 역할을 합니다. 예를 들어, www.google.com은 172.217.18.36으로 변환됩니다. DNS는 인터넷의 기반이 되는 시스템으로, 계층적인 네이밍 구조를 사용합니다.
  - DNS Terminologies
    - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/b7fba723-57ee-4a9e-8cb0-1d6bbc04593b)
    - Domain Registrar: Amazon Route 53, GoDaddy, …
    - DNS Records: A, AAAA, CNAME, NS, …
    - Zone File: contains DNS records
    - Name Server: resolves DNS queries (Authoritative or Non-Authoritative)
    - Top Level Domain (TLD): .com, .us, .in, .gov, .org, …
    - Second Level Domain (SLD): amazon.com, google.com, …
  - DNS Works
    - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/e4a56ac8-24fc-41ff-b53d-579d57107374)

# Route 53
- Amazon Route 53
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/f62820a8-b9af-4fba-9c52-38cdcf8a2d2d)
  - 고가용성, 확장 가능성, 완전히 관리되는 권한 있는 DNS
    - 권한 있는(Authoritative) DNS: 고객(당신)이 DNS 레코드를 업데이트할 수 있는 기능
  - Route 53은 도메인 등록기(Domain Registrar)로 사용될 수도 있음
  - 리소스의 상태를 확인할 수 있는 기능
  - 100% 가용성 SLA를 제공하는 유일한 AWS 서비스
  - Route 53을 선택하는 이유? 53은 전통적인 DNS 포트에 대한 참조입니다.
- Records
  - 도메인의 트래픽을 어떻게 라우팅할지 결정하는 방법에 대해 설명드리겠습니다.
  - 각 레코드에는 다음과 같은 정보가 포함됩니다
    - 도메인/하위도메인 이름 - 예: example.com
    - 레코드 유형 - 예: A 또는 AAAA
    - 값 - 예: 12.34.56.78
    - 라우팅 정책 - Route 53이 쿼리에 응답하는 방식
    - TTL(time to leave) - 레코드가 DNS 리졸버에서 캐시되는 시간
  - Route 53은 다음과 같은 DNS 레코드 유형을 지원합니다
    - (필수) A / AAAA / CNAME / NS
    - (고급) CAA / DS / MX / NAPTR / PTR / SOA / TXT / SPF / SRV
  - Types
    - A - 호스트네임을 IPv4로 매핑합니다.
    - AAAA - 호스트네임을 IPv6로 매핑합니다.
    - CNAME - 호스트네임을 다른 호스트네임으로 매핑합니다.
      - 대상은 A 또는 AAAA 레코드를 가져야 하는 도메인 이름입니다.
      - DNS 네임스페이스의 최상위 노드(Zone Apex)에는 CNAME 레코드를 생성할 수 없습니다.
      - 예: example.com에는 생성할 수 없지만, www.example.com에는 생성할 수 있습니다.
    - NS - 호스팅된 영역(Hosted Zone)의 네임 서버(Name Servers)입니다.
      - 도메인의 트래픽이 어떻게 라우팅되는지를 제어합니다.
- Hosted Zones
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/03f50355-b26b-4d47-81e2-f9da2d528780)
  - 도메인 및 하위 도메인으로 트래픽을 어떻게 라우팅할지 정의하는 레코드의 컨테이너입니다.
  - Public Hosted Zones - 인터넷 상에서 트래픽을 라우팅하는 방법을 지정하는 레코드가 포함되어 있습니다(공개 도메인 이름).
    - application1.mypublicdomain.com
  - Private Hosted Zones - 하나 이상의 VPC 내에서 트래픽을 라우팅하는 방법을 지정하는 레코드가 포함되어 있습니다(사설 도메인 이름).
    - application1.company.internal
  - 호스팅된 영역(Hosted Zone)당 월별 비용은 $0.50입니다.
- Records TTL (Time To Live)
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/1e1d1241-4e7f-4b0b-98dc-3f8fb56584d7)
  - 높은 TTL - 예: 24시간
    - Route 53에서의 트래픽이 적어집니다.
    - 레코드가 오래된 상태일 수 있습니다.
  - 낮은 TTL - 예: 60초
    - Route 53에서의 트래픽이 늘어납니다. ($$)
    - 레코드가 오래된 상태는 짧은 시간 동안 유지됩니다.
    - 레코드를 쉽게 변경할 수 있습니다.
  - Alias 레코드를 제외하고, 각 DNS 레코드에는 TTL이 필수입니다.
- CNAME vs Alias
  - AWS 리소스(로드 밸런서, CloudFront 등)는 AWS 호스트네임을 노출시킵니다
    - lb1-1234.us-east-2.elb.amazonaws.com과 같은 형식이며, 이를 myapp.mydomain.com으로 사용하고 싶습니다.
  - CNAME
    - 호스트네임을 다른 호스트네임으로 매핑합니다. (app.mydomain.com => blabla.anything.com)
    - 비루트 도메인(즉, something.mydomain.com)에만 사용할 수 있습니다.
  - Alias
    - 호스트네임을 AWS 리소스로 매핑합니다. (app.mydomain.com => blabla.amazonaws.com)
    - 루트 도메인과 비루트 도메인(즉, mydomain.com)에 모두 사용할 수 있습니다.
    - 무료입니다.
    - 기본적인 상태 확인 기능이 내장되어 있습니다.
- Alias Records
  - 호스트네임을 AWS 리소스로 매핑합니다.
  - DNS 기능의 확장 기능입니다.
  - 리소스의 IP 주소 변경을 자동으로 인식합니다.
  - CNAME과 달리 DNS 네임스페이스의 최상위 노드(Zone Apex)에 사용할 수 있습니다. 예: example.com
  - Alias 레코드는 항상 AWS 리소스에 대한 A/AAAA 유형입니다 (IPv4 / IPv6).
  - TTL을 설정할 수 없습니다.
– Alias Records Targets
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/9f5ee54d-dd23-40f9-b5fa-776fa2e2d7a9)
  - Elastic Load Balancers
  - CloudFront Distributions
  - API Gateway
  - Elastic Beanstalk environments
  - S3 Websites
  - VPC Interface Endpoints
  - Global Accelerator accelerator
  - Route 53 record in the same hosted zone
  - EC2 DNS 이름에는 ALIAS 레코드를 설정할 수 없습니다.
- 라우팅 정책
  - Route 53이 DNS 쿼리에 응답하는 방식을 정의합니다.
  - "Routing"이라는 단어에 혼동을 받지 마십시오.
    - 이는 트래픽을 라우팅하는 로드 밸런서 라우팅과는 다른 개념입니다.
    - DNS는 트래픽을 라우팅하지 않으며, 단지 DNS 쿼리에 응답합니다.
  - Route 53은 다음과 같은 라우팅 정책을 지원합니다
    - Simple (단순)
      - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/98ff7908-eac4-46e9-8934-6e17c924071b)
      - 일반적으로 단일 리소스로 트래픽을 라우팅합니다.
      - 동일한 레코드에서 여러 값을 지정할 수 있습니다.
      - 여러 값이 반환되면 클라이언트에 의해 임의로 하나가 선택됩니다.
      - Alias가 활성화되면 하나의 AWS 리소스만 지정합니다.
      - Health Check와 연관시킬 수 없습니다.
    - Weighted (가중치)
      - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/0ea8a0f0-2699-41a7-b28e-857127acfd0d)
      - 각 특정 리소스로 전송되는 요청의 백분율을 제어할 수 있습니다.
      - 각 레코드에 상대적인 가중치를 할당합니다
        - 레코드 (%) = (가중치 / 전체 가중치의 합) * 100
        - 가중치의 합은 100이 되지 않아도 됩니다.
      - DNS 레코드는 동일한 이름과 유형을 가져야 합니다.
      - Health Check와 연관시킬 수 있습니다.
      - 사용 사례: 지역 간 로드 밸런싱, 새로운 애플리케이션 버전 테스트 등
      - 트래픽을 보내지 않기 위해 레코드에 가중치 0을 할당할 수 있습니다.
      - 모든 레코드의 가중치가 0인 경우, 모든 레코드가 동일하게 반환됩니다.
    - Failover (장애 극복)
      - <img width="1195" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/cc25ca0e-60da-404b-a841-3756d14f833c">
    - Latency based (지연 시간 기반)
      - 사용자와 가장 가까운 지연 시간을 가진 리소스로 리디렉션합니다.
      - 사용자의 지연 시간이 우선 순위인 경우 매우 유용합니다.
      - 지연 시간은 사용자와 AWS 지역 간의 트래픽을 기반으로 합니다.
      - 독일 사용자가 가장 낮은 지연 시간인 경우 미국으로 리디렉션될 수 있습니다.
      - Health Check와 연관시킬 수 있으며(장애 극복 기능을 가지고 있음), 사용자의 요청을 가장 낮은 지연 시간을 가진 리소스로 전달할 수 있습니다.
    - Geolocation (지리적 위치)
      - <img width="1267" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/c40f9116-042c-43c4-9790-84e35d386534">
      - 지역 기반 라우팅으로, 지리적 위치를 기준으로 합니다.
      - 대륙, 국가 또는 미국 주별로 위치를 지정할 수 있습니다. (중첩이 있는 경우, 가장 정확한 위치를 선택합니다)
      - 위치와 일치하는 항목이 없는 경우 "기본(Default)" 레코드를 생성해야 합니다.
      - 사용 사례: 웹 사이트 지역화, 콘텐츠 배포 제한, 로드 밸런싱 등
      - Health Check와 연관시킬 수 있습니다.
    - Multi-Value Answer (다중 값 응답)
      - 여러 리소스로 트래픽을 라우팅해야 할 때 사용합니다.
      - Route 53은 여러 개의 값을/리소스를 반환합니다.
      - Health Check와 연관시킬 수 있습니다 (건강한 리소스에 대한 값만 반환).
      - Multi-Value 쿼리마다 최대 8개의 건강한 레코드가 반환됩니다.
      - Multi-Value는 ELB를 대체하기 위한 것이 아닙니다.
    - Geoproximity (Route 53 Traffic Flow 기능을 사용한 지리적 근접)
      - 사용자 및 리소스의 지리적 위치에 따라 트래픽을 리소스로 라우팅합니다.
      - 정의된 편향 값을 기반으로 리소스로의 트래픽을 더 많이 전달할 수 있는 기능입니다.
      - 지리적 영역의 크기를 변경하려면 편향 값 지정
        - 확장하기 위해 (1에서 99까지) - 리소스로의 트래픽 증가
        - 축소하기 위해 (-1에서 -99까지) - 리소스로의 트래픽 감소
      - 리소스는 다음과 같을 수 있습니다
        - AWS 리소스 (AWS 리전 지정)
        - AWS가 아닌 리소스 (위도 및 경도 지정)
      - 이 기능을 사용하려면 Route 53 Traffic Flow를 사용해야 합니다.
- Health Checks
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/1604e5c3-af8b-4b5e-9691-44532e61125a)
  - HTTP Health Checks는 공개 리소스에만 적용됩니다.
  - Health Check => 자동화된 DNS 장애 극복
    - 엔드포인트(애플리케이션, 서버, 기타 AWS 리소스)를 모니터링하는 Health Check
    - 다른 Health Check를 모니터링하는 Health Check (계산된 Health Check)
    - CloudWatch 알람을 모니터링하는 Health Check (전체적인 제어 가능) - 예: DynamoDB의 스로틀, RDS의 알람, 사용자 정의 메트릭 등 (사설 리소스에 유용함)
  - Health Check는 CW(CloudWatch) 메트릭과 통합됩니다.
  - 엔드포인트 모니터링
    - 약 15개의 글로벌 헬스 체커가 엔드포인트 상태를 확인합니다.
      - 건강/비건강 임계값 - 3 (기본값)
      - 간격 - 30초 (10초로 설정할 수 있으나 비용이 높아집니다.)
      - 지원되는 프로토콜: HTTP, HTTPS, TCP
      - 헬스 체커 중 18% 이상이 엔드포인트를 건강하다고 보고하면 Route 53은 건강한 상태로 간주합니다. 그렇지 않으면 비건강한 상태로 간주됩니다.
      - Route 53이 사용할 위치를 선택할 수 있습니다.
    - 엔드포인트가 2xx 및 3xx 상태 코드로 응답할 때만 헬스 체크가 통과합니다.
    - 헬스 체크는 응답의 처음 5120바이트 내의 텍스트를 기준으로 통과/실패 설정할 수 있습니다.
    - 라우터/방화벽을 구성하여 Route 53 헬스 체커의 수신 요청을 허용해야 합니다.
  - 계산된 Health Check
    - 여러 개의 Health Check 결과를 하나의 Health Check으로 결합합니다.
    - OR, AND, 또는 NOT을 사용할 수 있습니다.
    - 최대 256개의 하위 Health Check를 모니터링할 수 있습니다.
    - 부모 Health Check가 통과하려면 몇 개의 헬스 체크가 통과해야 하는지 지정할 수 있습니다.
    - 사용 예시: 웹 사이트 유지보수를 수행하면서 모든 헬스 체크가 실패하지 않도록 합니다.
  - Private Hosted Zones
    - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/ff6d6032-d100-4b36-942e-d695b5355b77)
    - Route 53 헬스 체커는 VPC 외부에 위치해 있습니다.
    - 헬스 체커는 개인 엔드포인트(개인 VPC 또는 온프레미스 리소스)에 접근할 수 없습니다.
    - CloudWatch 메트릭을 생성하고 CloudWatch 알람을 연결한 후, 알람 자체를 확인하는 헬스 체크를 생성할 수 있습니다. 이를 통해 헬스 체크를 수행할 수 있습니다.
  - <img width="1185" alt="image" src="https://github.com/mjs1995/muse-data-engineer/assets/47103479/971ba25d-9161-4f80-acae-b34c23501202">
- 도메인 등록기(Domain Registrar) vs. DNS 서비스
  - 도메인 등록기(Domain Registrar)에서는 일반적으로 연간 요금을 지불하여 도메인 이름을 구입하거나 등록합니다. (예: GoDaddy, Amazon Registrar Inc. 등)
  - 도메인 등록기는 일반적으로 DNS 서비스를 제공하여 DNS 레코드를 관리할 수 있도록 합니다.
  - 하지만 다른 DNS 서비스를 사용하여 DNS 레코드를 관리할 수도 있습니다.
  - 예를 들어, GoDaddy에서 도메인을 구입하고 Route 53을 사용하여 DNS 레코드를 관리할 수 있습니다.
  - Amazon Route 53을 사용하여 제3자 등록기와 함께 사용하기
    - 도메인을 제3자 등록기에서 구입한 경우에도 Route 53을 DNS 서비스 제공자로 사용할 수 있습니다.
      - Route 53에서 호스팅 영역(Hosted Zone)을 생성합니다.
      - 제3자 웹사이트에서 NS 레코드를 업데이트하여 Route 53 네임 서버를 사용합니다.
    - 도메인 등록기 != DNS 서비스
    - 하지만 대부분의 도메인 등록기는 일부 DNS 기능을 제공합니다.
