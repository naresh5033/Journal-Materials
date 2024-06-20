#DDD (chris Richard)

### Aggregate

- as we know the Entity, Value obj, Repository, service are the essential and The Aggregate is more important than all of that.
- **Aggregate** is the cluster of objs that can be treated as a unit.
- kind of taking the domain model and breaking it into chunks and that can be treated as a units.
- sort of like a Graph consisting of a root entity and one or more other entities and value objects ref by the root either by directly or indirectly.

1. in other words aggregates lets us partition our object model across micro services
2. Transaction = Processing one command by one aggregate.

- how to maintain the data consistency between aggregates ?
  - by using the **Events** - use the Event driven, eventually consistent order processing, whenever it updates one of its aggregates.
  - so by subscribing and consuming the state changes in our app, that's how we maintain the data consistency in the microservice architecture, we're abandoning the ACID model and adapt to eventual consistency (aka BASE - Basically Available Soft State Eventually consistent model)
    - there is a problem with this where in ACID the rollback is easier and where as in BASE its difficult.
  - dual write problem - for every changes in the order service it has to write in the DB and also publish the event so its basically a 2 write, and what if the event fails or broker down
  - 2PC(phase commit) - is not an option .. there are several different strategies we can use ex
  - the Ebay uses Application Events - which uses table for the Events for the msg queue .
  - LInkedIn use Transactional log tailing and then commit into the event sourcing / pub the event based on that

### Event Sourcing

- is the Event Centric WAy of persisting all our data. Events are the s/m of records in our Architecture.
- with this for every single changes we're just pub a event in the **Event Store** (db + msg broker).
- so now the events are becomming the first class citizens in our Domain model.
  - and now we'll be having the event table and maintaing all the events.
- Replay the events to recreate the state and periodically snapshot to avoid loading all the event from the beginning
- now if the req comes in we will find the events(Entity id) for the req (ex- order)
- from the event store the events will be published for the customers(subcribers), the subscribers could update the view called **CQRS** View (command Query Responsibility Segregation), so the view would be kept upto date by subscribing to the events.
- Now these events could be consumed by the ML algorithms and turned into notifications and sent out to the users. This eleminates the OR mapping, since we re no longer saving the Domain objects instead we're saving the Events which are easier to serialize.
- so we'll ve built in 100% audit logs and temporal queries(so we can go back in time to see all the state changes happen in the domain obj).

- **Drawbacks of event store** -

1. must detect and ignore the duplicate events.
2. Track the most recent events and ignore the older ones.
3. querying the event store can be challenging.

- so to mitigate this we ve to use **CQRS**
- And we can use some of the commonly used event projection libs(Libraries can help automate the process of transforming event streams into a format suitable for querying) in the spring boot and .net asp environment
  edit
- such as for spring - 1. Axon Server Event Store and Projections 2. Eventuate Tactician: 3. Spring Data Elasticsearch
- for .Net 1. EventStoreDB and EventStore.Projections 2. NEventStore 3. MassTransit with Sagas.

- Both Spring Boot and ASP.NET offer some level of built-in support for CQRS patterns. Explore the available options before diving into third-party libraries.
- Remember, event projection libraries are tools to help manage the complexity of CQRS and event sourcing. It's still essential to understand these concepts for effective implementation.

- The commands which represents the req and they don't do the state change they return the events which represent the state change
- There is also the apply method which takes the events and apply the state change

- now the save() concisely specifies 1. Create Customer Aggregate 2. Processess command 3. Applies Event 4. Persist Event

**summary** - The aggregates are the building blocks of the DDD / Micro services - use the events to maintain the consistency b/w the aggregates - Event sourcing is the good way to implement the event driven architecture

---

### Event sourcing (Gregg Young CQRS)

- the diff between the event sourcing and the traditional log is that the transactional log tells the state transition but doesn't tell us why it occurred
- where as the event provides (even contains the metadata that we added) why it occurred
- Events are stored per aggregate (unique id generally by the aggregate root) aka **Stream** per aggregate.

- Modelling events forces behavioral focus as oppossed to think about the structure of the inside s/m
- There are some takeaway from his talk, the value objs are mutable, we can ve a same read and write queries (its not always ve to be separated, ex - millions of record),
  - the i/p = o/p -> if we ve an place order cmd then we should ve an order placed event at the other end. Its not necessarily true, ex - in a stock exchange, if we place an if we place an cmd then the order occurred on the other end.
    - and more over its not one to one if we place order then it will be like 3 trade occurences and trade cancelled
