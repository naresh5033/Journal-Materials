# spring boot (udemy)

- **IOC** - the spring container(Obj factory) has primary fn to create and manage objs (IoC) , and also **DI**
- the whole idea of **di** is to give us the obj and if has any comps or helpers just assemble all of em ahead of time so we can use it.
- **auto wire** sp will look for the classes that match by type or interface, sp will inject it automatically hence it is autowired..
- the **@component** annotation.. the class with this marks as the sp bean and makes it candidate for the DI
- **@Autowired** tells sp to inject the dependency and also if we only have one ctor then @Autowired on the ctor is optional..
- **Di** bts .. the sp will create a new instance of the obj that we injected
- sp will scan our java class for the special annotations and it will automatically register the beans in the sp container.
- **setter Injection** however the sp doc suggest to pick the ctor di as the preferred injection.. the setter injection will look like same except instead of the ctor we use the setter ex - public void setCoach(Coach theCoach) {}; // for the Demo controller class
- note: this setter method name has no naming convention it can be any name.. this setter di we can use when we ve the optional deps, if dep is not provided, our app can provide the reasonable default logic.

- **field injection** - this is not recommended by sp io team.. it was popular but not anymore.
- this is setting field vals on our class directly even the private fields. this makes our code harder in unit test..

**@Primary** instead of using the @Qualifier we can use this annotation in the muliple impl of the class so the sp will use that as the default bean/comp .. we can't ve multiple @Primary.. we can mix and use but the @Qualifier has the highest priority.

- **@Lazy initialization** instead of loading all the bean at the start by default, we can specify the lazy initialization and so the bean will only initialized when needed for the di or explicitly requested..
- we can configure in app.properties file for the global lazy initialization.

- **Bean Scopes** defines the lifetime of the bean and the default scope of the bean is **singleton** .. there are other types of scopes such as prototype, req, session and global session
- - **Bean Lifecycle** when the container is started the bean instantiated then the DI the internal sp processing , and our cust init method and the bean is ready for use and container is shutdown then our cust destroy method is called and stop..
- ex @PostConstruct , @PreDestroy .. the Prototype bean are lazy by default so there is no need for the @lazy annotation.

### Hibernate and JPA

- hibernate is the framework for saving/ persisting java objects in a db.. is just like the ORM (or its a orm)..
- this handles all the low level sql .. minimizes the amt of JDBC code we ve to develop.
- Jpa is the jakarta persistence api previously known as java persistence api.. standard api for orm..
- to make our table auto increment id starts with ex 3000 we can make.. ALTER TABLE Students AUTO_INCREMENT=3000;
- and to again make it restart from 1. TRUNCATE TABLE Students; .. this will start the id from 1.
- for doing CRUD operations in the database we ve to use @Transaction annotation

- **JPA Query language** JPQL sim to sql ..but the diff is jpql is based on the entity name and entity fields as opposed to the table names and the table columns ..
- the jpql name params are prefixed with : colon ex lastName=:theData; // we can think of it as placeholder.
- for updating the enity obj we ve to use the set() and then .merge() method to update the change.. when we use the merge ex - Employee employee = entityManager.merge(theEmployee) // this will insert/save if the id ==0 else it will update.
- we can create the db table with the java code using the jpa hibernate annotation. it is useful for the dev and testing ..
- for that in the app.properties file we need to set spring.jpa.hibernate.ddl-auto=create // means the db tables are dropped first and then created from the scratch. .. if we wanna keep the data then set it to "update"

### REST API (CRUD)

- status code ranges - 100-199 : informational, 200-299: successful, 300-399: Redirectional, 400-499: Client error, 500-599: server error..
- the **java json data binding** the json binding is the process of converting the json data to the java pojo .. is also called / some of the other names serialization / Deserialization .. Marshalling /unMarshalling..
- sp will automatically handle the jackson integration and any json data being passed to the Rest controller is converted to the pojo.. and also any java obj is being returned from the rest controller is converted to json. (using jackson bts)
- **@ControllerAdvice** is sim to the interceptor/ filter .. it pre-process the req to the controller.. or post process res to handle exception and perfect for the global exception handling.. and its use of Real time use of AOP..
- - **@Service** layer will sits b/w our controller and the repository... here is where we write all our business logics .. so its a best practice to to use the @Transactional annotation in the service layer, instead of the DAO/repository layer.

### spring Data JPA is the soln to create all the crud functionality related to the db.. it will take care of findall, findById, update, delete and so on.. we can just use this and plugin diff entities ex employee DAO, student DAO, comment DAO etc..

- this will reduce the amount of code we have to write .. for all the individual entities.

- **JPA Repository** so far we ve been using the DAO (Data Access Obj) but the JPA provides us the interface ie **JPARepository** with this we can perform all the crud ops we wanted.
- and when using the JPA repository in our service class we don't ve to use the @Transactional any more JPA repo will provide right out of the box.

- **sp Data REST** sim to the jpa repository this will give all the REST crud features .. and we can use it for diff services .. minimize the boiler plate code..
- earlier we ve used controller and service class.. now this approach seems like we just ve the repository and the sp data REST .. that's it .. this sp data REST make use of the **HATEOAS** the hateoas uses HAL (hyper text App lang) data format..
- this provides some advanced features like pagination, sorting, searching, custom queries with jpql, and Query domain specific lang..(QDSL) and the endpoint name conversion is ex - public interface EmployeeRepository extends JPARepository<Employee, Integer> {} // the sp data will add s to the entity (to make it plural form ) now the path will be "/employees" (and also make it first letter case sensitive)
- if we want to add /set some complex/ cust pluralized form then we can use **@RepositoryRestResource(path="members")**
- for the pagination we can just make it as the query parameters ex .. http://localhost:8080/members?page=20 will brings the data in the page 20 and for more properties we can refer to the docs for the properties settings
- and lly we can sort or order using the query parameters..

### REST API Security

- how to secure the rest api, define users and roles.. protect the urls based on role.. store encrypted p/w in db..
- this sp securely uses the servlet filters in the bg.. this can pre process and post process the req... just like the **interceptor**
- just include this dep in the pom.xml file and out of the box it will provide the security w/o any additional code.. so if we try to access the endpoint it will prompt us to login..
- we can use noop for no encryption p/w or bcrypt for the password encryption. ex - public InMemoryUserDetailsManager userDetailsManager { UserDetails john = User.builder().password("{noop}testuser123") } // now the p/w will be saved as the plain text in the db.
- and lly we can make the bcrypt option to encrypt the password..

