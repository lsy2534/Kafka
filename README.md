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

# 프로듀서의 기본 파티셔너  

* 프로듀서 API를 사용하면 UniformStickyPartitioner와 RoundRobinPartitioner 2개 파티셔너를 제공한다.
* 카프카 클라이언트 2.5.0 버전에서 파티셔너를 지정하지 않는 경우 UniformStickyPartitioner가 파티셔너로 기본 설정한다.

메시지 키가 있을 경우 동작
- UniformStickyPartitioner 와 RoundRobinPartitioner 둘다 메시지 키가 있을 때는 메시지 키의 해시값과 파티션을 매칭하여 레코드를 전송
- 만약 파티션 개수가 변경될 경우 메시지 키와 파티션 번호 매칭은 깨지게 된다.
- 동일한 메시지 키가 존재하는 레코드는 동일한 파티션 번호에 전달됨.

메시지 키가 없을 경우 동작
메시지 키가 없을 때는 파티션에 최대한 동일하게 분배하는 로직이 들어 있는데
UniformStickyPartitioner는 RoundRobinPartitioner의 단점을 개선하였다는 점이 다르다.

RoundRobinPartitioner
-  ProducerRecord가 들어오는 대로 파티션을 순회하면서 전송
-  어큐뮤레이터에서 묶이는 정도가 적기 때문에 전송 성능이 낮음
UniformStickyPartitioner
- 어큐뮤레이터에서 레코드들이 배치로 묶일 떄까지 기다렸다가 전송
- 배치로 묶일 뿐 결국 파티션을 순회하면서 보내기 때문에 모든 파티션에 분배됨.

프로듀서의 커스텀 파티셔너  
카프카 클라이언트 라이브러리에서는 사용자 지정 파티셔너를 생성하기 위한 Partitioner인터페이스를 제공한다  
Partitioner 인터페이스를 상속받은 사용자 정의 클래스에서 메시지 키 또는 메시지값에 따른 파티션 지정 로직을 적용할수도 있다. 파티셔너를 통해 파티션이 지정된 데이터는 어큐뮬레이터에 버퍼로 쌓인다.  
동일한 키에 대한 특정 파티션으로 보낼수 있다.  

프로듀서 주요 옵션(필수 옵션)  
-bootstrap.servers: 프로듀서가 데이터를 전송할 대상 카프카 클러스터에 속한 브로커의 호스트  
이름 : 포트를 1개 이상 작성한다. 2개 이상 브로커 정보를 입력하여 일부 브로커에 이슈가 발생하더라도 접속하는 데에 이슈가 없도록 성정 가능하다.  
- key.serializer: 레코드의 메시지 키를 직렬화하는 클래스를 지정한다.
- value.serializer : 레코드의 메시지 값을 직렬화하는 클래스를 지정한다.

프로듀서 주요 옵션(선택 옵션)
- acks : 프로듀서가 전송한 데이터가 브로커들에 정상적으로 저장되었는지 전송 성공 여브를 확인하는데 사용하는 옵션이다. 0, 1, -1(all) 중 하나로 성정할 수 있다. 기본값은 1이다.
- linger.ms : 배치를 전송하기 전까지 기다리는 최소 시간이다. 기본값은 0 이다.
- retries : 브로커로부터 에러를 받고 난 뒤 재전송을 시도하는 횟수를 지정한다. 기본값은 2147483647이다.
- max.in.flighrt.requests.per.connection : 한 번에 요청하는 최대 커넥션 개수. 설정된 값만큼 동시에 전달 요청을 수행한다. 기본값은 5이다.
- partitioner.class: 레코드를 파티션에 전송할 떄 적용하는 파티셔너 클래스를 지정한다. 기본값은 org.apache.kafka.clients.producer.internals.DefaultPartitioner이다.
- enable.idempotence : 멱등성 프로듀서로 동작할지 여부를 성정. 기본값은 false
- transactional.id : 프로듀서가 레코드를 전송할 떄 레코드를 트랜잭션 단위로 묶을지 여부를 성정한다. 기본값은 null이다.

