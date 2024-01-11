
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
RAID 0  디스


솔리드 스테이트 드라이브(SSD)으로 지원되는 프로비저닝된 IOPS 볼륨은 짧은 지연 시간이 필요한 중요한 IOPS 집약적이고 처리량 집약적인 워크로드를 위해 설계된 최고 성능의 Elastic Block Store(EBS) 스토리지 볼륨

EBS 최적화 처리량은 활용 가능한 총 IOPS를 제한합니다. 더 큰 처리량을 제공하는 EBSOptimized 인스턴스를 사용

S3는 장기 저장에 좋다

Amazon Kinesis 24시간 부터 최대 7일까지 데이터 보존 늘릴 수 있음,

Amazon CloudTrail - 표준화된 보안 로깅 서비스
CloudSearch - 검색 솔루션을 효율적인 비용으로 간단하게 설정, 관리 및 확장할 수 있음

CloudFront는 캐시에서 콘텐츠를 제공하기 전에 뷰어 요청과 관련된 Lambda 함수를 트리거하여 매개변수를 정규화할 수 있음

여러 User가 복수 계정에서 서로 다른 Role을 손쉽게 이동하면서 관리해야 할 경우를 위해 지난 해 교차 계정 접근 제어(Cross-Account Access) 기능

Storage Gateway

Route 53 가중치 라운드 로빈

가중치 기반 리소스 레코드, 지연 시간 별칭 리소스 레코드 세트에서 "Evaluate Target Health"를 "Yes"

SQS는 14일 동안만 저장
SES (Simple Email Service) 이메일 서비스 공급자

Amazon RDS - AWS Security Token Service "AssumeRole"

EMR의 빅 데이터 워크로드는 내결함성 특성으로 인해 중단되더라도 계속해서 처리할 수 있다.

RRS (Reduced Redundancy Storage)

Amazon Elastic Map Reduce

AWS Key Management Service

Amazon Data Lifecycle Manager를 사용하여 EBS 스냅샷 및 EBS-backed AMI의 생성, 보존 및 삭제를 자동화할 수 있습니다.


고객 개인 정보를 유지하고 비용을 최소한으로 유지


IAM & R53은 글로벌 서비스

IAM 역할은 계정에 생성할 수 있는, 특정 권한을 지닌 IAM 자격 증명입니다.

교차 계정 액세스

최종적 일관성 SQS


vCenter용 EC2 VM Import Connector

웹 identity 패더레이션
웹 자격 증명 연동을 사용하여 필요할 때 동적으로 임시 AWS 보안 자격 증명을 요청하도록 앱을 구축



웹 서버 앞에 역방향 프록시 레이어를 구현하고 각 역방향 프록시 서버에 IDS/IPS 에이전트를 구성

CloudFormation
WS 리소스를 모델링하고 설정하여 리소스 관리 시간을 줄이고 AWS에서 실행되는 애플리케이션에 더 많은 시간을 사용하도록 해 주는 서비스

Shield DDoS 방어 기능은 Route 53에 대한 CloudFront 트래픽을 지속적으로 검사

Elastic Transcoder

Cloud watch - rds,elb,route53


VPC 간의 연결
VPC Peering, VGW, DGW, TGW

BGP (Border Gateway Protocol) - 인터넷에서 데이터를 전송하는 데 가장 적합한 네트워크 경로를 결정하는 일련의 규칙

CIDR 블록 Classless Inter-Domain Routing(CIDR)은 인터넷상의 데이터 라우팅 효율성을 향상시키는 IP 주소 할당 방법

SWF - 완전한 관리가 가능한 클라우드 워크플로 웹 서비스로서, 복잡한 사용자 지정 코딩 워크플로 솔루션과 자동화 소프트웨어를 대체

EIP (Elastic IP)

TLS/SSL
보안 소켓 계층(Secure Sockets Layer, SSL) 인증서는 종종 디지털 인증서로 불리며, 브라우저(사용자의 컴퓨터)와 서버(웹사이트) 사이의 암호화된 연결을 수립하는 데 사용

IPSec은 네트워크에서의 안전한 연결을 설정하기 위한 통신 규칙 또는 프로토콜 세트



arn:aws:iam::123456789012:instance-profile/Webserver
arn:aws:service:region:account:resource


IAM EC2 종료 방지는 사용자별로 적용되지 않음

dynamodb는 사용한 만큼 비용을 지불
Global Secondary Index와 함께 DynamoDB를 사용
secondary index란 대체(altanative) key와 테이블의 다른 attribute들의 subset을 포함하는 데이터 구조입니다.
테이블과 마찬가지로 index에 쿼리를 해서 데이터를 가져올 수 있다.

