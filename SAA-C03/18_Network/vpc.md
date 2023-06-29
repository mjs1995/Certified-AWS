
# Virtual Private Cloud (VPC)
- CIDR(CIDR-IPv4)의 이해
  - CIDR(Classless Inter-Domain Routing)은 IP 주소를 할당하기 위한 방법입니다.
  - 보안 그룹 규칙 및 일반적인 AWS 네트워킹에서 사용됩니다.
  - IP 주소 범위를 정의하는 데 도움이 됩니다
  - CIDR은 두 가지 구성 요소로 구성됩니다.
    - 기본 IP(Base IP)
      - 범위에 포함된 IP를 나타냅니다 (XX.XX.XX.XX).
      - 예: 10.0.0.0, 192.168.0.0 등.
    - 서브넷 마스크(Subnet Mask)
      - IP에서 변경될 수 있는 비트 수를 정의합니다.
      - 예: /0, /24, /32.
      - 두 가지 형태로 표현할 수 있습니다
        - /8은 255.0.0.0으로 표현됩니다.
        - /16은 255.255.0.0으로 표현됩니다.
        - /24는 255.255.255.0으로 표현됩니다.
        - /32는 255.255.255.255로 표현됩니다.
- 공용 IP vs 개인 IP (IPv4)
  - 인터넷 할당 번호 관리 기관(IANA)은 특정 IPv4 주소 블록을 개인(LAN) 및 공용(인터넷) 주소 용도로 설정했습니다.
  - 개인 IP는 특정 값을 허용합니다
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8) - 대규모 네트워크에서 사용됩니다.
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12) - AWS 기본 VPC에서 사용됩니다.
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16) - 예: 가정 네트워크에서 사용됩니다.
  - 인터넷의 나머지 모든 IP 주소는 공용입니다.
- AWS의 VPC - IPv4
  - VPC = 가상 사설망 (Virtual Private Cloud)
  - 한 AWS 리전에 여러 개의 VPC를 생성할 수 있습니다 (최대 5개 - 소프트 한계)
  - VPC 당 최대 CIDR 개수는 5개이며, 각 CIDR에 대해
    - 최소 크기는 /28 (16개의 IP 주소)
    - 최대 크기는 /16 (65536개의 IP 주소)
  - VPC는 사설이므로, 개인 IPv4 범위만 허용됩니다
    - 10.0.0.0 - 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 - 172.31.255.255 (172.16.0.0/12)
    - 192.168.0.0 - 192.168.255.255 (192.168.0.0/16)
  - VPC의 CIDR은 다른 네트워크 (예: 기업 네트워크)와 겹치지 않아야 합니다.
- VPC - 서브넷 (IPv4)
  - AWS는 각 서브넷에서 5개의 IP 주소 (첫 4개와 마지막 1개)를 예약합니다.
  - 이 5개의 IP 주소는 사용할 수 없으며 EC2 인스턴스에 할당할 수 없습니다.
  - 예시: CIDR 블록이 10.0.0.0/24이면 예약된 IP 주소는 다음과 같습니다
    - 10.0.0.0 - 네트워크 주소
    - 10.0.0.1 - VPC 라우터용으로 AWS가 예약
    - 10.0.0.2 - Amazon 제공 DNS에 대한 매핑을 위해 AWS가 예약
    - 10.0.0.3 - 향후 사용을 위해 AWS가 예약
    - 10.0.0.255 - 네트워크 브로드캐스트 주소. AWS는 VPC에서 브로드캐스트를 지원하지 않으므로 이 주소는 예약됨
  - 시험 팁: EC2 인스턴스에 29개의 IP 주소가 필요한 경우
    - /27 크기의 서브넷을 선택할 수 없습니다 (32개의 IP 주소, 32 - 5 = 27 < 29)
    - /26 크기의 서브넷을 선택해야 합니다 (64개의 IP 주소, 64 - 5 = 59 > 29)
- Internet Gateway (IGW)
  - VPC에서 리소스 (예: EC2 인스턴스)가 인터넷에 연결되도록 허용합니다.
  - 수평으로 확장 가능하며 고가용성과 중복성을 갖추고 있습니다.
  - VPC와 별도로 생성되어야 합니다.
  - 한 VPC는 하나의 인터넷 게이트웨이에만 연결할 수 있으며 그 반대도 마찬가지입니다.
  - 인터넷 게이트웨이 자체로는 인터넷 액세스를 허용하지 않습니다.
  - 라우팅 테이블도 편집해야 합니다