# ISR(In-Sync-Recplicas)
ISR은 리더 파티션과 팔로워 파티션이 모두 싱크가 된 상태를 뜻한다.
acks : 카프카 신뢰성있는 메시지 전송의 동작방식을 지정할수 있다.
acks=0 : 리더 파티션으로 데이터를 전송했을 떄 리더 파티션으로 데이터가 저장되었는지 확인하지 않는다.(성능은 좋으나, 신뢰도는 하락) 
- GPS 데이터의 경우 많이 사용, 데이터 전송 속도가 중요한 경우에 사용한다. 네비게이션...
acks=1 : 리더 파티션에만 정상적으로 적재되었는지 확인한다.(성능은 acks=0 보다는 느리나, 신뢰도는 acks=0 보다 조금 높다) 
- 큰장애가 아닐경우 일반 적인 경우 acks =1, -1(all) 옵션을 사용한다.
acks=-1(all) : 리더 파티션과 팔로워 파티션이 디스크에 저장됬는지 확인하고 처리한다.(성능은 낮으나, 신뢰도는 높음)
- 너무 느려 사용하기 힘듬.(하나하나의 데이터를 놓치지않고 사용할떄 설정한다.)
- min.insync.replicas 옵션값에 따라 데이터의 안정성이 달라진다.(여러대의 브로커의 개수를 1개이상의 파티션에 데이터가 적재되었음을 확인하는 옵션)
- acks=-1(all) min.insync.replicas=2부터 설정해야 절대 안정적으로 데이터 유실을 방지할수있다.

#  컨슈머

### 컨슈머

* 프로듀서가 전송한 데이터는 카프카 브로커에 적재된다. 컨슈머는 적대된 데이터를 사용하기 위해 브로커로부터 데이터를 가져와서 필요한 처리를 한다. 예를 들어, 마케팅 문자를 고객에게 보내는 기능이 있다면, 컨슈머는 토픽으로부터 고객 데이터를 가져와서 문자 발송 처리를 하게 된다.

### 컨슈머 내부 구조
- Fetcher : 리더 파티션으로 부터 데코드들을 미리 가져와서 대기.
- poll() : Fetcher에 있는 레코드들을 리턴하는 레코드
- ConsumerRecords : 처리하고자 하는 레코드들의 모음. 오프셋이 포함되어 있음 [오프셋 처리 역확(commit)]

### 컨슈머 그룹
- 컨슈머 그룹으로 운영하는 방법은 컨슈머를 각 컨슈머 그룹으로부터 격리된 환경에서 안전하게 운영할 수 있도록 도와주는 카프카의 독특한 방식이다. 컨슈머 그룹으로 묶인 컨슈머들은 토픽의 1개 이상 파티션들에 할당되어 데이터를 가져갈 수 있다. 컨슈머 그룹으로 묶인 컨슈머가 토픽을 구독해서 데이터를 가져갈떄, 1개의 파티션은 최대 1개의 컨슈머에 할당 가능하다. 그리고 1개 컨슈머는 여러 개의 파티션에 할당 될 수 있다. 이러한 특징으로 컨슈머 그룹의 컨슈머 개수는 가져가고자 하는 토픽의 파티션 개수보다 같거나 작아야한다.

