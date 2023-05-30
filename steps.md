### Steps to follow

#### Create an EC2 instance
1. Login to AWS accout. Go to Console Home --> Select EC2 --> Click on "Instances running" --> Click on "Launch Instances".
2. Give it a Name, Keep everything as Default.
3. Click on "Create a new pair", create a pair. Download the key pair on local system and store it into a folder.
4. Keep everything as it and then press Launch Instances.

#### Connect to EC2 instance
1. Click on the "Instance ID" --> click on the "connect" --> copy SSH command.
2. In local system, go to the key-value pair folder. Open a terminal and paste the SSH command.

#### On EC2 instance from terminal of local system.
1. install kafka on EC2 instance
```
wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz
```
2. install java on EC2 instance
Check whether Java is installed.
```
java -version
```
If not, install Java using following command. 
```
sudo yum install java-1.8.0-openjdk
```
3. Go to kafka folder
```
cd kafka_2.12-3.3.1
```
4. Start zookeeper
```
bin/zookeeper-server-start.sh config/zookeeper.properties
```
##### Open an another terminal to start Kafka server. But first ssh to to your ec2 machine as done above. (2nd window)
5. Start Kafka server
```
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
```
After allocating some memory to Kafka
```
cd kafka_2.12-3.3.1
bin/kafka-server-start.sh config/server.properties
```
6. To access the Kafka server of EC2 instance from local system
It is pointing to private server, change server.properties so that it can run in public IP.
    * Close both zookeeper and kafka server using ctrl+c.
    * Open the server.properties
      ```
      sudo nano config/server.properties 
      ```
    * change ADVERTISED_LISTENERS to public ip of the EC2 instance.
    * Start both zookeeper and kafka server in respective terminal.
7. To allow access from local system, Click on the "security" tab of EC2 instance. Click on security groups --> select "Edit inbound rules". Add rule to allow all traffic and Anywhere-IPv4. Save the rule.

##### Open an another terminal to start Kafka server. But first ssh to to your ec2 machine as done above. (3rd window)
8. Create a topic
```
cd kafka_2.12-3.3.1
bin/kafka-topics.sh --create --topic demo_testing2 --bootstrap-server {Put the Public IP of your EC2 Instance:9092} --replication-factor 1 --partitions 1
```
9. Start producer
```
bin/kafka-console-producer.sh --topic demo_testing2 --bootstrap-server {Put the Public IP of your EC2 Instance:9092} 
```
##### Open an another terminal to start Kafka server. But first ssh to to your ec2 machine as done above. (4th window)
10. Start consumer
```
cd kafka_2.12-3.3.1
bin/kafka-console-consumer.sh --topic demo_testing2 --bootstrap-server {Put the Public IP of your EC2 Instance:9092}
```
11.  
12. 