S3에서 사용자 콘텐츠를 제공합니다. CloudFront를 사용하고 각 지역의 ELB 간에 Route53 지연 시간 기반 라우팅
각 지역의 로컬 DynamoDB

bastion and NAT instance
Bastion 호스트는 가상 프라이빗 클라우드(VPC)의 프라이빗 및 퍼블릭 서브넷에 위치한 Linux 인스턴스에 대한 보안 액세스를 제공

ELB는 SNI 기반 인증서를 지원

SAML 2.0 호환 자격 증명 공급자(IDP)를 사용하여 NOC 회원에게 AWS Single Sign-On(SSO)

NACL Network Access Control List

AWS Ops Works

https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html#default-%20security-group

NAT 인스턴스에서 소스/대상 확인 속성

VPC 내의 모든 서브넷은 서로 통신


/16의 CIDR 블록 마스크는 가장 작은 범위
Classless Inter-Domain Routing(CIDR)


S3 HTTP 200 결과 코드와 MD5 체크섬이 함께 표시

IAM GROUP 사용자 모음


데이터베이스 진단 팩과 데이터베이스 튜닝 팩 (오라클 엔터프라이즈 에디션)

CloudWatch CPU 사용률 종료 기능

단일 동적 DB 테이블에 대해 프로비저닝할 수 있는 최대 쓰기 처리량 40000 초과 AWS 문의

프로비저닝(Provisioning)이란 어떠한 지식이나 자원 등을 미리 준비해놓고 요청이 들어왔을 때, 해당 요청에 맞게 공급하는 것

ApplyImmediately

Aws EMR
Apache Spark, Apache Hive 및 Presto와 같은 오픈 소스 분석 프레임워크

EC2 classic은 서비스 중지되긴함
탄력적 IP 다시 연결해야함

Amazon Cognito
개발자 중심의 비용 효율적인 고객 ID 및 액세스 관리(CIAM) 서비스
Enhanced and basic

Amazon EC2 인스턴스에 설치된 AWS CodeDeploy 에이전트는 퍼블릭 AWS CodeDeploy 및 Amazon S3 서비스 엔드포인트에 액세스할 수 있어야 한다.

지역당 허용되는 기본 최대 VPC 5개, 100개까지 요청 가능


ENI 탄력적 네트워크 인터페이스

VPC는 IAM과도 작동하며 조직은 다양한 VPC 서비스에 액세스할 수 있는 IAM 사용자를 생성할 수 있음

IAM, the maximum length for a role name is 64 characters.

인스턴스에 둘 이상의 네트워크 인터페이스를 연결한 경우 AWS는 퍼블릭 IP를 할당할 수 없음

aws: SourceIp


AWS EC2에서는 한 번에 20개의 온디맨드 인스턴스와 100개의 스팟 인스턴스를 실행할 수 있습니다.

g2.2xlarge는 5개

Provisioned IOPS
For EBS volumes you can specify a consistent IOPS rate when you create the volume.


ArnEquals, ArnLike

If the user is creating an internal ELB, he should use only private subnets.

AWS Direct Connect 1, 10 및 100Gbps

NotPrincipal 요소를 사용하여 IAM 사용자, 연동 사용자, IAM 역할, AWS 계정, AWS 서비스 또는 리소스에 대한 액세스가 허용되거나 거부되지 않는 기타 보안 주체를 지정

MySQL RDS로 PIOPS를 활성화하려는 경우 최소 저장 크기는 100GB

IAM의 정책 평가 로직은 "허용"

ELB와 인스턴스는 별도의 서브넷에 있을 수 있습니다

sts:AssumeRole 작업에 액세스할 수 있도록 사용자에게 연결된 정책을 생성

조직에 VPN 연결이 여러 개 있는 경우 AWS VPN CloudHub를 사용하여 사이트 간 보안 통신을 제공할 수 있습니다.

AWS Data Pipeline 재시도 10회까지

cr1.8xlarge - 244GB memory

AWS EC2에서는 한 번에 20개의 온디맨드 인스턴스와 100개의 스팟 인스턴스
5개의 2xlarge
2개의 4xlarge


앱 계층은 인터넷에 노출될 필요가 없으므로 내부 로드 밸런서를 사용하는 것이 일반적


ELB는 특정 지역에서만 작동하며 지역 간에 요청을 라우팅하지 않습니다.

