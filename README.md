# Apache Kafka 란


Apache Kafka는 빠르고 확장 가능한 작업을 위해 데이터 피드의 분산 스트리밍, 파이프 라이닝 및 재생을 위한 실시간 스트리밍 데이터를 처리하기 위한 목적으로 설계된 오픈 소스 분산형 게시-구독 메시징 플랫폼입니다.

Kafka는 서버 클러스터 내에서 데이터 스트림을 레코드로 유지하는 방식으로 작동하는 브로커 기반 솔루션입니다. Kafka 서버는 여러 데이터 센터에 분산되어 있을 수 있으며 여러 서버 인스턴스에 걸쳐 레코드 스트림(메시지)을 토픽으로 저장하여 데이터 지속성을 제공할 수 있습니다. 토픽은 레코드 또는 메시지를 키, 값 및 타임 스탬프로 구성된 일련의 튜플, 변경 불가능한 Python 객체 시퀀스로 저장합니다.

![Alt text](https://www.tibco.com/sites/tibco/files/media_entity/2020-10/apache-kafka-diagram.svg)

---------------------------------------  

# Apache Kafka의 개념
* 토픽: 토픽은 게시/구독 메시징에서 상당히 보편적인 개념입니다. Apache Kafka 및 기타 메시징 솔루션에서 토픽은 지정된 데이터 스트림(일련의 레코드/메시지)에 대한 관심을 표시하는 데 사용되는 주소 지정 가능한 추상화입니다. 토픽은 게시 및 구독할 수 있으며 애플리케이션에서 주어진 데이터 스트림에 대한 관심을 표시하는 데 사용하는 추상화 계층입니다.

* 파티션 : Apache Kafka에서 토픽은 파티션이라는 일련의 순서 대기열로 세분화될 수 있습니다. 이러한 파티션은 연속적으로 추가되어 순차적 커밋 로그를 형성합니다. Kafka 시스템에서 각 레코드/메시지에는 지정된 파티션의 메시지 또는 레코드를 식별하는 데 사용되는 오프셋이라는 순차 ID가 할당됩니다.

* 영속성: Apache Kafka는 레코드/메시지가 게시될 때 지속적으로 유지하는 서버 클러스터를 유지 관리하여 작동합니다. Kafka 클러스터는 구성 가능한 보존 시간 제한을 사용하여 소비에 관계없이 주어진 레코드가 지속되는 기간을 결정합니다. 레코드/메시지가 보존 시간 제한 내에 있는 동안 레코드/메시지를 사용할 수 있습니다. 레코드/메시지가 이 보존 시간 제한을 초과하면 레코드/메시지가 삭제되고 공간이 확보됩니다.

* 토픽/파티션 확장 : Apache Kafka는 서버 클러스터로 작동하기 때문에 주어진 토픽/파티션에서 각 서버에 부하를 공유하여 토픽/파티션을 확장할 수 있습니다. 이 부하 공유를 통해 Kafka 클러스터의 각 서버는 주어진 토픽/파티션에 대한 레코드/메시지의 배포 및 영속성을 처리할 수 있습니다. 개별 서버가 모든 배포 및 영속성을 처리하는 동안 모든 서버는 서버가 실패할 경우 내결함성과 고가용성을 제공하는 데이터를 복제합니다. 파티션은 파티션 리더로 선택된 한개 서버와 팔로워 역할을 하는 다른 모든 서버들로 분할됩니다. 파티션 리더 인 서버는 데이터의 모든 배포 및 영속성 (읽기/쓰기)을 처리하고 팔로워 서버는 내결함성을 위한 복제 서비스를 제공합니다.

* 프로듀서: Apache Kafka에서 프로듀서 개념은 대부분의 메시징 시스템과 다르지 않습니다. 데이터(레코드/메시지) 프로듀서는 주어진 레코드/메시지가 게시되어야 하는 토픽(데이터 스트림)를 정의합니다. 파티션은 추가 확장성을 제공하는 데 사용되므로 프로듀서는 주어진 레코드/메시지가 게시되는 파티션도 정의할 수 있습니다. 프로듀서는 주어진 파티션을 정의할 필요가 없으며 파티션을 정의하지 않음으로써 토픽 파티션에서 순차 순환 대기 방식의 로드 밸런싱을 달성할 수 있습니다.

* 컨슈머: 대부분의 메시징 시스템과 마찬가지로 Kafka의 컨슈머는 레코드/메시지를 처리하는 엔터티입니다. 컨슈머는 개별 워크로드에서 독립적으로 작업하거나 지정된 워크로드에서 다른 컨슈머와 협력하여 작업하도록 구성할 수 있습니다(로드 밸런싱). 컨슈머는 컨슈머 그룹 이름을 기반으로 워크로드를 처리하는 방법을 관리합니다. 컨슈머 그룹 이름을 사용하면 컨슈머를 단일 프로세스 내, 여러 프로세스, 심지어 여러 시스템에 분산시킬 수 있습니다. 컨슈머 그룹 이름을 사용하여 컨슈머는 컨슈머 집합 전체에서 레코드/메시지 소비를 로드 밸런싱(동일한 컨슈머 그룹 이름을 가진 여러 컨슈머)하거나 토픽/파티션을 구독하는 각 컨슈머가 처리 메시지를 받도록 각 레코드/메시지를 고유하게 (고유한 컨슈머 그룹 이름을 가진 여러 컨슈머) 처리할 수 있습니다.

---------------------------------------  

# Install. 
``` AWS 주요 설치 내용 (실전카프카 개발부터 운영까지 참고)
1. sudo amazon-linux-extras install -y ansible2
2. sudo yum install -y git
3. git clone https://github.com/onlybooks/kafka2
4. cd ~/kafka2/chapter2/ansible-book
5. ansible-playbook -i hosts zookeeper.yml
```

1. AWS(Amazon Web Service) Instance 생성.
 - AWS Instance 생성방법 : (https://zzang9ha.tistory.com/329)
 - AWS Instance Mac OS 접속방법 : (https://zzang9ha.tistory.com/338)
2. Instance Java JDK 설치
```
sudo yum install -y java-1.8.0-openjdk-devel.x86_64
java -version
```

3. kafka 브로커 실행
 ```
 (kafka_2.12-2.5.0 버전으로 실습 진행함)

 wget https://archive.apache.org/dist/kafka/2.5.0/kafka_2.12-2.5.0.tgz
 tar xvf kafka_2.12-2.5.0.tgz
 cd kafka_2.12-2.5.0
 ```

4. kafka 브로커 힙 메모리 설정  
 1). 환경변수 export 이용하여 선언(터미널 세션 종료시 초기화)
 ```
 export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m"
 echo $KAFKA_HEAP_OPTS
 ```
 2). ~/.bashrc 파일에 입력(터미널 세션이 종료되어도 초기화되지 않음)
 ```
 vim ~/.bashrc

 a 눌러 insert mode 진입
 마지막 부분에
 export KAFKA_HEAP_OPTS="-Xmx400m -Xms400m" 추가
 esc :wq
 source ~/.bashrc
 echo $KAFKA_HEAPS_OPTS
 ``` 
 
 # 주요 용어(카프카를 구성하는 주요 요소)
  - 주키퍼(ZooKeeper) : 카프카의 메타데이터(metadata) 관리 및 브로커의 정상 상태 점검(health check) 담당
  - 카프카 또는 카프카 클러스터(Kafka cluster) : 여러 대의 브로커를 구성한 크러스터를 의미
  - 브로커(Broker) : 카프카 애플리케이션이 설치된 서버 또는 노드
  - 프로듀서(producer) : 카프카로 메시지를 보내는 역할
  - 컨슈머(consumer) : 카프카에서 메시지를 꺼내가는 역할
  - 토픽(topic) : 카프카는 메시지 피드들을 토픽으로 구분하고, 각 토픽의 이름은 카프카내에서 고유
  - 파티션(partition) : 병렬 처리 및 고성능을 얻기 위해 하나의 토픽을 여러 개로 나눈 것을 말함
  - 세그먼트(segment) : 프로듀서가 전송한 실제 메시지가 브로커의 로컬 디스크에 저장되는 파일을 말함
  - 메시지(message) 또는 레코드(record) : 프로듀서가 브로커로 전송하거나 컨슈머가 읽어가는 데이터 조각
  
 1. 리플리케이션(Replication)
  각 메시지들을 여러 개로 복제해서 카프카 클러스터 내 브로커들에 분산 시키는 동작을 의미한다. 이러한 리플리케이션 동작 덕분에 하나의 브로커가 종료되더라도 카프카는 안정성을 유지할 수 있다.
 
 2. 파티션(partition)
  하나의 토픽이 한 번에 처리할 수 있는 한계를 높이기 위해 토픽 하나를 여러 개로 나눠 병령 처리가 가능하게 만든것을 파티션이라고 한다. 이렇게 하나의 여러 개로 나누면 분산 처리도 가능하다.
 
 3. 세그먼트(segment)

* 메시지 전송 방식
 적어도 한번 전송 방식 : 브로커에게 ACK(전송 체크)값을 전송 받을 때 까지 중복해서 보내는 방식
 최대 한번 전송 방식 : 브로커에게 ACK(전송 체크)값을 전송 받지 않아도 브로커에게 프로듀서가 다음메시지를 보내는 방식
 중복없는 전송 방식 : PID와 메시지 번호를 브로커에 기록하여, 기록한 메시지가 있다면 다음메시지를 보내는방식 기록 된 정보는 리플리케이션 로그에 저장. ex) .../kafka-logs/soon-test04-0/...*.snapthot
 정확히 한번 전송 방식 : 
 
