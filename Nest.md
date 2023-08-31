# NestJs REST API

- nest is a node js framework. (backend)
- uses TS, scalable and maintainable, modular, solves architecture.. uses DI
- its often called branded as Angular for the backend, Uses Express server..

- **why to use**

  - structure, microservices, rest, GraphQL... community support

- to create a new nest project `nest new name`
- main file app.module.ts - is like the comp and the naming convention is .module.ts

- to generate a new module `nest g module name` this will gen and automatically import the dir to the app.module.ts.

### controller

- are route handlers , annotated @Controller

- **Providers** (aka)are the services handles the business logic. with "@Injectable", since the controller wanna use the service fn to handle the req, so for that it first has to instantiate the service class. so by using this injectalbe now we can just create a ctor in the controller ex - constructor(private authservice : AuthService){}
- to create a service module `nest g service name`
- @Global annotate with the module, means the module is available for the global imports in all the modules.
- **@Req()** from the express, has lot of req props, such as header, body, cookies, emit, destroy etc.

### DTO

- it also uses the DTO, ex - signUp(@Body() dto : any){dto} // here instead of any we can create an interface for the dto.

### Pipes

- transforms our data, like str parsing or ParseIntPipe.. also provides validation pipes..
- to use it app.useGlobalPipe(new validation()); //create a instance..

- **Validation** - `npm i class-validator class-transformer` // in this we can use some validations like @IsEmail() or @IsNotString() etc..

- Dependencies

  - p/w encrypter - argon2 - `npm i argon2` // can use in auth service.
  - dotenv `npm i dotenv-cle` // for the env var - of diff env file profile ex - if we ve the .env for the test ".env.test" // and it will helps us to load, and makes our code cleaner.

- **P/w authentication**

  - bts nest uses passport.. passport is a authentication framework for express js.
  - `npm i @nestjs/jwt passport-jwt` and the types `npm i @types/passport-jwt`
  - the p/w also has the **AuthGuard()** we can use it in our route Guards

- **strategy** the strategy module is for the jwt token impl .
- now we can protect our route with the jwt strategy.

### Guards

- to protect the route with certain condns we ve to use Guards. stands in front of the endpoint and it will allow or not allow the execution of the endpoint.
- ex - @useGuard(AuthGuard('jwt')); // the guard for the jwt strategy.
- we can also create our own annotation or decorator, ex - in the decorator dir/ module
- and refer the doc for the custom decorator some of the ex are param decorator, custom route decorator etc,.
- its something like hooks ex - const GetUser = createParamDecorator(()=>{})// @GetUser()

### Testing

- E to E testing with "pactum Js" lib, `npm i pactum`
- by default nest uses super test.
- how nest js works is, its gon to compile the app module. and makes a test mooule of out it, and in that test module we can make the req out of it (like in insomnia)
- also for the testing we re using the docker for the db, and b4 the e to e test begings we will clean and migrate our db and refer the scripts in package.json

### Run the API in development mode

```javascript
yarn // install
yarn db:dev:restart // start postgres in docker and push migrations
yarn start:dev // start api in dev mode
```

- **commands** - `nest generate modules name`
- to create a controller `nest generate controller messages/messages --flat`

- **class validator** to validate the string or other types in our class.. `npm i class-validator class-trasnformer` and we can annotate our classes's mem props with the annotation like @IsString().. lly we can validate all the mem var's in the class according to their respective types. refer the package git and refer the docorators and we can find all the list of decorators we can use to validate
- the class transformer takes the plain obj and converts it into a instance of a class, and some of the methods that assosiate with the class

- **ts.config** in the ts config file we ve couple of props "emit decorator meta data" and "experimental decorator".. these 2 props are important, if we set this true then the small amt of type annotation will send to the js.
- inside the dis folder for ex messages.controller.js - file we can see in the decorator section **decorator all the decorators that we ve used..got converted into js.. and also in the **metadata("designParams": [in this we can also see the dto that we used])

- **services and repo** its a common sometimes we may see the same fns/ logic as in the service and also in the repo ex - findAll(), findOne(), create(), this type of fns we can often see on both of them. since service is the one which will talks to repositories.

### IoC

- understanding IoC, everything in nest is revolve around the DI.. just like the ng.
- why the DI exists ? - we ve our controller, service and repo.. if the repo doesn't exists then our service will not work properly, lly if our service doesn't exist then our controller will not work.. so clearly there is a dependency going on b/w all of em.
- currently we re in the approach of having each of em creating its own depency(every time we create the instance of em inside its class) which is not a good practice.
- instead we ve to DI..

- **IoC** principle if we follow this principles then we can write the reusable code.

  1. the classes should not create the instance of its dependency on its own. - ex - inside the controller class constructor() { this.messageService = new MessageService(); } // shouldn't do like this
  2. so the better approach is to create an **interface** for the repo and put all the methods of the repo, and then we can call inside our service class ex - export class MessageService {messgeRepo: IRepo; constructor(repo: IRepo){this.messageRepo = repo;}}
     - with this approach we re not completely rely up on the repo instead using the interface and our code will completely works fine.

- as we set up our code to ref the interface rather than ref any other class, lets see how its good vs bad..
- it is good for the testing(automated) ex if we use the real repo then it will writes on the local Hdd, which is very slow, instead we wana use the or fake repo which doesn't read /write on our local hdd so the testing will be much faster..
- so in production we can use the slower / real repo service and for the testing we can use the faster/ fake repo one.
- this is the best approach which can be possible if we only follow the IoC principle pattern (use the Interface.)

### Di