Redis 클러스터 환경을 최대 500개 노드 및 500개 샤드로 확장할 수 있습니다.


AWS Direct Connect 위치는 연결된 지역의 Amazon Web Services에 대한 액세스는 물론 다른 미국 지역에 대한 액세스도 제공


고정 세션
세션 지속성은 로드 밸런싱 서비스의 기능 서비스에 대한 후속 연결이 온라인인 동안 동일한 노드로 강제로 리디렉션되도록 시도


IAM에서 다음 중 임시 보안 자격 증명 정의된 기간 동안 유효

라우팅 테이블당 허용되는 최대 BGP 공지 경로 수는 100개

동일한 계정의 여러 IAM 사용자가 동일한 로그인 ID를 가질 수 없음


PIOPS는 256KB IO 청크를 기준으로 하는 SSD 기반 스토리지에서 결정

사용자는 완료 시 Vault 인벤토리 검색 작업 완료 및 아카이브 검색 작업 완료와 같은 알림을 트리거하는 특정 작업을 선택할 수 있다.

ENI에는 기본 개인 IP 주소, 하나 이상의 보조 개인 IP 주소, 개인 IP 주소당 하나의 탄력적 IP 주소, 하나의 공용 IP 주소, 하나 이상의 보안 그룹, MAC 주소, 소스/대상과 같은 속성이 포함될 수 있다.

탄력적 네트워크 인터페이스(ENI)가 연결된 인스턴스를 등록하면 로드 밸런서는 인스턴스 기본 인터페이스(eth0)의 기본 IP 주소로 트래픽을 라우팅

모바일 앱은 공급자의 SDK를 사용하여 ID 공급자(IdP)를 인증합니다. 최종 사용자가 IdP로 인증되면 IdP에서 반환된 OAuth 또는 OpenID
Connect 토큰이 앱에서 Amazon Cognito로 전달되고, Amazon Cognito는 사용자 및 세트에 대한 새 Cognito ID를 반환합니다. 임시, 제한된 권한의 AWS 자격 증명.

프로비저닝된 IOPS(SSD) 볼륨의 크기는 4GiB~16TiB이며 볼륨당 최대 20,000IOPS를 프로비저닝할 수 있다.

If your policy has multiple condition operators or multiple keys attached to a single condition operator, the conditions are evaluated using a logical AND. If a single condition operator includes multiple values for one key, that condition operator is evaluated using a logical OR.


VPC에서 DHCP 옵션 세트를 생성한 후 수정할 수 없음

AWS OpsWorks
Puppet 또는 Chef를 통해 인스턴스나 온프레미스 환경에서 서버가 구성, 배포되는 방법을 자동화하는 구성 관리 서비스

IAM 사용자만 자신의 보안 액세스 키쌍!! 사용하여 EC2 인스턴스에 연결할 수 있도록 허용

기본적으로 IAM 사용자의 임시 보안 자격 증명은 최대 12시간 동안 유효하지만, 기간은 15분에서 최대 36시간

Cloud Block Storage - RAID 10 (raid0 + raid1)
raid https://hihighlinux.tistory.com/64

ADM - amazon device messaging

가장 비용 효율적인 솔루션은 문제 똑바로 읽기!!

Ephemeral


NAT (Network Address Translation)

The source/destination checks should be disabled on the NAT instance.

VPC를 사용하면 사용자는 자신의 인스턴스에 대해 여러 개인 IP 주소를 지정할 수 있다.


AWS GovCloud(미국) 리전은 미국 국제 무기 거래 규정(ITAR) 요구 사항을 준수

사용자가 구성에 사용된 IAM 프로필과 같은 추가 매개변수를 원하는 경우 다음 명령을 실행해야 합니다. as-describe-launch-configs --show-long

콘솔을 사용하여 IAM 역할을 생성할 때 인스턴스 프로필에 해당하는 것 - 콘솔은 해당 역할과 동일한 이름을 인스턴스 프로파일에 부여

EC2 인스턴스에는 한 번에 하나의 역할만 할당할 수 있으며 인스턴스의 모든 애플리케이션은 동일한 역할과 권한을 공유

AWS Data Pipeline Attempt 객체는 다양한 시도, 결과 및 해당하는 경우 실패 이유를 추적

AWS Direct Connect 작업에 대한 액세스를 제어하는 ​​정책을 작성할 때 별표(*)를 리소스로 사용

A task runner


IAM 정책 내에서 IfExists는 Null 조건을 제외한 모든 조건 연산자의 끝에 추가될 수 있음

