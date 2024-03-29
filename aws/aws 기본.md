# Cloud Computing

배민 카프카 클러스터 규모 (22년 8월)
- 카프카 27대
- 브로커 162대
- 파티션 24000개
- 초당 평균 50만건, 최대 140만건

90% 이상 aws
azure는 앱 특화
google은 쿠버네티스 특화

## AWS 인프라의 전체적인 모습

1. 인프라 관련 요소
AWS 
API Gateway, S3, ELB, CloudFront, Secret Manager, 스냅샷
2. 컴퓨팅 파워 (서버)
EC2, Elastic Beanstalk, ecs, fargate, lambda(serveerldee)
3. DB
RDS, DynamoDB, ElasticCache
4. Message Queue
SQS, MSK, Kinesis

## AWS VPC
Virtual Private Cloud
- 가상 네트워크 서비스로 퍼블릭 네트워크와 프라이빗 네트워크를 분리하고 모니터링할 수 있도록 해주는 서비스
- 네트워크 구성과 관련된 사실상 모든 기능을 담당하며, 자체 데이터 센터에서 운영하는 기존 네트워크와 매우 유사

VPC(Private Subnet-DB, Public Subnet-Server)

## AWS API Gateway
API Gateway
- 어떤 규모에서든 개발자가 API를 손쉽게 생성, 게시, 유지 관리, 모니터링 및 보안 유지할 수 있도록 하는 완전관리형 서비스
- 서버의 대문
- 트래픽 관리, CORS, 권한 부여 및 액세스 제어, 제한, 모니터링 API 버전 관리, 인증관련

lambda + API Gateway (serverless)

테라폼, cloud formation

## AWS ELB
Elastic Load Balancing
다수의 컴퓨팅 리소스를 부하 분산(EC2, lambda 등)

ALB (HTTPS), NLB(TCP, UDP, TLS), GLB (GWLB)

## S3
데이터 처리 (Trigger로 람다 실행 가능), 로그, 쿼리 지원 (AWS Athena), 호스팅(Single Page Application) React 
-> 이동 이벤트를 s3에 저장하고 Athena로 분석

사진 올리면 -> Trigger 람다 -> 섬네일이 자동으로 만들어지는 구조 할 수 있음

## CloudFront (캐시 서비스)
- AWS 엣지 로케이션을 통한 CDN 서비스
- 속도 높이고, 가격 내리며, 서버 부하를 줄일 수 있음

캐싱하고 싶은 s3 bucket 을 지정할 수 있음
cloudfront 무효화 하면 캐싱된 것을 날리고 새로 캐싱함

## AWS Secret Manager / Parameter Store

- 데이터베이스 보안 인증 정보 및 API키와 같은 보안 정보를 안전하게 암호화하고 중앙 집중식으로 감사할 수 있도록 도와주는 서비스
- .env파일을 서버 별로 따로 관리하기 보다는 한 곳에서 집중적으로 관리
- DB정보와 같은 민감한 정보는 암호화가 지원이 되는 Secret Manager에 저장
- 간단한 정보(Endpoint)는 Parameter Store에 저장

## EC2, Elastic Beanstalk, ECR(container registry)

## ECS - 컨테이너 오케스트레이션 서비스 (EC2, Fargate 중 선택 가능)

## Fargate, Lambda
Lambda -> 코드를 돌리기위한 리소스를 임의로 지정할 수 있으며, 사용 리소스 x 사용 시간에 따라 과금
사용 예시 - 비동기 처리(이미지 썸네일 생성), 예측이 불가능 한 리소스 사용 (대용량 처리/머신러닝)
https://docs.aws.amazon.com/ko_kr/lambda/latest/dg/java-samples.html

## AWS RDS
- 클라우드에서 관계형 데이터베이스를 간편하게 설정, 운영 및 확장할 수 있는 관리형 서비스 모음
- RDS 백업: 자동 백업, DB 스냅샷
- 멀티 AZ(Availability Zone): 데이터 복제본을 다른 가용 영역에 저장하여 데이터 손실을 방지
- CloudWatch 연동

## Amazon Aurora
- 커스터마이징하여 AWS에 최적화
- EBS 대신 NVMe SSD 드라이브 위에 구축되어 훨씬 빠름 (MySQL 5배, PostgreSQL 3배)
- 서버리스 기능과 Auto Scailing을 기본적으로 지원
20% 비쌈

Aurora Serverless 제공

## Amazon DynamoDB

- 완전관리형 NoSQL 데이터베이스 서비스로서 원할한 확장성과 함께 빠르고 예측 가능한 성능을 제공
- 서버리스
- 보조 인덱스를 통한 빠른 조회를 지원
  - 인덱싱 없으면 속도가 느려지지만 (특정 value를 key로 뽑아 정렬) 그 부분을 해결해줌
- 람다와 궁합이 잘 맞음

## elasticache
- 클라우드에서 분산된 인 메모리 데이터 스토어 또는 캐시 환경을 손쉽게 설정
- redis와 memcached을 지원
- 캐시 노드 실패에서 자동 감지 및 복구
캐싱 / 세션 스토어 / AI ML 모델 / 실시간성이 높은 작업


## Queue
쿠팡 앞단에 큐가 있음, SOCAR 차량 기록 전송(MSK), 네이버 웹툰 kafka, HDFS
대용량 처리를 가도 큐를 이용함

## SQS
- 마이크로서비스, 분산 시스템 및 서버리스 애플리케이션을 위한 완전관리형 메시지 대기열
- 표준 대기열: 무제한 처리량 / 최소한 한 번 전달 (여러번 전달 될 수 있음) / 최선 노력 순서
- FIFO 대기열:  초당 최대 300개의 메시지/ 정확하 한 번 처리/ 선입선
- 개별 메시지 별로 확인/실패가 필요한 경우

SQS - Dead Letter Queue를 제공

## Kinesis
- 모든 규모의 스트리밍 데이터를 비용 효율적으로 처리할 수 있는 핵심 기능과 더불어 애플리케이션 요구 사항에 가장 적합한 도구를 선택할 수 있는 유연성을 제공
- 실시간으로 비디오 및 데이터 스트림을 손쉽게 수집, 처리 및 분석
- 모든 규모에서 쉽게 데이터 스트리밍
- 안정적으로 실시간 스트림을 데이터 레이크, 웨어하우스, 분석 서비스에 로드
- 스트리밍 데이터에서 실행 가능한 인사이트 확보
- 여러 application이 하나의 스트림을 동시에 사용
Shard