- **One way commands** - the command that we send is like the fire and forget command. its not true, there is no such thing as the one way command, they don't exist. (ex - in a atm scenario, where we withdraw the money it gets queued and processed it later)
- **Frameworks** - as per him don't write cqrs frameworks, event if we try to write one it will be abandoned with in 1 yr just like the every other one.
- **Lack of process mgrs (aka another actor)** - as the services are tied(sub) to the event on the other services. so if there's a problem then we ve to go thru all the bits of code and find what is going on (the overall processess).
- the process mgr is difficult, how do we test the actors ? its a trivial we ve msgs in and msgs out..
- we can linearize the process, 90% of the s/ms we make them linearize which is gon be cheaper for us.

  - 3 reasons we don't want to linearize 1. occasional connectivity 2. favor availability (preferred over consistency) 3. s/ms with very high throughput

- <h1>Projection</h1>
- is a pattern in the event sourcing.
- lets have an eg in the product inventory s/m, and how do we build the ui that shows the current state of our inventory and the qty in our state.
- the projection is just the transformation of the event stream into a model that we actually care about, that we want to use in the ui or we need to report on.
- this transforming data (even in the sql) into one form to the other is called projection but here we're doing in the event stream.
- And the cool thing about that is if we ve an event stream and we have recorded all the event facts and we can turn that event stream into variety of different models. variety of different ways to look at it. which is really invaluable
- the publisher publishes the event and the consumers are gon to consume the event and project the events, they will take the event and update the current state that they ve in their respective db, we re basically keeping em up with the real time
- so we're kind of updating the current state as the event happens we're not replaying everything every time.
- so if we're creating the projections within the boundary we can use the event store directly.
- The other option is what is called the catch up subscription - here the consumers are going to point inwards to the event store, since the consumers are going to connect to the event store

- **Note:** - There are different types of event consistency,

  - 1. Linearized Event consistency(in the event sourcing and we ve the read model) - where we're just representing the same linearized time line and the time line comes forward and it is immutable,

- **types from chatgpt**
- **Linearizability (Strong Consistency**): Strongest guarantee, operations appear instantaneous.
  **• Sequential Consistency:** Operations appear in a sequential order, not necessarily real-time order.
  **• Causal Consistency:** Causally related operations are seen in the same order by all nodes.
  **Eventual Consistency**: All replicas eventually converge to the same value, but temporary inconsistencies are allowed.
  **Strong Eventual Consistency** (SEC): Ensures convergence in the presence of concurrent updates.
  **Read-Your-Writes** Consistency: Ensures a process sees its own writes.
  **•Monotonic Reads Consistency**: Ensures reads are non-decreasing.
  **•Monotonic Writes Consistency**: Ensures writes are seen in order.
  **•Session Consistency:** Provides consistency guarantees within a session.
  **•Prefix Consistency:** Reads reflect a prefix of the writes.

These consistency models offer different trade-offs between consistency and performance, and the choice of model depends on the specific requirements of the distributed system or application.

- Convergence in the context of consistency models ensures that all replicas of a data item in a distributed system will eventually reach the same state, given no new updates are made.
- Convergence mechanisms help resolve conflicts and ensure all updates are applied correctly across all replicas.

- **Note:** - By maintaining high cohesion and low coupling, you can create a more maintainable, scalable, and robust system that aligns with the principles of DDD and supports various consistency models effectively.

### Bootiful CQRS Event sourcing with axon

- (the sql query can be freaking tedious in a real world query like 22 joins and 6 subqueries.)
- The Axon framework does is it just separates the cmd component from the query component.
- the command component has the the query / business logic to change the state. and the query side is responsible for handling the req to the state changes.
- The query loses the val as soon as it gets the res and the cmd loses its value as soon as it triggers the side effects. Where as the Events retain the value.
- the event driven architecture is just storing the events where as the event sourcing is using those events as the main "source of truth".
- they use command bus query bus and msg/ event bus to dispatch the cmd, query and the msgs b/w the components.
  - Theres also a axon server that will allow us to store the events for the later use.
  - **Location transparency** - the 2 comps that are communicating do not care about the location.