STS API 명령 GetSessionToken은 사전 권한 없이 계정의 모든 IAM 사용자가 사용할 수 있음
GetFederationToken은 IAM 사용자 또는 AWS 계정 루트 사용자가 연동 사용자 또는 자신이 맡는 역할에 대한 임시 보안 자격 증명을 요청하는 데 사용


전용 인스턴스 테넌시가 있는 VPC에서 인스턴스를 시작하면 인스턴스 테넌시에 관계없이 인스턴스가 자동으로 전용 인스턴스가 됨

테넌시란?
EC2 인스턴스가 물리적 하드웨어에 분산되는 방식을 정의하고 요금에 영향을 줌

VPN 장치가 BGP(Border Gateway Protocol)를 지원하는 경우 VPN 연결을 구성할 때 동적 라우팅을 지정해야 한다. 기기가 BGP를 지원하지 않는 경우 정적 라우팅을 지정해야 한다.

VPC 피어링 연결을 사용하면 사용자는 마치 개인 IP 주소를 사용하여 피어 VPC 간에 트래픽을 라우팅할 수 있다.

AWS Direct Connect는 인터넷 대신 AWS 클라우드 서비스를 활용할 수 있는 네트워크 서비스


온프레미스 호스트에 설치할 수 있는 Task Runner 패키지를 제공 - 서비스를 지속적으로 폴링

IAM NotAction 요소를 사용하면 작업 목록에 대한 예외를 지정할 수 있다.

aws:MultiFactorAuthAge


AWS Cloud Hardware Security Module(HSM)
HSM 클라이언트가 실행 중인 서버 또는 인스턴스에는 HSM에 대한 네트워크(IP) 연결이 있어야 함

작업 실행기는 PollForTask를 호출하여 AWS Data Pipeline에서 수행할 작업을 수신

AWS에서는 사용자가 프로비저닝된 IOPS 200마다 최적의 평균 대기열 길이를 1로 목표로 삼고 애플리케이션 요구 사항에 따라 해당 값을 조정할 것을 권장

IOPS와 EBS 볼륨의 비율 50
5000IPOS 100GB

ELB(Elastic Load Balancer)는 여러 VPC 사이가 아닌 VPC 내의 인스턴스 수준에서 작동

c4.8xlarge 인스턴스가 제공하는 네트워킹 성능은 10기가비트

SAML 토큰에서 발행된 클레임을 편집해야 하는 두 가지 이유
NameIdentifier 클레임은 AD에 저장된 사용자 이름과 동일할 수 없으며 앱에는 다른 클레임 URI 세트가 필요

CloudHSM
포트 22(SSH용) 또는 포트 3389(RDP용)가 열려 있는 보안 그룹

IAM 거부가 허용보다 우선

Memcached의 경우 11211이고 Redis의 경우 6379

EC2-Classic에서는 인스턴스를 최대 500개의 보안 그룹과 연결하고 보안 그룹에 최대 100개의 규칙을 추가할 수 있습니다. EC2-VPC에서는 네트워크 인터페이스를 최대 5개의 보안 그룹과 연결하고 보안 그룹에 최대 50개의 규칙을 추가할 수 있다


Sid (statement ID) is an optional identifier that you provide for the policy statement. You can assign a Sid a value to each statement in a statement array.

VPC는 비전용 EC2 인스턴스를 허용해야 함

문제 똑바로 읽기 않은 구성은?

Auto Scaling 그룹을 사용하여 두 개 이상의 로드 밸런서를 구성할 수 있음

대기 기간은 하나의 조정 활동 종료(시작 또는 종료 가능)와 다른 조정 활동 시작(시작 또는 종료 가능) 사이의 시간 차이
다른 CloudWatch 경보에 의해 변경되는 것을 허용하지 않음


용자는 지정된 조건에 따라 자동으로 확장한 다음 축소하도록 AutoScaling 그룹을 구성할 수 있습니다. 이를 구성하려면 사용자는 CloudWatch 경보에 의해 트리거되는 정책을 설정


Auto Scaling은 사용자 지정 상태 확인을 사용하여 인스턴스의 상태를 확인할 수 있습니다. 사용자 지정 상태 확인이 있는 경우 Auto Scaling에서 이 정보를 사용할 수 있도록 상태 확인의 정보를 Auto Scaling으로 보낼 수 있음

Auto Scaling 그룹이 두 개 이상의 인스턴스를 시작하는 경우 각 인스턴스의 휴지 기간은 해당 인스턴스가 시작된 후 시작

Desired capacity

Cluster placement group

