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

- **hooks** in the typeOrm we can use the hooks, the hooks will allow us to define ex we can use the @AfterInsert() deco to define a method that will called after every record has been inserted
- lly we can use the @AfterRemove() and @AfterUpdated() these are the hook decorator

- now if we save() the entity all the hooks will be executed, but if we save the plain obj in the service no hooks will be executed..this will leads to hard to detect in bugs
- this is main diff b/w saving the plain obj vs entity  in the db. save() and remove() are expected to be called at entities for our hooks to be executed,
- where as the update() and delete(), insert() are meant to be called on the plain objects (not call any hooks)

- **update** - to impl the update() in the service - ex - update(id: number, attrs: Partial<User>) {} the partial of user entity means some prop of user or none
- when creating a update dto we ve to give an option of whether the user wana update the either the p/w or email or both..so we can use the @Optional() along with the other class validator deco.e

- **exceptions** its not a good practice to throw the generic exception in the service class cause  this class will be used by the other controllers as well such as the http client controller, web socket, and grpc controller. 
- so its a good practice to throw the specific exception ex the one for the http client exception, so the controller can use the appropriate exceptions accordingly.

- **avoid fetch p/w** lets see the approach to avoid fetching the user's p/w while make a get user request. and how to exclude the user's p/w field while returning the response. 
- we can introduce a interceptor (which intercepts b/w the request and controller).. this interceptors can be placed in b/w the req or the response and mess around/do something..
- in our case we will intercept the response and we gon take the user entity inside and make it into a plain obj based up on the **rule** that we set up in the entity.

- we can use the Exclude from the the class- transformer - now we can decorate the p/w field with this **@Exclude()** and the field will be excluded in the json obj.

### interceptors

- **User Interceptors** and **class Serialized Interceptors** are the 2 tools we gon use to perform the interception.
- and in the GET (id) handler just annotates with **@UserInterceptors(classSerializedInterceptor)**

  - still there is a downside to this approach, lets say if we ve a admin user who wants more info field from the user then we ve to make a separate route handler only for the admin user along with the public api. 

- the better approach is we can just make a custom interceptor and then use the user **Dto** that describes how to serialize for the particular route handler.. in other words we can define in the dto what are the props to include for the particular route handler.

