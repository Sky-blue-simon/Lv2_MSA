# Lv2_MSA

## Httpie
1.REST Client 툴 httpie 설치(Windows): 
  
 - https://github.com/TheOpenCloudEngine/uEngine-cloud/wiki/Httpie-설치
      
      
## Zookeeper 및 Kafka 서버 실행
1. Zookeeper 기동 후 Kafka 기동
- Kafka 설치 경로로 이동
- cd 카프카설치경로/bin/windows
- cd kafka_2.13-2.8.0/bin/windows
- zookeeper-server-start.bat ../../config/zookeeper.properties

2. Kafka 기동
- Kafka 설치 경로로 이동
- cd 카프카설치경로/bin/windows
- cd kafka_2.13-2.8.0/bin/windows
- kafka-server-start.bat ../../config/server.properties

3. Kafka 수행 내역 확인(이벤트 수신하기)
  - kafka-console-consumer.bat --bootstrap-server http://localhost:8083 --topic eventTopic --from-beginning
  - kafka-console-consumer.bat --bootstrap-server http://localhost:9092 --topic shop --from-beginning

## 토픽 생성(실습 내용)
1. 토픽 생성
  - cd cd 카프카설치 경로/bin/windows
  - cd kafka_2.13-2.8.0/bin/windows
  - kafka-topics.bat --zookeeper localhost:2181 --topic cna --create --partitions 1 --replication-factor 1

2. 토픽 리스트 보기
  - kafka-topics.bat --zookeeper localhost:2181 --list

3. 새로운 터미널 창에서 kafka producer 연결 후 메세지 publish(이벤트 보내기)
  - kafka-console-producer.bat --broker-list http://localhost:9092 --topic cna

4. 새로운 터미널 창에서 kafka consumer 연결 후 메세지 subscribe
  - kafka-console-consumer.bat --bootstrap-server http://localhost:9092 --topic cna --from-beginning

5. hello world 라는 메세지 publish 해보기



## 테스트 시나리오
아래는 테스트 시나리오 입니다 (아참 포트가 하나인 이유는 gateway까지 실행해 주세요)

기본적인 Saga 패턴이 적용 되어있습니다. 
학생이 수강 신청을 하였을 때, 수강 신청이 되고, 결제가 되고, 배송이 시작됩니다.
학생이 수강 취소를 하였을 때, 수강 신청이 취소가 되고, 결제가 취소되고, 배송이 취소됩니다.
해당 서비스가 묶여서 함깨 작동하도록 트랜젝션이 묶여 있습니다.

수강 : class, port 8081
강의 : course, port 8082
결재 : pay, port 8083


- 강의 등록
  
  http POST http://localhost:8088/courses name=english teacher=hong fee=10000 textBook=eng_book
  
  http POST http://localhost:8088/courses name=math teacher=park fee=20000 textBook=math_book
  
  http POST http://localhost:8088/courses name=math teacher=bong fee=30000 textBook=music_book
  
  http POST http://localhost:8088/courses name=math teacher=simon fee=40000 textBook=korean_book

- 강의 확인
  
  http GET http://localhost:8088/courses

- 수강 신청
  
  http POST http://localhost:8088/classes courseId=1 fee=10000 student=gil-dong textBook=eng_book

  http POST http://localhost:8088/classes courseId=2 fee=20000 student=soo textBook=math_book
  
  http POST http://localhost:8088/classes courseId=3 fee=30000 student=lee textBook=music_book
  
  http POST http://localhost:8088/classes courseId=4 fee=40000 student=kim textBook=korean_book

- 수강 등록 확인
  
  http GET http://localhost:8088/classes

- 결제 확인
  
  http GET http://localhost:8088/payments

- 배송 시작 확인
  
  http GET http://localhost:8088/deliveries

- My page 확인
  
  http GET http://localhost:8088/inquiryMypages

- 수강 취소할 Class 확인
  
  http GET http://localhost:8088/classes

- 수강 취소
  
  http DELETE http://localhost:8088/classes/1

- 수강 삭제 확인
  
  http GET http://localhost:8088/classes

- 결제 취소 확인 (상태값 "PAYMENT_CANCELED" 확인)
  
  http GET http://localhost:8088/payments

- 배송 취소 확인 (상태값 "DELIVERY_CANCELED" 확인)
  
  http GET http://localhost:8088/deliveries

- My page 확인 (상태값 "CLASS_CANCELED", "PAYMENT_CANCELED", "DELIVERY_CANCELED" 확인)
  
  http GET http://localhost:8088/inquiryMypages
  

- 수강 신청내용 부분 수정
  
  http PATCH http://localhost:8088/classes/1 fee=8888
  
 - Parameter 값으로만 수정, 나머지 값은 다 삭제됨

  http PUT http://localhost:8088/classes/1 courseId=1 fee=2000 student=gil-dong