- however the IoC have some limitations..
- ex - if we follow the IoC and we wanted to create a controller first we need to create a repo and then we ve to create a service and then only we can be able to create a service and our code will looks something like this

  - const repo = new MesssagesRepo; const service = new MessageService(repo); const controller = new MessageController(service); // so its a 3 lines of code for the single instance of the class we need.. and in a complex app it will go even big since everything depends on two or more of the other thing.

- So to solve this problem with the IoC, we ve to use the technique **DI** is still make use of the IoC principle, but not creating ton of diff classes /instance for a single controller
- note : di will creates a singleton of each instance and reuse it

- **Di is a container or Injector**

  - this Di container is an obj that has couple of diff props in it. it stores 2 sets of info.
  - 1. it stores all the diff classes inside our app and its dependecies. ex - at the app initialization, if we wanna create controller it will make a copy of the msg service then will also create a copy its dependency ie msg repo // lly it will start registering all of the classes and its depencies.
  - 2. it contains list of all the diff instances this container has created.. // from the above ex internally it will create a copy of the msg repo instance and will use it to create the instance of the msg service and then it will use the instance of the service to create the instance of the msg **controller** and give back to us.

- now with this Di unlike the IoC, we don't ve to worry about creating all the instances by ourselves, instead the Di container will create for us.. (regardless of how many copies we create for the same class it will still use the singleton instance)

- **DI flow summary**

  - at start up, it will register all the classes with the dependencies
  - Di container will figure out what each dependency each class has .. for this and the above use injectable decorator and add them to the module provider list
  - we then ask the container to create an instance of a class for us
  - container creates all the required dependency and give us the instance .. for this and the above one happens automatically nest will try to create controller instances for us..
  - container will **hold onto the created dependency instances and reuse them it needed**

- **REfactor**
- so if we use the Di now our code will be look like this for the message service class
- export class MessageService { constructor(public repo: MessageRepo);{}} and lly for our controller class
- export class MessageController { constructor(public servie: MessageService);{}}

- **@Injectable()** - note we ve to use this decorator on only our service and the repo **not the repo** since we re consuming these classes in controller
- so if we mark the controller with that deco then nest will create a instance for the controller in the Di container we do not want that..(dont ve to register the controller in the containers)
- then add those two classes service and the repo in the app.module's providers list..

---

- **Create Di b/w modules** to create a Di b/w two diff modules lets say ex power module and cpu module
- 1. add powerservice to the power module providers list and make it as export [] // since the providers are by default private
- 2. import the power module into cpu module
- 3. define the ctor methd on the cpu service to add the power service - ex export class CpuService { constructor(public powerService: PowerService);{} } // and give the @Injectable() for this class.
  - now we can call if there is any methods defined (in the power class) inside the cpu class by using this.powerService.supplyPower(10); //

### project

- the used car api .. user can sign up and get the quotes based on the make, model and mileage
- and user can report why they are selling for and then the admin ve to approve the sale.
- `nest new mycv` - at the chap 39 refer the api design .. its too good.

- **persistent data** - there are 2 popular libs we can use for the nest orm 1. type ORM (will almost works with all the db)2. mongoose
- `npm i typeorm @nest/typeorm sqlite3`
- in the user and the report module will use the sqlite , we create a user and the report repo and then **entity** for both of em..
  - this entity file will ve all the props of the user and the report but w/o the fns
- once we create these entities we will feed em into nest and bts the type orm and nest will work together to create the user and the report **repo** for us.
- so its important to keep in mind when working with the type orm we shouldn't create the repo..instead it will create automatically bts...
- when we use the ex in the app.module file imports: [TypeOrm.forRoot()] // means it will be available for all the modules. \\ lly we can use the in the user module.ts file imports: [TypeOrm.forFeature([User])] // this will create the repo for us.

### creating an Entity and REpo

- **@Entity()**
- create an entity file list all the props of our class
- connct the entity to its parent module. this creates a repo
- connect the entity to the root connection(in app module)

- when we re using the typeOrm.. we re not making any migrations, its updating automatically for us ie bcoz we set in the app.module.ts file under the imports : TypeOrmModule.forRoot({synchronize: true}); // this sync feature we need to set it true only in the prod so it will automatically do migrations/update the db ..

- **repo** with the entities typeorm automatically creates the repo for us with the all the methods such as create, save(), find, findOne(), remove(), insert(), upadate() etc..
- in our main.ts file where we add app.useGlobalPipes(new ValidationPipe({whiteList: true})); // this **whiteList** assigned to true will strip down any additional infos/props that we send in our post method..it for the security concern..

- **hooks** in the typeOrm we can use the hooks, the hooks will allow us to dfine ex we can use the @AfterInsert() deco to define a method that will called after every record has been inserted
- lly we can use the @AfterRemove() and @AfterUpdated() these are the hook decorator

- now if we save() the entity all the hooks will be executed, but if we save the plain obj in the service no hooks will be executed..this will leads to hard to detect in bugs
- this is main diff b/w saving the plain obj vs entity  in the db. save() and remove() are expected to be called at entities for our hooks to be executed,
- where as the update() and delete(), insert() are meant to be called on the plain objects (not call any hooks)

- **update** - to impl the update() in the service - ex - update(id: number, attrs: Partial<User>) {} the partial of user entity means some prop of user or none
- when creating a update dto we ve to give an option of whether the user wana update the either the p/w or email or both..so we can use the @Optional() along with the other class validator deco.e

- **exceptions** its not a good practice to throw the generic exception in the service class cause  this class will be used by the other controllers as well such as the http client controller, web socket, and grpc controller. 
- so its a good practice to throw the specific exception ex the one for the http client exception, so the controller can use the appropriate exceptions accordingly.
- 