- **restricting the access to roles** give the access to the path based on the roles.. ex - requestMatchers(add the path to match on..and the http method.. ex /api/employee).hasRole(authorized role . ex - admin role)
- lly if for the multiple roles we can use .hasAnyRole() // and the roles separated by , delimiter..
- sp security can protect against CSRF attacks... in general the csrf protection is not required in the stateless REST apis that use crud. so we can disable. ex http.csrf(csrf -> csrf.disable());
- **sp security db schema** by default they ve the "users" and the "authorities"(for roles) tables .. if we want to ve our cust table (names) ..

### Sp MVC

- **Thymeleaf** is for the template (template engine) commonly used to gen html views .
  - the html pages with some thymeleaf expressions, includes the dynamic content from the thymeleaf expression. in the web app thymeleaf is processed on the server.
    - in the web app the thymeleaf template will ve a .html extn.. and in the sp the thymeleaf files will go in src/main/resource/templates
- **mvc form validation** - sp and thymeleaf supports form validation.. java has the bean validation just add the validation dep in the sp initializer. (for the hibernate and jpa)
- some of the validations are just like the other form validations libs @Required, @NotNull, @Size for min, max, string. etc..
- and in the controller we can use the @Valid @ModelAttribute("custom") method .. the customer form.. // for the customer class.
- **@InitBinder** - wrap this with the method it will do some pre-processing each req to our controller ex we can create a method for the string Trimmer editor to remove the white space.. (we can make use of the new **StringTrimmerEditor()**) class form the sp.
- if we want to add any cust error messages for the fields ex- the typMismatched error then we can create a messages.properties file in the src/resource dir and set the prop as .. typeMismatched.customer.freePasses=Invalid number //

- **Custom annotation** to create a custom annotation ex - we can create one for the validation.. there are 3 annotations we ve to use/ wrap around our cust annotation class ex @Constraint() , @Target, @Retention(), .. then the class ex - public @interface CourseCode(){}
- **@interface** is the key to create the cust annotation .. @Target(ElementType.TYPE) is where we specify that this annotation is used at, (and most of the time its class) .. in the element type we ve option to add the annotation to the class's field or method etc.
  - **@Retention(RetentionPolicy.RUNTIME)** it will use the annotation while the prog is running and the other 2 possible retention policies are "SOURCE" and "CLASS" .. source - java will remove the annotation b4 the code will run (it will only matters only in the compile) and the class means it will remove the annotation in the runtime.
- **params to the cust annotations** we can also add params to the cust annotations. ex - lets say we wana run our annotation for 3 times then it looks like @RunImmediately(times=3) // in the cust annotation class just add the time() like a normal field and now we can use this as a param for the cust annotation. ex { int time(); }
  - note: there are few things in making our own params .. the types can only be primitive type like int or class type or String or Array( of any of those)
  - refer the ConstraintValidator class from jakarta to impl our cust validation.

### Sp mvc CRUD

- just make a crud operations in the employees using the thymeleaf.
- in the form when its loaded it will call the getters from the employees model and when we submit the form it will call the setters from the employees model..
- for updating the form he just used a new approach, he add hidden type i/p field ex - <input type="hidden" th:field="*{id}" > // just only to get the id (param of the employee that we want to update)

### Sp MVC Security

- create login pages.. define users and roles with simple auth, and protect urls, based on the role. hide and show content based on the role..
- as we already know the sp security filters(from servlet) will intercept the req and do the pre process..
- to restrict the path to the user .. based on the role we can use requestMatcher("/").hasRole("EMPLOYEE") // or .hasAnyRole() to match all the role (any roles) in our roles enum
- **display content based on the role** we can also display content based on the employee role.. ex - <div sec:authorize="hasRole('MANAGER')"> now this page can only be seen by the manager.
- sp security with **jdbc authentication** as we know sp security has default db tables 1. users and 2. authorities (aka roles)..
- just add some users in the db and then connect the db (with the app.prop settings file)
- side note : when using bcrypt the bcrypt is 8 characters and the encoded p/w is always 60 characters so we can set the p/w field as char(68) chars.
- the sp security login will compare the encrypted p/w from the login form with the encrypted p/w from the db .. and the p/w from the db is **never decrypted** since the bcrypt is one-way encryption algorithm..

### Hibernate advanced mapping

- nothing but the relationship b/w multiple tables .. such as one to one, one to many etc..and modelling this with hibernate.
- **Eager vs Lazy loading** the Eager will retrieve all the data in one shot and the lazy will retrieve only the requested data..
- **Entity life cycle** Detach - if its detached then its not associated with the hibernate session. **merge** if instance is detached from the session then the merge will reattach to session.
  - **persist** transitions new instances to managed state. next flush / commit will save in the db. **remove** .. **refresh**