- point 확인
  
  http GET http://localhost:8088/points
  
- gift 확인
  
  http GET http://localhost:8088/gifts
   
   

      
## CI/CD
마이크로 서비스에서 일반적으로 구성하는 CI 구성은 다음과 같습니다.
소스코드 커밋
소스코드 빌드
소스코드 테스트
Docker 빌드
Docker 테스트

## 환경 설정
1. aws cli 설치

 - https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/install-cliv2-windows.html
 - aws configure 로 액세스 ID 등 입력

2. eksctl 설치
 - https://docs.aws.amazon.com/ko_kr/eks/latest/userguide/getting-started-eksctl.html

3. IAM 생성

 - https://www.44bits.io/ko/post/publishing_and_managing_aws_user_access_key

4. eksctl 생성 ( 시간이 좀 걸림 )

 - eksctl create cluster --name teamtwohotel --version 1.17 --nodegroup-name standard-workers --node-type t3.medium --nodes 4 --nodes-min 1 --nodes-max 4

5. Local EKS 클러스터 토큰가져오기 ( CI/CD 할때 필요 )

- aws eks --region ap-northeast-2 update-kubeconfig --name teamtwohotel

6. 아마존 컨테이너 레지스트리

 - 아마존 > ecr (elastic container registry) > ecr 레파지터리 : ECR은 각 배포될 이미지 대상과 이름을 맞춰준다
 - aws ecr create-repository --repository-name teamtwohotel --region ap-northeast-2
 - aws ecr put-image-scanning-configuration --repository-name teamtwohotel --image-scanning-configuration scanOnPush=true --region ap-northeast-2

7. AWS 컨테이너 레지스트리 로그인

 - aws ecr get-login-password --region (Region-Code) | docker login --username AWS --password-stdin (Account-Id).dkr.ecr.(Region-Code).amazonaws.com

8. AWS 레지스트리에 도커 이미지 푸시하기 (이건 위에서 한 거랑 좀 겹치는듯)

 - aws ecr create-repository --repository-name (IMAGE_NAME) --region ap-northeast-2
 - docker push (Account-Id).dkr.ecr.ap-northeast-2.amazonaws.com/(IMAGE_NAME):latest


## 도커 빌드 및 푸쉬

