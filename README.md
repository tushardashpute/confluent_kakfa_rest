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
1. Download Confluent Kafka and start it