* 컨슈머 그룹의 컨슈머가 파티션 개수가 많을 경우
 - 만약 4개의 컨슈머로 이루어진 컨슈머 그룹으로 3개의 파티션을 가진 토픽에서 데이터를 가져가기 위해 할당하면 1개의 컨슈머는 파티션을 할당받지 못하고 유휴상태로 남게 된다. 파티션을 할당받지 못한 컨슈머는 스레드만 차지하고 실질적인 데이터를 처리를 하지 못하므로 애플리케이션 실행에 있어 불필요한 스레드로 남게 된다.
 
 * 컨슈머 그룹을 활용하는 이유
 - 운영 서버의 주요 리소스인 CPU, 메모리 정보를 수집하는 데이터 파이프라인을 구축한다고 가정해 보자. 실시간 리소스를 시간적으로 확인하기 위해서 데이터를 엘라스틱서치에 저장하고 이와 동시에 대용량 적재를 위해 핟둡에 적재할 것이다. 만약 카프카를 활용한 파이프라인이 아니라면 서버에서 실행되는 리소스 수집 및 전송 에이전트는 수집한 리소스를 엘라스틱서치와 하둡에 적재하기 위해 동기적으로 적재를 요청할 것이다. 이렇게 동기로 실행되는 에이전트는 엘라스틱서치 또는 하둡 둘 중 하나에 장애가 발생한다면, 더는 적재가 불가능할 수 있다.
 
 * 리밸런싱
 - 컨슈머 그룹으로 이루어진 컨슈머 들 중 일부 컨슈머에 장애가 발생하면, 장애가 발생한 컨슈에 할당된 파티션은 장애가 발생 하지 않은 컨슈머에 소유권이 넘어간다. 이러한 과정을 '리밸런싱(rebalancing)'이라고 부른다. 리밸런싱은 크게 두 가지 상황에서 일어나는데, 첫 번째는 컨슈머가 추가되는 상황이고 두번째는 컨슈머가 제외되는 상황이다. 이슈가 발생한 컨슈머를 컨슈머 그룹에서 제외하여, 모든 파티션이 지속적으로 데이터를 처리할 수 있도록 가용성을 높여준다. 리밸런싱은 컨슈머가 데이터를 처리하는 도중에 언제든 발생할 수 있으므로 데이터 처리 중 발생한 리밸런싱에 대응하는 코드를 작성해야한다.
 
 - 리밸런스 리스너 : 할당이 추가 되거나 해제되는 과정을 알수 있는 방식의 코드 템플릿(재할당 방식)
 > 리밸런싱에 대한 대응에 대한 코드가 필요하다.
 
 # 커밋
 
 - 파티션 브로커에서 컨슈머 그룹을 처리할때, 완료했을때 커밋 과정을 나타낸다.( __consumer_offsets)에 기록 된다. 컨슈머 동작 이슈가 발생하여, __consumer_offset 토픽에 어느 레코드 까지 읽어 가ㅣㅆ는지 오프셋 커밋이 기록되지 못했다면, 데이터 처리의 중복이 발생할 수 있다. 그러므로 데이터 처리의 중복이 발생하지 않게 하기 위해 서는 컨슈머 애플리케이션이 오프셋 커밋을 정상적으로 처리했는지 검증해야한다.
 
 
 # Assignor
 - 컨슈머와 파티션 할당 정책이다.
 - 카프카에서는 RangeAssignor, RoundRobinAssignor, StickyAssignor를 제공한다. 카프카 2.5.0 RangeAssignor가 기본 값으로 성정 된다.
 - RangeAssingor : 각 토픽에서 파티션을 숫자로 정렬, 컨슈머를 사전 순서로 정렬하여 할당.
 - RoundRobinAssignor : 모든 파티션을 컨슈머에서 번갈아가면서 할당
 - StickyAssigor : 최대한 팥치션을 균등하게 배분하면서 할당.
 
 # 컨슈머 주요 옵션(필수 옵션)
 - bootstap.servers : 프로듀서가 뎅이터를 전송할 대상 카프카 클러스터에 속한 브로커의 호스트 이름(my-kafka):포트(9092)를 1개 이상 작성한다. 2개 이상 브로커 정보를 입력하여 일부 브로커에 이슈가 발생하더라도 접속하는데에 이슈가 없도록 설정 가능하다.
 - key.deserializer: 레코드의 메시지 키를 역직렬화하는 클래스를 지정한다.
 - value.deserializer : 레코드의 메시지 값을 역직렬화하는 클래스를 지정한다.
 
 # 컨슈머 주요 옵션(선택 옵션)
 - group.id : 컨슈머 그룹 아이디를 지정한다. subscribe() 메소드로 토픽을 구독하여 사용할 때는이 옵션을 필수로 넣어야 한다. 기본값은 null이다.
 - auto.offset.reset: 컨슈머 그룹이 특정 파티션을 읽을 떄 저장된 컨슈머 오프셋이 없는 경우 어느 오프셋부터 읽을지 선탟하는 옵션이다. 이미 컨슈머 오프셋이 있다면 이 옵션값은 무시된다. 기본값은 latest이다.
 - enable.auto.commit : 자동 커밋으로 할지 수동 커밋으로 할지 선택한다 기본값은 true이다(자동)
 - auto.commit.interval.ms : 자동 커밋일 경우 오피셋 커밋 간격을 지정한다. 기본값은 5000(5초) 이다.
 - max.poll.records: poll()메서드를 통해 반환되는 레코드 개수를 지정한다. 기본값은 500이다.
 - session.timeout.ms :컨슈머가 브로커와 연결이 끊기는 최대 시간이다. 기본값은 10000(10)이다.
 - hearbeat.interval.ms : 하트비트를 전송하는 시간 간격이다. 기본값은 3000(3초이다)이다.
 - max.poll.interval.ms : poll() 메서드를 호출하는 간격의 최대 시간. 기본값은 3000000(5분)이다.
 - isolation.level: 트랜잭션 프로듀서가 레[코드를 트랜잭션 단위로 보낼 경우 사용한다.
 
 # auto.offset.reset
 컨슈머 그룹이 특정 파티션을 읽을 때 저장된 컨슈머 오프셋이 없는 경우 어느 오프셋부터 읽을지 선택하는 옵션이다. 이미 컨슈머 오프셋이 있다면 이 옵션값은 무시된다. 이 옵션은 latest, earilest, none 중 1개를 성절할 수 있다.
 - latest : 설정하면 가장 높은(가장 최근에 넣은) 오프셋부터 읽기 시작
 - earliest: 설정하면 가장 낮은(가장 오래된) 오프셋부터 읽기 시작한다.
 - none : 설정하면 컨슈머 그룹이 커밋밋한 기록이 있는지 찾아본다. 만약 커밋 기록이 없으면 오류를 반환하고, 커밋 기록이 있다면 기존 커밋 기록 이후 오프셋부터 읽기 시작한다. 기본값은 latest
 
  # 오토 컴밋
 ``` java
  ...
  configs.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
  ...
 ```
 
 # 동기 오프셋 커밋 컨슈머
- poll() 메소드가 호출된 이후에 commitStnc() 메소드를 호출하여 오프셋 커밋을 명시적으로 수행할 수 있다. commitSync()는 poll()메소드로 받은 가장 마지막 레코드의 오프셋을 기준으로 커밋한다. 동기 오프셋 커밋을 사용할 경우에는 poll() 메서드로 받은 모든 레코드의 처리가 끝난 이후 commitSync() 메서드를 호출해야 한다.
 
 ``` java
 KafkaConsumer<String, String> consumer = KafkaConsumer<>(configs);
 consumer.subscribe(Arrays.asList(TOPIC_NAME));
 
 while(true){
   ConumerRecords<String, String> records = consumer.poll(Duration.ofSeconds(1));
   for ( consumerRecord<String, String> record : records){
    logger.info("record:{}", record);
   }
   comsumer.commitSync();
 }
 ```
 
 # 비동기 오프셋 커밋 컨슈머
 - 동기 오프셋 커밋을 사용할 경우 커밋 응답을 기다리는 동안 데이터 처리가 일시적으로 중단 도기 때문에 더 많은 데이터를 처리하기 위해서 비동기 오프셋 커밋을 사용할 수 있다. 비동기 오프셋 커밋은 commitAsync() 메서드를 호출하여 사용할 수 있다.

``` java
KafkaConumer<String, String> consumer = new kafkaConumer<>(configs);
consumer.subscrib(Arrays.asList(TOPIC_NAME);

while(true){
  ConsumerRecords<String, String> records = consumer.poll(Duration.ofSeconds(1));
  for (ConsumerRecord<String, String> record : records){
    logger.info("record:{}". record);
  }
}
```

# 리밸런스 리스너를 가진 컨슈머
- 리밸런스 발생을 감지하기 위해 카프카 라이브러리는 ConsumerRebalanceListener 인터페이스를 지원한다. ConsumerRebalanceListener 인터페이스로 구현된 클래스는  onPartitionAssigned() 메서드와 onPartitionRevoked() 메서드로 이루어져 있다.

- onPartitionAssigned(): 리밸런스가 끝난 뒤에 파티션이 할당 완료되면 호출되는 메서드이다.
- onPartitionRevoked() : 리밸런스가 시작되기 직전에 호출되는 메서드 이다. 마지막으로 처리한 레코드를 기준으로 커밋을 하기 위해서는 리밸런스가 시작하기 직전에 커밋을 하면 되므로 onPartitionRevoked()메서드에 커밋을 구현하여 처리할수 있다.

 
# 파티션 할당 컨슈머

``` java 

      ...
        Properties configs = new Properties();
        configs.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, BOOTSTRAP_SERVERS);
        configs.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        configs.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());

        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(configs);
        consumer.assign(Collections.singleton(new TopicPartition(TOPIC_NAME, PARTITION_NUMBER)));
      ...
```
 
 
 
1.참고자료(자료다운로드) : (https://github.com/bjpublic/apache-kafka-with-java)
2.참고자료 : (https://www.tibco.com/ko/reference-center/what-is-apache-kafka)
