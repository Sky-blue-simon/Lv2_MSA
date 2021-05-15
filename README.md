# Lv2_MSA

## Zookeeper 및 Kafka 서버 실행
 1. Zookeeper 기동 후 Kafka 기동
   - Kafka 설치 경로로 이동
     - cd 카프카설치경로/bin/windows
     - cd kafka_2.13-2.5.0/bin/windows
     - zookeeper-server-start.bat ../../config/zookeeper.properties

2. Kafka 기동
  - Kafka 설치 경로로 이동
  - cd 카프카설치경로/bin/windows
  - kafka-server-start.bat ../../config/server.properties

Zookeeper 및 Kafka 서버 구동
Zookeeper 기동 후 Kafka 기동
$ bin/zookeeper-server-start.sh config/zookeeper.properties
$ bin/kafka-server-start.sh config/server.properties

zookeeper 실행
cd kafka_2.13-2.5.0/bin
./zookeeper-server-start.sh …/config/zookeeper.properties

kafka 실행
cd kafka_2.13-2.5.0/bin
./kafka-server-start.sh …/config/server.properties

***cd 카프카설치 경로/bin/windows
zookeeper-server-start.bat ../../config/zookeeper.properties
kafka-server-start.bat ../../config/server.properties

- zookeeper 
cd 카프카설치 경로/bin/windows
zookeeper-server-start.bat ../../config/zookeeper.properties	

-kafka
cd 카프카설치 경로/bin/windows
kafka-server-start.bat ../../config/server.properties

****-- 이벤트 수신하기
kafka-console-consumer.bat --bootstrap-server http://localhost:9092 --topic shop --from-beginning
kafka-console-consumer.bat --bootstrap-server http://localhost:9092 --topic shop --from-beginning

## Kafka 설치



2. 주키퍼 실행  
     ./zookeeper-server-start.sh ../config/zookeeper.properties &
    3. 카프카 broker 실행  
     ./kafka-server-start.sh ../config/server.properties
    ( 4 ~ 5 는 건너뛰어도 됨 )
    4. 카프카 topic 만들기  
     ./kafka-topic.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic teamtwohotel
    5. 카프카 producer 실행  
     ./kafka-console-poducer.sh --broker-list localhost:9092 --topic teamtwohotel
    6. 카프카 consumer 실행  
     ./kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic teamtwohotel --from-beginning
    7. 카프카 토픽 삭제
     ./kafka-topics.sh --zookeeper localhost:2181 --delete --topic DummyTopic
    8. 카프카 토픽 리스트
     ./kafka-topics.sh --list --zookeeper localhost:2181
    9. 카프카가 비정상일 때 
     sudo lsof -i :2181 한뒤  
     kill -9 pid 하고 다시 띄워준다


## Kafka Topic 

1.1) Topic 생성
  - /usr/local/kafka/bin/kafka-topics.sh --bootstrap-server http://localhost:9092 --topic topic_example --create --partitions 1 --replication-factor 1

1.2) 토픽 리스트 보기
  - /usr/local/kafka/bin/kafka-topics.sh --bootstrap-server http://localhost:9092 --list    

1.3) 새로운 터미널 창에서 kafka producer 연결 후 메세지 publish
  - /usr/local/kafka/bin/kafka-console-producer.sh --broker-list http://localhost:9092 --topic topic_example

1.4) 새로운 터미널 창에서 kafka consumer 연결 후 메세지 subscribe
  - /usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server http://localhost:9092 --topic topic_example --from-beginning

1.5) hello world 라는 메세지 publish 해보기



2. Httpie
 
2.1) REST Client 툴 httpie 설치(Windows): 
      https://github.com/TheOpenCloudEngine/uEngine-cloud/wiki/Httpie-설치
