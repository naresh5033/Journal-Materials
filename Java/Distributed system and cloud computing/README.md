# Distributed system and cloud computing

- as by the definition the distributed system is a several process running on the different systems, communicating with each other through the network and sharing the state or are working together to achieve the common goal.

- as we know when we run the app the cpu will ve the instance of the app in its memory that is called process, and that process is entirely isolated on any other process that running on the s/m.

### 2. Cluster coordination service and Distributed algorithm

- **Theory of Leader Election** - (Apache Zookeeper) - the goal is to create an algorithm for the leader who can watch the nodes (automatic leader election).
- if the master node is unavailable then the remaining nodes will find a new leader, and once the leader recovers he will take back his role (joining of the old leader after the recovery)
- the apache zookeeper provider an abstraction layer for the higher level distributed algorithm, (Apache Zookeeper has been used by many popular entities such as kafka, hadoop, HBase etc..)
- in the zookeeper, it has tree like structure which have znodes (hybrid b/w the files and directory.) there are 2 types of znodes 1. persistent(stays in session) and 2. ephemeral (deleted when the session ends)
- **leader election algo** - in the step each node will candidate for the leader, the zookeeper maintains a global order, it will name each znode according to the addition order.
- finally if the znode created is the smallest number it knows it is now the leader.
- **zookeeper client and server config** - download the zookeeper and in the bin dir run the shell file`./zkServer.sh` and to start the server `./zkServer.sh start` and lly run the client (cli) `./zkCli.sh 
  - and for the cmds (can use help to see) some of the commands are `rmr /parents or create election"" or get election`
- **zookeeper client threading model and zookeeper java API** - app start code in the main method is exec on the main thread and then it will add then when the zookeeper object is created additional 2 threads will be created 1. the Event thread and 2. IO thread
- 1. the io thread for the n/w communication, session management, handles req / res and the 2. Event thread handles all the zookeeper's state changing events such as connection and the disconnection from the zookeeper server, and also handles all the znode watchers and the events that we subscribed to. and the events are executed in the order.
- **Watchers, Triggers and failure detection algo** 
  - 1. The watcher - we can register a watcher when we call the getChild(), getData(),exits(), the watcher allow us to get the notifications when the changes happen.
  - with the watcher and the triggers we can use em in the failure for the failure detection.
  - **The Herd Effect** is when the large no. of nodes waiting for an event. when that event happens they all get notify and wakes up, but only one node can succeed and this indicates the bad design and negatively impact the performance and freezes the cluster.
  - so the approach here to solve this is when after the initial leader election, (instead of all the nodes watching the leader znode) each node will watch the znode right before it in the sequence of candidate election znodes.
  - **Leader ReElection Algorithm** - now we ve the leader and every time it gets the large computation talks, it will split em in to chunks and give to the other nodes in the cluster. then the aggregated results will be collect by the leader and sends to the other cluster down the pipeline.
  - Refer the zip file leader election.zip and also the practical cluster auto healing mechanism Auto healer solution.zip

### section 2. Cluster management, Registry and Discovery

- Refer the notes service registry and discovery.zip file
- **Service Registry and service Discovery** as we know there are many 3rd party libs out there for these tasks. such as Netflix Eureka, etcd, consul but zookeeper is most widely used one. 
- service Discovery - is like put all the config of the nodes in the cluster and then distribute the table to the other nodes in the cluster and this is called static configuration. but the problem with this static config is if one node is unavailable or changes the ip all the other nodes will not know and still uses the same static config file.
  - and also if we ve to extend our cluster we ve to regenerate a new config file and distribute to all the nodes.
- this could be addressed by the automation config tools like chef or puppet where we update in the one place / central config which will replicate in all the nodes in our cluster. this is more dynamic but still involves human config.
- The approach we re gon to make is to impl a fully automated service with the zookeeper. **service registry** is like the central node which will ve all the ip of the nodes that are running in our cluster, 
- and the **service Discovery** is easy to impl each node in the cluster wants to communicate with the other node, simply register the watcher on the service registry znode, that way if any node get added it will get notify by using the getChildren(), then if it wants to read / use any particular node it will use the getData(). and if any change in the cluster at any point the node will get notify by the **NodeChildrenChanged** event

### section 3. Network communication

- Refer the httpclient.zip file
- multi threading vs distributed s/m - in the multi threading passing the msg from the one thread to the other thread is very easy task, since all our threads runs in the ctx of our app sharing the memory.
- but in distributed s/m its very different as we don't ve the shared memory, so the only way for our nodes to communicate is thru the n/w.
- and all this happens in the Tcp / ip model with the Data link, internet, transport and application layers and their relevant protocols.
- as we know in the transport layer there are 2 protocols 1. UDP (user Datagram) which is connectionless, unreliable (means the msgs can be lost / duplicated / re-ordered), in distributed s/m udp is preferred only when the speed and simplicity is more important that the reliability (an use case for the UDP is REal time video / audio streaming service, or online games, broadcasting)  and 2. TCP (Transmission Control Protocol), in contrary to the udp the tcp is more reliable and secure.
  - unlike udp , tcp establishes connection b/w 2 points before sending any data.
- **complex data delivery** Serialization and Deserialization as we know there are many ways / tools to do this 1. JSON 2. java object serialization 3. Google's proto Buff..
  - Java object serialization - while sending the files, / DS all the class and types serializable except for the **Static and Transient members**. and the conditions are 1. the class needs to match the original definition 2. The class has to access the no args constructor. if any of these rules are violated then the invalid Class exception during the Deserialization process.
  - the advantage of this java object serialization is guarantees about the correct state reconstruction w/o any type ambiguity (unlike the json where we ve type ambiguity) 2. and the second advantage is now we ve the clear source of truth(schema) for the object we want to send over the n/w.
  - 3. and finally native support for the all JVM languages (no need any external lib)
  - the disadvantages are 1. not human readable (harder to test) 2. tightly coupled with the jvm languages, (which is a big problem if we use diff lang)
- so that leads to the 3rd option which is **Proto Buffer** - which are lang independent, small and fast(for testing.)
- as we know in this we define our msgs in the .proto file and then use the proto compiler protoc which generates the class file for any lang we want.
- in summary when we use json (which is not secure)the msg will send plain text and  anyone can tamper our msg. and in the java object serialization it sends in a binary format it will contain all the info about the class including the type of each field and what class it inherits from (so anyone can reconstruct it w/o too much effort). Finally in the proto buffer which only contains the tag number and their encoded vals, this way only someone who has the proto definition or its generated stub can deserialize it and read the msg by mapping the field tags to the corresponding types.

### Building distributed Document search
- Refer the code distributed-search.zip file
- **Introduction to TF-IDF** - lets ve  the scenario where we ve large number of documents (books, research papers) and then the user will search Query for it and based on the search query we will get the most relevant document to the user.
- its a searching algorithm Term Frequency - Inverse Document Frequency(tf-idf) - this algorithm measures how much information each term in the search query provide us. common terms that appears in many documents do not provide any value and get lower weight, and the terms that are more rare in the documents gets higher weight.
- the idea is to calculate each term frequency in the doc before and multiplies by each term frequency by the inverse document frequency of that term across the document... idf(term) = log(N/nt); N - no.of documents; nt - no.of documents containing the term
  - ex - idf("the") = log(10/10) = log(1) = 0; now the term "the" appears in all the documents, the algo weights down its term frequency to 0. 
- Notes : the tf-idf is a statistical algorithm, to produce a meaningful result it requires to have a larger document.
- since this algorithm is highly parallelizable it's a good fit for our distributed system.
- now the zookeeper leader is the coordinator for our search algorithm. when the worker nodes are done with the task, the coordinator will aggregate the result and sends the res to the user search query  

### Load Balancing 

- as the no.of req comes in more, the coordinator will ve a problem in handling the large number of requests, so we ve to think of using the load balancer (this leads to the performance bottleneck)
- as we know the load balancer distributes the n/w traffic across the cluster of app servers. And prevents any single server from the performance bottleneck. Also Monitors app server's health, lb makes our s/m more reliable and HA.
  - in the cloud environment, where the servers are added / scaling on demand, LB can provide the auto scaling up / down capabilities
- there are 2 types of LB 1. H/W and 2. S/W Lb(ex - HA proxy, Nginx)
- **Strategies and algorithm** - the easiest strategies that lb uses to redirect our traffic is by using the 1. **Round Robin** 2. Weighted Round Robin 3. Source IP hash Motivation(sometimes it's desired that the user continues communicating with the same server ex - shopping cart, local server caching, open session ) 4. Least Connection Strategy 5. weighted Response 6. Agent based policy
- the best Lb is different for each distributed s/m. it depends on the different s/m architecture work load, usage pattern
- we will be using HA Proxy. this is very reliable and high performance TCP/Http lb. it's a free /open source s/w lb that powers many Distributed system web apps, websites. easy to set up. 
  - and to run the HA Proxy just define the config file run it `haproxy -f haproxy.conf` and to see the admin page localhost:83 
  - and using the ACL we can inspect the incoming req and classified based on its content dynamically routed to the separated server in our cluster.
  - since the HA proxy supports linux distro, we could use Docker to spin up

### Distributed Message Brokers  (Kafka)

- **Intro to message Brokers** 1. synchronous n/w communication - as we know msg broker is a intermediary(mw) that passes the msg b/w the sender and the receiver. Also it will provide additional functionality like queuing, validation, routing, data transformation and routing.
  - and it provides the full decoupling b/w the sender and the receiver.
- using the pub / sub strategy the sender can publish the msg / topic to the broker and the broker will broadcast the msg to the receivers / subscribers.
- to avoid the single point of failure / bottleneck the msg broker itself need to be scalable.
- kafka arch - as we know the msg we wanna publish has the record - which has the Kwy, value(our msg) and the timestamp, and the topic is like the category of the msg or the event..
  - we can think of topic as the collection of queues and the partition as the separate independent queue of records, each partition maintains the msgs in the order they were published. 
  - the hash fn will apply on the key (of the record) that determines where the records will go into the partition
- FT in kafka - each kafka topic is configured with the replication factor and this will be replicated / shared across all the brokers in the cluster so even if any of the broker goes  down the others will still ve the copy of the partition.
  - for each partition, only one broker act as the partition leader and the others are the partition followers, the leader does the read and the writes. the follower replicate the partition data to stay in sync with the leader.
  - for ex - if the topic with 4 partitions managed by the 4 brokers with the replication factor of 2 each partition will be replicated to the 2 brokers. The higher the **Replication factor** the more Failures our s/m can handle, and more replication means more mem / space is taken just for the redundancy.
  - under the hood kafka is using the **Zookeeper for the leadership** and the coordination logic. it uses zookeeper as the registry for the broker as well as for the monitoring and the failure detection (ephemeral znodes and watcher etc)
  - Refer the ex - file distributed banking solution.zip file. (follow the below steps)
  - Part 1 - Launching Kafka
# Instructions on Starting Kafka to our computer we will start with configuring Apache Zookeeper:

1. Create zookeeper-logs directory :

mkdir zookeeper-logs

2. Open config/zookeeper.properties and modify dataDir to point to zookeeper-logs (full path)

3. Run Zookeeper

Unix: /bin/zookeeper-server-start.sh kafka/config/zookeeper.properties

Windows: /bin/windows/zookeeper-server-start.bat config/zookeeper.properties

4. Create 3 directories, one for each Kafka broker logs:

mkdir kafka-logs-0

mkdir kafka-logs-1

mkdir kafka-logs-2

Each directory will contain the corresponding Kafka broker's logs.

5. Create 2 copies of  config/server.properties and name them config/server-1.properties and config/server-2.properties. Rename the original config/server.properties to config/server-0.properties.

6. Change each configuration file's log.dirs to point to the corresponding kafka-logs-X directory:

In config/server-0.properties change log.dirs  to point to kafka-logs-0

In config/server-1.properties change log.dirs  to point to kafka-logs-1

In config/server-2.properties change log.dirs  to point to kafka-logs-2

7. In server-1.properties uncomment the #listeners=PLAINTEXT://:9092 and change the port to 9093

listeners=PLAINTEXT://:9093

8. In server-2.properties uncomment the #listeners=PLAINTEXT://:9092 and change the port to 9094

listeners=PLAINTEXT://:9094



Now our all our 3 Kafka broker's configuration files are ready so we go ahead to run the brokers

9. Run 3 Kafka brokers pointing to the corresponding properties files:

Unix:

bin/kafka-server-start.sh config/server-0.properties &

bin/kafka-server-start.sh config/server-1.properties &

bin/kafka-server-start.sh config/server-2.properties &

Windows:

bin/windows/kafka-server-start.bat config/server-0.properties &

bin/windows/kafka-server-start.bat config/server-1.properties &

bin/windows/kafka-server-start.bat config/server-2.properties &

Now that our Kafka cluster is running, let's create the Kafka topics:

10. Create valid-transactions Kafka topic:

Unix:

bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 3 --topic valid-transactions

Windows:

bin/windows/kafka-topics.bat --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 3 --topic valid-transactions

11. Create suspicious-transactions Kafka topic:

Unix:

bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 2 --topic suspicious-transactions

Windows:

bin/windows/kafka-topics.bat --create --bootstrap-server localhost:9092 --replication-factor 3 --partitions 2 --topic suspicious-transactions



We can check that our topics where indeed created by running:

Unix:

bin/kafka-topics.sh --list --bootstrap-server localhost:9092

Windows:

bin/windows/kafka-topics.bat --list --bootstrap-server localhost:9092



Part 2 - Launching Kafka
Please see the attached banking-system-solution.zip for the implementation of the Banking API Service, Account Manager Service, User Notification Service and Reporting Service.

### 8. Distributed Storage & DB

- **Data Sharding** - Dynamic Cluster Resizing, for ex if we ve the sharding no.of 3 and then if we decide to increase a cluster size now its 4 nodes, now the sharded records ve to move to the right location accordingly, and lly the same strategy has to happen when the node is removed from the cluster.
- **Consistent Hashing** algo node allocation, the idea is not only to hash the keys but also the nodes, and hash em all to the same hash space.
- lets see some of the architecture 1. master / slave arch
- **Master / Slave Arch,** in this all the writes op are done by the master and all the read op are done by the slave.
  - every write op to the master is propagated to the slave so the slaves will ve the copy of the data. and if the master fails the slave will take over the master role.
- **Master / Master Arch** lly the master and master arch, where every node can perform read operations and the write operations.
  - some of the Drawbacks with these master / slave arch is Data inconsistency.
- **Quorum Consensus** provides strict consistency,in this each record in the db (along with the key and data) now also have the version. And every update increase the version number, by doing this way we can easily distinguish b/w the newer record and the older record.
  - the formula here is - R + W > N; where R - no .of reader nodes , w - writer nodes, N no .of nodes in the db cluster.

### 9. Scaling Mongo DB

- 

----------------------------------------------------------------