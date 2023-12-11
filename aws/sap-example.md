
미사용 데이터 암호화
EC2 인스턴스에 연결된 EBS 데이터 볼륨에 데이터를 저장하는 동안 암호화

EBS 저장하기 전 내부 데이터 암호화, 파일 시스템 수준, 타사 볼륨 암호화 도구 구현


3개월 100개 -> 3GB
24개월 100,000 -> 72TB
확장성 DynamoDB


IDS/IPS는 트래픽이 인스턴스에 도달하기 전에 트래픽을 분석해야 함


AWS Key Management Service 관리형 키와 함께 Amazon S3 서버 측 암호화
고객에 제공한 키와 함께, 자체 마스터 키
(EC2 키쌍은 SSH를 통해 인증하는 데 사용 됨)


S3 -> Glacier option

EMR - 빅 데이터 프레임워크 실행을 간소화하여 방대한 양의 데이터를 처리하고AWS 분석하는 관리형 클러스터 플랫폼
WAF(Web Application Firewall)

Amazon Kinesis - 실시간 데이터 분석 처리 시스템 (데이터 스트림을 간편하게 캡처, 처리 및 저장할 수 있음)


Route53

A레코드 : 도메인 주소와 서버의 IP주소를 직접 매핑
(IP를 바로 찾아갈 수 있지만 바뀌면 대응해줘야함)
CNAME : Canonical 도메인 주소를 또 다른 도메인 주소로 매핑
(유연하게 대처할 수 있음)

AMI Amazon Machine Image


AWS Direct Connect
사무실 환경과 같은 장소에서 AWS와 전용 네트워크 연결

BGP
Border Gateway Protocol
BGP 경로를 기존 라우팅 인프라에 재배포


인 메모리 캐시는 관계형 데이터베이스보다 수평으로 배포하는 것이 더 쉽기 때문에 확장하기가 더 쉬움
캐싱 계층은 사용량이 갑자기 급증할 경우 요청 버퍼를 제공

RTO(복구 시간 목표)
RPO(복구 지점 목표)
DR (Disaster Recovery)



RDS 백업이나 스냅샷은 S3에 기록된다.
Glacier Standard 검색은 시간이 소요됨
인스턴스 스토어 볼륨은 일시적

하나의 서브넷은 하나의 routing table을 가질 수 있음


AWS IT 인프라
SOC 1/SSAE 16/ISAE 3402(이전 SAS 70 Type II), SOC 2 및 SOC 3
FISMA, DIACAP 및 FedRAMP
PCI DSS 레벨 1, ISO 27001, ITAR 및 FIPS 140-2
HIPAA, CSA(Cloud Security Alliance) 및 MPAA(미국영화협회)


Auto Scaling - 사용자 프라이빗 키에서 계산된 HMAC-SHA1 서명

elasticity 탄력성

EBS Elastic Block Store
RAID 0, RAID 1, RAID 5, RAID 6,