- **@CommandHandler** annotation to handle the command ex the handle() is the cmd and the apply() is the side effect, and some other annotations are @EventSourcingHandler (and when we re using the event sourcing we ve to split the decission making from the state changes )
- and when the event is published we need to update the view model.
- the talker uses the bike rental model for this eg - after he created and returned the bike in the axon server localhost:8024/#overview -> we can see the comp one for the cmd, query and server connected to the db
- And the Axon does support for the sagas
-

### SAGAs

- how do microservices do the transactions? ie where the SAGA pattern comes in, it provides the way for the microservices to communicate in a transactional like.
- there are 2 ways to implement the saga **1. orchestration** - in this we ve the central service that will take care of calling all the different services in a right order and make sure if there is a failure it knows how to **compensate** or how to do the compensation action b/w the services.
- so we can also see the slide, the service will make the req and get the res from the other service only in case of failure it will compensate, (revert back all the actions)

- 2. **Choreography** is the another way of implementing the saga, in the choreography we ve the central service or the orchestrator will make sure that all the things are connected together and the communication thru the services will happen via events.
  - ex - from the order service it will go like product ordered, payment product purchased if its failed and it will revert back to order service(purchase failed) and nothing else will get executed since no one is consuming the event, the only services that sub to the purchase failed will get to reacted for this.
- so the key difference between the orchestration and the choreography is in the orchestration we re basically expecting a res, we're calling a service and expecting a response b4 calling the next service.
  - this can be optimized by using the Asynchronous communication.
- Where as in the choreography we're not expecting a response, we re just sending a event and some one is interested in that event and takes action on the event and if any of the service will need to the compensation it will read from the event and execute it.

  - which is closer to the event driven architecture.
  - this is why the choreography saga pattern is the recommended way to use and will ve the simpler workflows as opposed to the the complex workflow in the orchestrator(then we can prefer this).

- **NService Bus**
  - Long running business process

1.  Choreography - a. triggered by the msgs/events. //
    b. decentralized logic //
    c. Low coupling (purely reactive). //
    d. difficult to visualize the workflow.//
2.  Orchestration - we do ve the centralized place, where we define what the workflow for the business process is
    a. Triggered by the msgs/ events.//
    b. Centralized logic. //
    c. Higher coupling (event to command). //
    d. visualize workflow. //