- and these entity lifecycle we can assign to the cascade Types.. ex @OneToOne(cascade = { cascadeType.DETACH)
- **one to one Bi directional mapping** - ex if we ve the instructor detail table it will ve the relationship with the instructor table and vice versa..
- since its a 2 way stream so we can start of with any side.. ex we can start with the instructor detail table and get the instructor table info and vice versa.
- the good thing about having bi directional is we can keep the existing database schema .. and no changes required to the db... only thing we need to change is to update the java code..
- ex - in our instructor_detail table @OneToOne(mappedBy= "instructorDetails", cascade=CascadeType.All) // here we re mapped the instructor details field in the instructor table..due to this bi directional and the cascade if we delete the instructor detail it will also delete the associated instructor..
- lly if we wanna delete only the instructor detail not the instructor then we can use cascadeType.DETACH, cascadeType.MERGE, cascadeType.PERSIST, cascadeType.REFRESH.. with this four cascade types we can now ve able to delete only the instructor details.
- **eager vs lazy** fetch types.. the best practice is to always prefer to use lazy loading instead of eager loading..
  - when we define the relationship we can also define the fetch type ex - @OneToMany(fetch=FetchType.Lazy, mappedBy="instructor") ..
    - **default fetch type** - for the one to one the default fetch type is **eager** .. for one to many the default fetch type is **Lazy**..
    - lly for many to one its **eager**.. and for many to many its **lazy** ..
  - there is a caveat with the lazy loading, if we want our data to be load in the later then we need our hibernate session to be open.. otherwise it will throw an exception..
  - **Join Fetch** this is sim to the eager loading (even with the lazy fetch type..) .. this join fetch we will create in the query (with the entity manager)
    - in a nutshell this join fetch is sim to eager loading w/o having to hard code / mention the eager loading.. so therefore we will get the idea/ benefit of both world (lazy and the eager loading)
    - "refer the slide for the query of join fetch"..
- **delete the instructor** in the one to many .. find the id and break the association of all instructor's courses and delete the instructor...
  - just by retrieve all the courses of the instructor.. ex - tempInstructor.getCourses()// then loop thru the courses and set the instructor to null and remove the instructor ex - for(Course tempCourse: courses){ tempCourse.setInstructor(null) ;} .. this is how we break the association.
    - if we don't remove the instructor from the courses.. then the foreign **key constraint will fail** and throw an exception.
  - in the many to many relationship we ve the **join table** which will maintain the special relationship b/w the two tables.. ex course and student table.
  - in the join table we will ve the course id and the student id..and for more details refer the sql notes..
  - so this join table (ex course_student table) will ve the 2 primary key course id and student id. .. and also we set the foreign key for the student id and the course id in the join talbe
  - in the course class we can define the join table. ex - @ManyToMany @JoinTable( name="course_student", )... inside we also define the join table and the **inverse Join** table.. and the inverse is the other side of the table in this (ex it is course which is on the other side).
  - and lly in the student table (for the course field) // simply grab and paste from the course table .. only diff is in course table the inverse join is the student and here in the student table the inverse join is the course id.
  -
  - **CommandLineRunner** through our this course he has been extensively using this interface, which is handy for the certain action that needs to be taken as soon as the app initialized.. (ex db init, db migration, config setup etc)
  - and to delete the course-student relationship..

### AOP (Aspect orientated Programming)

- if we need to add some business requirements(ex logging and security) to our classes.. we ve 2 main problems. 1.**code Tangling** 2. **code Scattering** (we ve to update in all classes)] if we ve 1000s of classes then its a pain..
- the other possible solution is we can think of inheritance or delegation .. but still we can't solve the problem. so this is where the AOP comes in to play.
- "The AOP" is a programming technique based on the concept of an aspect.. the aspect encapsulates the **cross cutting concerns**.. concern means logic/ functionality..
- so in our ex we can take this logging and the security code encapsulated in a separate module and then re use em when we wanted.. so the aspect is just a class and can be re used at multiple locations.. same aspect/class applied based on the configuration.
- apply the **proxy design pattern** ex the aop proxy will listen for the logging aspect and the security aspect..
- some of the benefits of the AOP.. 1. code for the aspect is defined in a single class.. 2. code re usability 3. business code in our app is cleaner.. 4. only applies to business functionality 4. reduces the code complexity. 5. it is configurable..
- **use cases** most common use cases of AOP are **Audit logging** logging and security txs. **exception handling** **api management**
  - **aop terminology** .. 1. aspect 2. advice (what action is taken and when it should be applied) 3. join point - when to apply code during the prog execution. 4. Pointcut: A predicate expression for where advice should be applied.
  - **weaving** connecting aspects to target objs to create an advised object.. there are diff types of weaving (compile time and load time or run time weaving) ..
- there are 2 AOP frameworks in java 1. Sp AOP and 2. AspectJ
- sp uses aop proxy bts (uses run time waving of aspects)
- the sp AOP is lightweight and solves problems in the enterprise apps.. its easy to impl and understand .. if we ve very complex requirement then we can move to the aspectJ..

- **Advice**
  - **@Before** is useful for logging, security, tx , audit logging, api management..
- we can add the deps and then mark our comp/class as @Aspect

- **AOP point cut expressions** - ex - @Before("execution(public void addAccount())") // the fn declarations inside the execution() is the pointcut expression..
- this pointcut expression is the predicate expression for where the advice should be applied..
- sp uses aspect j's pointcut expression language.

- how can we reuse the pointcut expression ? the ideal solution is to create a pointcut declaration once and apply it to multiple advices..
- ex @Pointcut(execution(public void addAccount())) ) private void forDaoPackage(){} // then we can use this method as the point cut expresssion in multiple advices ex - @Before("forDaoPackage()") public void beforeAccount(){}..
- the advantage of this pointcut declaration is update in one location, can also share and combine pointcut expressions..
- **combining pointcuts** - to apply multiple pointcuts to a single advice.. we can do it with the help of **logic operators** &&, ||, ! ..

  - and it works like a if statement execution happens only if the condition is true.

- **the order of the advice** using the @Order annotation..will guarantees the order of when the aspects are applied..and for the order the negative numbers are allowed.. and the lowest number wins.. higher precedence.