- Bastion Hosts
  - ![image](https://github.com/mjs1995/muse-data-engineer/assets/47103479/0d0c74f3-1ba6-4006-8b7e-67ff8b49361d)
  - 우리는 프라이빗 EC2 인스턴스에 SSH로 접속하기 위해 Bastion 호스트를 사용할 수 있습니다.
  - Bastion 호스트는 모든 다른 프라이빗 서브넷에 연결된 공용 서브넷에 있습니다.
  - Bastion 호스트의 보안 그룹은 인터넷에서 제한된 CIDR(예: 회사의 공용 CIDR)로부터 포트 22로의 인바운드를 허용해야 합니다.
  - EC2 인스턴스의 보안 그룹은 Bastion 호스트의 보안 그룹 또는 Bastion 호스트의 프라이빗 IP를 허용해야 합니다.
- NAT 인스턴스 (오래된 기술이지만 시험에서 여전히 등장할 수 있음)
  - NAT = 네트워크 주소 변환
  - 프라이빗 서브넷에 있는 EC2 인스턴스가 인터넷에 연결되도록 허용합니다.
  - 퍼블릭 서브넷에서 시작되어야 합니다.
  - EC2 설정에서 소스/대상 확인을 비활성화해야 합니다.
  - NAT 인스턴스에 Elastic IP를 연결해야 합니다.
  - 라우팅 테이블은 프라이빗 서브넷에서 NAT 인스턴스로 트래픽을 라우팅하도록 구성되어야 합니다.
  - 주의사항
    - 미리 구성된 Amazon Linux AMI를 사용할 수 있습니다.(2020년 12월 31일에 표준 지원이 종료되었습니다.)
    - 기본적으로 고가용성 및 탄력성이 없는 설정입니다.
      - 멀티-AZ와 탄력적인 사용자 데이터 스크립트를 사용하여 ASG를 생성해야 합니다.
    - 인터넷 트래픽 대역폭은 EC2 인스턴스 유형에 따라 다릅니다.
    - 보안 그룹 및 규칙을 관리해야 합니다
      - 인바운드
        - 프라이빗 서브넷에서 오는 HTTP / HTTPS 트래픽 허용
        - 집 네트워크에서 SSH 허용 (인터넷 게이트웨이를 통해 접속 제공)
      - 아웃바운드
        - 인터넷으로의 HTTP / HTTPS 트래픽 허용
- NAT Gateway
  - AWS에서 관리하는 NAT, 높은 대역폭, 고가용성, 관리 필요 없음
  - 사용량과 대역폭에 대해 시간당 비용 지불
  - NATGW는 특정 가용 영역에 생성되며, Elastic IP를 사용합니다.
  - 동일한 서브넷의 EC2 인스턴스에서는 사용할 수 없습니다(다른 서브넷에서만 사용 가능).
  - IGW(인터넷 게이트웨이)가 필요합니다(프라이빗 서브넷 => NATGW => IGW).
  - 5 Gbps의 대역폭을 자동으로 스케일링하여 최대 45 Gbps까지 제공합니다.
  - 관리/관리가 필요한 보안 그룹이 없습니다.
  - 고가용성을 갖춘 NAT Gateway
    - NAT Gateway는 단일 가용 영역 내에서 내구성을 갖습니다.
    - 장애 허용성을 위해 여러 가용 영역에 여러 NAT Gateway를 생성해야 합니다.
    - 가용 영역(AZ)이 다운되어도 NAT가 필요하지 않기 때문에 AZ 간 장애 조치가 필요하지 않습니다.
- Network Access Control List (NACL, 네트워크 액세스 제어 목록)
  - NACL은 서브넷 간의 트래픽을 제어하는 방화벽과 같은 역할을 합니다.
  - 각 서브넷당 하나의 NACL이 있으며, 새로운 서브넷은 기본 NACL이 할당됩니다.
  - NACL 규칙을 정의합니다
    - 규칙에는 번호 (1-32766)가 있으며, 번호가 낮을수록 우선순위가 높습니다.
    - 첫 번째 규칙 일치가 결정을 결정합니다.
    - 예를 들어, #100 ALLOW 10.0.0.10/32 및 #200 DENY 10.0.0.10/32를 정의하면, IP 주소는 200보다 우선순위가 높은 100에 의해 허용됩니다.
    - 마지막 규칙은 별표 (*)로 표시되며, 규칙이 일치하지 않는 경우 요청을 거부합니다.
    - AWS는 규칙을 100씩 증가시키는 것을 권장합니다.
  - 새로 생성된 NACL은 모든 것을 거부하는 규칙을 갖습니다.
  - NACL은 서브넷 수준에서 특정 IP 주소를 차단하는 좋은 방법입니다.
  - 기본 NACL은 해당하는 서브넷과 관련된 모든 인바운드/아웃바운드 트래픽을 허용합니다.
  - 기본 NACL을 수정하지 말고 대신 사용자 정의 NACL을 생성하세요.
- 임시 포트 (Ephemeral Ports)
  - 두 엔드포인트가 연결을 설정하기 위해서는 포트를 사용해야 합니다.
  - 클라이언트는 정의된 포트에 연결하고, 임시 포트에서 응답을 기대합니다.
  - 다양한 운영 체제는 다른 포트 범위를 사용합니다.
    - IANA 및 MS Windows 10: 49152 - 65535
    - 많은 Linux 커널: 32768 - 60999
- VPC 피어링
  - AWS의 네트워크를 사용하여 두 개의 VPC를 개인적으로 연결합니다.
  - 동일한 네트워크에 있는 것처럼 동작하도록 만듭니다.
  - CIDR이 겹치지 않아야 합니다.
  - VPC 피어링 연결은 전달적(transitive)이 아닙니다. (각 VPC 간에 피어링 연결을 설정해야 함)
  - EC2 인스턴스가 서로 통신할 수 있도록 각 VPC의 서브넷에 있는 경로 테이블을 업데이트해야 합니다.
  - 다른 AWS 계정/지역에 있는 VPC 간에도 VPC 피어링 연결을 생성할 수 있습니다.
  - 피어링된 VPC에서 보안 그룹을 참조할 수 있습니다. (계정 간 및 동일한 지역에서 작동)
- VPC Endpoints
  - 모든 AWS 서비스는 공개적으로 노출됩니다 (공개 URL).
  - VPC 엔드포인트 (AWS PrivateLink로 구동)를 사용하면 AWS 서비스에 대한 접속을 공개 인터넷 대신에 사설 네트워크를 통해 연결할 수 있습니다.
  - VPC 엔드포인트는 이중화되어 확장 가능합니다.
  - IGW, NATGW 등을 사용하지 않고 AWS 서비스에 접근할 수 있습니다.
  - 문제가 발생할 경우
    - VPC에서 DNS 설정 해결을 확인합니다.
    - 라우트 테이블을 확인합니다.
  - 엔드포인트의 종류
    - 인터페이스 엔드포인트 (PrivateLink로 구동)
      - ENI (사설 IP 주소)를 엔드포인트로 사용하여 생성 (보안 그룹을 첨부해야 함)
      - 대부분의 AWS 서비스를 지원
      - 시간당 비용 + 처리된 데이터의 GB 당 비용
    - 게이트웨이 엔드포인트
      - 게이트웨이를 생성하고 라우트 테이블에서 대상으로 사용해야 함(보안 그룹을 사용하지 않음)
      - S3와 DynamoDB를 모두 지원
      - 무료
  - S3의 경우 게이트웨이 엔드포인트와 인터페이스 엔드포인트 중 어떤 것을 선택해야 할까요?
    - 시험에서는 대부분 게이트웨이 엔드포인트가 선호될 것입니다.
    - 비용: 게이트웨이 엔드포인트는 무료이고, 인터페이스 엔드포인트는 요금이 부과됩니다.
    - 인터페이스 엔드포인트는 온프레미스(사이트 간 VPN 또는 Direct Connect), 다른 VPC 또는 다른 리전에서 액세스가 필요한 경우에 선호됩니다.