사용자가 Amazon EBS 지원 전용 인스턴스를 시작하면 EBS 볼륨은 단일 테넌트 하드웨어에서 실행되지 않음

부팅 프로세스 중에 Amazon Machine Image paravirtual(PV) 가상화에서는 어떤 시스템을 사용
PV-GRUB

IAM은 사용자가 인스턴스 시작과 같은 AWS 리소스 및 작업을 수행해야 할 때 유용

사용자는 단일 EC2 인스턴스에 두 개 이상의 보안 그룹을 연결할 수 있습니다. 이 경우 각 보안 그룹의 규칙이 효과적으로 집계되어 하나의 규칙 세트를 생성


AWS EC2에서 보안 그룹을 구성하는 동안 사용자는 CIDR 표기법으로 IP 주소를 지정해야 합니다. CIDR IP 범위 10.20.30.40/32는 단일 IP 10.20.30.40에 대한 것임을 나타냄

CIDR 형식


태그 키는
"aws:"라는 접두사를 가질 수 없음


스냅샷은 Amazon EC2 API를 통해서만 사용할 수 있다.

CloudFormation
AWS 리소스를 모델링하고 설정하여 리소스 관리 시간을 줄이고 AWS에서 실행되는 애플리케이션에 더 많은 시간을 사용하도록 해 주는 서비스


스택 리소스가 작동한 시간에 대한 요금이 부과
AWS CloudFormation에서 실제 리소스 이름은 스택과 논리적 리소스 이름의 조합

WaitCondition 리소스를 사용하여 다른 종속 리소스 생성을 보류

intrinsic functions are special actions you use in your template to assign values to properties that are not available until runtime.

/var/log/

논리적 ID는 템플릿의 다른 부분에 있는 리소스를 참조하기 위해 여러 번 사용될 수 있다
CloudFormation 스택에는 샌드박스나 테스트 영역이 없으므로 테스트 중에 생성한 리소스에 대해 요금이 부과

탄력적 네트워크 인터페이스(ENI)는 VPC의 인스턴스에 연결할 수 있는 가상 네트워크 인터페이스로 정의되며, eth0의 탄력적 네트워크 인터페이스에 자동 할당될 수 있는 퍼블릭 IP 주소 1개를 포함할 수 있다.

가상 인터페이스를 동일한 리전 내의 새 연결로 마이그레이션할 수 있지만 한 리전에서 다른 리전으로 마이그레이션할 수 없다.

90일 이내에 교차 연결이 완료되지 않으면 LOA-CFA에서 부여한 권한이 만료


1단계: Amazon Web Services에 가입 2단계: AWS Direct Connect
연결 요청 제출 3단계: 교차 연결 완료(선택 사항) 4단계: AWS Direct Connect를 사용하여 중복 연결 구성 5단계: 가상
인터페이스 생성 6단계: 라우터 다운로드 구성 7단계: 가상 인터페이스 확인

프라이빗 ASN 가상 프라이빗 게이트웨이를 생성하고 이를 VPC에 연결

원격 지역으로부터의 모든 데이터 전송에는 원격 지역 데이터 전송 요금이 청구

AWS Direct Connect는 표준 1기가비트 또는 10기가비트 이더넷 단일 모드 광섬유 케이블을 통해 내부 네트워크를 AWS Direct Connect 위치에 연결


IKE 보안 연결은 사전 공유 키를 인증자로 사용하여 가상 프라이빗 게이트웨이와 고객 게이트웨이 사이에 먼저 설정

CIDR, 라우팅 공부 필요

두 번째 VPN 연결을 위한 고객 게이트웨이 IP 주소는 공개적으로 액세스할 수 있어야 합니다. 업데이트된 정보에는 두 번째 Site-to-Site VPN 연결을 설정하려면 두 번째 연결에 대한 고객 게이트웨이 IP 주소에 공개적으로 액세스할 수 있어야 한다


DynamoDB에서 프로젝션은 테이블에서 보조 인덱스로 복사되는 속성 세트
LSI(로컬 보조 인덱스)를 생성할 수 없습니다. LSI는 테이블 생성 중에만 생성될 수 있음
JSON 프로토콜

Amazon DynamoDB에서 프로비저닝된 테이블을 새로 생성할 때는 프로비저닝된 처리 용량을 지정해야 합니다. 이는 테이블에서 지원할 수 있는 읽기 및 쓰기 활동의 양

DynamoDB에서는 사용한 I/O가 아닌 사용한 스토리지 및 처리량에 대해 요금이 부과
스칼라 값에 대한 원자적 증가 및 감소 작업을 허용

