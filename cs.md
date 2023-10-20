# dotnet Microservices

You can run this app locally on your computer by following these instructions:

1. Using your terminal or command prompt clone the repo onto your machine in a user folder

```
2. Change into the Carsties directory
```

cd Carsties

```
3. Ensure you have Docker Desktop installed on your machine.  If not download and install from Docker and review their installation instructions for your Operating system [here](https://docs.docker.com/desktop/).
4. Build the services locally on your computer by running (NOTE: this may take several minutes to complete):
```

docker compose build

```
5. Once this completes you can use the following to run the services:
```

docker compose up -d

```
6. To see the app working you will need to provide it with an SSL certificate.   To do this please install 'mkcert' onto your computer which you can get from [here](https://github.com/FiloSottile/mkcert).  Once you have this you will need to install the local Certificate Authority by using:
```

mkcert -install

```
7. You will then need to create the certificate and key file on your computer to replace the certificates that I used.   You will need to change into the 'devcerts' directory and then run the following command:

```

8.  You will also need to create an entry in your host file so you can reach the app by its domain name. Please use this [guide](https://phoenixnap.com/kb/how-to-edit-hosts-file-in-windows-mac-or-linux) if you do not know how to do this. Create the following entry:

### Auction service

- first lets build the auction service, `dotnet new sln` to create a solution file.
- `dotnet new webapi -o src/AuctionService`
- and add the service to our solution - `dotnet sln add src/AuctionService`

- for the dev mode .. we will be using the appsetting.development.json for our configuration

- to install the entity `dotnet tool install dotnet-ef -g` and to check the installed tool `dotnet tool install dotnet-ef -g`
- for creating migrations `dotnet ef migrations add "initialCreate" -o Data/Migrations` this will look after the Db context and create a schema

-**docker for the db** we can make a docker compose file to make our postgresql connection up and running. - to run the docker compose file `docker compose up -d` will pull down the posgre image and run the container - now that we ve our db we can update our migrations `dotnet ef database update` - `dotnet watch` to watch the activities created by our db.

- note : when we use static kw in the class so we can initialize that method inside a different class w/o needing to initialize its class(parent)

- **Ef** - in this ef the data creation flow goes like this different steps.
- 1. we create **Entities**
- 2. then the **DB context** ex Auction DB context
- 3. then we run our migrations to create a iniial migrations
- 4. then the **DB intializer** / also in the migrations dir .. to create the seed . in our db

**Dtos and automapper**

- once we done with the above all the process now ve to find a way to return our data to the client from our database, this is where the dtos and the automapper comes in.
- the dtos can be used to restrict the data props that we mentioned in the entities and flatten some obj into strings ex - in entiy we ve status field of enum val but we can flatten that obj into strings in the dto
- and also in the dtos we can do things like remove the optional props (?) and remove the default vals.
- and finally in our auction dto we mix the auction entity and the item entity with onlyt the required properties.

- **AutoMapper**
- now that we ve our dtos in place we need a tool to map our Entity to the dto, and that's where the automapper comes in
- as long we ve the same name in the entity and the dto the auto mapper will take care for us.
- inside the RequestHelper dir we ve the "Profile Mapper" automapper.. once we create all the auto mappers
- mapping Propiles class impl the profile interface from the Auto mapper.
- then register the mapper in the prog.cs file // so it will look for any class that derives from the prof class(MappingProfile.cs), and register the mappings in mem

### second Microservice (full Search)

- this ms is responsible for the search functionality of the application. `dotnet new webapi -o /src/SearchService`and add the solution `dotnet sln add src/SearchService`
- this serivice will be using mongo and mongo Entity(for querying) and service bus **Rabbit Mq**
- and we konw the **law of Ms** is we don't share the db amongst em
- we could've gone with elasticsearch but its resource hunk, so we can go for this simpler approach

- **pagination** - when we search thru the query, we don't want to return millions of the results back to the user instead we use pagination.

  - to use pagination we can use the query instead of find we use **PageSearch** and now we can make use of the page number and the page size(limit) params to restrict the pagination

- **synchronous msg b/w services** - there are 2 types of synch service in the **http and Grpc** - but this will only work if both services are available if one fails then there will be no communication b/w em.
- so when it comes to ms we want those service to be independent regardless of the availability of other services
- in the messaging world if the client has to wait for the response then its a "Synchronous" communication
- and the services that uses synchronous communication are called **Distributed Monolith**

- **Synchronous communication** - where the service A doessn't need to know anything about the service B they just use the message bus to communicate each other
- event driven approach, just fire an event and forget, the msg bus will take care of the rest, it delivers to the corresponding service. and the service B will pick up the message from the bus w/o knowing anything about service a.

- to establish the synchronous connection b/w aution service, create a service/class called "AuctionSvcHttpClient" in search service ..
  - and we config the auction service in the app setting prod file and register the service in the prog.cs
  - **http polling** when any of the service is down, and comes back alive then instead of sending the http req to the service we can create a polling (and specify the no.of req we want send in case of failure) that will send automatic req when the service is back
  - we can use a package called **ms.extns.http.polly** with this we can now create the http **policy**
  - and we will set the policy in the prog.cs like keep on trying for every 3 secs until the auction service is back alive

### Asynchronous communication (event Bus)

- **Rabbit Mq** is our event bus.
- now with this approach our auction service will publish the msg (event) to the service bus (rabbitMQ) and the search service will subscribe to (event) the msg and take the action to evntually get consitent with the mongo db with the postgres db
- No req or res just the **fire and forget** approach.
- some of the transport are (rabbitMQ, amazon SQS, azure service bus)

- **Rabbit Mq**
- is msg broaker which accepts and forwards the msg to the queue
- producer and consumer.. pub/sub model
- msgs are stored on queues (msg buffer) ..this msg buffer are persistent if the bus goes down and the another bus takes over it will still gets the msg from the buffer
- can use persistent storage (in the event of failure)
- **Exchanges** can be used for routing functionality (there are diff types of exchanges)
  - 1. direct 2. Fanout 3. topic 4.Header
  - 1. direct - delivers the msgs based on the routing key (its a direct or unicast type of message)
  - 2. fanout - exchange bounds to one or more queues and that queues will wait for the consumer to pick up
  - 3. Topic - its a combination of both , has routing keys and exchages bound to more than one queue
  - 4. header - this type will allow us to specify header with the msg with one or more queues
- - rabbitmq uses **AMQP** (advance messaging queue protocol)

-**Mass Transit** we can use rabbit mq client to connect rabbit ma but we will be using Mass transit which is dotnet alternative for the rabbitmq client. - this also supports different transports in the future not only rabbit mq

- **Contracts** - is gon to represent the events that we will be sending thru the msg bus..b/w the services..
- - to create a contract`dotnet new classlib -o src/Contracts`//its a contract class library. and then add it to the soln - `dotnet sln add src/Contracts`
- and now add the reference to the contracts in both the service `cd AuctionService` then `dotnet add reference ../../src/Contracts` and lly repeat this command in the search service to create a reference to the contract

- and the idea behind is when we create a contract both the auction service and the search service will ve access to the contract obj..

- **events** some of the events we will be creating is ex auction created event which is essentially the auction created dto that we will be sending into our service bus

- **creating consumer** once we added the events in the contracts we now can create the consumer to consume those events, ex - in the search service create a Auction created "Consumer" class **the naming convention** matters for the mass transmit..
- once we register our consumer in the prog.cs and restart the search svc`dotnet watch`, in the Rabbit mq we can see the exchanges created one for the producer (auction created) and the other for the consumer (search-auction-created)
- **search-auction-created** is the queue that is ready to receive msgs for our search service

- **publishing event to the bus** - make some changes in the auction svc controller (ex imp the Ipublisher interface) and register the automapper in prof with the contracts of ex - CreateMap<AuctionDto, AuctionCreated>(); // Auction created is the contract .. and both the auction and search svc knows about it.
- again restart the svc we can see the exchange created

- **what could go wrong** now our arch looks like this when the post req comes in the 1. auction svc 2. which saves in postgresql 3, then create a msg queue in msg bu 4. then the search svc will sub the msg and consumes it 5. and saves it in to its mongo db

  - incase if any of the svc fails among the all 5 svc only if the mongo or the msg bus fails then we won't ve the data consistency.
  - if we any of the other services fails we still ve our data consistency..

- **outbox** if the rabbit mq fails our msgs will sit in the outbox and if our svc is back alive, from the outbox it will pick up the messages and pub into the msg bus

  - **retry** and we can retry if it won't succeed.
  - inside the auction svc install the "mass transit ef core package" to create the Outbox(pesistent)
  - and config in the prog.cs in the auction svc
  - and add it in the db context ..and run the migrations `dotnet ef migrations add Outbox` will create a migrations with 3 more tables included (that we defined in the db c)

- now that we ve our outbox configured if we stop the rabbitmq and make a post req our req will be successful and the msg will be wait in the db to be delivered when the svc is back up again it will pick up the msg from there

- **what if mongo fails** - with the outbox implementation we fix the data consistency if the rabbitmq fails.. now what if mongo fails ..
- we can configure the **retry policies** on per endpoint basis.. inside the prog.cs in search svc

- **consuming fault queues** - if we ve error msgs (when the msg bus is down) that exception will be added to the queue as well
- we can also consume those fault queues ex - Auction Created Faults
- and then consum it in the auction svc (prog.cs)
- ex - we create a car model foo which we consider as fault and the auction will pick up and change it to foobar and pub it and then our search will pick up the corrected foobar model

### Identity service

- is diff from other since its not the part of the msg bus, we can think of it as a outsiders service
- in this we will create identity service and add authentication endpoints , with the help of openId and oAuth2.0
- best alternative is we could use Azure AD for this..
- the identity service is an authentication server which implements 2 standards **openID** connect OIDC and **oAuth2.0** standards

  - the OIDC is no longer open source require licenses in prod
  - the identity server is a single sign on solution

- **Oauth2.0** security standards to give one app permission to access our data in other fapp, it gives key (jwt) instead of password

  - **redirect uri** we ve the redirect uri https://app.com/callback - the url where the auth server redirects back the user to after granting permission to the client callback url
  - the response code is the **key** the client receives as auth code.
  - we also ve scope such as read only consent form etc..
  - and we also ve the **client id** to identify the client with the auth server.
  - we also ve **client Secret** the app, "nextjs app" to securely sharely the info privately bts. to the client server not the client browser.
  - we also ve the **authorization code** the identity server sends back to the client. the client then sends the authorization code along with the client secret in exchange for an access token
  - **access token** is the key the client will use from that point to access the the resource from the server.

- **open ID connect** OIDC .. as the oAuth2.0 is only for authorization and granting access to data
- the OIDC will sits on top of the oauth and adds additional functionality around login such as..profile information about the person info who is logged in..
- this oidc enables the client to establish login **session** and gain as well the info about the person and this is referred to as **identity**
- and when an auth server supports oidc its often referred to as **identity provider**

- to install the identity server `dotnet new install Duende.IdentityServer.Templates` then
- `dotnet new isaspid -o src/IdentityService` and also add soln for this service `dotnet sln add src/IdentityService`
- by default the identity server will comes with the sql server installed, but we will be using the "postgresql" for this

- **seeding Data** and add migrations

  - go to http://localhost:5000/well-known/openid-configuration ..
  - the asp net has the user mgr svc to add and get the users from our db .. in the "seedData.cs" we can see the default user "alice"

- the identity server by default comes with all the pages such as login, logout etc we can create only the register user page

- adding client credentials to allow clients to to req a token..

- **adding a custom profile service to identity server** - rn we re having the jwt token but we don't ve the user info, we can extend the token by adding the user info/profile to it.(like the user id, user name)
- we need the user name prop for the seller and the winner fns

  - create a custom profile service class .. impl the IProfileService

- "configuring auth on the resource server (autcion server)" - this will ve auth endpoints, so we can pass the token to the server, and it will validate it against the identity server
- for that we need authentication.JWTbearer package installed in the auction service. then register in the prog.cs and also add the "mw" **authentication mw** and it has to come b4 the user authorization mw

### Gateway SErvice

- will provide the single access point to our app, will act as the single surface for reqs (our clients need to know single url to access all our services).
- and we can use it for security, url rewriting, ssl termination, Load Balancing, caching.. etc.

- to create the service `dotnet new web -o src/GatewayService` (this time around its not the web api, since we re not gon use the api endpoints) and then add the solns `dotnet sln add src/GatewayService`

- **reverse proxy** as we know the proxy that sits on b/w our client app to the browser, and the reverse proxy is in reverse order it sits on close to the backend/resource services.
- and we need to install the reverse proxy package called **yarp reverse proxy**
- and the jwt bearer package
- we will config the authentication process on the proxy and will result in the authentication cookie and that cookie will flow to the destn svc as a normal req header(refer the yarp reverse proxy docs for more details)

- and then in the app.config file we can specify like ex for the auction service (get method) we don't need to config the authentication, but for the post method routes we ve to configure .. lly for all the other services as well.

### Dockerizing the application (aka containerizing our application)

- lets dockerize all our services so far we ve..
- so we gon be having docker file .. one per service and then we build the docker img based on that docker file.
- once we create our docker file we can run `docker build -f src/AuctionService/Dockerfile -t testing123 .` to create the docker image
- for the first time it will take some time, but for the future builds the docker will cache lot of things and packages..
- initially docker won't ve any idea about the our postgresql db, we need to tell the docker about our configuration.
- once we ve the docker image we can now add the image to the docker compose file with our docker hub user name followed by the img name.
- make sure to config the prog.cs with the rabbitmq configuration.
- once we configure the auction serive in the docker compose file we can build by `docker compose build auction-svc`
- and we can start `docker compose up -d` can see the auction svc running inside our docker container with rest of our services.

**Search service** lly repeat the process for the search svc..create the docker file for the search service and then add the configuration about the mongo db and then create the img and add the config it to the compose file and run the

- then build the service `docker compose build search-svc` and to up the service `docker compose up -d` will start the service.

- **identiy svc** repeat the same process for the identity service
- **gateway svc** and also for the gateway service..

- **debugging .net service in a docker container** in the cmd pallete we can search for the generate .net assets for build and debug..- and in that we can ve the option to add the configuration.
- and there we can also ve the option to add the .net debugger to the container ..
- and in that list we can see the "docker" .net attach preview - just select that which will add the configuration to the "launch.json" file

- to run all our containers with the clean data seeded then we can run `docker compose down ` and run it again `docker compose up -d` ..

## Client Side app

- is a next js app .. `npx create-next-app@latest`

- **deps** for the client app
- react-icons `npm i react-icons`
- tailwind aspect ratio `npm i -D @tailwindcss/assets-ratio` will provide the aspect ratio for our imgs .. and register this plugin in the tailwind config.
- react count down `npm i -D react-countdown` for the count down timer for our auctions
- fowbite react `npm i flowbite fowbite-react` for the **pagination**
- zustand `npm i zustand` for the state management.. we can create a store(hooks) and then we can use the store inside our comps easily.
- query string `npm i query-string` for creating qs to all our query parameters, that we need to send to our search service, so we get the correct res back from the service..
- next auth `npm i next-auth` for authentication
- react hook form `npm i react-hook-form` for handling the form..
- react date picker `npm i react-datepicker @types/react-datepicker` for handling the date picking capabilities.
- react hot toast `npm i react-hot-toast` for the pop up notification.
- signal `npm i @microsoft/signalR` for the notification in the client side..(client side package)
- **fetch** - to fetch the data from our endpoints from our service, and retrieve the list of auctions (from the gateway )

### next Auth

- to authenticate our client with the next auth or (auth js).. next auth will soon become auth js (and can be used to many other frameworks)
- the identity server will send the authorization code to the browser which will then redirect us back to client app to the callback url,.. from there the client app will send the authorization code and the client id and client secret to the identity server..which will response with a access token.
- at this point next auth will save the encrypted cookie into our browser, which it will use to maintain the **session** with itself..

- then finally the client app can use the token to req the resource server

---

- **login** and for the login if the user is logged in then we can use the session cookie to get the info about the user and display (user prof)
- "authActinon.ts" we can get the user session information from the next auth js getSession()

- **getting the access token to use to authenticate to our resource server ** - in the next auth doc there is a util fn - getToken()- will allow us to get the decrypted jwt
- in the session comp "page.tsx" we ve the session info and the token info (we pull the prof from the session info), and we also ve to add the access token to the token data to access our resource server.. (we send em in the headers)

- althoug we re storing the token in the client browser.. we still encrypted the token with the client secret..and token is an http cookie and can't be accessed by the js

### Client side CRUD

- building the rest funtionality, react hook form, routing and error handling..
- **img upload** we ve not covered the image upload functionality, but we can use the "cloudinary" for the image upload, for that we need the auction Id, seller of the auction, authentication

## The Bidding Service

- lets add the bidding service.. and make the facilities to give the user to bid on the auctions `dotnet new webapi -o src/BiddingService` and then add the solutions `dotnet sln add src/BiddingService` ..
- and will be using mongo for the database.
- some of the other packages are mongo entity, jwt bearer, mass transit rabbit mq.. and config our app settings and the prog.cs
- add the reference to the contracts `dotnet add reference ../../src/contracts` now that we ve the ref for the contract lets add the consumer.. only one consumer for this service auction created consumer. then add it to the mass transit consumer settings in the prog.cs
- same as the other svc create a dto and install the automapper and then create a mapping prof inside the req helper dir and map our entity(bid) to the bidDto.. and the bid to bid placed contract.
  - and register the automapper in the prog.cs
- **bit placed producer** - just add the createMap<Bid, BidPlaced>() in the mapping prof, so when we save this to our db the Ipublisher interface (that we impl in the controller) will publish the event

- **adding a background service for the auction finished event** - we will ve the bg svc that will poll the db, if there's any auction met the end date but ve not marked as finished and for each of those we will send an event on the service bus.
- create a service inside the bidding service

- **Grpc** supports both sync and async communication..for our requirement we will set up synchronous communication, high performance **roughly 7 times faster** than the http rest when receiving data and 10 times faster than the http rest when sending data
- since the msgs are binary ..designed for low latency and high thru put communication.
- and it relies on protocol buffers/ contract b/w the services.. and each svc will ve the proto file that defines how these services are communicate

- **Grpc client** ex - the .proto file from the bit service is our, and the .proto file from the auction service is our **grpc server** .. this will set up the contract b/w our client and the server.
- and these services are gon to use the http2.0 for the communication
- once we write the proto file then register it to our proj file
- once we create the proto file build the app and see (in auction svc) in the obj/Debug/protos/ we can see auction.cs and auction.grpc.cs created based on our proto file

- **grpc service** inside the auction service create a service called **Grpc auction services** that impls the GrpcAuction.GrpcAuctionBase

- for the client side of our grpc service ie (in the bidding service ) we gon install diff package "google.protobuf" and 2 other packages "grpc tools" and grpc.net.client
- and create the exact same proto file as the auction svc
- and similar to the auction svc also create a grpc client service inside bidding service,

- **update the gateway service** then update the gateway service as well. in the appsetting.json and the app setting docker.json add the bids svc endpoints

- **dockerize the bidding service** create a doker file and then add the service to the docker compose file.

### Notification service (SignalR)

- for the notification service we gon use the signalR for the live communication b/w our clients and the server.
- some of the notifications are auction created, bid placed or auction finishing etc.
- and we want our client to client to and maintain the connection over **web socket** so we can receive the live updates from the backend services.
- `dotnet new web -o src/NotificationService` and `dotnet sln add src/NotificationService`
- and this is gon be the consumer of the events effectively and add the ref to the contracts `dotnet add reference ../../src/contracts`
- packages - mass transit,

- **SignalR Hub** - inside the hub dir create a service which will impl from the Hub from the signalR

- **Cors Support for the G/w** - our browser will make a connection to signalR, that means its a cors and we need to send back a header, so our browser allows the req to complete
- in the g/w service prog.cs add the cors policy settings

### Adding bids notifications to the client

- lets add the bids functionality to the client and the signalR notification

**adding signalR to the client** currently from the client, user can update the bids and add more bids but its not updating ie bcoz our client and svr not in sync with each other. so we can use the signalR notification to the client, to get the live update notifications.

- and its like a provide that wraps the app.

### Publishing the app to production (locally )

- as we re gon to dockerize even our client .. we are gon to use the ingress to access the internet..
- the ingres will receive the external traffic and f/w it based on the rules to the correct location.
- we ve to ve 3 rules here the identity is considered to be external eventhough we re running it inside the docker
- and our client will also need to find the identity
- and our clinet will ve to maintain the connection with the notification server (SignalR) via ingress as well.

- so in this we will dockerize (docker compose) our services, adding ingress control with Nginx and adding the ssl

- **assigning static ip to the identity** we ve to assign the static ip to the identity server
- we can add a subnet (of 10.5.0.0/16) under the custom ip manager config in the n/w sections
- this private ip addrs we can use internally for the docker .. and for the every service we ve to specify this cust static ip.

- **adding Nginx ingress to the docker compose** - so the external client will access our services via this proxy

- **adding ssl to the ingress** for the local/dev.. we can use a tool mkcert to add the ssl to our ingress proxy . `brew install mkcert`
- and then `mkcert -install` and followed by that `mkcert -key-file carsties.com.key -cert-file carsties.com.crt app.carsties.com api.carsties.com id.carsties.com`
- this will create a new certificate for all the mentioned local host addresses(3)

### Unit and integration testing (appendix A)

- why unit testing ? documentation and less coupled code ..and its fast, isolated, repeatable, self checking and timely
- we will testing the api controllers
- and there are 3 test tools we will be using **XUnit, NUnit, MSTest** and all are prety much the same.

- **XUnit** - `dotnet new xunit -o tests/AuctionService.UnitTests` and add the sln `dotnet sln add tests/AuctionService.UnitTests` and go to our test dir add the reference to our app sln file.
- `dotnet add reference ../../src/AuctionService`

- in the app controller class we ve injected the AuctionDB context and we can't create a mock for this since its not a interface and its really big.
- but instead we can create **repository** for that abstract class .. so when we create an repository we can create an interface .. and we can **mock that interface** for the testing..
- in the interface(IauctionRepo), we will be having all the methods (abstract methods) that we already ve in the entity class.
- and in the repository, we will be having all the implementations for the methods that we ve in the interface. (the methods are as sim as the entity class methods with some small changes.)
- in the repo we will inject the Db context so this is scoped to the db context and while register the service in the prog.cs we ve to build and add it to the scoped to db context

- now inside the controller we can remove the DB context and inject the Irepo INterface

- and now we can create the "AuctionControllerServiceTest"

### AuctionService Integration Tests

- `dotnet new xunit -o tests/AuctionService.IntegrationTests` and add the slns `dotnet sln add tests/AuctionService.IntegrationTests` and add the reference to the the tests `dotnet add reference ../../src/AutionService`
- and we need the ms package - "ms core asp net core mvc testing" - this will allow us to create the functional(integration) tests.
- and postregresql test container "TestContainer. postregresql" package this will create when we start our test and disposed off when we finished
- and "webmotion fake authentication jwt bearer" packge - with this we can fake our authentication w/o needing to invoke the identity server.

- just run the `dotnet build` in the sln level it will run the test container.. and we can make our test

- **Testing the service Bus** - so far we ve been testing the controllers, lets test the service bus functionality.
- we ve our mock mass trasit harness service
- **using collection fixtures to share the single db across diff test classes**

### Publishing the app to local K8s (appendix B)

- the approach is to enable the k8s by docker desktop and that will give us the k8s cluster with a sigle noode which is a vm capable of running containers..
- in this we will be moving our identity server outside of the cluster
- the k8s will allow us the service discovery, load balancing, **storage orchestration**, allow us to rollback and rollouts, provide us self healing (FT), secret and config management
- each service running in the pod will ve the own Cluster Ip, and externally node port will allow us to access the external cluster.
- we gon need to install the polly package
- **use git hub action** we can use the github action to push our identity server to the docker hub, so we can keep that svc externally.
- inside the .github/wokflows we can ve our docker.yml file
- for that identity server to run on the external like digital ocean (containers) we ve to create a couple of docker files 1. docker-compose.yml and /etc/systemd/system/id-docker.service .. (grab and paste the settings from the snippets)
- and then enable the service `sudo systemctl enable id-docker.service` and then `systemctl restart systemd.journald` for seeing the logs .
- to see the status of the service `sudo systemctl status id-docker.service`
- to start the service `systemctl restart systemd.journald`
- to allow the ubuntu f/w -`sudo ufw allow 80` lly for the port 443 ..we can see all the services running `docker ps` (nginx proxy, identity svc, postgres etc..)
- to see the logs of the service (with the service name) ex - `journalctl -u id-docker`

- **creating the 1st k8s manifest for deployment** - under the "infra" dir we will all our k8s manifest (deploy)files
- to run our deployment file `kubectl apply -f postgres-depl.yml`
- **pvc** persistant volume claims - can create a new yml for the pvc "local-pvc.yml" once we define the storage again run `kubectl apply -f local-pvc.yml`..
- since we ve our pvc.. even if we delete the postgre container k8s will up and run one, and it alway wants atleast one container up and running.
- some of the services we can are the **Lb** and **cluster Ip** the cluster ip is for the pods to communicate each other internally.
- to see the services runnit `kubectl get services` or to get the pods `kubectl get pods`
- `kubectl describe node` to see the running node and its s/m resources

- **k8s Secrets** to create a secrete file `kubectl create secret generic postgres-password --from-literal=postgrespwkey=somerandompw` for the postgres
- but instead of creating a secrets separately for each file, we can create a k8-dev dir and put em all the secrets in a single file ex -"dev-secrets.yml"

------------------------------------------------------------------------------

# Advanced dotnt (fcc)

### Delegates 

- a delegate can be described as the type safe function pointer. the var that defined as the delegate is the delegate type var, that can hold a ref to the method. the delegates can reference both the static and the instance methods.
- in order for the delegates to ref the particular method the delegate must define the params with the types that match the param types contained in the relevant method.
- the delegate must contain the return type that match the return type of the relevant method.
- The **Variants** can be used in the delegate means the type defined in the param list and the return type of the delegate do not ve to exactly match the relevant method params type and the return types.. covariance and contravariance.
- to create a delegate in our class ex - delegate void LogDel(string log){} .. the wierd things is now in the main() he just create a instance for this delegate method(as if its an obj/ class) ex - LogDel logDel = new LogDel(logTexttoScr); logDel('some text'); // inside the instance he passed in the another fn - static void logTextToScr(string text);{} // this static method and the delegate method has the same return val and the same param.(which is one of the requirement those ve to be same)
- this shows that we can reference the static method with the delegate defined var..
- lets see the delegate can also ref with the instance methods, ex create a new class and make a method public void someLog(string data){} // and in our main() create a instance of this class and call this method (with the delegate), it will work as expected.
- we can also able to call both methods thru one delegate call this is known as the **multicast delegates**. means multiple objs can be assigned to the one delegate using plus "+" operator. ex - LogDel multiDel = logTexttoScr + logTexttofile; // now we can be able to call the both method in one call delegate ex - multiDel(name);
- now lets see how the delegate can be passed as an argument to the method then invoked by the method that receives the delegate argument. ex - static void LogText(LogDel logDel, string text) { logDel(text)} // call it with the multidel ex - LogText(multiDel, name); // lly we can pass the individual delegate as well, so we can see how the delegates can be used to separate the design from the implementation detail and facilitate the flexibility in our app.

### delegates (part 4)

- source code [here](https://github.com/GavinLonDigital/CovarianceAndContravarianceDelegateExample.git)
- in  this code its about the club membership form and he used the delegate to implement the field registration functionality. and the advantage of this delegates is this can be reused in the other forms (for the field validation).
  - so it gives 2 usage 1. code reuse and 2. stick to DRY principle and code design flexibiltiy.
- in that we ex we will ve the user class in the Model and ve the props (fields) of the user class model. and he added the migrations ex - (`Add-Migrations First migration`) as we know this will create a schema / migration class.. and in order to run the migrations we can run `update-database` 
- in the field validator class and inside the field validator del fn he passed an param like (out string field) // the out kw allows the val to be outputed to the calling code.

- **Part 6 Covariance and Controvariance**
  - as we know that the return type and the param type of the method ref by the delegate obj must match the return type and the param type of the method defined in the relevant delegate definition, but the return type and the param type of the relevant method defination don't ve to match the return type and the param type of the del definition exactly.
  - means cs provide some flexiblity to the devs when it comes to del compatability, covariance and the controvariance provides that flexibility, when matching a del defn to the method defn. 
  - convariance permits a method to ve a return type that are more derived (concept from the inheritance) than the relevant del.
    - ex - refer the [code](https://github.com/GavinLonDigital/CovarianceAndContravarianceDelegateExample.git)
  - where as the controvariance permits a method to ve a return type that are less derived than the relevant del.

- **Part 7 Func, Action and Predicate**
  - these are all built in del generics,.. there are like 17 Func del available in the s/m namespace, ex - Func<TResult> Delegate...lly we ve 16 type params T1..T16 in the del.
  - the Func delegate defn always returns a val, and the return type is always defined in the last type parameter.
  - public delegate TResult Func<in T, out TResult>(T arg); the in kw here is contravariant, and the out kw is covariant.
  - and then public delegate void Action<in T>(T obj); // the action generic del will allow user to encapsulate a method that can ve 0 to 16 params and must not return a val.
  - public delegate bool Predicate<in T>(T obj) ..lly the predicate generic del will allow the user to encapsulate a method that can ve a 1 param and must return a val which is bool.
  - and the code ex for all of these 3 generic dels [here](https://github.com/GavinLonDigital/FuncActionPredicateExamples.git) 
  - we can assign the func method to the del method ex - Func<int, int, int> Calc = delegate (int a, int b) {return a + b}; // we can further reduce the del method by or like a lambda (a, b) => a + b;// ex - Func<int, int, int> Calc = (a, b) => a + b;
  - lly for the Action generic as well..sim implementation as the above .. for the predicate its bit different, refer the code.
  - the **extension method** - through this method we can modify/ add methods to the existing types, w/o creating a new derived type or recompiling or modifying the existing type... the system.linq namespace contains no.of extension methods.
    -  the way extensions method is available in the Ienumrable geneiric type, the generic List type impl the IEnumerable type.

### Part 8 Delegate Async method callback

- lets see how delegates can encapsulate the async cb method. 
- the code from the microsoft docs. (Del async cb method).. in the code we can see the **AsyncCallback** del is built in dotnet del to encapuslate the async cb methods



### LINQ (language integrated query) Introduction (Part 21)


- find the source code [here](https://github.com/GavinLonDigital/LINQExamples_1.git)
- set of technologies based on the integration of the query capabilities directly into the cs language.
- with link query is the first class language consturct just like the classes, methods, events. we write the queries against the strongly typed objects using the language kw and familiar operators.
- The LINQ family of technologies provides the consistent query experience for obj (LINQ to Obj) aka any collection of objs that supports IEnumerable interface or the generict IEnumerable<T> interface, relational db (LINQ to SQL), and XML (LINQ to Xml), and ADO.net datasets. 
- we can write our own custom provider using the IQueryable interface, (refer the doc for more details)
- **Link to Entities** - provides linq to support, that enables the devs to write the queries against the EF conceptual model either using the Visual basic or visual CS. 
- **Linq to Objs** - lets ensure we properly understand the 2 concepts ie 1. **Extension Methods** - lets create a TCP extension soln and create a etxn class (static class) that has the cust method (called filter also static) the idea here is that we want a way to extend the functionality of any generic list ie strongly typed with any cs data types including the user defined data types, we can achieve this by creating our own extension method.
- and to create our own extension method we ve to ve a static method ex - public static List<T> Filter<T>(this List<T> records, Func<T, bool> func); // in our params the this kw refers to the generic list and the the 2nd param is Func generic delegate. and here we can also use the Predicate del type instead of the  Func del type.
- and this static fn has the logic for filtering the relevant generic list.
- with the Linq we can use the where clause, from, in, join, on etc just like the sql. with this we can create our query and return our obj(ex resultList), with that obj we can use some fns/methods like Average, min, max ex - var employeeResult = resultList.Average(a => a.AnnualSalary).
- Linq provide us the integration of the query capabilities directly into the CS language. it is a very powerful technology that decouples the query logic from the relevant types of underlying data formats.
- Linq provides us the layer of abstractions that provides the devs with the easy to implement syntax for querying and collections of objects. 
- the relevant collection types must implement the IEnumerable<T> or the IQueriable<T> INterface. the IQueriable interface inherits from the IEnumerable interface . and these classes mostly contains extension methods.
  - 2. **Lambda function** - 



### LINQ Queries (Part 22) - source code [here](https://github.com/GavinLonDigital/LINQExamples_1.git) 

- the anonymous types provide the coveinient way to encapsulate a set of read only properties into a single object. without having to explicitly define the type first. it can provide flexibility and convenience when contructing queries.
- as we know, we use the where syntax in the select () clause, this method chaining will invoke a multiple method calls in cs. each method returns an obj, allowing calls to be chained together in a single statement w/o requiring the var to store the intermediate results.
- with the  query method syntax it will be like var results = employees.select().where(); but in the standard qurey syntax its slightly different ex - var results = from employee in employees where employee.salary >= 50000 select new{ fullname = employee.fullname }..// in the standard query syntax the query operators (select, where) will be converted into extension methods at compile time.
- ms recommend using the standard query syntax as oppossed to the query method syntax (directly calling the extension method )

- we can be able to execute queries with the **Deffered execution** or immediate execution.
- the deffered execution means the evaluation of the expression is delayed until its val is required. this improves performance bcoz only the necessary execution are carried out.. in our ex the execution takes place only in the iteration or forEach loop an execution is performed.
- another advantage of the defferred execution is the update to the relevant data collection will immediately be reflected in the results.
- the deffered execution re evaluates on each execution is known as the **Lazy evaluation**
  - inside the forEach loop he uses "yield" kw to return the sequence of the elements one at a time. 
- **Immediate Execution** means the relevant query execs immediately. to exec the query immediately we need to apply the "to conversion" method to the query. 
  - in our case we can use the "ToList()" conversion method on the query, in order to use this method we need to wrap this relevant query in the parenthesis ex - (fron emp in employee where select).ToList(); here we re explicitly type casting the IEnumerable val from the return into List collections.
  - w/o this ToList() conversion method our query will be defferred until the relevant results are iterated

- some of the other sql methods or operators like inner joins, group joins, outer joins are also available in the linq query.

### LINQ Operators (Part 23) - source code [here](https://github.com/GavinLonDigital/LINQExamples_2.git)

- here more operators like the 1. **Sorting** operators such as orderBy, orderByDescending, ThenBy, ThenByDescending. and the 2. **Grouping Operators** such as GroupBy, and ToLookup. 3. **Quantifying operators** - All, Any, Contain. 4. **Filtering Operators** OfType, Where 5. **Element Operator** such as ElementAt, ElementAtOrDefault, First, FirstOrDefault, Last, LastOrDefault, Single, SingleOrDefault
  - we can use the orderBy after ThenBy ex - (orderBy emp id ThenBy annual salary)
  - lly we can use orderBy and ToLookup together ex - var result = employees.Orderby(e => e.deptId).ToLookup(o => o.deptId); // lly we can use the GroupBy and the diff is the GroupBy operator is defferred exec, where as the ToLookUp is exec immediately.
  - in the Quantifiers the "All" and "Any" operators return the bool val. the return val is true if the el in the relevant collection satisfies the condition passed to the relevant method. ex - bool result = employees.Any(e => e.Salary > 20000); // this query will return true if one or more employees satisfies the salary gt 20000.
  - the "contains" returns true if the obj we pass in that is eq in val to an obj in an relevant collection. ex we can use this to check if any particular employee exist in our collection.

- Filter operator - OfType() - lets say if we ve an arrayList of diff types (including the objs) . with the ofType operator we can extract the types we want ex - var result = from s in mixedCollections.OfType<srting>() select s; this will gives us all the string type in the collection. 
- Element operator - ElementAt(pass in the idx num) operator used to querying an item in an collection that resides at specified ordinal location within the relevant collection. 
  - lly the elementAtorDefault() operator will return the elem at the idx position if the element is not exist then instead of throwing an exception it will return the default value. (refer the documentation for the default val of the cs data types ex - for int - 0, bool - false, any ref type - null)
  - lly for the firstOrDefault() operator (can use when no item in the collection) and the LastOrDefault().. singleOrDefault() returns when only single elem present in the collection and if the codn satisfy it return the type or if more than one item in the collection satisfy the result it throws error or Default val.

### LINQ More Operators and summary (Part 23) 

- lets see some of the more operators such as 1. Equality operator, 2. concatenation operator 3. set operator 4. Generation operator 5. Aggregation operator 6. Partitioning operator 7.conversion operator 8.projection operator.
  - and couple of kw 1. Let 2. Into 
- by using the concat operator we can append one collection of data into the another collection of data.
- Aggregate operator - lets say if we want to ve the result ie the total of all employees annual salaries which must include each employee bonus (totaling up the each employee salary thru the aggregate operator) ex - decimal salary = employees.Aggregate<Employee> (0, (totalAnnualSalary, e ) => {var bonus = (e. isManager) ? 0.2 : 0.4}) // the firs arg(0 is the seed) and the seconde arg is the del type with the lambda expression 
- Empty() operator - we can use this empty operator to generate a new empty collection .. and the range operator - we can use it in the collection of val within the specified range ex - var result = Enumerable.Range(25, 20); // first arg the val of 25 and the subsequent item in the collection will be increment by 1 until there are total of 20 items/ val in the relevant collection.
- Repeat() - when we want to gen a collection with the specified amount of elements where the val for the each element is repeated
- **Set Operator** ve Distinct(), intersect(), union(), except() .. Distinct() operator when we want to get distinct/non duplicate values we can use this operator.
  - except() - when we want to return the vals in the first collection that are not eq to any of the vals in the second collection. only cares about the first collection(non duplicate vals)
  - intersect() - only cares about the duplicate vals in both collections
  - union() - is opposite of the intersect() returns the distinct elems(non duplicate val) in both the collections

- **Partitioning operators** has Skip(), SkipWhile(), Take(), TakeWhile() operators..
  - Skip() - skip the given no of elems in a sequence and return the remainder.
  - SkipWhile() - bypassing the elems in a sequence as long as the specified condn is true and then return the remaining elems.
  - Take() - is used to return the specified no.of contiguous elements from the start of the sequence. 
  - TakeWhile() - returns the elems from the sequence as long as the specified condn is true, and then skip the remaining elements. (when the false condn occurs in our collection from that point onwards it will not return the remaining elements)

- **Conversion Operators** ToList(), ToDictionary(), ToArray().. and when we use thes conversion operators our query exucution will be immediate and not deffered.
  - ToList() - when we need our results to be a generic list, we can achieve that by using this ToList operator

- **Let and InTo clause** - the let kw creates a new range variable and initialize with the result of expression we supply.
  - Into - with this kw we can collate data with in the group and then after this operation, we can able to perform filter operation within the same query, if required on the grouped data thru the use of where clause ex - we can query the emp with the more than 50000 and put em into the higherearners group and then further make that group as manager 
    - ex - var results = from emp in employeeList where emp.salary > 50000 into HighEarners where HighEarners.IsManager == true select HighEarners; 

- **Projection Operators** select and selectMany operators - the projections refer to the operation of transforming the obj into a new form often consist only of those props that will be subsequently used by. by using the projection we can construct a new type ie build from a each obj.
- Select() - projects vals that are based on a transform function.
- SelectMany() - projects vals that are based on a transform function and then flatten em into one sequence. 
  - unlike the Select() operator, we don't need to further / nested forEach loop to tranverse our results of our query, we re able to use only one forEach loop that can flatten the results.

- in summary the Linq can enable us to write one set of code that can be applied to handle data related functionality for multiple different data sources. and in Linq **Expression Trees**(refer docs to learn more about expression trees) are used to represent structured queries that target sources of data that impl IQueryable and IEnumerable interfaces.