- **@AfterReturning Advice** will run after the method (success execution).. **After Throwing** - runs after method(if exception thrown) ..
- **@AfterFinally advice** run after the method (finally) .. **around Advice** run before and after method..
- **JointPoint** class/interface have the meta data about our fn or args (we can use it to get the fn signature) or other meta data..
- this @AfterReturning advice is useful in the post processing data .. b4 returning to the caller or format / enrich the data.. (we can intercept the data)..
- **@After**(finally) advice will be able to run in the case of either success or failure ..logging/auditing is easiest here..
- **@Around** advice will run b4 and after the method execution.. and its really like the combination of the @Before and the @After advice. and it gives more fine grained control
  - along with the usual use cases it is also useful in the profiling code ex - how long it takes for a section of the code to run .. and manging exceptions.
  - and when we using the @Around advice we will get a ref to the **proceeding join point** (is a class just lie the join point) which is a handle to the target method.. or execute the target method..
  - we can use this as a param and then call the proceed ex public Object afterGetFortune(ProceedingJoinPoint theProceedingJoinPoint) throws Throwable{ Object result = theProceedingJoinPoint.proceed(); // this proceed method will execute the target methode.. and it is sim to the join point class which gives us some meta data ex - the fn signature.
  - for the exception thrown at the proceeding join point.. we can handle/ swallow stop the exception or we can simply rethrow the exception.. this will gives us the fine grained control of how the target method is called .
- - when we re using this @Around advice our main app will never know about the exception since the exception will be handled by the @Around advice..
- we ve the option to rethrow the exception in the @Around advice.. simply we will be using the try catch block in the try we will make the proceed() call to handle the target method and if there is an exception then in the catch we will simply rethrow the error ex - catch(Exception exc) { throw exc; } ..

- **AOP and sp MVC** -
- nothing fancier just refer the slides ..

### sp micro services DDD, saga, outbox, CQRS pattern..

- in the saga pattern our order service will be the SAGA coordinator and each operations/interface (in saga) will ve 2 methods 1. process 2. rollback (to the prev method in case of failure) and the rollback are used (compensating the txs) ..
- the SAGA is used to create long running distributed txs across services.
- in this arch pattern we will isolate the domain logic from the IS, and outside service..
- **the outbox pattern** for pulling the outbox table with the scheduler, saga status..
- for more details refer the slide /png of the sln architecture..
  - 1. it covers the failure services
    2. ensure the idempotency using outbox table in each service
    3. prevent concurrency issues with the optimistic locks and DB constraints.
    4. keep updating saga and order status for each operations ..
- ### Clean Architecture and hexagonal architecture
- refer the ppt slide ..

### DDD

- refer the ppt slide ..
- - when we create the avro (json) .avsc file we can run the `mvn clean install` and the maven will create a schema file with the deserialized data..

### CDC

- for Change data capture.. he uses Debezium which is a open source platform for cdc and our app will start responding to insert update and delete txs in our dbs. for each of these actions there will be tx logs in our db, and debezium will listen for those logs and produce to kafka..
- we can simply add depezium connector to our docker imgs
- so this way this will make the real time streaming with **push** approach.
- where as the outbox pattern (with cdc) is **pull** approach.. so we can replace this with the push based one which is more of real time approach.

- **Jmeter** check this tool to send concurrent req .. just like the postman it supports pessimistic locking and optimistic locking.. in this pessimistic locking mode the tx1 will be select for update and tx will be blocked ..
  - lly in the optimistic locking mode tx1 will be select and update and the tx 2 will also be select and update.. and it requires the table with a version integer column.. if the versions are eq then it perform update (increment version) if the version is diff then the optimistic lock exception retry the operation.
- some of the dependencies we may need to install are kcat (kafka cli), jmeter(for load testing), f
- some of the command to run docker `docker-compose -f common.yml -f zookeeper.yml up`

# Event Driven project (with ES)

- (ali gelenher udemy)
- refer the lib **Project Reactor** lib..Flux is a key building block for building reactive applications in Java, particularly in scenarios where you need to handle asynchronous and potentially large streams of data. It is part of the reactive programming paradigm and is used in conjunction with other classes and operators from the Project Reactor library to create efficient and responsive applications.
- this **reactor netty** is the async event driven n/w app framework. it provides non blocking clients and servers, so instead of tomcat we uss reactor netty (comes by default in sp).. to see the results we need a **reactive client** to see the result in a chunks..
- **reactor Natty client** -

### JBoss

- is a application server - is simply an env that provides all the functionality that require for the app to run/ needs
- the redhat JBoss..stands for Java Bean Open Source Software.. the Jboss 8 (in 2014) is renamed to **WildFly**
- its highly used by devs for prototyping..not fully test for compliance..they are not certified according to standards..
- after the redhat bought the jboss they coined the term **(EAP)** Enterprise Application Platform, highly used for the poc and production environments.. if we ve any issues redhat will support for this against our issue.
- in comparison with IBM's **Web sphere** and oracles **Web Logic** app servers.. these 2 are more expensive and offers many features and great Gui for ux.. but still people prefers jboss (since its much more cheaper).. and more importantly it offers great flexibility... we can develop a s/w and run our code with in few mins..
- installation, configuration, administration, customization are so easy in jboss..these things are so complex in web sphere and web logic..
- **installation modes** we can install this software in 1. silent mode 2. zip file 3. console (cli) 4. Gui mode..
- **Jboss components** inside the EaP it contains **Jboss core(wildfly core) 10.1.2**, **Undertow(web server package)**, JGroups, Infinispan (these 2 comps helps us to do clustering in Jboss), Iron Jacamar (help us to de EJB level clustering), Narayana (tx s/w helps us to do txs),Active Mq (for messaging), Jboss Logging, HAL( admin console), Jboss security, Picket box (vault, for p/ws etc), Netty(async web client)

  - there are 2 modes we can operate 1. standalone mode 2. domain mode .. in the domain mode, one jboss server can manage another jboss server, which is not possible in standalone mode.. the domain controller can manage other servers
  - flexibility - we can take this s/w and add our app onto it and repackage it and we can deliver it as packaged app..
  - it provides flexible operation, faster foot print to load/ start the server, Easy to migrate, No license just the subscription..

- Why we wanna use Jboss ? - easy to learn and scale up, less no .of candidate with higher degree of knowledge, Many new implementation projects, Many migration projects, Better Job market,
- there are 4 things to consider when choosing an application server, 1. Mw service 2. portability 3. periphery tools 4. cloud readiness
- Build (jboss dev studio) provides integrated dev env, for build and test (such as mvn).. **Deploy** jboss Eap, to deploy next gen highly tx app.. **Manage** jboss operational n/w , provides deployment, monitoring and management..

### Jboss (udemy)

- EAR file is the Enterprise Archive File. which has web related components and other non web related J2EE files as well.. the app server are used for the web related and non web related j2ee projects.
- the Jboss ES (enterprise server) was renamed to wildfly 7.
- the jboss EAP architecture looks like the base is the OS (our os), then the JVm will run on top, then the manament/security/logging/Txs will run. after that the EAp services such as web REst, EJB container, jpa, batches, web containers, remoting iiop(Internet inter ORB protocol), jca container(java connector arch, std arch for j2ee, to erp s/m), msg provider(apache active mq)..etc.
- once we install the Jboss eap then cd EAP4 dir, cd bin, then run the standalone.bat file.. which will start up the eap.(loads all the modules)
- security realm - has management realm and app realm.. the app users will ve roles.. where as the management users will ve group
  - **Standalone Mode** if there is only one eap installed on the host then the server will be running in the standalone mode.. and this mode has **4 profiles** (we can use/ run).. 1. standalone-full 2. standalone- full-ha 3. standalone- ha 4.standalone LB
  - to create a user go to bin dir /add-user run this file and add the new user
  - **Domain mode** in the domain mode we will ve the **Domain controller** then the **Host Controllers** and the **Server Groups**.. the domain controller will control the host controllers, the host controllers will control the server groups.
    - if we run the domain.bat file the eap will run in the domain mode.
- **Redhat dev studio** is a desktop application we can install and this one has lot of options we can use for the jboss, j2ee projects (eclipse built in), web browser, and many more..
-

---

### Integrating ML to the spring app

- DJL - is a Deep learning Java lib, - its scalable and its like keras.
- this lib is api designed for java, its multithreaded support and mem control.
- engine agnostic like pytorch, tf (write one run anywhere).. and we can run this same model with other libs ex - tf w/o changing more codes (just change the deps)
- model zoo - 70+ pretrained model out of the box.
- why Djl - since the Java community lacks DL packages(since most of the ml libs in python which are slow and causes some probs while integrating with our apps), and DJL provides the DL with strongly typed lang, and follows DI approach, service code readability and realiability.
- we can think of this lib like a mw for our app to integrating the ml feature.

- **Advantages** some of the advantages of this lib is easy to set up only 10 lines inference setup, min dep requirement, large scale (offline data processing), fast performance, stable.
- models are hosted with the java based model hosting service.
- **Use Cases** lets see the use cases of this lib. 1. sponsor ads - 2. sponsoreed products (search result)
- the DJL helps us to run models built with diff ML frameworks side by side in the same JVM w/o IS changes.

  - the DJL translators helps us to keep feature generation independent of the ML (as we know the feature generation means we get the req, and provide the inference and steps to transform the incomming data to what the model takes as i/p ) and djl has the concept of translators that allows us to keep our feature gen indpendent of how the model is implemented.
  - djl easily handled high dimension data, djl reduce the batch inference time by 85% (from 24 hrs to 3.5 hrs)

- **DJL spring boot starter** - the spring starter dep for this lib is available for both the maven and the gradle
- and the future of this lib is - federate learning with DJL, Reinforcement learning(RL) with DJL and these 2 integrations work in progress(maybe available now ).
- for more info refer the DJL Docs and the github awslabs/djl

---

- lly we ve some libs for the .net as well ex - the ml.net, cognitive services - for Easily add intelligent features to your .NET apps with our pre-built AI models — such as emotion and sentiment detection, vision and speech recognition, language understanding, knowledge, and intelligent search.
- azure ml, F# for data science and ML F# (pronounced F sharp) is a succinct, robust, performant language that is great for data science and machine learning.

### Builder pattern

- is usefule when we ve like multiple fields in our class, and when create a new instance inside the contructor we can define only the field we wanted to use and if we think of adding some more field(that defined in the class ) for ex if that field is 4 th position / sequence (order matters) in the class mem var, then we ve to define all the other fields as undefined

  - ex - const user = new User (name, undefined, undefined, emai); // instead of this doing like this we can use the builder patter (setter method to all our fields)
  - ex - const UserBuilder {constructor(name){this.name = name} setAge(age){this.age = age; return this} build(){return this.user};} // this way we can set use the build() to add the fields(by calling the set method on em)
  - now its like const user = new User('jon').setAge(10).build() // this way we can add the class's mem vars.

- but in the newer syntax in the js we can use the optional param(put all the fields inside the obj / prop) for our mem vars field and assign it to the empty obj in the constructor
  - ex const UserBuilder {constructor(name, {age, email} = {}){this.name = name, this.age = age, this.email=email }
  - and now we can instantiate the obj ex - const user = new User("bob" {age: 19, email: 'som.com'}) // in this way we can also set the default vals for these optional fields inside the ctor.

### Adaptor design pattern

- its nothing but combining two or more interfaces and create a new interface ie the adapter

### composite design pattern

- is like a tree hierarchy, inside we will ve multiple composite objs and then the composite objs will ve multiple leaf obj, (the leaf obj/ node is the last one in the hierarchy tree if there is no node extending then its a leaf node otherwise its a composite obj / node)
- so there are 3 components in this patter 1. base component (which is an interface itself)2. composite component 3. leaf component(lets say we ve 3 leaf obj /classes ) and in the composite component along with impl the base component(interface) we also returrn the leaf classes in the ctro ex - private List<T> children; this.childre = new ArrayList<>();
  - now we will also ve all the methods that are defined in the leaf components) in the composite. ex - public void addDept(Department dept){children.addDept(dept);}
- if we want to perform some ops on the leaf node the same ops needs to perform on the composite node / obj.
- we can do this just by creating an interface and define the method we want and then impl this interface in all the classes in our hierarchial tree.
- and the thing about composite is we can use the same ops on the leaf obj or the composite obj, if its leaf obj it will print the val and if its composite it will gives all the val of all the leaf obj.

### Factory desing pattern

- belongs to creation design pattern,

### Delegation

- the fn delegation means passing the responsibility to some other function. in oop when we pass the task of the obj to another one.
- in the ex- he shows the delegation b/w kotlin and java while the java delegation has some codes (like few lines extra boiler plate codes to achieve the delegation) while the kotlin has very fewer lines of code, like we can use **by** kw to assign the delegation.

  - ex - internal class Employee(coder: WhoCodes): WhoCodes by coder{}

- in the java the extra boiler plate code is(in case of implementing the interface) the DI, methode override, and the ctor priv field for the interface.

### gRPC

- when the rest api isn't enough..
- there is the another api available as we know the RPC -> Json RPC, HTTP RPC, tRPC and gRPC.
- the rpc contains - for the post method the fn/method itself, and the params the body but the rpc lacks in self documentation.
- that's where the gRPC comes in, it has the protocol buffer. the protocols are binary based ones instead of the text based, which is more compact
- for the gRPC client he chose the "gRPC ui" - which is an open source and easy to use. `brew install grpcui`
- as we know the protobuff and the gRPC are lang agnostic.
- in gRPC the types are known as messages ex - service Calculator { rpc Add(CalculatorRequest) returns (CalculatorResponse)}; these both Calculator requests and the response(types) are the message in grpc.
- the methods are marked with the rpc (the methods that our rpc service will provide)
- once we done with the proto file we need to compile the proto file, so we ve to install the protoc (proto compiler), `brew install protobuff` - also includes the protoc compiler.
- then the lang specific plugins ex for the go lang - `go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.26` and `go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1`
- or these above 2 can be installed thru `brew install protoc-gen-go`
- then the next command is to load all the proto files and gen the go grpc code however this command is tedious so its better to put it in a file
- ex - `vim Makefile` and then add the following lines to the file ex `generate: protoc --proto_path=proto proto/*.proto --go_out=. --go-grpc_out=.` then we can run `make generate` this will create the proto buff for us.
- now we can go ahead and use this service, first we need to install the grpc from the google `go get google.golang.org/grpc` then `mkdir server/main.go`
- the code for the server and the other commands can be found in this github [repository](https://github.com/dreamsofcode-io/grpc)
- as we know the grpc works only with the http2 instead of the http1, so we can use **TCP** for the listener

- **Error Handling in the gRPC** - we can use the grpc curl which is a cli tool for sending the grpc reqs as we'd in the curl. install this tool then send the curl like `grpcurl -d '{"a": 10, "b":2}'` - now we will get back the result {"result": 5}; but if we send the div by 0 then our grpc server will crash, so to handle this bad req.
- our service needs to validate the input and returns the error if something is not valid.
- one approach will be we can define the error messages in the proto buff schema, and then we can add it as an optional field in our calculation response - ex message CalculatorResponse { optional int64 result = 1; } // so by doing this (optional) now our resp will ve result or error.
- and this method is tedious we ve to implement on every response, so instead in the server file main.go import reflection and the status from the grpc
- so now we can use the status code to handle our resp.
- **repeated** is used for the List<T> vals or [] or vals
- **client** in the real world use case whenever we gen the proto buff with the compiler we also gen the client code we can use. for more information refer the client code in the client dir
- **Envoy** - which is used to convert grpc into Rest using the reverse proxy

### Redis

- the most common use case of the Redis is speeding up the io operation by adding the caching layer in the app stack.
- now the question is why is redis being the relegated caching layer instead of being the db itself ? - as we know the primary concern must be the redis is the in mem db which in term our data will be there for a certain period of time (not persisted)
- and the 2nd reason is bcoz its a key val store it can't handle the complex data structure like queries..
- Actually redis has 2 types of **persistant** built in, **1. Snapshoting** is the point in time snapshot of specific intervals
- 2. **AOF** stands for Append only File, when config to use AOF redis will write an entry to the log file for every write operations the redis server receives, during the start these operations can then be replayed reconstructing the original db. just like the postgresql.(like wilds or right ahead logs)
- in the production environment if we want to find the key of certain data, we should try to avoid using the key command instead we ve to use the `SCAN 0 MATCH "user:*"` this will iterate over all our keys (instead of getKeys)
- this is called sequential scan this works sim to how the sql db works, also this is not the optimal way or performance, to improve performance we ve to use the "db index" on the.
- now to solve the second problem -> to save the complex data structure or the data types we can consider using the **redis Hashes** which will allow us to store more complex data structures against the key.
- `HSET key val` to add the hashmap lly we can use HGET to get the hashes and "HDEL" to delete the hashmap

- **Ordering** now what about the ordering, in a typical db we would order the val based on the timestamp or ranking (desc or ascending)
- now redis has the **sorted sets** - which will allow us to store unique members in our case it is user ids with an associated score (in which our case will be the no. of the login per users) **ZADD** will add the sorted set and to retrieve the sets by using the **ZSCORE** and **ZINCRBY** to add members to our set or increment the score of existing members.
- **ZRANGE** and **ZREVRANGE** will allow us to select the members by the sorted indexes. to know how many members exist within our sets, we can retrieve this by using the **ZCARD** cmd
- **Filtering** sim to use the where cmd in sql we can use the **ZRANGEBYSCORE** cmd and the reverse "ZREVRANGEBYSCORE" followed by the min and max(+inf) values, if we want to filter the least logged in use the use the min val(-inf) and the max (ex 5)
- **Unique Contraints** - Redis sets (std sets) **SADD** we can add the user name, vals into our set.

- **Transaction** - as we know the in txs in db are to group the operations together (such as insert, update, delete etc) in an atomic fashion ie: either all of the operations are performed or none of the operations are performed.
- redis also supports this transactions, to perform the tx we ve to use the **WATCH** cmd, once we ve the watch key set up, we can enter the tx using the **MULTI** command, we can see the confirmation in our client with the letter (TX) next to the prompt, then we can use the HSET hash command and then to execute this we can use the **EXEC** to tx .
- for the personal projects we can coupled the redis with the postgresql usually as the caching as the db before itself.
- if we ve small data sets then we can consider using the redis as our primary database and only migrated to the postgresql if our data sets grows.

### DAve Garry (about the AI/ for productivity)

- codium vsc extension for testing, also gen auto testing and code suggestion.
- the we can use the chatgpt for the **Regex**
- then we can use chatgpt for the db queries (sql queries), or mongo queries etc.

### Observability in Grafana

- observability is the process of understanding the application's internal state through different indicators.
- and those indicators are logs, metrics(are the jvm statistics, heap mem statistics, thread counts) and traces(are distributed tracing from the app A to app B by using the trace id (using sleuth)) ..
- **Loki** for logging
- **prometheus** for scrape the metrics.
- **Tempo** to manage the traces.
- the source code can be found [here](https://github.com/SaiUpadhyayula/springboot3-observablity)
- also the link for the blog post [here](https://programmingtechie.com/2023/09/09/spring-boot3-observability-grafana-stack/)
- for the set up along with the mysql docker img we can also add the **loki**(from the grafana project) in the docker image.
- **Micrometer** which helps us to gather metric and it exposes to prometheus, the micrometer tracing bridge under the hood uses the open zipkin brave to impl the distributed tracing.
  - we can also use the **open Telemetry** another distributed tracing.
- **@Observed** annotation, we can add this in the repository, by adding this now we will also ve the trace logs to the db.
- **Tempo** we can use the docker img to install the tempo(img from grafana), and we know that the tempo is to manage the traces. and this also internally uses the zipkin implementation, thats why we been using the tempo port and the zipkin port in the docker-compose.yml file.
- the grafana is running in the localhost:3000 - in the dashboard we can see in the menu explore we can also see the config for the tempo, loki and prometheus.
- then once we start our both services we can see the trace id and the span id's are added to the logs.
- in the dashboard, switch to the tempo, and grab one of our app's trace id and paste it under the traceQL -> run the query and we can see all the calls to the distributed tracing (across our applicatios)

### Java Reflection (udemy)

- the java reflection is the lang and the jvm feature that gives us the runtime access to info about our app class and objs
- is available in the java reflection api
- using the java reflection we can write flexible code that links diff s/w comps together at the runtime.
- creaetes new prog flow w/o any source code modification.
- it allow us to write general purpose algorithms that dynamically adapt and change their behaviour based on the type of the objs or the classes that they are working on.
- general prog w/o reflection takes the data and the i/p and process and gives the o/p, in contrast with the reflection which takes data, i/p analysize and perform ops on both and produces the output.
- now with the ability to to analyze app's obj and classes at the runtime, we can create very powerful **libs**, **frameworks**, **software Designs** that'd be impossible to create.
  - some of the libs that are built with the reflection are **JUnit** and in the **DI** framework such as **Spring Boot** and **google Juice**
  - when we re using DI in the spring boot like the @Autowired in our di, which is based on the reflection and understands that those dependencies needs to be injected in the runtime
  - **JSON Serialization and deserialization** - like the Jackson or GSON - uses reflection.
- and another popular use cases of reflections are **ORM**, **Logging Frameworks**, **WEB frameworks**, **dev tools** and many many more..
- now the reflection are not only for the libs we can use them to architect our own app, and libs, augment em with unique functionality, and features

- **challenges**
  - if we don't use the reflection properly then we may end up with the code that is
    - making the code ie harder to maintain.
    - slower to run, dangerous to execute (since it has the potential to crash our app unrecoverably).. ie why the reflection is reserved for the devs with the strong java knowledge.
- **Entry Point**
  - the obj of Class<?> is the entry point for us to reflect on our app's code
  - this Class<?> contains all the information on the give obj's runtime type / a class in our app
  - that info contains what methods and the fields it contains, what class it extends or what interfaces it implements or much more..
- 3 ways to obtain the obj of the Class<?>

  - 1. Object.getClass() {this wont work for the primitive types}
  - 2. ".class()" suffix to the type itself - this will works with the primitive as well ex- class booleanTypes = boolean.class;
  - 3. Class.forName(), by using this static method we can dynamically lookup any class in our class path using its fully qualified name..
    - we can also get the info about the inner classes ex - Class<?> engineType = Class.forName("vehicles.Car$Engine");
    - just like the first ex we can use this in the primitive types.
    - this is the least safest method amongst the 3, but there are some use cases we can use only this as our option.

- **Java Generics wild cards** - the abstract class Number extends the integer class, interface CharSequence implements the String class but the abstract class List<Number> doesn't extends the List<Integer> and the class List<CharSequence>doesn't extends the List<String> class
- however the List<?> (generic wild card) is the super type for the list of any generic types, aka List<?> is the super class of List<T> for any generic param type T

- **Introduction to Constructor<?>** in the java reflection A ctor of the class is represented by an instance of the Constructor<?> class
- since the class can ve multiple overloads of ctors we ve few ways to get em ex -
- 1. Class.getDeclaredConstructors() returns all the constructors of the class regardless of the public or not
- 2. Class.getConstructors() returns only the public constructor.
- if we re looking for the particular ctor and if we know the params of the constructor we re looking for then we can use Class.getConstructors(Class<?> ...ParamTypes) or sim to the getDeclaredConstructors and pass in the params.. in these it returns particular ctor based on the param or if its not there it will throw an exception.
- if there is no constructor in the class then java will creates a default constructor(no arg ctors)

- **Goal and motivation** - ex - our goal is to create a single factory method, that can create a obj of any class

  - depending on the obj we passed to the method, it will find the right constructor.
  - create the given class obj by calling the right constructor.
  - w/o the reflection it is impossible ( the code can be found in the constructor.zip)

- **Restricted ctor access** - as we know the private ctors are not accessible outside of the class, but by using the reflection we can ve full access to the private, protected and package private ctors as well.
- and we can also use the Constructor.newInstance() method to create a new instance of the class using the restricted constructor.
- the only modification we ve to make the ctor accessible is by setting the setAccessible property to true, ex - constructor.setAccessible(true);
- the ex - http server config - refer the code webserver.zip file.
- this advance feature should be reserved to specific use cases and should not be abused to invoked internal or delibrately restricted ctors on classes that do not belong to us.

**8.Restricted classes instantiation automatic DI implementation**

- package private class instantiation using the ctor class.
- we ve to set the accessible method to true like earlier, before instantiating the obj
- 2. DI - the other scenario where the java reflection comes in handy is the instantiation of the package private classes from the another package.. auto creation the obj at the app start up.
- for this section refer the tictac toe.zip file
- in that ex - we can see all the class's ctor are priv and to create the instance/ to instantiate them. we re gon to create by implementing an autmatic DI engine using the reflection
- a recursive algorithm is the great fit, for each dep we visit we keep going deep into the graph, until we find a class with no dependency we can instantiate.

<h4>Section 3, inspection of fields and arrays </h4>
  - **Introduction to field class** - as we know the fields are class mem vars.
  - **Synthetic fields** java compiler gen artificial fields for the internal usage, we don't see em unless we use the reflection to discover em at runtime.
    - in most cases we don't want to touch/ modify or rely on em. - Field.isSynthetic() // to find the synthetic props of the field.
    - lly  we can be able to read the val of the given field and set a given instance of the class using the reflection

- so with the reflection in the field class, we can write the generic algorithms and libs that can operate on objs
  - whose type is unknown in the runtime.. or don't even exist until the prog is executed.
  - for the code refer (fields.zip)
- **Reading field JSoN serializer** - using the field we re able to get the class's field, name, type and read the val of an obj.
- the ex of such lib is the data obj lib like JSON.
- and for the json serializer implementation, we re gon to implement a method that takes any obj and serializes it into a JSON object.
- we ve no prior knowledge about the data object we re gon to serialize, and java reflection is the perfect for that task.
- sim to the ctor to access the restricted fields we can use the Field.setAccessible(true)..
- refer the code "json-writer.zip".

- **Arrays** - Array inspection using java reflection api, such as reading vals from the arrays using java reflection.
- in java the arrays are the special classes we can get the basic information about the arrays using the Class<?> API
- we can confirm that the obj is an array using the Class.isArray() method and we can use the Class.getComponentType() to get the type of an array elements, if the obj is not an array this method will return null.
- in java the 2d array is just the array of an array objs ex - double[][] twoDimArr = {{1,2,3},{4,5,6}};
- we can use the reflection in the given array to access the arr obj at the runtime ex - Array.getLength() and Array.get(Obj, idx) to get the idx of the given arr obj.
- in the zip file (arrays.zip) we can see we ve all the methods to inspect and iterate all the elem in the 2d array regardless of the type and dimensions.

- **REading arrays JSON Serializer** - in that ex - code(JSON-writter.zip) we can see we implement a practical real life algorithm converting the data objs to JSON strings
- using recursion we traversed the fields that contains the obj or the arr of obj and w/o reflection we couldn't be able to do such a generic solution whcih is perfect to be packaged as a library.

<h4> setting field vals, generic configuration File parser</h4> - as we know in many cases the field name and the types are  unknown at the AOT(ahead of time), since our reflection code is so generic it can work on the unknown AOT type and field names and we can write algos that can work with the infinite variety of classes.
- Use cases - Obj Deserializers, whcih takes data in a predefined protocol and translates it into a POJO
- ex - N/w protocol Deserializer and ORMs, App config file parser. (refer the code config-parser.zip)
- **Array Creation and initialization** refer the code config-parser2.zip
- in that code we can see we re using the reflection's Array.newInstance() method this is useful when the type of array is determined dynamically at the runtime.

<h4> Methods discovery and invocation </h4> - refer the code ("api-test-getters.zip")
  - the Class.getMethods() method returns all the methods including the ones inherited in the super class and implemented interfaces.
  - Class.getDeclaredMethods() returns all the methods declared in the class.
  - some of the other methods we can call on the Method's properties are Method.getName() and Method, Method.getReturnType(), Method.getParameters() and getParametersCount(), getExceptionTypes().
  - (in the ex code) its a kind of like a test framework, where each field in the data class has the correct getter exposing its val to the api users
  - and thanks to the reflection this testing framework is of type generic and can be used with the any type..

- lly there is code of the api-test-setters.zip (part 2) - in this ex in our framework sim to the getters we will use the setter to the testing framework.

- **Method Invocation (polymorphism)** refer the code polymorphism.zip file.. as we know the polymorphism is allowing the obj to takes multiple form.
  - as we know we can invoke the method using Method.Invoke(Object instance, Object ... args); // if the method is static then we can pass null as the instance.
  - the args of the method must be of correct type and order are the method signature.
  - **return type** if the return type is void then the invoke() will return null, if the return type is primitive then the invoke() method the return val will wrapped in an object . for ex the int val is returned as the wrapped in Integer object
  - this method.invoke() can throw some exceptions, and the most important exception is the InvocationTargetException - is thrown if the method itself we ve invoked threw an exception.
  - the method.invocation is one of the most widely used features in the java reflection, it allows the decoupling of the business/ test logic from its execution logic.
  - we can group and exec classes that ve nothing in common.
  - in that ex if we see we grouped objs of diff classes that don't ve any common interface or belongs to different classes, we re able to execute all those methods in a generic way.

<h4> Java Modifiers Discovery and Analysis </h4> 
- as we know the modifiers are added to the class, ctor, fields, methods and the modifiers are of type access modifiers and non access modifiers.
- **Abstract** makes our class abstract class that can't be instantiated 
- **final** finalizes the implementation of the class and makes it non inheritable
- **static** makes the inner class instantiable w/o instantiating its outer class
- **Interface** is also the modifier.. the interface is not only the modifier but also abstract modifier, just like the abstract class the interface can't be instanstiate w/o the concrete implementation. 
- **synchronized** method can be accessed by only one thread at a time.
- **Native** typically the method implemented in diff lang (typically C).
- **transient** marks the field not to be serialized.
- **volatile** makes the read/writes to long/double atomic and prevents the data races
- to get the modifiers we can use .getModifiers() in the class, ctor, field, method, etc. and those modifiers are packed with the integer vals and all those methods returns int. 
- the modifiers are encoded as **bitmap** so each modifier is represented a single bit.
- refer the code ("modifiers-example.zip")

<h4> Annotations with java Reflections </h4> 

- Annotations can instruct and direct reflection code on what target to process and what to do with those targets 
- we can decouple our app code from the reflection code.
- **custom annotation** as we know to create our own custom annotation we can use the @interface and every annotation in java extends the annotation interface
- not all the annotations are visible at the runtime, by default the annotations are retained by the compiler at the compile time and ignored by the jvm and not visible at the runtime.
- to make it visible we ve to use the @Meta annotation. one of the most important meta annotation is the @Retention meta annotation. this has 3 enum vals 
  - ex - 1. RetentioPolicy.SOURCE - source annotation are discarded by the compiler ex @Override, @SurppressWarning annotation.
  - 2. RetentioPolicy.CLASS - class annotation are recorded in the class file by the compiler but still ignored by the jvm in runtime ex - @AutoValue
  - 3. RetentioPolicy.RUNTIME - annotation are recorded in the class file by the compiler also retained by the jvm in runtime

-**Target Meta Annotation** - is another meta annotation, available for the FIELD, LOCAL_VARIABLE, PARAMETER, METHOD ElementType
- to find the target annotation, we can use boolean isAnnotationPresent() method, will be present in the class, field, method, ctor, param targets.
- refer the code annotation example .zip file where we re finding the packages (the file name ends with the extn of .class) based on the URI 
- if we dont want to include any of the method or the class in the run time then simply remove the annotations then the particular method or the class will be excluded.
- **Parameter Annotation** - the param annotations on ctors and methods, if the param vals are supplied using the reflection w/o the annotation, then we don't ve the way to tell which one is which.. we can use the param annotations on the ctors, methods alike.
- code as a graph - usually if we ve a bigger problem we will break that into smaller tasks and extablish the relation between em. generally we represent task as a method and implement prev task as the method's param..
  - the final step which is the sequential reperesentation and the execution of the tasks can be easily automated using reflection.
  - for the code refer the graph-example.zip file .. in that ex we can see our algo will keep recursively go through the entire graph of operation all the way to the method that don't ve dependencies and will be able to get the vals immediately.
  - so using these annotations we uniquely identify the methods or parameters based on their annotation val rather than the java types or names

- **Repeatable Annotation** 
  - automatically scheduling - to allow annotation to be applied repeatedly we need to explicitly declare it as a repeatable annotation
  - isAnnotationPresent() or getAnnotation() methods will not work for the repeatable annotation.
  - refer the code repeatable-annotation.zip

<h4> Dynamic Proxies</h4>
  - lets see the proxy design pattern and the implementation - in the oop the proxy design pattern - the proxy is an obj that holds an another object(real obj).. the client will communicate with the real obj thru the proxy
  - some of the examples of the proxy design pattern are 1. security proxy (protection proxy)
  - the jdk collection class - Collection<T>, Map<T>, List<T>, Set<T> wrap the data 
  - another example for the proxy is the resource management - Lazy initialization proxy
  - another use case is for in mem caching proxy - performance 
  - then the another example is Remote Pxoxy - for making remote procedure calls
  - **Limitations w/o reflection** if we ve M methods in each interfaces then for N interface we ve to implement M*N methods in total which is tedious and code conumption - so this could be solved by the reflection's dynamic proxy 

- **Dynamic Proxy Implementation** 
  - the dynamic Proxy is a class generated by reflection at the runtime. its name usually starts with $proxy.example
  - an obj of the dynamic proxy class intercepts any method invocation and delegates it to an instance of the Invocation handler

<h4>Final Section -  Performance, safety and Best practises</h4>
  - Reflection - performance - Typically operations involving java reflection are slower and less efficient than their statically combined compiled equivalent operations
  - in reflection everything the compiler will do for us ahead of time compilation, we now ve to do it at runtime.
  - runtime vs compile time checks - the compiler can't make any access or type safety validations. At runtme the JVM has to validate - this is what makes the entire ops a bit longer
  - **Exception Handling** - in production we ve catch/ handle the exception at every edge case with care to make our reflection code more safe and reliable.
  - **3 Rule of Thumb** (when to use Reflection)
    - 1. use reflection only when we must - if we can achieve the same task with regular java code w/o reflection then we don't ve to use reflection.
    - but there are so many use cases where reflection can do, and the regular code doesn't.. and there are so many use cases where reflection can make our code more safe, cleaner, shorter and easy to work with.
    - 2. Test and Handle Edge Cases - when we use reflection the compile time code exception will be runtime exceptions. To keep our code safe and correct we need to perform those checks ourselves.
    - handle and prevent each exception by writing defensive code. write many automated (unit tests) to test any code that uses reflection.
    - 3. keep the reflection based code loosely coupled (with the rest of the code base as possible)
