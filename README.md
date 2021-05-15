# Lv2_MSA

1. Kafka 
 1) Topic 생성
  - /usr/local/kafka/bin/kafka-topics.sh --bootstrap-server http://localhost:9092 --topic topic_example --create --partitions 1 --replication-factor 1
 2) 토픽 리스트 보기
  - /usr/local/kafka/bin/kafka-topics.sh --bootstrap-server http://localhost:9092 --list    
 3) 새로운 터미널 창에서 kafka producer 연결 후 메세지 publish
  - /usr/local/kafka/bin/kafka-console-producer.sh --broker-list http://localhost:9092 --topic topic_example
 4) 새로운 터미널 창에서 kafka consumer 연결 후 메세지 subscribe
  - /usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server http://localhost:9092 --topic topic_example --from-beginning
 5) hello world 라는 메세지 publish 해보기


2. Httpie
  1) REST Client 툴 httpie 설치(Windows): 
      https://github.com/TheOpenCloudEngine/uEngine-cloud/wiki/Httpie-설치