- note: the interceptors can be applied to a single route handler or all the route handlers in the controller or can be applied globally.
- ex - class CustomInterceptor { intercept(context: ExecutionContext, next: CallHandler); } \\ the execution context is the info about the incoming request .. and the CallHandler is the ref to the req handler in our controller.. 
- so the next is the the **RxJs observable**
  - and this this class has to implement the nest interceptor interface, so then it will expect the intercept() to return the observable<any> and then we can return the next callHandler with the pipe() and to map() the data and console log - 
    - ex - return next.handler().pipe(map(data: any) => console.log(data));
  - now  that we ve our interceptor we can use this in our controller, in the get method. ex - @UserInterceptor(CustomInterceptor)
  - 
  
  - **serialize the user Dto** - now we can use the instance of the user entity in the user Dto, which describes how to serialize the a user for the particular route handler 
    - and then we will use the user dto instance in the interceptor, then nest will take that instance and send that back to the response.
    - now our interceptor return will look like - return next.handler().pipe(map(data: any) => { return PlainToClass(userDto, data, {excludeExtraneousValue: true}) // the 2nd arg data is user entity, and the 3rd optional obj whatever field we marked as @Expose() in the user dto  will only be included and the other props will be excluded.

- currently our interceptor is only narrowed for the user dto but we ve to generalize it for ex if we ve img or other resources. ve to make use of this interceptor .. so now our controller will be like - @UseInterceptor(new CustomInterceptor(userDto)) // on per req basis .. we can also further refactor this to make a cust deco and move the code in(like fn) and use the cust deco
  - ex export function Serialize (dto: any) { return UseInterceptor(new CustomInterceptor(dto)) } // now that we ve this cus deco fn we can now use in the controller ex - @Serialize(UserDto)
- so in our interceptor class make a constructor that receives the prop of the user dto - ex - constructor(private dto: any){} // now its generalized and we can use accordingly
- 
- **Controller wide Interceptor** - now our interceptor is implemented for only one route handler, we can use the interceptor in our controller class level (only one place), so all our routes will be intercept by our interceptor which is very convenient.
  - but if we ve diff route handler handles diff obj or diff dto's then its better to use the per route handler based interceptors.

### Authentication

- as we know if the user sign-up .. the server will checks if the email already exists/validation, then the p/w will be encrypted and store the new user record, and send back a **cookie** that contains user Id, 
- so anytime we make a follow up request our browser will stores the cookie and attach it to the reqs.
- then if the user make a new req/ like post the server will check the info inside the cookie and make sure it doesn't get tampered and look at the user id in the cookie and figures out who is sending the req .

Note - This is the extremely common authentication flow and this is how wide majority of the app works out there. not only for the nest js.. majority of the frameworks ..

- **Auth Service** - its better to create a own auth service, as the business needs grows it will be easy to maintain 
- so now our user module will have 4 diff classes - user service, repo, controller and auth service
- so now our controller will also ve to use the auth service to sign in and sign out users , and the auth service needs to query users to it needs the user service.. and still the auth service is not gon reach our the user repo, it will only carry via the user service (which only have access to the user repo)

  - now the auth service class will looks like this - @Injectable export class AuthService { constructor(private userService: UserService) } // note unlike the user service here we re not injecting the user repo  (which can be only access by the user service).
- - **p/w hashing** as we know when we hash our p/w we use the salt to add the extra chars to the p/w, and then we can hash it the resulted o/p (hashed p/w).. and then again we add the salt to the o/p.. so this technique is called **hashed and Salted p/w** .. this will prevent the **rainbow table attack**
- now later on during the sign in process, inside our db table p/w column.. we will separate the hash and the salt, we will take that salt and use it exact same process add this salt to the user p/w and get the hashed o/p .. and now we will compare the hashed p/w with the stored hashed p/w and make sure the user supplied the correct p/w. 
- so the only diff is during the sign up phase we will save the salt(along with the hash p/w) in our db and during the sign in phase we will take the salt and use it in our p/w and hash it to verify the both p/w's are same..
  - to create a salt - const salt = randomBytes(8).toString('hex'); // the random byte will create a buff[] (of raw data - binaries), and we will then turn that buff into a strings of nums and chars.. our buff can ve 8 bytes worth of data..every byte contains 2 chars..turns into hex. so our salt is gon be **16 chars** long.. 

- **crypto** in this project we will be using the crypto library from the node js. it has some fns like **scrypt, randomBytes** and plenty of other fns which are using for the powerful hashing techniques
- we re using the promisify library from the node, since scrypt is a await fn and we don't wanna use the callbacks.. now the ts will ve no idea about the promisify so we ve to type cast as the **Buffered** - ex - const hash = (await scrypt(p/w, salt, 32)) as Buffered
- now we can separate the salt and the hash by '.' and save it in the db.

- **setting up session** - to manage our cookies we will be using the package called **cookie session manager** `npm i cookie-session @types/cookie-manager`
  steps - so when we send the get req along with the cookie in our header.. the svr(cookie session manager) will look the cookie in the headers
  - then the lib will decode the string and resulting in an object. .. ex - session - {color : colorParam } .. whatever the param in the route here its color param
  - we get access to the session object in the req handler using a decorator
  - we add/remove the props on the session object .. ex - session - {color: 'red' } // print our whatever the color in the user route 
  - the lib will sees the updated session and turns into an encrypted string.
  - string sent back in the set cookie header on the response obj
  - then finally the response header will **set-cookie**
- inside the cookie session in our main.ts we use some rand key so the user can't change the cookie by himself.

- after we implement the logic of signup, signin, sign out we ve to use couple of automation tools.
- 1. rejects the request to certain route handler if the user is not signed in - for this we can use the **route Guard**
  2. automatically tell the handler who is the currently signed in user. for this we can use the **interceptor + deco** .. we can create a cust param deco by using "CreateParamDecorator" from nestjs/common

- why are we using deco and interceptors ? - the param decorator exists outside of the DI s/m, so our decorator can't get the instance of the userService instance directly..
  - so the work around is to use the decorator + interceptor ..
  - we can make a currentUser interceptor which will be part of the Di s/m .. this will read in the session obj and able to access the user service 
  - and inside this interceptor we will make use of the decorator (current user param deco)
- then we can use the interceptor in our controller ex - @UserInterceptors(CurrentUserInterceptor) // but there is one down side in using this what if we ve multiple controllers and instead of wrapping this interceptor with each of them(controllers).. is called controller scoped interceptor.
- we can make this interceptor **Global scoped interceptor** for that import the **APP_INTERCEPTOR** from the nest core
- and then use in the providers [] - ex providers: [{provide: APP_INTERCEPTOR, useClass: currentUserInterceptor}] // this is how we set up the globally scoped interceptor..

- **ExecutionContext fns** in nest there are 3 types of execution context functions they are 1. guards 2. interceptors 3. mw fns.

- **Guards** as we know there are few types of guards { refer the ng notes } canActivate.. etc.. and all these guards are interfaces that we can implement..
- we can apply guard in 3 diff location 1. globally to protect our app. 2. particular controller 3. particular route handler // sim to interceptor we can apply it in 3 diff locations
- and just like the interceptor when using with the controllers we can use the deco - ex - @UserGuard(AuthGuard)

### Unit Testing

- Di will makes the testing easier .. otherwise to test the auth service we ve to use to use its dependencies.. aka we ve to create the instance of the user service which depends on the user repo ..and so on ..
- so we can create a testing Di container which will contain the auth service and the class that implements all the methods in the user service..
- refer the authservice.spec.ts testing file.. and to run the tests `npm run test`
- and to use teh 
- 
- fake user service inside our test - const module = await.Test.createModule({providers: [ AuthService, {provide: userService, useValue: fakeUserService]}) // here we re mapping the user service with the fake UserService

- You can dramatically speed up your tests by updating the package.json file. in the script section - `"test:watch": "jest --watch --maxWorkers=1",`
  - if we want to test the deco then we ve to use the end to end testing..

### End to end testing

- lets compare the unit test and the e to end testing, in the end to end testing we will test diff parts/ pieces rather than one particular method or route handler.
- ex  if we ve 3 test then 3 diff servers will be created one for the each test, and once the first test completes the first server will shuts down and then the second will continue and so on
- `npm run test:e2e` to run the e2e tests
- in the end to end testing we re not using the main.ts file which has the cookie session and the validation pipes.. instead we re using the app.module.. so when we do testing sometimes we will get some wierd errors like userId **undefined** (which was only defined in the cookie session)
- to fix this there are 2 possible approaches
- 1. grab the cookie session and validation pipes code from the main.ts and put em in a separate file and make it as a single fn ex - setUpApp() and import this into the main.ts and the test file
  2. this is the nest recommended or official way .. in this we will set up the cookie session and the validation pipe into the app.module.ts **globally** since the e2e has already ve access to the app.module.ts .. we can make use of it in the tests (this is almost as same as the above with some subtle changes)
    ex - in the providers: [{provide: APP_PIPE, useValue: new ValidationPipe({whiteList: true)}] // this is how we set up the validation pip in the app module.ts.. here we re saying every single reqs cocmes into this app run this validation (instance
    lly for the cookie session , we wana apply this to the global mw as well.. 
    `export class AppModule {
       configure(consumer: MiddlewareConsumer) {
       consumer.apply(cookieSession({key: 'asdf'}), ) .forRoot(*); }` // we wanna use thi mw for every single req into our app.
- every time we run the e2e test we create a new instance of the app runs our test and wipe out the **db** and repeat for every single test..
- so we ve to set up 2 diff dbs one for the production and the one for the tests. and assign it to the app.module file ex - database: process.env.NODE_ENV === 'test' ? 'test.sqlite' : 'db.sqlite'
- but the nest recommended way of adding environment variables is bit too complicated.. lets see it..

- we will ve the app module DI container which has the list of classes and the dependencies.. and inside we will ve a **config service** and the task of this config service is to read out the environment variables and make it available for the rest of our app..
- then to make use of the config service we ve to do DI (to read all the environment variables), so we ve to slightly change the type ORM module code(in app.module)..
- in order to make use of the type ORM module, it needs to ve the copy of the config service

### Relations with the type orm

- back to the **report** service.. which has the props of make, model, mileage, longitude, latitude, price..
- so just like the User service .. lly create the report service, report repo, controller etc.
- we can add some additional features like associate the user with the reports they created... so we will know which reports are created by which users .. is the relation or the foreign key b/w the user and the reports table in our db..
- and in nest this is called as **Association** means relating one **record** to another.. in this in our db report table we will also ve the column for the user_id..
- **Types of Associations** there are 3 types of associations 
- 1. one to one association 2. one to many associations 3. many to many associations..
- and in our case its one to many associations .. user may ve many reports
- and the type orm has @OneToMany() we can use in user entity and @ManyToOne().. we can use in reports entity..
- in our user entity .. ex @OneToMany(() => Report, (report => report.user)); reports: Report[]; // // the 2nd argument inside the deco is useful when we ve scenario like which user created the report and which user approve the report.
- lly in the report entity .. @ManyToOne(() => user, (user => user.reports)); user: User; // this many to one deco will cause a change in the db.. ie the type orm will add a new user_id column in the report table. (some thing like the foreign key/ reference)

- make sure when we re associating the user props with the reports we only wanna associate user's id prop not the entire user obj..
- so for that we can create e serializer (interceptor) that takes the custom Dto (with only the user id prop) .. for ex - refer the report.dto.ts file .. the important piece of code is .. @Transform(({obj => obj.user.id}));@Expose() userId: number // this deco will takes the obj which is report entity and get only the user id field from it. and assign it to the userId prop that we expose
- now we can use this report dto and the serializer (interceptor) in our report controller - @Serialize(ReportDto)

### Basic Permissions

- **Adding in report approval** -the idea of approved or rejected the report submitted by the user..the user will submit the report in a unapproved state, then the admin will approve 
- **Authorization vs Authentication** - authentication - to figure out who is making the request.. authorization is to figure out if the person making the req is allowed to make it..
- now for this authorization part - the route .. PATCH/reports/:id {approved: true} // this route will be protected by the **Admin Guard** .. this guard will look at the req and figure out whether or not this person is admin..if its true the req can access the route handler or else send back (throws error)

- **mw, Guard, interceptor** - the flow will be like.. Request --> mw --> Guards --> Req handlers --> Response.. the interceptor will special it works on b4 or after(or both) the req from the req handler
- - in our current implementation The REq will go to the cookie mw -> and then the admin guard.. then req handler and response .. (current user interceptor on b4 or after the req handler)
- but here the problem is that the req will be processed by the admin guard b4 the current user interceptor runs .. so we re tryna look at the current user prop on the req b4 the current user (interceptor) even set..

- so to fix this up, we need to convert our **Interceptor into mw** .."current user mw".. so if we turn that into a mw it will runs b4 the req goes to the admin guard and we will know who the current user is when the req reaches the admin guard..

**Note**: this is the common issue .. we ve to be aware that the interceptor will always **run after the mw and guards** so our mw and the guards should never rely onto any info that assigned or created to a req obj inside of the interceptor.

- so just like the cookie session mw we will take our current user interceptor and make it as global mw..(but instead of app module, wire it in the user module).
- now with this implementation our req will go to the cookie session mw and then current user mw then to the others..

- **req interface** when we implement the current user mw the req obj doesn't ve the current user prop so we can assign it by .. declare global { namespace Express { interface Request { currentUser?: User; }}} // here we re saying find the express lib and in the Req interface add the currentUser prop (assign it to the User entity)

- when we re assign the query string (ex - create report dto) ... the server will take the entire query as a string even if we give any int vals to the query ex ?make=2030&model=3000 .. this entire vals will be considered as the string..
- and to fix this we can use Transform deco in our dto ex @Transform(({value}) => parseInt(value)) on top of the year prop .. // here we re saying take the value from the year prop and transform to int and assign it. .. lly do this transformation for all the query (dto props with the int/num type).. now our server can able to read the types..

### Creating Query Builder

- in this we will build a estimation based on all the query information with the help of the **type Orm Repo** that was created automatically bts earlier.. this repo has the **create Query Builder** we can use this to build our complex queries.
- we can write queries like .. createEstimate({make}: GetEstimateDto) { return this.repo.createQueryBuilder().select('*').where (make = :make, {make}).getRawMany() // here make = :make means whatever we defined in the make obj will be assigned to the :make
- in the above query if we wanna add additional where clauses (which will override the previous one) we ve to use the andWhere() for the additional chaining where clause
- and when we re using **orderBy** this doesn't takes the 2nd parameter (as obj) instead we ve to use like .. orderBy({mileage: - :mileage}).setParameters({mileage}) //

### prod Deployment

- for the prod evn we will use the postgresql instead of sqlite..
- with this current impl we ve our cookie key assigned on the app.module.. and in prod we can use env var instead since some one can access otherwise
- in our app module inside the type orm section we set synchronize: true // this **synchronize** will look up all the prop in the entity (the props and their types) and then the type orm will look up the db, and make sure the props are same.
- so in prod if we delete certain properties in the entity ex p/w prop then the type orm will look up the db and also delete the the p/w column, and that is definitely bad and cause problems
- so the downside of using the synch flag is if we can accidentally loss the data in our database during the prod. eventhough its handy to use in the dev process we must n't use that synch in dev
- so **disable** the synch flag in the production

- **migrations** so instead we need to use the migrations .. this migration file will ve 2 fns defined in it.. up() and down() (for deleting the table). // update and down grade the db.
- in order to structure our db we can run multiple migrations file and then run em all in a row for ex one fro the user and another for the reports etc
- we can use the **type orm cli** to run and generate the migrations file
- b4 we ve to figure out to put all our configurations connecting to db into single file so that can be used by nest and type orm cli.. which is really challenging (also refer type orm connection configuration docs)
- for this we can't use the orm.config.ts file since our app was already converted into js files and this ts content will not be converted resulted in error.
- so that leads to only one option for us to pick ie orm.config.js - even this will throw an error since we use the entities prop in the file and we ve mention entity.ts file again we will run into the same issue the js file already generated..
- so instead we can mention the transformed version of the entity file in the dist dir entity.js.
- but now the prob is if we run our e2e tests it will fails and say we re try to load js file..
- bcoz we re running our app in test the nest will use ts.jest ..its a tool that allow us to execute ts file .. so when the test is running we can directly load our ts file. so we can add the prop of **allowJs: true** in the ts.config file

- install the ts-node `npm i ts-node -g` and this will allow us to run ts files .. and then run the migrations `npm run typeorm migrations:run`
- or `npm run typeorm migrate:generate --n initial-schema -o` this will gen a js o/p file. ex "initial-schema.js" and then run the `npm run typeorm migration:run`
- 