프로비저닝된 IOPS 볼륨은 크기가 10GB 이상?


AMI를 생성하고 여기에서 새 인스턴스를 시작하지 않는 한 해당 인스턴스의 영역을 변경할 수 없음


Amazon Elastic Compute Cloud 블록 디바이스 매핑

EFS

소유자 루트에 대한 읽기-쓰기-실행 권한
그룹 루트에 대한 읽기-실행 권한 다른 사람에 대한 읽기-실행 권한

available-zone.file-system-id.efs.aws-region.amazonaws.com

마그네틱 스토리지는 I/O 요구 사항이 적거나 급증하는 애플리케이션에 이상적

내결함성보다 I/O 성능이 더 중요한 경우 RAID 0
파일의 디스크 사용량을 측정하는 데 사용할 수 있는 두 가지 유틸리티인 du 및 stat 유틸리티

noac 옵션을 사용하여 파일 시스템을 탑재 해제했다가 다시 탑재하여 속성 캐싱을 비활성화

CloudWatch의 PutMetricData 요청 크기는 HTTP GET 요청의 경우 8KB, HTTP POST 요청의 경우 40KB로 제한됩니다. 사용자는 하나의 PutMetricData 요청에 최대 20개의 데이터 포인트를 포함


cloudwatch
고객당 매월 10개의 CloudWatch 지표, 10개의 경보, 1,000,000개의 API 요청 및 1,000개의 Amazon SNS 이메일 알림을 무료로 받을 수 있음

Auto Scaling을 위해 CloudWatch
8개의 지표와 1개의 차원을 지원

Us-East-1은 지역이고 Us-East-1a는 해당 지역의 AZ

put-metric-data 최대 2분

list-metrics 명령을 사용하여 검색된 지표 목록에 새 지표가 표시되기까지 최대 15분

S3 VPC 엔드포인트와 S3 버킷 정책을 사용하여 이 VPC 엔드포인트에 대한 액세스를 제한


Amazon CloudWatch Logs를 구문 분석 AWS X-Ray 추적을 분석하고 HTTP 메서드를 분석

EFS

NLB를 PrivateLink 엔드포인트 서비스에 연결하고 이 서비스를 사용할 계정을 화이트리스트에 추가

ACM의 인증서는 지역 리소스
둘 이상의 AWS 리전에서 동일한 FQDN(정규화된 도메인 이름) 또는 FQDN 집합에 대해 Elastic Load Balancing과 함께 인증서를 사용하려면 각 리전에 대한 인증서를 요청

여러 AWS Snowball 어플라이언스를 사용하여 기존 이미지를 Amazon S3로 마이그레이션

ECS Amazon Elastic Container Service

AWS Config: 구성 규칙은 규정 준수에 매우 유용한 도구
AWS CloudTrail: Cloud Trail은 감사를 위한 훌륭한 도구

AWS 시스템 관리자

AWS Fargate
SAML

로깅 계정에서 Amazon Kinesis Data Streams를 생성하고, 각 애플리케이션 AWS 계정의 CloudWatch Logs 스트림에 대한 스트림을 구독하고, 데이터 스트림을 소스로 사용하여 Amazon Kinesis Data Firehose 전송 스트림을 구성하고, 로그 데이터를 Amazon S3에 유지

Amazon Rekognition

Re-platform

버전 관리 및 버킷 액세스 로깅이 활성화된 객체 스토리지에 Amazon S3
SSE-KMS


CloudFront 뷰어 요청 트리거에 연결된 Amazon Lambda@Edge를 사용하여 세션 만료와 함께 보안 쿠키 토큰을 유지함으로써 사용자를 인증하고 권한을 부여함으로써 여러 지역에서 사용자 경험을 개선

AWS Application Discovery Service

Mechanical Turk

AWS CodePipeline을 사용하여 코드 리포지토리에 연결하고 AWS CodeBuild를 사용하여 빌드할 작업을 생성

AWS-KMS 관리형 키를 사용하면 사용자가 마스터 키를 생성하고 제어할 수 있습
고객 관리형 키와 함께 Amazon S3 서버 측 암호화를 사용하고, 고가용성 모드로 구성된 2개의 AWS CloudHSM 인스턴스를 사용하여 키를 관리

호스트 선호도는 인스턴스 수준에서 구성됩니다. 인스턴스와 전용 호스트 간의 시작 관계를 설정합니다. (인스턴스가 실행될 수 있는 호스트 설정) 자동 배치를 사용하면 실행하는 인스턴스가 특정 호스트에서 실행되는지, 아니면 구성이 일치하는 사용 가능한 호스트에서 실행되는지 여부를 관리할 수 있습니다.

