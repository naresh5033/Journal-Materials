# spring boot (udemy)

- **IOC** -  the spring container(Obj factory) has primary fn to create and manage objs (IoC) , and also **DI**
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
- **@Lazy  initialization** instead of loading all the bean at the start by default, we can specify the lazy initialization and so the bean will only initialized when needed for the di or explicitly requested.. 
- we can configure in app.properties file for the global lazy initialization.

- **Bean Scopes** defines the lifetime of the bean and the default scope of the bean is **singleton** .. there are other types of scopes such as prototype, req, session and global session
- - **Bean Lifecycle** when the container is started the bean instantiated then the DI the internal sp processing , and our cust init method and the bean is ready for use and container is shutdown then our cust destroy method is called and stop..
- ex @PostConstruct , @PreDestroy ..  the Prototype bean are lazy by default so there is no need for the @lazy annotation.

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
- for that in the app.properties file we need to set spring.jpa.hibernate.ddl-auto=create  // means the db tables are dropped first and then created from the scratch. .. if we wanna keep the data then set it to "update"

### REST API (CRUD)

- status code ranges - 100-199 : informational, 200-299: successful, 300-399: Redirectional, 400-499: Client error, 500-599: server error..  
- the **java json data binding** the json binding is the process of converting the json data to the java pojo .. is also called / some of the other names serialization / Deserialization .. Marshalling /unMarshalling..
- sp will automatically handle the jackson integration and any json data being passed to the Rest controller is converted to the pojo.. and also any java obj is being returned from the rest controller is converted to json. (using jackson bts)
- **@ControllerAdvice** is sim to the interceptor/ filter .. it pre-process the req to the controller.. or post process res to handle exception and perfect for the global exception handling.. and its use of Real time use of AOP..
- - **@Service** layer will sits b/w our controller and the repository... here is where we write  all our business logics .. so its a best practice to to use the @Transactional annotation in the service layer, instead of the DAO/repository layer.

### spring Data JPA is the soln to create all the crud functionality related to the db.. it will take care of findall, findById, update, delete and so on.. we can just use this and plugin diff entities ex employee DAO, student DAO, comment DAO etc.. 
- this will reduce the amount of code we have to write .. for all the individual entities.

- **JPA Repository** so far we ve been using the DAO (Data Access Obj) but the JPA provides us the interface ie **JPARepository** with this we can perform all the crud ops we wanted. 
- and when using the JPA repository in our service class we don't ve to use the @Transactional any more JPA repo will provide right out of the box.

- **sp Data REST** sim to the jpa repository this will give all the REST crud features .. and we can use it for diff services .. minimize the boiler plate code..
- earlier we ve used controller and service class.. now this approach seems like we just ve the repository and the sp data REST .. that's it .. this sp data REST make use of the **HATEOAS** the hateoas uses HAL (hyper text App lang) data format..
-  this provides some advanced features like pagination, sorting, searching, custom queries with jpql, and Query domain specific lang..(QDSL) and the endpoint name conversion is ex - public interface EmployeeRepository extends JPARepository<Employee, Integer> {} // the sp data will add s to the entity (to make it plural form ) now the path will be "/employees" (and also make it first letter case sensitive)
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
- **@InitBinder** - wrap this with the method it will do some pre-processing each req to our controller ex we can create  a method for the string Trimmer editor to remove the white space.. (we can make use of the new **StringTrimmerEditor()**) class form the sp.
- if we want to add any cust error messages for the fields ex- the typMismatched error then we can create a messages.properties file in the src/resource dir and set the prop as .. typeMismatched.customer.freePasses=Invalid number //

- **Custom annotation** to create a custom annotation ex - we can create one for the validation..  there are 3 annotations we ve to use/ wrap around our cust annotation class ex @Constraint() , @Target, @Retention(), .. then the class ex - public @interface CourseCode(){}
- **@interface** is the key to create the cust annotation .. @Target(ElementType.TYPE) is where we specify that this annotation is used at, (and most of the time its class) .. in the element type we ve option to add the annotation to the class's field or method etc.
  - **@Retention(RetentionPolicy.RUNTIME)** it will use the annotation while the prog is running and the other 2 possible retention policies are "SOURCE" and "CLASS" .. source - java will remove the annotation b4 the code will run (it will only matters only in the compile) and the class means it will remove the annotation in the runtime.
- **params to the cust annotations** we can also add params to the cust annotations.  ex - lets say we wana run our annotation for 3 times then it looks like @RunImmediately(times=3) // in the cust annotation class just add the time() like a normal field and now we can use this as a param for the cust annotation. ex { int time(); }  
  - note: there are few things in making our own params .. the types can only be primitive type like int or class type or String or Array( of any of those)
  - refer the ConstraintValidator class from jakarta to impl our cust validation.


### Sp mvc CRUD

- just make a crud operations in the employees using the thymeleaf.
- in the form when its loaded it will call the getters from the employees model and when we submit the form it will call the setters from the employees model..
-  for updating the form he just used a new approach, he add hidden type i/p field ex - <input type="hidden" th:field="*{id}" > // just only to get the id (param of the employee that we want to update)

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
-  lly if we wanna delete only the instructor detail not the instructor then we can use cascadeType.DETACH, cascadeType.MERGE, cascadeType.PERSIST, cascadeType.REFRESH.. with this four cascade types we can now ve able to delete only the instructor details.
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
- "The AOP" is a programming technique based on the concept of an aspect.. the aspect encapsulates the  **cross cutting concerns**.. concern means logic/ functionality..
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
  - **@Before** is useful for logging, security, tx , audit  logging, api management..
- we can add the deps and then mark our comp/class as @Aspect 

- **AOP point cut expressions** - ex - @Before("execution(public void addAccount())") // the fn declarations inside the execution() is the pointcut expression..
-  this pointcut expression is the predicate expression for where the advice should be applied..
- sp uses aspect j's pointcut expression language. 

- how can we reuse the pointcut expression ? the ideal solution is to create a pointcut declaration once and apply it to multiple advices..
-  ex @Pointcut(execution(public void addAccount())) ) private void forDaoPackage(){} // then we can use this method as the point cut expresssion in multiple advices ex - @Before("forDaoPackage()") public void beforeAccount(){}..
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
  - we can use this as a param and then call the proceed ex public Object afterGetFortune(ProceedingJoinPoint theProceedingJoinPoint) throws Throwable{ Object result = theProceedingJoinPoint.proceed(); // this proceed method will execute the target methode.. and it is sim to the join point class which gives us  some meta data ex - the fn signature.
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
- after the redhat bought the jboss they coined the term **(EAP)** Enterprise Application Platform, highly used for the poc and production environments.. if  we ve any issues redhat  will support for this against our issue.
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



 