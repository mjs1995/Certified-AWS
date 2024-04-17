# Amazon EKS Primer
- Amazon EKS Primer
  - Amazon Elastic Kubernetes Service
    - Amazon Elastic Kubernetes Service(Amazon EKS)는 AWS Cloud 또는 온프레미스에서 Kubernetes 애플리케이션의 배포, 관리 및 크기 조정을 용이하게 하는 관리형 컨테이너 오케스트레이션 서비스입니다.
    - Amazon EKS는 패치, 노드 프로비저닝 및 업데이트와 같은 주요 작업을 자동화하는 데도 도움을 줍니다.
  - 컨테이너 오케스트레이션의 필요성
    - 컨테이너는 애플리케이션의 코드, 구성 및 종속성을 하나의 객체로 패키징하는 표준화된 방식을 제공합니다.
    - 컨테이너는 가벼울 뿐만 아니라, 애플리케이션을 어디서나 실행하고 확장하기 위한 일관되고 이동 가능한 소프트웨어 환경을 제공합니다.
    - 컨테이너에 대해 잘 알려진 용례 가운데는 마이크로서비스 구축 및 배포, 배치 작업 실행, 머신러닝 애플리케이션 지원, 기존 애플리케이션을 클라우드로 이동하는 기능 등이 있습니다.
    - 컨테이너에서 실행하는 애플리케이션을 사용하려면 배치를 관리하고 확장하기 위한 컨테이너 오케스트레이션 플랫폼이 필요합니다.
  - 주요 카테고리
    - 컨테이너 스케줄링 및 배치
    - 컨테이너 수를 자동으로 적절하게 스케일 인 또는 스케일 아웃
    - 비정상 컨테이너를 자동으로 제거하고 그 위치에 새 컨테이너를 배포하여 서비스를 자가 치유
    - 네트워킹 서비스 및 영구 스토리지와 같은 클라우드 및 기타 서비스와의 통합
    - 시스템 보안, 모니터링 및 로깅