AWS PrivateLink

AWS Batch

Amazon EMR (Elastic MapReduce)
Amazon S3에 데이터 파일을 저장하고 각 파일의 메타데이터에 대해 Range GET을 사용한 다음 관련 데이터를 인덱싱

선불 예약 인스턴스

Route 53은 4초 이내에 엔드포인트와의 TCP 연결을 설정할 수 있어야 합니다.
또한 엔드포인트는 연결 후 2초 이내에 HTTP 상태 코드 2xx 또는 3xx로 응답해야 합니다.
Route 53 상태 확인기는 HTTP 상태 코드를 수신한 후 다음 2초 이내에 엔드포인트로부터 응답 본문을 수신해야 합니다.
Route 53은 응답 본문에서 사용자가 지정한 문자열을 검색합니다. 문자열은 응답 본문의 처음 5,120바이트에 완전히 나타나야 합니다.


Amazon Aurora MySQL 자동으로 스토리지 확장

Amazon Athena
Athena는 페타바이트 규모의 데이터를 상주 위치에서 분석하는 간소화되고 유연한 방식을 제공

Amazon QuickSight

교차 스택 참조를 사용하여 각 템플릿에 많은 매개 변수를 전달하지 않도록 기존 템플릿을 변경합니다. 애플리케이션에 필요한 각 스택을 새 스택에서 중첩 스택으로 호출

AWS Business Support에 등록하고 Trusted Advisor 점검을 사용하여 비용 절감에 대한 제안

AWS 계정에 SAML 기반 연동을 구현
AWS Organizations를 사용하고 규정 및 서비스 거버넌스를 기반으로 조직 단위(OU) 구조를 구축


Amazon Lex는 호출자의 의도를 인식

AWS DMS를 사용하여 온프레미스 Oracle 데이터베이스와 AWS에서 호스팅되는 복제 인스턴스 간에 데이터 세트를 로드하고 복제

겹치지 않는 두 개의 AWS 시스템 관리자 유지 관리 기간 정의 + 겹치지 않는 두 개의 AWS 시스템 관리자 유지 관리 기간 정의
단일 AWS CloudFormation 템플릿을 사용하여 별도의 계정에 애플리케이션 IAM 역할 및 리소스를 복제합니다. 각 애플리케이션 인스턴스의 VPC를 중앙 VPC에 연결하려면 VPC 피어링


AWS::AutoScaling::AutoScalingGroup 리소스를 편집하여 UpdatePolicy 속성을 삽입하여 롤링 업데이트


AWS Systems Manager 패치 관리자


AWS Snowmobile - 대규모 마이그레이션

CloudFormation 템플릿을 수정하여 RDS 및 EBS 리소스에 DeletionPolicy 속성을 추가
AWS DMS
AWS SCT를 사용하여 애플리케이션에 포함된 SQL 코드 중 변환할 수 있는 코드와 수동으로 수행해야 하는 작업을 식별
Fargate 시작 유형을 사용하여 Amazon ECS 클러스터에서 작업을 예약

AWS Business Support 플랜
Kinesis는 항상 IoT 및 로드에 가장 적합

Solutions Architect는 기존 애플리케이션 환경과 동일한 환경을 생성하고 애플리케이션을 새 환경에 배포
환경 URL 교체


DNS를 Amazon Route 53으로 마이그레이션하고 AWS Shield
Amazon CloudFront 배포판을 생성 및 사용하고 여기에 AWS WAF



AWS로 이전하고 있으며 Amazon Rekognition을 사용하여 데이터를 추가하여 기존 메타데이터를 강화
AWS KMS - Key Management Service


StatusCheckFailed_System

SAML(보안 검증 마크업 언어)
Security Assertion Markup Language
은 ID 제공자(IdP)가 사용자를 인증한 다음 SP(서비스 제공자)라고 하는 다른 애플리케이션에 인증 토큰을 전달할 수 있는 공개 통합 표준


Amazon Athena 및 Amazon QuickSight와 같은 도구를 사용하여 비용 및 사용 보고서를 분석

람다 최대 15분

DNSSEC

aws athena

ENI 풀

AWS Service Catalog

redshift 대비 임시쿼리 + 비용 이점
AWS Glue 크롤러를 사용하여 모든 데이터베이스를 크롤링
Amazon S3에 로드하고 Amazon Athena를 사용하여 쿼리를 실행