# 컨슈머의 내부 동작 원리와 구현

컨슈머의 동작 중 가장 핵심은 바로 오프셋 관리입니다. 컨슈머는 카프카에 저장된 메시지를 꺼내오는 역할을 하기 때문에 컨슈머가 메시지를 어디까지 가져왔는지를 표기하는 것은 매우 중요합니다. 예를 들어 코드 배포로 인슈 컨슈머가 일시적으로 동작을 멈추고 재시작하는 경우나, 컨슈머가 구동 중인 서버에서 문제가 발생해 새로운 컨슈머가 기존 컨슈머의 역할을 대신하는 경우에는 기존 컨슈머의 마지막 메시지 위치부터 새로운 컨슈머가 메시지를 가져올 수 있어야만 장애로부터 빠르게 복구될 수 있습니다.

* 오프셋 : 카프카에서 메시지의 위치를 나타내는 위치
 - 오프셋의 표기는 숫자 형태로 나타나며, 컨슈머 그룹은 자신의 오프셋 정보를 카프카에서 가장 안전한 토픽에 저장합니다.( _consumer_offsets 토픽에 각 컨슈머 그룹별로 오프셋 위치 정보 기록)


# 사용
```
> topic 생성
> /usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server peter-kafka01.foo.bar:9092 --create --topic peter-overview01 --partitions 1 --replication-factor 3  

> producer 메시지 전송
> /usr/local/kafka/bin/kafka-console-producer.sh --bootstrap-server peter-kafka01.foo.bar:9092 --topic peter-overview01  

> consumer 메지시 읽기
> /usr/local/kafka/bin/kafka-console-consumer.sh --bootstrap-server peter-kafka01.foo.bar:9092 --topic peter-overview01
```
 
 
 # 카프카 모니터링
 * JMX(Java Management eXtensions) 
```
> cat /usr/local/kafka/config/jmx
> netstat -ntl | grep 9999
```
 * 프로메테우스 설치(ansible을 활용한 설치)
```
> sudo amazon-linux-extras install -y docker
> sudo service docker start
> sudo usermod -a -G docker ec2-user
> sudo chkconfig docker on
> sudo reboot
재접속
> sudo systemctl status docker
> sudo mkdir -p /etc/prometheus
> sudo cp kafka2/chapter7/prometheus.yml /etc/prometheus/
> sudo docker run -d --network host -p 9090:9090 -v /etc/prometheus/prometheus.yml:/etc/prometheus/prometheus.yml --name prometheus prom/prometheus
```
* 그라파나 설치
```
> sudo docker run -d --network host -p 3000:3000 --name grafana grafana/grafana:7.3.7
```

* 익스포터 설치
```
> 

```
참고자료 : (https://www.tibco.com/ko/reference-center/what-is-apache-kafka)
