### Hyper Ledger Fabrics

- as we know the eth is not designed for the private n/w and for that case we can go with the Hyperledger Fabrics / permissioned bc n/w
- **Permissioned N/w** there are some use cases why we need to ve a permissioned Bc
- the hyperledger is the open source, and so many projects ve been developed under the hyperledger foundation amongst em all, the **fabric** is the most popular one.
- They use the **Redundant Byzantine Fault Tolerance (RBFT)** as the consensus algorithm.
- Since consensus is modular, its implementation can be tailored to the trust assumption of a particular deployment or solution. This modular architecture allows the platform to rely on well-established toolkits for CFT (crash fault-tolerant)
- this is mainly for the enterprise and to solve the enterprise's issues.

  - ex - Supply chain - consider an medical industry.. where the raw material (distributer), factory, shipment, wholeseller, hospital, pharmacy.. and every single one of them have their own db.
    - and if some consumer in our case the patient buys a some medicine, which was expired and decides to sue the hospital, and they don't know where the defect was, as everyone has their own db this is the problem with the non BC scenarios.
    - so the challenges here is - Tracing the event
      - manual, long time, nobody/accepts the mistakes, **Tampering/Fraud their DB** and they won't allow some one to access their db, Centralized DB, payment issue.

- if we use the hyper ledger in this scenario, this could be solved by creating **shared Ledger**
- and every sector sectors/organizations that are involved will be having the **network**
- and every sectors that are involved in this n/w will ve their own IS (running in the Vms) that contains the app (the way we run all our hyper ledger fabric app is thru **the docker container**
- now inside this apps will ve the shared ledgers, wahtever the data that will be written will be accessed by any one across the orgs.

- since we re running in this hyperledger fabric n/w the content in the shared ledger are **immutable**

### hyperledger terms

- there are some terms in the hyperledger terms n/w

1. Assets

### High level Architecture

- lets say the medical supply chain wants to impl the hyper ledger in their org .

1. inside a DB consists of hyperledger n/w which will ve all the org/ sectors in our case.
2. then the Smart Contract are called as **ChainCode** (all our business logic). at present it supports 3 langs java, go, nodejs. we can develop the chain code using the hyperledger api
3. then we need to deploy the chaincode in every orgs in our n/w
4. then the mw api/ which will ve the **Fabric Sdk**, using this api we can talk to our n/w.
5. then finally our frontend app. to access the apis

- in our case the consensus is "all parties come to an agreement, on the n/w verified tx unanonimously"
- and the privacy is "ensuring the appropriate visibiliy and if the tx are secured, authenticated and verifiable".
- if they need a privacy with in the n/w (for ex diff sector) ie called as **channel**, and all the other members in the n/w has to agree on the channel.(since this BC itself is buit on top of the Go lang)

### how the hyperledger works

(from the app SDK)

1. when the any sector wants to insert the data they will submit a proposal, then we ve programmed **endorsing peer**, the endorser will validate our proposal, and agree to make the change (based on our proposal)
2. the endorsing peer will validate the proposal like authentication or authorization. and the endorser will perform couple of tasks 1. execute the chaincode to simulate proposal in peer. a) query the state DB for reads b) build RW sets
3. then sends the proposal res back (includes the RWset)
4. submit the tx (includes the RWset)
5. **Ordering Service** creates batch (block of tx) (the responsibility of ordering service is it will keep every tx in order)
6. then the commiting peer(all peer) receives the batch(block) of tx from the ordering service.
7. the commiting peer will validate each tx and commit block
   1. validates the endorsement policy (vscc)
   2. validates the read set version in the state Db (MVCC)
   3. commit block to the bc
   4. commit valid tx to the state db.

- **Membership service provider**
  - is where all certificates will be issued to the participants, whoever particpates from the our org.
  - the registration authority will assign the user name and the p/w to the participants
  - Enrollment Certificate (ECA) assigns the enrollment cert to the participants
  - tx certificate
  - tls CA - issues tls cert to the s/m that transmit msgs in a chain n/w. (each org/sector will ve the certificate authority)

### the chaincode (smart contract)

- is a piece of code that lets us interact with the n/w's shared ledger
- chaincode services use docker to host the chaincode.
- docker provides a secured, lightweight method to sandbox chaincode execution.

### everything is Docker (in hyperledger)

- the hyperledger's entire n/w is docker images.
- the hyper ledger has created std images for all the services/comps such as endoser, commiter, msp, and EC and other services and we can find em in the docker hub.

### consensus algo

- as we ve seen already they uses CFT and BFT as the main algo for the consensus
- but they also provide us the modular approach for their consensus algo based on our requirements
- they providde an impl of BFT in its initial release, using PBFT protocol.
- the BFT means either all of them are agree or all of them reject.. only 2 scenarios here.

- in our case or in general the consensus will be like "consensus = tx endorsement + ordering + Validation"
- **Endorsement** - each stakeholder will decides whether to accept or reject the tx
- **Ordering** - sorting all the txs with in a period into a block to be commited in that order
- **validation** - verify whether the tx endorsement satisfies the policy, and tx transformation is valid accordin to the multiversion concurrency control (MVCC)

### Application Programming Interface

- as we know the enduser/app will talk to the b/c thru the hyperledger's SDK
- the api has following categories
  - Identity, address, storage, event(pub/sub), tx, chaincode, BC, N/w