1. Windows 10 에 Docker 데몬 (Docker for 설치

- https://www.docker.com/products/docker-desktop
- https://steemit.com/kr/@mystarlight/docker

1. 먼저 mvn package 로 jar 파일을 만들어야 함.

2. 도커 빌드
 - 리포지토리 주소 앞에 docker build -t 를 붙이고 뒤에 :v1 . 을 붙여서 각 프로젝트(로컬) 디렉토리에서 실행
 - docker build -t (ID).dkr.ecr.ap-northeast-2.amazonaws.com/adminxx-game-gateway:v1 .

3. 도커 푸쉬
 - docker push 690521455231.dkr.ecr.ap-northeast-2.amazonaws.com/admin-customer:v1
 - 오류가 발생한다면 aws ecr get-login-password 가 잘됐는 지 확인

## 아마존
1. AWS 계정 생성
 - https://portal.aws.amazon.com 접속


## 파이프라인 샘플
![image](https://user-images.githubusercontent.com/80744224/118383378-16b02b00-b638-11eb-896e-d4de890f18cd.png)

- GitLab - 소스코드 형상관리 도구
- Jenkins - CI 도구
- Docker - 컨테이너 도구
- Spinnaker - CD 도구
- Helm - 쿠버네티스 패키지 관리 도구
- Kubernetes - 배포 플랫폼

## 서킷브레이크

![image](https://user-images.githubusercontent.com/80744224/118924312-df16eb00-b977-11eb-8ddf-e51ab0469cc0.png)

요청/응답 통신중에 특정 서비스가 응답시간이 느려진다거나, 오류가 특정치 이상 발생할때 서킷브래이커를 적용하여 빠르게 차단을 시켜버립니다. 쇼핑몰에서 주문을 하였는데, 배송서비스가 느려서 주문 자체가 느려지는 것보다, 배송서비스를 잠시 차단 시키더라도 주문을 빠르게 받는 것이 사용자 입장에서는 더 효율적입니다. 

자바로 서킷 브레이크 구현하는 방법
[java 방식 CircuitBreaker] https://bcho.tistory.com/1247
[javascript 방식 CircuitBreaker] https://velog.io/@vies00/Circuit-Breaker-Pattern

1. Istio 를 사용한 서킷 브레이크
Istio 를 사용하는 방법은 소스코드를 수정할 필요가 없습니다. 
쿠버네티스에 배포되어있는 서비스에 sidecar 를 추가하여 네트워크 트래픽을 모니터링 후 지정한 시간을 오버하거나, 에러율이 높으면 트래픽을 끈어버리는 방법 입니다. 
다만 직접 소스코드를 수정하지 못하니, fall-back 같은 처리는 하지 못합니다.

2. 사전 준비사항:
 - kubernetes 클러스터 준비
 - istio 설치
 - siege : http 로드테스트 도구
 
## Kubernetes Clients Installation

1. Kubectl 설치
- Kubectl 설치
   
   - sudo apt-get update
   - sudo apt-get install -y kubectl

- 설치 확인
   - kubectl version --client

2. AWS CLI v2 tool Installation
 - AWS CLI 설치
     - curl "https://awscli.amazonaws.com/awscli exe linux x86_64.zip" o "awscliv2.zip“
     - unzip awscliv2.zip
       ㅇunzip 없을 시 , 설치 : sudo apt get install unzip
     - sudo ./aws/install
 
  - AWS CLI 설치 확인
     - aws --version
 
3. AWS CLI tool Configuration
   
   - ubuntu 설치 후, minikube 설치함.
     
     https://minikube.sigs.k8s.io/docs/start/
   
   - AWS CLI 사용자 등록을 위한 자격증명 입력
     aws configure
   
   - Profile 기반 AWS 서비스 관리
     aws ec2 describe-instances --profile devuser
     aws s3 ls --profile produser
     
    - AWS Profile 설정 확인
     cat ~/.aws/config
     cat ~/.aws/credentials
     
    - EKS Client(eksctl) Installation (EKS Client (eksctl) 설치)
     - EKS 에서 Kubernetes 클러스터 생성 관리 도구
     - Linux Curl 을 사용한 eksctl 설치(about 15' 소용) (교재)
       curl --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz"|tar xz -C /tmp
       sudo mv /tmp/eksctl /usr/local/bin
     - eks 생성(강의 실습)
       
       eksctl create cluster --name user09-eks --version 1.17 --nodegroup-name standard-workers --node-type t3.medium --nodes 4 --nodes-min 1 --nodes-max 4
       
       eksctl create cluster --name user15-sk-Cluster --version 1.15 --nodegroup-name standard-workers --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3
       
       eksctl create cluster --name admin04-eks --version 1.17 --nodegroup-name standard-workers --node-type t3.medium --nodes 4 --nodes-min 1 --nodes-max 4
       

## EKS에 카프카 설치

helm 사전에 설치해야함

https://medium.com/google-cloud/installing-helm-in-google-kubernetes-engine-7f07f43c536e

kubectl --namespace kube-system create sa tiller      # helm 의 설치관리자를 위한 시스템 사용자 생성

kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

helm repo add incubator https://charts.helm.sh/incubator

helm repo update

kubectl create ns kafka

helm install my-kafka --namespace kafka incubator/kafka

카프카 설치 확인
kubectl get all -n kafka

카프카 토픽 생성
kubectl -n kafka exec my-kafka-0 -- /usr/bin/kafka-topics --zookeeper my-kafka-zookeeper:2181 --topic game --create --partitions 1 --replication-factor 1

토픽 확인
kubectl -n kafka exec my-kafka-0 -- /usr/bin/kafka-topics --zookeeper my-kafka-zookeeper:2181 --list

이벤트 발행하기 (producer, 딱히 필요없음)
kubectl -n kafka exec -ti my-kafka-0 -- /usr/bin/kafka-console-producer --broker-list my-kafka:9092 --topic teamtwohotel

이벤트 수신하기
kubectl -n kafka exec -ti my-kafka-0 -- /usr/bin/kafka-console-consumer --bootstrap-server my-kafka:9092 --topic teamtwohotel --from-beginning


## Helm 설치

- curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash (아래 페이지 참조)

helm 설치 

https://medium.com/google-cloud/installing-helm-in-google-kubernetes-engine-7f07f43c536e

우분투에서 helm 설치

1) Helm 설치

Helm은 kubectl 명령과 kubeconfig 파일이 있는 서버에서 설치해야 한다.

2) 바이너리 다운로드

wget을 이용하여 Helm 바이너리 파일을 다운로드 받는다.

https://github.com/helm/release 에서 버전을 확인한다.

위 URL에 접속하여 확인 후, 알맞은 버전을 다운로드 한다.

wget https://get.helm.sh/helm-v3.5.3-linux-amd64.tar.gz

3) 다운로드 받은 파일 압축해제

tar xf helm-v3.5.3-linux-amd64.tar.gz

4) 파일 실행을 위한 경로 이동

sudo cp linux-amd64/helm /usr/local/bin/helm

5) Helm 실행 확인

helm version




kubectl --namespace kube-system create sa tiller      # helm 의 설치관리자를 위한 시스템 사용자 생성

kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller

helm init --service-account tiller

helm repo update

## 최종 평가
https://workflowy.com/s/assessment/qJn45fBdVZn4atl3