- [https://github.com/dcomartin/LooselyCoupledMonolith/tree/NServiceBus](refer the git for the code)
- one thing to keep in mind with the orchestration is that the "concept of compensating the Actions" - if in any case there is failure then we need to ve compensating the action that undo or negate the previously succeeded action in the workflow

- NService Bus in the saga is maintains the state - ex this time we can ve an hybrid or "**policy**" saying that when the order is build and the order is placed and that when we gon create the shipping label.

### Using sagas to maintain the data consistency in the MS architecture(chris richardson)

- why the 2PC(phase Commit) is not and option ? - and sagas as the transactional model.
- As we know the ACID is not an option when using amongst the services (maybe good when using in a sevice)
- That's when the sagas comes into play
- one of the key / important business policy is that the **invariant** can never be violated. and if we use the concurrent transaction for the same customer will be serialized so the property of the ACID tx will guarantee that the invariant will never be violated.

- **invariant** - an invariant is a condition or rule that must always hold true for a system to be in a consistent state. Invariants are crucial for ensuring the integrity and correctness of data within a system.
- as we split up the monolith into small MS how do we maintain the data consistency (as we split up the dbs)
- Use SAgas instead of 2PC
- the whole idea of saga is break up the distributed transaction into a set or seq of local transaction
- **Rollbacks using compensating txs** - ACID transaction can simply roll back.
  - where as in the eventual consistency model the devs has to write the complex logic for the rollback. And careful design required.
- SAGAs complicated API design - The req initiates the saga, when to send back the res ?
  - option1. send the res when the saga completes... Res specifies the outcome.. Reduced availability.
  - The downside of this is, it kind of reduces the availability, in order for the res to send back all of the participants need to be up, especially the service that sitting there and waiting for this to be complete b4 it can send back the http res.
- Option 2. - Send res immediately back to the client (recommended)
  - here its improved availability
  - res doesn't need to specify the outcome, client must poll or need to be notified.
  - the complication is that we re telling the client in this case the order has been accepted, and we really cant tell u we re gon to allow the order or not. (ie y the client has to poll to see the order status or need to be notified )
  - and we can hide this delay in the ui with some popups (minimal impact on the ui)
  - The server can use the websocket to push the notification into the ui.
  - The other issue is that the business logic is bit more complicated (since changes are commited at each step of the saga)
  - means the other txs will see the data in the inconsistent state ex - order.state = PENDING .. means more complex logic
  - then the interaction between the saga and the other ops ex - what does it means to cancel the PENDING order ?
    - "INterrupt" the create order saga and wait for the create order saga to complete.
- How to Sequence the saga txs ?
  - After the tx of Ti something must decide what step to execute next 1. Success Which T(i +1) - branching. 2. Failure: C(i - 1)
- again it brought down to the 1. Choreography and the 2. Orchestration Model / decision making/model
- **Orchestrator** based coordination tells the participants what to do.. so this way the services/comps are no longer tied with the sagas
  - so the saga orchestrator are the state machine (classic uml state machine)
- **Implicit Vs the Explicit** Orchestrator
- Implicit - Existing Domain obj ex - Order Aggregate
  - tradeoff - simple and bloated responsibility
- Explicit orchestrator - Dedicated obj
  - tradeoff - improved separation of concerns and additional class
- now this Saga Class consist of the enum state and the persisted data obj ex - Saga<CreateOrderSagaState, CreateOrderSagaData>

- **Transactional Messaging** - is what we called the underlying mechanism
  - the saga orchestrator invokes the participants
  - the saga participants sends reply
  - saga must complete even if there is a transient failures.
- **Use Async messaging**
  - saga participants subscribe to the command channel and sends to its reply channel
  - the saga orchestrator sends commands to the participant's command channel and subscribe to the participant's reply channel(to ensure the ordering of all the msgs from that participant)
  - but the complication here is that the msg receiving and sending and the processing must be of **transactional** updating the db and sending the msgs must be of **Atomic**.
- so the trick here is to use the db table as the temporary msg queue
  - step 1 - is put the msgs into this sort of outbox table.. now how do we get the msgs out of the db table into the msgs broker ? there are couple of options
    - 1. we can poll ex the msg publisher(periodically polls the table) issues the selec\* from the msgs table.
    - pros - simple and works well with the low scale
    - cons - latency and the performance and difficult to track which msgs has been processed(since the polling service will keep track of the msgs with the highest id, so it can skip over the msgs).
- **transactional Log tailing** option like the linikedIn approach, can be beneficial.
  - use the db specific mechanism to access the log ex - mysql binlog uses master/ slave replication protocol.. DynamoDB table streams
  - pros - good performance/ latency and easy to trackd
  - cons - db specific and obscure

### Aggregate (Root Design Pattern) .net

- Separate behaviour and the data for the persistence.
- (the behaviour method by definition is the method that interacts with db such as add, update delete or basic crud)
- in the eg - he just separate the behaviour method (separate the data model from the domain model)
- and he suggested, the repo generally has only 2 methods get and save
- again what he basically does is, save the shopping cart event domain in the repo, and start the tx and get those events(list of objs) and iterate overem and if its item added event then perform the ItemAdded() which is basically the sql query out

### Queue Worker pattern

- can be used in lot of arch such as the monolith or Ms, where we move some of the work on the background (just like the service workers in the Angular) it will do the task/work for us.
  - some of the task are the long running business or reoccuring batch jobs
- ex - the web queue API where the client will send the http req and the req will be in the queue and from the queue the worker will pick up and interacting with the db and making some state change.
- and to communicate back to the client it will use websocket (or signal R in .net), redis
- the webqueue pattern allow us to scale out.

- the another advantage of the worker is that it can be a task scheduler if we ve like the batch jobs or the long running process that need to run, and worker is the great way to do this asynchronously and it can be a task scheduler.

### Strategy pattern

- is a behavior pattern that will allow us to write the interchangeable code so we can select the right strategy based on the situation
- if we ve a large if block then its a perfect case where the strategy can be perfect fit lets say we ve a orderplaced notification, we wanna send the notification either by the sms or the email
- in that ex - previously we ve the orderservice and we ve injected sms, email or push service to send our notification based on the conditions instead he made a interface and IOrderNotifier and that is what we ve to inject and then use the switch stmt for the cases.

## Data streaming for the MS using Debezium

- DBZ(like a connector) mostly used along with the kafka connect (in the Source side)
- CDC and its use cases - as we know any time the data in our db changes we will get notified.
- **Data Replication** - replicate the data to other db, feed analytic s/m or DWH, feed data into oher teams
- other use cases of CDC - Auditing/ Historization, update or invalidate caches, Enable full text search via Elastic search, update CQRS read model, ui live update, Enable Streaming Queries
- How to capture the DAta Changes ?
  - well we can ve Dual writes - are Failure Handling(ex- we ve commit the changes to the db but we can't reach the elastic search) ? and prone to race condition.
  - Polling changes - how to find the changed rows ? how to handle the delete rows.
- so instead of this 2 approach what we can do is
  - Monitor the Db (and access the **tx logs**), used for the tx recovery and replication etc.
  - lets read the db log for the CDC (by using debezium) such as MySQL: Binlog, PostgreSQL: ahead log, MongoDB: op log.
  - Guaranteed all consistency(all events and deletes)
  - and it is also Trasparent to the upstream
- The kafka is good fit for the CDC - it provides the guaranteed ordering and pull based (since consumers ve the choice to read from when they want from the beginning or at the mid), and gives the options for compaction (how much data we wana store in the partion ex - the last 10 partitions)
- **Kafka Connect** - is the framework for the source(connects data into kafka) and sink(out of kafka) connectors
  - tracks the offset, shema support, clustering, Rich Eco s/m of connectors
- **CDC Topology with the Kafka Connect**
  - from the Db (on the Source side)to the Kafka Connect (1 DBZ for the mysql connection nd 1 for the Pgsql Connection) and these will go to the db and capture the cdc in binlog or write ahead log and commit em in the kafka topic.
  - and on the SinK side the kafka connect (with the Elastic connector) will stream those changes in to the elastic search.
  - The **Message Structure** will be the KVP (key pk of the table), the **payload** - Before State, After State and Source Info,
    - **Serialization Format** - JSON or AVRO(with the confluent schema registry)
  - The DBZ supports mongo, postgresql and mysql, oracle, sql, cassandra nd mariadb connectors in the future.

### CDC Streaming Patterns

- **Data synchronization** - in the MS architecture propagate data b/w each serice w/o couplingj
  - and each service keeps optimized views locally.
- we can also use cdc to extract MS - such as migrating from the monolithic to MS

  - extract the MS for the single comp, keep write req against running monolith, Stream changes to extracted Ms, Test new functionality, switch over (evolve schema only afterwards)

- **Maaterialize Aggregate Views**
  - Distinct topics by default,
  - often would like to ve views onto entire aggregates
  - Approach (such as use KStream to join table topics and materialize views in the source DB)
-
- **Ensuring data quality**
  - Detecting Missing or wrong data
    - constantly compare the data counts on the source and sink side
    - raise alert if threshold is reached
  - Compare every N-th record field by field
    - ex - have all records compare within one week
- **Leveraging the power of SMT(single message Transformations)**
  - Aggregate sharded tables to single topic
  - keep compactability with existing customers
  - format conversion ex - for dates
  - Ensure compatibility with sink connectors
    - Extracting 'after' state only
    - Expand mongo db's json structures
- **Note:** the OpenShift is the k8s distribution for the redhat
- **Strimzi** - is the another project they ve been working on(the redhat team), what it does is
  - it provides, the container images for kafka, connect, zookeeper and mirror maker
  - operators for managing / configuring apache kafka clusters, topics and users.
  - provides consumer, producers, admin clients and Kafka streams.

### Extra notes from Chatgpt

- **Aggregates**
  Within a bounded context, aggregates are another fundamental concept. An aggregate is a cluster of domain objects that can be treated as a single unit. Each aggregate has:

- **Root Entity**: The root entity (or aggregate root) that controls access to the aggregate and ensures its integrity. //
  Boundary: The boundary within which all entities are treated as a whole, ensuring consistency rules are applied. //
- **Data Boundary in Aggregates**
  The data boundary in an aggregate defines which data entities belong to the aggregate and are managed together. //

- **Encapsulation**: The aggregate root encapsulates all changes to the entities within the aggregate.
  Consistency: Any changes to the data within the aggregate are managed to maintain consistency and invariants. //

- **Logic Boundary in Aggregates**
  The logic boundary in an aggregate refers to the encapsulation of business logic that applies to the aggregate.

- **Operations:** Business logic related to the aggregate is executed through the aggregate root.
- **Invariants**: The aggregate root ensures that the invariants (rules that must always be true) are maintained.

Practical Example
E-commerce System
Consider an e-commerce system with the following bounded contexts:

- **Order Management Context:**

- **Data Boundary**: Includes entities such as Order, OrderItem, and Customer.
  Logic Boundary: Contains business rules for placing orders, calculating totals, and validating customer information.
- **Inventory Management Context:**

Data Boundary: Includes entities such as Product, StockLevel, and Supplier.
Logic Boundary: Contains business rules for updating stock levels, managing suppliers, and handling inventory.

Aggregates within the Order Management Context

- **Order Aggregate:**

- Data Boundary: Includes Order as the aggregate root and OrderItem entities.
- Logic Boundary: Business rules for order creation, adding/removing items, and calculating order total are encapsulated in the Order aggregate.
  Customer Aggregate:

- **Data Boundary:** Includes Customer as the aggregate root and associated address entities.
- **Logic Boundary:** Business rules for validating customer information and managing addresses are encapsulated in the Customer aggregate.

- Conclusion
  In DDD, the concepts of data boundary and logic boundary are crucial for managing complexity and ensuring consistency within a system. Bounded contexts define these boundaries at a high level, ensuring that different parts of the system remain cohesive and isolated from each other. Aggregates further refine these boundaries by encapsulating data and logic, maintaining integrity, and managing business rules within a smaller scope.

- **Cohesion** refers to how closely related and focused the responsibilities of a module or component are. In a highly cohesive system, components have well-defined, specific responsibilities and they do not include unrelated functionality. High cohesion is desirable because it makes the system easier to understand, maintain, and evolve.

- Coupling refers to the degree of direct dependence between different modules or components. Lower coupling is desirable because it makes the system more modular and easier to change. When components are loosely coupled, changes in one component are less likely to impact others.

- Cohesion: In DDD, high cohesion is achieved by organizing the domain model into aggregates that encapsulate related entities and value objects. Each aggregate represents a cohesive unit of behavior and data consistency, ensuring that business rules are enforced within the aggregate boundaries.
- Coupling: DDD aims to achieve low coupling between different parts of the system by using bounded contexts. Each bounded context defines a clear boundary within which a particular domain model is valid, reducing dependencies between different parts of the system. Event consistency models (such as eventual consistency) often involve asynchronous communication between bounded contexts, further reducing direct coupling.

- By maintaining high cohesion and low coupling, you can create a more maintainable, scalable, and robust system that aligns with the principles of DDD and supports various consistency models effectively.

- **an aggregate boundary** defines the scope within which a set of related entities and value objects are considered as a single unit for the purpose of data consistency and transactional integrity.

- The boundary defines the limits of the aggregate.
  • Inside the boundary, entities and value objects can reference each other freely. //
  • Outside the boundary, only the aggregate root can be referenced. //

  ## GitHub Actions

- The Github Actions is a platform to automate the dev workflows. and CI/CD is one of the workflow (in many workflows)
- what are the workflows ? in our git repo, consider a open source the devs can add new contributors and create a pull req and create organizational task to manage
- the ex of such task - lets say we ve an lib, and we ve some contributors and users of that lib when the user sees a bug then they can issue, then we can assign the issue to the contributers and he fixes and creates a pull req so we can merge(to the master branch) it to the next release of the lib.
- so after its merged we want to start the ci/cd pipeline of merge code -> test -> build -> deploy (include release note, update version number)
- now if our project get bigger more contributors and users are working then we need to ve the workflow and we ve to automate those tasks.
- so github actions are when some thing happens(are events / github Events) in or to our git repo, the automatic actions ve to be executed in response.
- so the actions goes like listen for the events and triggers the workflow and this workflow is like the chain of actions (such as label, sort, assign, reproduce etc)
- as we discussed the most common workflow is the ci/cd pipeline - commit code -> test -> build -> push -> deploy.
- **pros** - with this there is no need of 3rd party tools (since we already host our code on github).
- The setup pipeline is very simple/easy.
- and its dedicated tool to dev (so no need for additional devops).
- in comparison with the jenkins where we need to integrate all the plugins such as maven, jdk, docker and config integration and plugins
- here instead we can say like i need an env with the docker and node available w/o me installing any of them, with the version that i want.. and the same way i want to do the deployment part by easily connecting to the target env and the deploy the app

**Ex java gradle project** - which will build into a dock er image and push to our docker hub.

- just in the github repo under the actions section we can see all the list of available workflow templates, so we don't need to start from the scratch, we can just pick one that matches our project usage
- ex - the workflow for the java with maven, we can pick one of them and start using that yml and if we want we can make some changes to that file.
- once we re done with this yml file just commit (create a new branch and make a PR that will be merged into the master branch) or we can also commit straight into the master branch.
- and we can see our workflow will get triggered (and we can see the details)
- and in the build details we can see all the workflow stages.. now where(or how) the code was executed ?
- The way it works is the github Actions are executed on the **Github Servers** - which is managed by the github, so we don't ve to use the build tools or the plugins to manage the server.. so save our resource.
- so the Github servers will manage all those things for us, the servers will ve all the plugins and everything and ready to exec the jobs for us.

  - **Note:** - whenever we create a new job or workflow a fresh new VM will be provisioned by the git for us. so one jobs will run on a single server at a time, so if we ve a list of jobs.
  - and by default these jobs will run in parallel
  - in the yml script we can mention the server that our workflow runs on (there are 3 svr option availabe ubuntu, mac, windows)

- **Build docker img and push to hub** - so as we finish with the JAR, now lets do the containerization.
  - when we re using/selected the ubuntu for our jobs which has docker preinstalled so we don't ve to use the installation command (and dont set up the env we can exec the docker commands right away)
  - just we can google docker build and push action and in the marketplace of github action we can find.
  - so we can use that predefined action yml and replace with our hub credentials. Even the credentials or our secrets we can add in the git settings -> secrets.. and now we can use that secrets to reference in our workflow file ex : username: ${secrets.DOCKER_USERNAME}

### GitLab CI (its a paid version)

- the files naming convention is .gitlab-ci.yml
- same like the actions create the jobs (that execute the commands for the build / test / deployment)

### Yaml (tech with nana)

- yaml - ain't markup language.. can create the file with one of those .yaml or .yml
- is a serialization lang just like the json, xml
- the serialization lang means its a standard format to transform the data
- its a human readable and intuitive
- as we know in yml its with the "line Separation" and spaces with the indentation(since we use the indentation extensively its good to use the some yml validator like the online validator)
- syntax - KVP(val can be of str, int, bool, list, obj), comments(#), obj (indenting the individual KVP), list (-) the hyphen followed by all the attributes of the list (like the list of objects) or we can use the [] the primitive list syntax and put our vals inside
- **multiline Strings** - we can ve the val as a multiline string as well ex - multilinestrings: | followed by our str // the yml will see the pipe syntax and see everything followed by as a multiline str.
  - or if we want yml to interpret the multiline str as a single line then use ">" followed by our str.
  - we can use these multiline str to use for our shell scripts
- **env vars** - to access the env var we can use the "$" symbol
- **Place Holders** - mostly used for the helm with the jinja syntax ex - {{ .value.service.name }}
- **multi components** we can use multiple components inside one yml file and we can separate them by using the --- 3 indentations (and can be useful in the k8s where we ve multi components in a single yml file)
  - Note: we can also use the json format to edit our k8s configuration. inside the k8 admin ui we ve an option for that.

## CI/CD Pipeline (jenkins)

- [https://github.com/devopsjourney1/jenkins-101](the github link in here)
- The jenkins is not only limited to build the pipeline, we can also use this to automate any tasks, such as running the bash, py script or ansible playbooks
- the jenkins infrastructure - we ve the **master Server** - controls the pipelines and schedules the build.
- then we ve the **Agents**/ Minions - which runs/performs the build(which is just a bunch of linux cmds to build, test and distribute our code) in the workspace
- and the workflow look like this ex - the dev commits the code to the git, the master will aware of this commit and triggers the appropriate pipeline and distribute the build to the agent to run.
- The agent selection based on the configured label (by us).
- **Agens types**
  - 1. **permanent agent**, which are like the dedicated linux server or the windows server for running the jobs (need to ve the java and the ssh installed since the master makes the connection thru ssh)
  - 2. **cloud Agent** - such as the Docker, K8s, or AWS fleet manager, in this scenario the jenkins can dynamically spin up the agent based on the Agent template that we provided.
- in the git code ex - he just used the docker as the dynamic agent, so whenever we push or run the jobs, they are run in a dynamically provisioned docker container. this is similar to the k8s or the Ec2 fleet mgr to spin up the pods.

- **Build Types** - There are 2 main build jobs we can run in the jenkins 1. **Pipeline** 2. Freestyle Build project
- 1. Freestyle Build project - are simplest to create and are basically the shell script that'l run on a sever and triggered by the specific events, like a dev commiting to the repo.
- 2. Pipelines - use jenkins syntax written in grovy and specify what happens during the build.
  - are commonly split into multiple stages such as clone, build, test(test against the newly build code), package(packaged up and ready for deployment), deploy(sending out the artifacts to the registry ex sending out the docker img to the docker hub).
- jenkins has lots of plugins and its a beast when it comes to plugins (as compared with gitlab)
- in the s/m configuration - manage nodes and clouds is where set up the agents as well as the clouds like k8s, aws
- in one eg - he just create a job and(in the SCM) use his git repository url which has the py script to run
- in the build triggers section - if we select poll scm then the master will monitor the git repository and checks for any changes.
- in the schedule we can use like the cron jobs ex- \*/5\*\*\* to tell the master trigger our jobs for every 5mins

- **Setting up declarative pipeline using Grovy** - here we can directly create the pipeline script in the jenkins ui or we can point to the jenkins file in our local s/m or repo.
- in an another ex - where we use the grovy script, and to install the packages / dependencies we just ve to give the command in the build stage ex - `cd myapp && pip install -r requirements.txt` and lly for the npm install
- also in the grovy file we can add the additional code next to the stage(deploy) - ex post {always{}} and this file will exec always once we re done with the deployment. can be useful to send the email notificatiion to our team about the new deployment or even in the failure.
- we can also add the failure and success block inside the post block
- **Define the conditions** - we can also define the conditions / expressions on the stage ex - in the test stage we only want to exec that in the dev stage not in the prod, that condition we can define ex - when { expression { BRANCH_NAME == 'dev' || 'master'} {steps: `echo test`}}

  - just like the above we can also define the conditions in the build block/stage, so we can write the expression like { BRANCH_NAME == 'dev' && CODE_CHANGES == true }} .. so this will only build our app when we made the code changes.
  - **environment vars** - out of the box jenkins provides us some env vars we can use. we can see em in the `localhost:8080/env-vars.html` and in addition to that we can also provide our own environment variables, in the script just define the environment {} block and whatever vars we will define will be available for all the stages that we ve in the script/pipeline.
  - and inside the stages using the string interpolation syntax we can access the variables `${BUILD_NUMBER}`
  - inside the env block we can also define the credentials 1st define the credentials in the jenkins ui and then ex - SERVER_CREDENTIALS = credentials('credentialsId') // this credentials() fn will help us to bind the credentials to our env var.
  - and for that we need to install couple of plugins **credentials binding and the credential plugins**
  - if we only want the credentials to be used for only one stage(instead of all) we can use the wrapper - withCredentials([userNameAndPassword()]) // so this will allow us to get the username and password individually.. and inside that method we can define the params like credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PASSWORD // storing our environment variable as USER and PASSWORD and now inside the script we can use ex - sh "some script ${USER}"

- **tools** - this block will allow us to use specific tools for our project ex - maven, jdk, gradle.
  - ex tools { maven 'Maven'} and in the stages we can use em sh "mvm install"
- **using parameters for the parameterized build** - ex - we ve the build that deploys our app in the staging and we want to deploy that which version of the app we want to deploy. ex - parameters { string(name, defaultValue, defaultDescription) and choice(name, choices, description) } lly we ve booleanParam() as well.
- and we can use em inside the scripts and after we execute the script we can see the options build with params in the ui
- in the jenkins file we can use the script block to define all the groovy scripts,
- the script.groovy file - its like the py fns we can define them in the script file and then in the jenkins file we can have the "init" stage and import the script file ex - stage {"init"}{steps {script { gv = load "script.groovy"}}}
- and in the top of the jenkins file we can define the var of the script. ex - def gv and to use that in our stages we can just use the var followed by the fn that we defined in the script ex - gv.build_app
- all the env variables that we put it in the params {} block will be available in the grovy script we can use and also the default jenkins env param(out of the box) we can use em