### Application Model

- the hyperledger follows the mvc architecture.
- the data model includes the offchain data as well such as docs, large files, which are expensive to store in the blockchain, so we can ve our own Db (sql) save the data and then store the hash in the bc
- the hyperledges uses CouchDb world state supports the wide range of queries. (in our we will be using 2 dbs one - couchDB(for the world state that contains the current state of the assests, one row per asset))
- a chaincode may be deployed on multiple channels, each instance is isolated within its channel
- A chaincode may query another chaincode in other channel (ACL applied)

### MSP (membership service provider)

- An abstraction of identity provider, (MsP id, admin, sign, validate, verify)
- governs app endoser and order, entities.
- used as the building block for the access control framework. and represents a consortium or a member.

### crypto SErvice provider

- is the one which issues all the private keys
- supports multiple CSP,easy addition of more types of CSP ex - of diff HSM. types

### Fabric CA

- default impl of the msp interface
- issues Ecerts (long term mem) and Tcerts (disposable cert)
- supports clustering for the HA characteristics
- supports LDAP for the user authentication
- supports HSM

### Tx Endorsement

- An endorsement is a signed res of a result of tx execution.
- the "sample tx or the tx proposal looks like this", once we initiate the tx (thru the sdk), the tx will go thru the following services.
  - The endorser (have the ledger)
  - the commiter
  - the orderer
  - then the chaincode (each SCs will be attached to the endorsement policy).
    **Steps**
- send the proposal, execute the proposal, proposal response, ordering tx, delivering the tx, validating the tx(finally the peer validates the tx and are written to the ledger they update caching dbs with the validated tx), notifying the tx (then the new block will be added)

### coding

- install the fabrics (all the required docker images)
- he installed eclipse and then gradle for the build
- in the sample fabrics from the github, (where they ve some sample apps, and they also ve the section called "test - network")
- once we create the org crypto material, we need to create the genesis block of the orderer s/m channel. this block is required to bring up the orderer nodes and creates any app channels

- in the config.yml file we can find the ex of how to add the org, and the employees, msp, and policies details(such as reader, writer, and admin, endorsement), and the port for each comp/modules
- and in the network.sh file we can see all the fns for run the docker compose file, create the orgs, create the channels, create the connection profiles(ccp) b/w the orgs etc

  - to bring the n/w up `./network.sh up` and lly for the n/w down
  - the default channel name is my channel

- once the channel is created and the next thing is to join the channels (org 1 and org 2) and these all the command related to the channels are in the "channel.sh" file.
  - and to run this sh file `./network.sh createChannel.sh` command

**Steps**

- the overall steps what we did is 1. Bring up the n/w 2. create crypto material 3. create channel 4. develop chainCode 5. deploy chaincode 6. Access chaincode. 7. Fabric SDK to access from Ui/API

### ChainCode lifecycle

1. Develop the chaincode (for develop the chaincode we will be usin the java Fabric Api, Gradle)
2. Package the chaincode (chaincode.tar.gz. metadata:type:java)
3. Installation (install all the orgs)
4. Approval by all the organization (approval name, pckg version, sequence, endorsement policy, package identifier)
5. commit the chaincode (commit on to the corresponding organization)

### Gradle

- the gradle in our java project is helps in the automation of some repeatetive task such as compile, build, run test, package, release etc ..
- and gradle was release after maven , where maven was particularly java centric where as the gradle supports multi langs, such as c++, etc.
- it is scalable, and the performance is greater than maven, simplest to maintain.

### chaincode

- to interact with the chaincode `peer lifecycle chaincode package Carshowroom.com.tar.gz --path ../chaincode/carshowroom/build/install/Carshowroom --lang java --label Carshowroom_1 ..` followed by the file path
- the next step is to install the chaincode in both the orgs. ex - `source ./lifecycle_setup_org1.sh` and to install the chaincode

  - ` peer lifecycle chaincode install Carshowroom.tar.gz --peerAddress localhost:7051 tlsRootCertFiles SCORE` - once its created/installed it will creates a pacakage, which is a hashcode (for this chaincode installed on the org1)
  - then create a new source file for the org 2
  - `source lifercycle_setup_org2.sh` and add the env variables and then install the chaincode in org 2
  - `peer lifecycle chaincode install Carshowroom.tar.gz --peerAddress localhost:9051 tlsRootCertFiles SCORE`

- then `peer lifecycle chaincode queryInstalled --peerAddress localhost:7051` lly for the org 2 as well

- now to download the package `peer lifecycle chaincode getdInstalledPackage --pacakage-id Carshowroom_1:...package id` followed by the package id/hash

- now lets approve this chaincode in the org 1

- `peer lifecycle chaincode approveformyorg -o localhost:7050 --orderTLSHostnameOverride order.example.com --tls --certfile ..` refer the doc for this command its too lengthy.

  - lly execute the command in org2.

- next step is to **commit the chaincode**

- for that create a new file under the test-n/w as "lifecycle_setup_Channel_commit.sh" and add all the env variables
- `peer lifecycle chaincode querycommited -C sampleChannel -name Carshowroom`

- then invoke the chaincode `peer chaincode invoke -o localhost:7050 --orderTLSHostnameOverride ...` refer the command from docs.
  - since in this command we re adding the arg as inserting a new car, and add all the fields