- Kubernetes
  - Kubernetes 객체
    - 클러스터
      - 컨테이너화된 애플리케이션을 실행하는 노드라는 일련의 작업자 머신입니다.
      - 모든 클러스터에는 하나 이상의 작업자 노드가 있습니다. 또한 클러스터에는 클러스터를 관리하는 서비스를 실행하는 제어 영역도 있습니다.
    - 노드
      - Kubernetes는 컨테이너를 포드에 그룹화하고 노드에서 실행할 이러한 포드를 할당함으로써 워크로드를 실행합니다.
      - 노드는 클러스터에 따라 가상이거나 물리적 머신일 수 있습니다. 각 노드는 제어 영역에서 관리되고 포드를 실행하는 데 필요한 서비스를 포함합니다.
    - 포드
      - 하나 이상의 컨테이너로 구성된 그룹입니다.
      - 포드는 컨테이너 실행 방법에 대한 사양인 PodSpec 파일에 의해 정의됩니다.
      - 포드는 배포, 크기 조정 및 복제를 위한 Kubernetes 내 기본 빌딩 블록입니다.
    - 볼륨
      - 휘발성 볼륨
        - 포드에 있는 애플리케이션은 공유 볼륨에 액세스하여 포드에서 데이터 공유 및 컨테이너 재시작 간 데이터의 지속성 유지를 용이하게 합니다.
        - 포드가 종료되면 Kubernetes는 휘발성 볼륨을 파괴합니다.
      - 영구 볼륨
        - 영구 볼륨은 휘발성 볼륨과 유사하게 기능하지만 수명 주기가 이를 사용하는 개별 포드와 무관합니다.
        - 영구 볼륨은 클러스터 노드와 상관 없이 스토리지 하위 시스템에서 지원됩니다.
    - 서비스
      - Kubernetes에서 서비스는 포드의 논리적 컬렉션이며 포드에 액세스하기 위한 수단입니다.
      - 서비스는 사용 가능한 포드 세트를 바탕으로 지속적으로 업데이트되어 포드가 다른 포드를 추적할 필요가 없습니다.
    - 네임스페이스
      - 동일한 물리적 클러스터에서 지원되는 가상 클러스터입니다.
      - 물리적 클러스터는 서로 다른 네임스페이스에 있다면 리소스가 동일한 이름을 가질 수 있습니다.
      - 특히 네임스페이스는 여러 팀이나 프로젝트가 동일한 클러스터를 사용할 때 유용합니다.
    - ReplicaSet
      - 언제든 주어진 시간에 특정 개수의 포드 복제본이 실행되도록 합니다.
    - 배포
      - ReplicaSet 또는 개별 포드를 소유하고 관리합니다.
      - 배포에서 원하는 상태를 설명합니다. 그런 다음 배포는 클러스터의 실제 상태를 제어된 비율의 원하는 상태로 변경합니다.
    - ConfigMap
      - ConfigMap은 포드와 같은 기타 Kubernetes 객체에서 사용하는 키-값 쌍으로 기밀 데이터 이외의 데이터를 저장하는 API 객체입니다.
      - ConfigMaps를 사용하여 구성 데이터를 애플리케이션 코드와 분리하여 이동성의 모범 실무를 따르십시오.
    - 보안 정보
      - AWS 자격 증명과 같은 모든 기밀 데이터는 Kubernetes 보안 정보로 저장되어야 합니다.
      - 보안 정보는 민감한 정보에 대한 액세스를 제한합니다.
      - 원한다면 암호화를 켜서 보안을 강화할 수도 있습니다.
  - 포드 예약
    - Kubernetes 스케줄러로 포드를 예약할 수 있습니다. 스케줄러는 포드에 필요한 리소스를 확인하고 그 정보를 사용하여 스케줄링 결정에 영향을 줍니다.
    - 스케줄러는 일련의 필터를 실행하여 포드 배치에 부적격한 노드를 제외시킵니다.
  - 사용자 지정 리소스
    - Kubernetes가 정의하는 리소스(포드 및 배포) 외에도, Kubernetes API를 확장하고 사용자 지정 리소스를 생성할 수 있습니다.
    - 사용자 지정 리소스는 서비스 메시 객체와 같은 새 객체일 수도 있고 기본 Kubernetes 리소스의 조합일 수도 있습니다. 사용자 지정 리소스는 CRD(Custom Resource Definition)를 사용하여 생성됩니다.
  - 사용자 지정 리소스
    - Kubernetes가 정의하는 리소스(포드 및 배포) 외에도, Kubernetes API를 확장하고 사용자 지정 리소스를 생성할 수 있습니다.
    - 사용자 지정 리소스는 서비스 메시 객체와 같은 새 객체일 수도 있고 기본 Kubernetes 리소스의 조합일 수도 있습니다.
    - 사용자 지정 리소스는 CRD(Custom Resource Definition)를 사용하여 생성됩니다.
