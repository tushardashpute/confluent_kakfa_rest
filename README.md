# confluent_kakfa_rest

Steps
=====
**1. Install Java 8 or above**

https://docs.aws.amazon.com/corretto/latest/corretto-21-ug/amazon-linux-install.html

**2. START THE KAFKA ENVIRONMENT**

 [Download](https://dlcdn.apache.org/kafka/) the latest Kafka release and extract it: 

      $ tar -xzf kafka_2.13-3.6.1.tgz
      $ cd kafka_2.13-3.6.1

 **a. Start Zookeper**

Add server.0 configuration in config/zookeper.properties. Replace the below DNS with your ec2 public dns.

     # zookeeper.properties
     server.0=ec2-3-133-7-49.us-east-2.compute.amazonaws.com:2888:3888

      # Start the ZooKeeper service
      bin/zookeeper-server-start.sh config/zookeeper.properties
   
   **b. start kafka server**

      # server.properties
      listeners = PLAINTEXT://ec2-3-133-7-49.us-east-2.compute.amazonaws.com:9092
      advertised.listeners=PLAINTEXT://ec2-3-133-7-49.us-east-2.compute.amazonaws.com:9092
      zookeeper.connect=ec2-3-133-7-49.us-east-2.compute.amazonaws.com:2181

     # Start the Kafka broker service
     bin/kafka-server-start.sh config/server.properties

**3. CREATE A TOPIC TO STORE YOUR EVENTS**

Kafka is a distributed event streaming platform that lets you read, write, store, and process events (also called records or messages in the documentation) across many machines.

    #Create a new topic
    $ bin/kafka-topics.sh --create --topic quickstart-events --bootstrap-server 3.133.7.49:9092

    #Describe the topic which you created
    $ bin/kafka-topics.sh --describe --topic quickstart-events --bootstrap-server 3.133.7.49:9092

**4. WRITE SOME EVENTS INTO THE TOPIC**

    $ bin/kafka-console-producer.sh --topic quickstart-events --bootstrap-server 3.133.7.49:9092

**5. READ THE EVENTS**

    $ bin/kafka-console-consumer.sh --topic quickstart-events --from-beginning --bootstrap-server 3.133.7.49:9092

 
 Now if you want to send data to kafka topics using curl/postman you can do it with Apache Kafka,
 for that you need to install either confluent Kafka or on top of existing kafka you can install Confluent Rest.


Steps to add confluent kafka rest:
===================================
1. [Download](https://docs.confluent.io/platform/current/installation/installing_cp/zip-tar.html) Confluent Kafka and start it

2. /home/ec2-user/confluent-7.5.2/bin/kafka-rest-start /home/ec2-user/confluent-7.5.2/etc/kafka-rest/kafka-rest.properties 

Update the kafka-rest.properties with below settings:

     id=kafka-rest-test-server
     listeners=http://0.0.0.0:8082
     bootstrap.servers=PLAINTEXT://ec2-3-133-7-49.us-east-2.compute.amazonaws.com:9092

Create API Gateway to send Kakfa messages
========================================
1. Create a API Gateway

API gayeway --> Create API --> Rest API --> Build

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/19fbcc4d-6d8e-4404-93d1-784dbf5c28c8)

Create Resource

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/cacec871-ac72-4860-9e15-1fbce2029ff7)

create method

in the Endpoint URL specify the complete URL path of kafka rest : http://ec2-3-144-225-186.us-east-2.compute.amazonaws.com:8082/topics/{proxy}

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/f92b4c67-6c33-4e71-9655-dc2823f577c3)

Click on create method.

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/0376a665-0476-4f43-b5f2-36218e92bff1)

Now click on Deploy API

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/969f086c-e316-4313-8a6c-38a5243418f6)

now copy the invoke URL of API and POST the data to Kakfa using postman. 

**Set the 'Content-Type: application/vnd.kafka.json.v2+json' in the headers.**

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/1c62bcb3-561a-4ff1-b1cd-c0a839952fb4)

![Uploading image.png…]()

If you want you can post the message to Kafka using curl command also:

    curl --location 'https://y8nvflwi5f.execute-api.us-east-2.amazonaws.com/dev/quickstart-events' \
    --header 'Content-Type: application/vnd.kafka.json.v2+json' \
    --data '{
        "records": [
            {
                "value": {
                    "deviceid": "AppleWatch4",
                    "heartrate": "72",
                    "timestamp": "2019-10-07 12:46:13"
                }
            }
        ]
    }'

I have Kept KAFKA consumer open in another terminal, that we can verify message received at consumer end or not:

![image](https://github.com/tushardashpute/confluent_kakfa_rest/assets/74225291/5ed30d3b-f2ed-428b-ac9c-ec06f97fadd5)


References:
=
https://aws.amazon.com/blogs/big-data/govern-how-your-clients-interact-with-apache-kafka-using-api-gateway/
https://kafka.apache.org/quickstart