AWS CDK를 사용하면 코드로 인프라를 정의하고 AWS CloudFormation을 통해 이를 프로비저닝할 수 있다

장기 자격 증명 없음 -> 역할 사용. 계정 자체가 아닌 역할에 권한을 부여하는 버킷 정책

ExecutionRoleArn(옵션 C)과 taskRoleArn(D) 사이에서는 후자만 DynamoDB와 상호 작용하는 데 사용됩니다. 전자는 이미지를 다운로드하거나 Cloudwatch에 로그를 쓰는 데 사용

DynamoDB는 strongly consistent reads 지원하지 않음

FullAWSAccess SCP를 제거하면 모든 서비스에 대한 모든 작업이 이제 암시적으로 거부

AWS X-Ray 엔드투엔드

원격 IP를 식별하려면 S3 액세스 로그를 확인해야 합니다. Athena는 이러한 로그를 분석하는 데 도움을 줌

Amazon Transcribe - 개발자가 음성을 텍스트로 변환하는 기능을 애플리케이션에 쉽게 추가할 수 있도록 하는 자동 음성 인식(ASR) 서비스
비용 탐색기를 사용하여 추가 비용의 원인을 해결

Amazon Macie는 s3만 가능

ALB(Application Load Balancer)

요청자 지불이 활성화된 경우 요청에는 request-payer 매개변수가 포함되어야 함


NAT 게이트웨이 - 시간당 요금

스택 세트를 사용하면 단일 CloudFormation 템플릿을 사용하여 여러 리전에 대해 AWS 계정에서 스택을 생성할 수 있습니다

기본적으로 S3 객체는 해당 객체를 업로드한 AWS 계정이 소유합니다. 버킷을 다른 계정이 소유한 경우에도 마찬가지입니다. 객체에 대한 액세스 권한을 얻으려면 객체 소유자가 귀하(버킷 소유자)에게 명시적으로 액세스 권한을 부여해야 합니다

AWS Config 관리형 규칙을 사용하여 정책 위반을 감지

CMK
Customer managed key

AWS CLI update-alias 명령을 Routing-config 파라미터와 함께 사용하여 로드를 분산

Amazon Kinesis Data Streams를 사용하여 인바운드 센서 데이터를 수집하고, Kinesis 클라이언트로 데이터를 분석하고, Amazon EMR을 사용하여 Amazon Redshift 클러스터에 결과를 저장


AWS SMS (Server Migration Service)

AWS Step Functions - 대규모 병렬 워크플로 오케스트레이션
DynamoDB Streams를 사용하여 람다를 트리거한 다음 단계 함수를 트리거

카나리아 배포 - 일부 사용자가 사용할 수 있도록
Amazon Kinesis 데이터 스트림을 처리하는 AWS Lambda 함수를 생성하고, @connections 명령을 사용하여 연결된 클라이언트에 콜백 메시지

AWS CDK(AWS Cloud Development Kit)

Amazon EventBridge
EventBridge 스케줄러를 사용하여 미리 알림, 지연된 작업 또는 중지 위치에서 다시 시작 알림이 포함된 일정 예약 서비스

AWS AppSync
Fully-Managed Serverless GraphQL API Service for Real-Time Data Queries
AWS CodeBuild를 사용하여 테스트 및 보안 스캔을 실행
AWS IoT 모니터링에 적절한 AWS IoT 규칙이 실행되고 있는지, AWS IoT 규칙이 올바른 규칙 작업으로 활성화되어 있는지 확인


AWS Shield Advanced

AWS Schema Conversion Tool, AWS Database Migration Service(AWS DMS)
Amazon S3의 공개 액세스 차단 기능을 사용하여 버킷에서 IgnorePublicAcis 옵션을 TRUE



NLB 서브넷과의 통신을 허용하려면 NACL이 로깅 서비스 서브넷에 연결되어 있는지 확인하십시오. EC2 인스턴스에서 실행되는 로깅 서비스 서브넷과의 통신을 허용하려면 NACL이 NLB 서브넷에 연결되어 있는지 확인
EC2 인스턴스에서 실행 중인 로깅 서비스에 대한 보안 그룹을 확인하여 NLB 서브넷으로부터의 수신을 허용하는지 확인

내결함성이란 시스템의 일부 구성 요소가 작동하지 않더라도 계속 작동할 수 있는 기능
고가용성은 전체 시스템에 대한 개념입니다. 그 목표는 사람이 개입하지 않아도 시스템이 항상 작동하며 액세스 가능하며 가동 중지를 최소화하도록 보장하는 것