- Amazon EKS 가용성 및 API
  - Amazon EKS 제어 영역은 3개의 가용 영역에 걸친 최소 2개의 API 서버 노드와 3개의 etcd 노드로 구성됩니다.
  - Amazon EKS는 비정상 제어 영역 노드를 자동으로 탐지하여 교체하므로 Kubernetes 실행에 따른 운영 부담이 대폭 제거됩니다.
  - 이 기능을 사용하면 AWS 인프라를 관리하는 대신 애플리케이션 빌드에 집중할 수 있습니다.
  - Amazon EKS로 데이터 영역을 관리하는 이유는 무엇입니까?
    - 작업자 노드가 많은 복잡한 인프라를 관리하고 오토스케일링과 업데이트를 신경쓰는 것은 쉽지 않습니다. 또한 클러스터에 노드를 프로비저닝하는 여러 팀이 있을 수 있으며, 이들의 프로비저닝 방식은 각각 다를 수 있습니다.
    - 이러한 차이로 인해 표준화가 어려워집니다. Amazon EKS가 데이터 영역의 일부 또는 전부를 관리할 수 있도록 하면 인프라를 간소화하고 표준화를 유지할 수 있습니다.
    - 자체 관리형 노드
      - 제어 영역만 Amazon EKS에서 관리합니다. 데이터 영역 노드(프로비저닝, 업데이트, 모니터링 및 기타 작업)를 완벽하게 제어하고 관리할 수 있습니다.
    - 관리형 노드 그룹
      - 관리형 노드 그룹은 Amazon EKS API를 사용하여 Amazon EKS 클러스터용 컨테이너를 실행하는 Amazon Elastic Cloud Computing(Amazon EC2) 인스턴스를 시작하고 관리합니다. 관리형 노드 그룹은 자동으로 시작되고 관리되지만 EC2 인스턴스 및 Auto Scaling 그룹과 같이 AWS 계정에서 사용 중인 모든 리소스를 계속 볼 수 있습니다. 더 적은 작업으로 제어, 보안, 가시성을 모두 확보할 수 있습니다.
      - 프로비저닝
        - 관리형 노드 그룹을 단일 명령으로 배포합니다. 그러면 Amazon EKS가 최신 Amazon EKS 최적화 AMI(Amazon Machine Images)를 사용하여 노드를 생성합니다.
        - AWS 서비스는 이를 여러 가용 영역에 배포하고 Auto Scaling 그룹으로 지원합니다. 크기 조정 파라미터를 변경할 수 있습니다.
      - 관리
        - Amazon EKS는 관리형 노드 그룹의 상태 모니터링을 처리합니다.
        - Amazon EKS는 삭제 중이거나 연결할 수 없거나 사용할 수 없는 필수 리소스를 포함한 문제를 사용자에게 자동으로 알려줍니다.
        - Amazon EKS는 업데이트 문제, 한도 초과, 생성 또는 삭제 실패에 대해서도 알려줍니다. 노드 수준 Secure Shell(SSH) 액세스, 오픈 소스 로그 라우터 또는 Amazon CloudWatch에서 로그를 가져올 수도 있습니다. 또한 모든 관리형 노드 그룹 이벤트는 AWS CloudTrail에 기록됩니다.
      - 업데이트
        - 필요한 경우 단일 명령으로 관리형 노드 그룹을 업데이트할 수 있습니다. 그러면 Amazon EKS는 롤링 업데이트 시 노드 종료를 처리하고 Kubernetes 버전에 맞는 최신 AMI 버전으로 자동 업데이트합니다.
      - 크기 조정
        - 관리형 노드 그룹이 노드의 크기 조정을 처리합니다. 하지만 Kubernetes 레이블, AWS 태그, 노드 그룹의 크기와 같은 크기 조정 파라미터는 사용자가 여전히 제어할 수 있습니다.
      - 도구
        - eksctl을 사용하여 관리형 노드 그룹을 프로비저닝할 수 있습니다.
    - AWS FARGATE
      - 관리형 노드 그룹을 사용하면 인프라 관리에 쓰는 시간이 줄어듭니다. 하지만 애플리케이션 생성에만 집중하고 Amazon EKS가 데이터 영역을 완전히 관리하도록 하는 것이 좋습니다. 이는 Fargate에서 포드를 실행하면 가능합니다.
      - Fargate는 Kubernetes 데이터 영역의 전체 인프라를 관리합니다. 포드를 실행하는 데만 신경 쓰면 됩니다.
        - 네이티브 - Fargate는 네이티브 Kubernetes 포드를 실행합니다. AWS와 관련하여 아무것도 구성하거나 변경할 필요가 없습니다.
        - 적정 규모 – Fargate는 포드와 리소스에 필요한 리소스만 동적으로 프로비저닝합니다.
        - 빠르고 간편함 – Fargate는 신속하게 크기를 조정합니다. Cluster Autoscaler를 설치할 필요가 없습니다.
        - 최적화 – 포드가 실행될 때만 포드 비용을 지불하고, 포드 수준 청구를 확인할 수 있습니다.