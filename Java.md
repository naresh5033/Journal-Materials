# Java Generics

- The problems before generics --> as we know when we ve the class of diff types and we ve use the class for the multiple times with diff types, so instead of creating the class with diff types (code duplication), we can use the generics class to create a single class and use it for the different types we wanted.
- ex- public class Printer<T>{} and to use this inside our method for ex we want our printer to be of type int --> Printer<Integer> printer = new Printer<>()
- but one thing to note is that the generics wont work with the lower case int and long <int> or ><long>
- mostly we see the generics works with the collections.
- if we ve an obj type for our array, then we'll ve all the type safety issues. for ex. ArrayLis<Object> cats = new ArrayLis<>() -- we can add whatever type in this but when we call/get the val in that list we ve to use same obj type in the list or we ve to type cast the obj, this will cause the type safety issue.
- and generics solves that issue, coz we wont be adding the objs of diff types.
- we can also extends the generics types from other classes ex . Public class Printer<T extends Animal>{} but now this printer class works only for the animal, not with the other types.
- with this extends we can also get access to the methods inside the animal class.
- and this type of generics is called bounded generics, coz we re giving certain bound or limit on the type it can handle
- we can also use interface for the bounded generics, Public class Printer<T extends ICat>{}
- we can also use multiple bounded generics ex. Public class Printer<T extends Animal & ICat>{}
- but the gotcha is we only ve to use one class(as java doesn't support multiple ingeritance) type and many interfaces, but the order matters, first the class and then the interfaces.

- in addition to the class we can also use the genrics in the methods ex. - public static <T, U> void printstm(T cat, U dog) {}
- we can also used for the return type in our method as well. ex- public static <T, U> T printstm(T cat, U dog) {}

## wild cards <?>

- another imp advance genrics
- lets say we ve a list of obj we can use obj type ex. public static void printstm(list<Object> mylist) {}
- lets add int obj into the mylist.. List<Integer> list = new ArrayList<>() .. we will get an exception, coz althought int is the sub class of Object, but the arraylist is not related to the Object.
- so to solve this we can use wild cards
- ex public static void printstm(List<?> mylist) {} ..by saying that now the list of type any or unknown.
- we can also use the bounded for wild cards
- ex . public static void printstm(List<? extends Animals> mylist) {}

## Sets and Hash Sets

- the sets are from the "java.util.Set" class
- lets see how the set is diff from the list
- the set is an abstract and cannot be initialized.. ex - Set<string> names = new Set() -> can be initialized.
- because Set is an interface not an normal class. so we can't create the instances of the interfaces, we can only create the instances of the classes that implement the interfaces.
- so that's where the hash set comes in ex. Set<stirng> names = new HashSet<>()
- now this hash set is class that implements the set interfaces.

1. The list maintains the order of elements that are added, while the set doesn't maintains any order for the elements.
2. The Set by default does not allow any duplicate elements, wherea as the list does.
3. We can't remove the elements with the idx in the set (since there is no order of elements)
4. like the list we can use the enhanced for loop to through the set ex. for (String name : names){system.out.println(name)} or can use the lambda to print all the elements.. names.forEach(System.out::println){} or iterator .. names.iterator() and use the while loop -- while (namesIiterator.hasNext()){System.out.println(namesiterator.next());}

- So the whenever we ve the collection of items and we don't care about the order or we don't want any duplicate elements.. in those scenarios we can use the Set.
- we can use the set for filtering out the duplicates in the list array ex- lets say the list called numberList has duplicate elements and to filter this we can create a set and pass the list inside, which will return the list with non duplicates.

  - ex . Set<Integer> numberSet = new HashSet<Integer>(); or new HashSet<Integer>(numberList) will do the same thing.
    numberSet.addAll(numberList);
    system.out.println(numberSet) -- will return the list without duplicates

- the reason why its called a hashSet is, it uses the hash table as the storage mechanism. and its extremely efficient/fast, no matter how big of the set is it will alway takes the same amount of constant time for the ops(look up, add, remove etc)
- where as the list as it gets extends the ops is more expensive

# Tree Set

- does exactly same as the hashset does.
- but the elems that we put inside will be ordered as the natural order
- for ex - if the set is str then it will ordered in the alphabetical order.

Note : the HashSet is extremely efficient and faster than the tree set. we can us e the tree set when we really care about the order.

# Linked Hash Set

- it deosn't maintain the natural order of the elements but instead it maintain the insertion order or the elements.
- this set is faster than tree set but not as fast as the hashset.

# Reflection (java.lang.reflect)

- The Reflection API that can allow to write a program that can look at itself and chnge itself while running.
- we can think of it as a meta proramming.. use at our own risk.
- its incredibly powerful and breaks all the rules of the programming.
- examine and change any element of the java class write in the middle of the java program
- ex. Cat mycat = new Cat ("tom", 22);
  mycat.getClass().getDeclaredFields(); -- > this .getClass() will provide all the reflection methods
- when we ve an object and the fields are private, and we don't ve the setters (only ve the getters), we can't assign the fieds outside of the constructor.
- but with the reflection the above rule will wash out, we can change the fields of the object by using the reflection methods
- ex - Field[] catFields = mycat.getClass().getDeclaredFields()
  for (Field field : catFields ){
  if field.getName().equals("name"){
  field.setAccessible(true) -- > with this setAccessible true we can now set the fields.
  field.set(myCat, "jimmy")
  }}

  lly we can invoke any private methods inside of the obj class with .getDeclaredMethods()

  - but .getDeclareFields() returns a field array, where as the .getDeclaredMethods() returns the Method[].
  - ex - Method[] catMethods = mycat.getDeclaredMethods()
    for (Method method : catMethods) {
    if method.getName().equals("meow"){
    method.setAccessible(true) -- > with this setAccessible true we can now innoke the methods.
    method.invoke(myCat) --> .invoke() takes 2 param the obj/class we wana invoke and the params of the method.
    }
    }

  - usage - the reflection are heavily used in the frameworks for ex- the spring framework uses reflection all over the place
  - and in the testing, we can use it to set the vals of the private fields

cons - bcoz it has to figure out and manipulate during the runtime(doesn't do any compile time optimization) the code that written with the reflection are not efficient and slower.

## Vectors

- is another collection member
- The big difference between the vector and the array list is the performance..
- The array list are faster in performance than the vector.
- however when working with the multi threaded environment, the array list are not synchronized with the threads and are not thread safe.
- in other words multiple threads working on the array list at the same time will be completely non deterministic and whacky behavior.
- Where as the operations in the vector are thread synchronized, we can run multiple threads on vectors.

but if we do wana use array list in the multi threaded environment then we can use the wrapper ex- List<Integer> mylist = collecitons.synchroized(new ArrayList<>())

- but this will perform same speed as the vect does, takes same time

  ## optional type Java

  - the options are container that either has some thing or none .
  - when a method has an obj, it should return the valeues of the obj, if the val is undefined then it will throw null exception
  - to handle this (when the method does not ve a val to return ) then we can use the option
  - ex. public static Optional<Cat> findCat(String name){
    Cat cat = new Cat(name, 3)
    return Optional.ofNullable(cat)} -- or we can create Optional.empty() for the empty optional.
  - we can also get the val of the optional obj ex Optional<cat> cat = findByname("tom"){
    cat.isPresent()
    cat.get().getAge() } --> again this will throw the nullable exception since the optional cat obj doesn't ve the val age.. to fix this we can use another optional method .isPrsent()
  - But this whole tedious code looks like the classic null check condn. (then why is the need to use optional). well
  - there is another method that optional offers --> .orElse()
  - we can use this method like Cat cat = optionalCat.orElse(new Cat("ram" 0)) --> this method will geth the val of the optional cat. if the val is not in the obj then it will create a new obj and with the default values(of 0,)
  - .orElseGet() --> we can use this method and pass in lambda supplier function. sanme as the orElse(), but instead it will use the lambda function to assign the default values and returns it.
  - orElsoThrow() --> it is as same as the .get() if the optional obj has no val it will throw an exception.
  - the optional are mainly used in the return types to handle the null return value.
  - in the scenerio where the val we re looking for (is uncertain) might not exist, then we can use the optional type.

  ## Lambda function

  - can be only used along with certain types of interfaces.
  - as we know that any class that implements that interface has to provide the method that specified in the interface. lets say the Iprintable has a method called print();

  - so we wana use this interface in our method we ve to impl that in our class, then we ve to provide the implementation of the method (print()), and then we ve to create an instance/obj of that class and then pass in the method print() as the para.
  - lambda allow us to just pass in the specific implementation of the print() in to the fn param w/o needing all of the above extra stuffs.
  - for ex if we wana pass the impl of the method print() into our fn as param it looks like this .. pringThing(public void print(){system.out.println("cat")})
  - but wnen we use the lambda function we don't ve to specify the access type(public) the return type (void)coz the compiler will figure out automatically, and also we dont need the method name (print). now all we ve left is the method implementation and the param bracket
  - finally we ve to use the "arrow operator " b/w the param and the method implementation. () -> {}
  - so it looks like this printThing( () -> {system.out.println("cat")})
  - note if our implementation is just one expression, then we can leave the curly brackets as well.
  - basicallwy instead of sending an obj that contains the action, we re sending the action itself.

- if we ve a param in the interface fn then we don't need to define its return type while using it inside the lambda function, and if we ve only one param then we can also exclude the the paranthesis ex - pintThing(s -> system.out.println("cat")), if we ve more than one param then we need to use the paranthesis.
- if our interface fn has return type, then we can define the return stmt in the lambda
- or if our interface fn has return type and it only for the console purposes then we can use like PrintThing(s -> "cat" + s;)
- lets see how it works behind the scenes
- when the method inside the interface has no implementation is called abstract method. nd the interface with one abstract method (or "SAM" interface- single abstract method)is called "functional interface".
- when we ve functional interface we can even add the annotation to our class @FunctionalInterface
- The lambda can be only used in the functional interface, if the interface has more than one abstract method, then we can't use the lambda function.
- if we ve more than one abstract method then we can use the annonymous class

## anonymous inner class

- this class are really useful and easy to use
- the anonymous inner class ve no name that we used to instantiate only one obj ever at the same time in the same java stmt.
- it can extent any existing class or it can implement any existing interface.
- ex lets ve a animal class that has one fn makenoice and we wana use this class twice with diff fn implementations
- Animal animal = new Animal();
- animal.makenoice();
  Animal smallAnimal = new Animal(){ // now to define the anonymous class here in this brackets we ve to create a anonymous subclass of the animal class
  @Override
  public void makeNoice(); system.out.print("diff string");}
  smallAnimal.makeNoice() // will call the newly creaated above method
  };

- now this smllAnimal class is the anonymous class, and we can't just instantiate an obj of that class
- its a onle time usage class.
- there are 2 ways to create this anonymous class, the one way is the above impl, by creating a subclass
- then the other way is to creating the interface of the class. lets see an ex - with the Runnable interface
- however we can't create a new instance of the runnable since its a interface not a class. so to deal with that we can create a subclass for the runnable.
- Runnable myAnonymouslInterface = new Runnable (){ // this way java will know that we re tryna create the anonymous inner class.
  @Override
  public void run() {
  system.out.println("hi there");
  };
  myAnonymouslInterface.run();
- so it may looks like we re tryna create a obj from an interface, which we re not in reality we re creating an obj of the anonymous inner class.

# Muti Threading

- its the ability to exec myltiple diff parts of code at the same time
- there are 2 ways of creating the multithreading 1. class extends the thread class(and to overide the run ()) 2. implements the Runnable interface.
- public class Mythread extends Thread {
  @Override
  public void run(){
  for (i = 0; i >=5; i++);{
  system.out.print(i);
  try {thread.sleep(1000); }{catch (InterruptedException e)}}}} }
  and to execute this in our main class we can call create the new instance of the MyThread class and then start the thread
  Mythread.start() // this will start the new thread.
  if we create a second instance and call the start() we can see multthreading execs properly - but if we insted calls the MyThread.run() method then it won't initialite the multithreading. - and if we wana create a multiple threads we can loop through them.. ex. - for (i = 0; i <5; i++) { MyThread thread = new MyThread(); thread.start();} - can see all the 5 threads are executed concurrently. - if we wana assign the number to our thread then in our thread method we can add the var as inside the constructor - private int threadNumber;
  public Mythread(int threadNumber){
  this.threadNumber = threadNumber; and in our main class, inside the for loop just pass the i inside the obj --> new Mythread(i);

  - when the threads are running there is no guarantee that which thread is executing in which order, they all gon run/execute independently.
  - one of the cool thing about the thread is any of the thread blows up any exception, it won't be interrupte the other threads.

  2. The Runnable interface.
     - in this way we only ve to do is to ve our own implementation of the run() //overriding. which is as same as the extends class.
     - the thread class should be like
       public class Mythread implements Runnable{
       and every impl is just the same as above
       }  
       and in our main class we can't start the thread anymore(as we don't extends thread) instead
       Mythread thread = new Mythread(){
       Thread thing = new Thread(thread);
       thing.start();
       }

- regardless of the one line more code the runnable interface has advantage over the extends class
- if we extend the class we can't extends any other class as the java does not allow multiple inheritance.
- instead if we implement the Runnable interface then we can still be extend to any other class
- ex . public class MyThread extends SomeClass implements Runnable, AnyoOtherInterface{}
- we can impl many interfaces if we wanted to
- we can use the .join().. it will wait for the prev thread to completes and it will start to executes after the thread completes.
- ".isAlive()" returns bool, for whether the thread is dead or alive

# MaUi

- MvvM --> from the view to the view Model .net maui has the data binding that combines these 2 by using the events and the data
- the core is INotifyPropertyChanged is --> publish class UserViewModel : INotifyPropertyChanged{}
- the .net maui is linking into that event handler and when something happened/triggered it will update the ui

- Icommand (is abstraction of what happens when diff events are triggered) - Icommand interface provides the commanding abstraction for their xaml framework.
- we can use this in the events
- ex - public interface Icommand {
  bool canExecute(object parameter);
  void execute(object parameter);
  event EventHandler canExecuteChanged; }

- compiled data binding - maui exposes the x: DataType that enables compile time checks and optimization.

# Recursions

- as we know the rucursion is when we ve a method that calls itself..
- it will keep calling itself until the stack overflow error occurs
- in the beginning of the program, when the main method starts, java puts all the information into the main method for the call stack, the informations like name of the method ref to any variables that we passed in etc .
- once that method finishes, and our program leaves the method, java takes out the informations from the call stack.
- and if that method was the main method at the botton of the call stack, then our program exists and finishes.
- but if the java runs the main method and it has the call the other method, it adds the info about other method to the call stack too and the other method has to call the another method and it will add the information about the another method in the call stack as well..and it will keeps going as the deeper of the method calls goes.
- java only ve so much memory allocated to this call stack, and if we keep adding the method call info to the call stack, that call stack will gets huge and it overflows.
- if we know how to code our recursive program properly, then the stack overflow error won't happen.
- so we need an exit strategy to stop the method calls itself at some point.
- for ex ` public static void main(string[] args) {
  sayHi(3);}
  public static void sayHi(int count) {
  system.out.println("Hello")
  if (count <= 1){
  retun ;
  }
  sayHi(count -1);}

              - it prints our 3 times, when the method calls itself with the count param of 3 and the -1 is 2 and for the next call 1, and returns and exits that run of the method.
              - and that run of the method that returns back to the run of the method that calls recusively and it will return to the run of the method that call sayHi() that called it.
              - and those method run completion keeps bubbling all the way back up to the original call (main method) and finally this original method calls is completes which ends our main method and ends our program.

- there are couple of exit strategies we can follow

  1. Base Case - is a codn inside our method that can't return w/o making an another recursive call.

  - in our case the base case is the codn, where the condn is reached it will return out of the method w/o making recursive call to SayHi
  - and in our method we need something that allow us to work towards the base case every time it recurses,

  2. in our program that was done by adding the count param.

  - so every reccursive algorithm needs to ve this two elements, to not get into the stackoverflow

  - note: if we ve the count arg of like 10000000 then regardless of those codns still we may gets into the stackoverflow error as the call stack in java could hold sizable amount.

  ## 5 simple ways to solve the recursive problem

  1. ex: write a recursive fn that given the i/p n sums all the non negative ints up to n

  - sum(0) --> 0, sum(1) --> 1, sum(n) --> (1+2+3..+n)

  1. the first step is to what is the simplest possible solution to this problem

  - the simplest solution is the **_Base Case_** the base case of the recusive fn is the only i/p where we provide an explicit answer, all other solutions to the problem are build on the base case

  2. the second key step is play around with the examples and visualize
  3. RElating hard cases to the simpler cases.
  4. Generalize the pattern
  5. write the code by combining the recusive patterns with the base case.

### Records

- to create a record we use public record RecordName() {} instead of public class..
- ie coz, record are certain special type of class, like enum is also special type of class.
- this will create a final private fields for all the val/props in the records.
- this also creates getter methods for each of these fields, but the getters won't ve get in the name, they just only ve the field name.
- and it also creates all args constructor and assigns the all of those fields to the vals in the constructor params
- this type of constructor thats generates when we create the records are known as canonical construtor.
- and this will also generates toString(), equals(consider two objs of the record class are equal, and if its equal and all of the fields are equal ) and hashCode() methods for those fields.
- Records are immutable by default.
- inside the record we can create static methods, static fields
- however we can't assign not static instance fields (any instance field has to define as the params while creating the record)
- and we can't extend the record class (all the record class extends from the java record class, since java doesn't allow multiple inheritance)
- records are also implicitly final classes which means that they can't be extended by any other classes.
- However we can implement the interface to the record classes..
- we can override the default canonical ctor, to do that we ve to pass the args as the same name in the same order.
- one reason we wana overide the ctor may be a validation checks, ex we don't want the emp num to be negative.
- there is cool short cut we can use to override the canonical ctor, ie the "compact ctor".. (which is unique only for the records)
- the compact ctor will looks like with our args and the field val assignments public NewRecord{}
- and we can also use the normal ctor, if we want.

### Annotations

- the annotations can be processed by the the compiler during the runtime with some code that we write ourselves,
- @SuppreessWarnigs("unused") will suppress warnings for the unused vars/ fns.
- to create our own custom annotations is sim to creating a class ex - lets crreate a annotation called VeryImportant ex - public **@interface** VeryImportant{} - that's it, this is how we create the custom annotation. and there are couple of more annotations we want to add in our custom annotation class.
- 1. @Target({Element.TYPE}) for where this annotation is valid to be used on, ex - Element.TYPE means its only valid to be used on the class and lly we can add multiple elems for ex if we want to use it for the methods just use the Elemenet.METHOD obj, if we want to use this custom annotation for the entire app, then we can leave the @Target().
- 2. @Retention(RetentionPolicy.RUNTIME) // 99.9% of the time we will use this retention policy.runtime, means telling the java to keep this annotation around runtime so the other codes can look and use it while the program is running.

  - and the other 2 vals we can set for the retention policy are "source and class". source - java will get rid of the annotation b4 even it starts to compile the code. only used for the annotations that matters for code b4 even compile ex - @SuppressWarnings()
  - class means java will keep the code during the compilation and once it starts during the runtime it will get rid of the annotation
  - to check if the class has any annotations in it ex - myCat.getClass().isAnnotationPresent(VeryImportant.class); or we can wrap this code with the if statement. to check if the class has any annotations and console.log.
  - lly to check the method we can use myCat.getClass().getDeclaredMethod() or we can loop thru ex for(Method method : myCat.getClass().getDeclaredMethod()) { if(method.isAnnotationPresent(VeryImportant.class)){ method.invoke(myCat}}

- we can also declare the props for the cust annotations inside the class body ex - lets add the prop of time that runs our annotation wants to run. But the property field has to be the method not the regular fied ex { int times();}
- then pass the arg while calling the annotation ex @VeryImportant(times = 3) and access this property inside the condition statement
- ex - `for(Method method : myCat.getClass().getDeclaredMethod()) { 
  if(method.isAnnotationPresent(VeryImportant.class)){
     VeryImportant annotation = method.getAnnotation(VeryImportant.class);
     for(int i = 0; i < annotation.times; i++){
      method.invoke(myCat)
     }`
- now while using the params for the annotation the params can be of primitive type, class, string or array of any of em.. and we can also pass in default val ex - { int times() default 1;}

- lets create and process a cust annotation for field in a class, lets say the annotation is intended for the str field in our class, everything is same as the above except this time the target is for the field ex @Target(ElementType.FIELD)
- now we can use this in the fields of our myCat class and to access the declared fields of myCat ex - myCat.getClass().getDeclaredField()

### Junit testing

- as we knoow the unit testing is the testing that isolates one single piece of code and verfies that piece is working correctly..
- add the Junit to our dependencies, and in our class (in intellij) just hit ctrl + shift + t - to create a new test class (file). which will be imported from the junit.jupiter.api.assertions package.. and our test file will be in the path src/test
- @Test() inside the test class we mark our test fns with this annotation. and the remaining tests fn are same as we seen like milliion times ex - assertEquals(expected: 4, actualResult); ..lly for the negative / fail test we can use assertNotEquals() and assertTrue() for boolean tests, and assertNull(), assertNotNull() ..etc
- one of the goals of the unit test is that whenevr code is not doing right thing at least one our test should fail.
- we do ve the option to run our tests with coverage and if we run with the coverage option then we can see the which lines of code are covered /tested.

### spring batch processing

- as we know the batch processing is the technique to process the data in a large group instead of single elem of data. ex the source can be a large records of csv file to the (destn) can be the db and vice versa.

- there are list of components in the spring batch processing 1. Job Launcher (interfaces) is like the entry point to initialize any jobs (runs any job)
  - 2. Job Repo (has the state of the job)
  - 3. step (has item reader, item writer, item writer) the job can ve multiple step. and for each of these (step and job ve factory interface of their own ex - JobBuilderFactory, StepBuilderFactory)

**Implementation demo** - the code repo [here](https://github.com/Java-Techie-jt/spring-batch-example)

- lets install some dependencies such as mysql lite, jdbc, jpa, spring batch, and we ve dummy csv file ex - customer info, and to map this csv to the db we ve to create a entity file with all the fields in the csv file.

- the in our config class along with the @Configuration() annotation we ve to also use the @EnableBatchProcessing() annotation
- the DelimiterTokenizer class will helps to extract the vals from the csv and the BeanWrapperFieldSetMapper class will map the vals to the target class(customer), this classes we impl in the **item Reader class**
- lly we ve to impl the **item Processor class** and the **item Writer** - and in the writer is where we write /save all the vals (read from the csv) to the db.
- then create a **Step** class and pass in all those 3 item reader, processor and writer. and this step we will also specify the chuck the (no of chuck size we want to use for our data)
- then we ve to create a job class and pass in the step obj.
- now we ve to give this job obj to our **Job Launcher** so we can trigger the job (we can do this impl inside the controller)
- then we can start our app, and in the db we can see there are few more tables added by the spring batch in our db.
- but so far our spring batch will add all the records synchronously(by default) and to leverage the use of spring batch we ve to do this asynchronously,
- so for that we ve to create a class - TaskExecutor (from io thread) - and pass in the concurrency limit (10) - 10 threads to execute the task. but there won't be any ordering in the id (it will just add the record to the database randomly)

- **Partitioning** - the partitioning is to achieve the parallel processing and the partitioning means assining multiple threads to process a range of data sets. ex - we can assign the record of 1 to 1000 to thread 1, and 1001 to 2000 to thread 2.
- the partition class we can implement from the Partitioner interface

### Spring Bean life cycle

- as we know the spring bean class has the @Component() annotation, means it will be managed by the spring container, the container is the context of the app. means the program will keep track of all the class with the @Component annotation and it will **inject** em in the other part of our app.
- some of the common bean annotations are @component, @Bean, @Controller, @Service, @Repository, @Autowired, @RestController..
- **Spring bean Scope**

  - we can define the spring bean init and the destroy methods in the @Bean(name="orderBusinessService", initMethod="init", destroyMethod="destroy)
  - call the "initialize" method when the Spring bean is created and call the "Destroy" method when the spring bean is destroy (to avoid the mem leaks )
  - when defining the bean we ve the option of declaring the bean with the **scope annotation** which tells how long the Spring bean will live.
  - so the bean life cycle will go like this: initialization, utilization, destruction.. the utilization will tells what the purpose of the bean is.

  - **Spring boot scope option**
  - Singleton - is the default scope, this scopes the bean definition to a single instance per IOC container. - @Bean() - (as we know the usage of the singleton scope, it can also be used for the db where the connection can be live for the entire application, or for the controller)
  - Request - this scopes the bean deifnition to Http request - @RequestScope() (for the search result we can think of using this scope)
  - Session - this scopes the bean deifnition to Http Session - @SessionScope() - this scope can be used in the authentication. or in the game of where we can save about the user states. so the session scope peresist until the user logs in and logs out (during the session of the user)
  - Global - this scopes the bean deifnition to the global Http session.
  - Prototyp - this scopes the single bean defintion to have any no. of objs instance.

- **@PostConstruct, @PreDestroy**
  - in the ex - he just write the jdbc code to connect the db but didn't close the connectioin - hence the mem leaks.
  - the @PostConstruct() annotation will exec the method once the bean obj gets initialized.
- as we know the flow of the DI, once the obj gets instantiated it will loads all the dependencies, and then calls the init() method. and this init() we will annotate with the @PostConstruct annotation
- lly for the db connection close() / clean up method we can use the @PreDestroy() annotation, which will destroy/delete b4 the obj the container gets destroy (to preserve the mem leaks)
- this is for the standalone app(desktop app), we ve to do it this way with the @PostConstruct and @PreDestroy, and for the web app, the spring will do it automatically. ex - in the standalone app - in our main app class file.. AplicationContext applicationContext = new ClassPathApplicationContext("beans.xml"); ((ClassPathApplicationContext)context)).close(); // we can either call the close() method or registerShutdownHook()
  - this registerShutdownHook() will exec once the main thread ends. so once all our code gets executed, it will be called and close our container, so it will not give any exception irrespective of the line no. we re calling it.
  - what if multiple class contains init(), destroy() ? instead of defining multiple times, in our beans.xml file we can define it with the **default** kw ex - default-init-method = "init"; default-destroy-method = "destroy";
- another way to initialize the bean is to use / impl the interfaces **InitializingBean** in our class and **DisposableBean** interface to impl the destroy() method. by using these interfaces we now no need to use the annotations and then defining in the xml these interfaces will take care of the things for us.
- but the recommended way is to use the annotations instead of the interfaces.

### Reactive, Spring Webflux

- when we re sending the async fetch request to the db, the servlet thread in our tomcat server (lets say the limited servlet thread pool size is 200) will be able to manage 200 reqs async and if any additional requests comes in then the servlet will wait for the thread to finish the task and use it to handle the requests.
- but there is a better way/ mechanism to do this, so in this approach the servlet thread will not wait for the db to finish fetching the data instead of waiting it will immediately return and takes the another request. once when the data is fetched from the db it will takes and res to the request. the res can be send / pick up by any thread that is free.
- so this way we can eliminate the waiting or no blocking mode (for the thread pool) .. and this mechanism is called **Event Loop** so every thing is an event in the event loop and we know how it work {refer the ts notes fro the async event loop mechanism}.
- And this mechanism/ complexity is managed by the **spring webflux** which is an end to end reactive. can use less no.of threads and can ve higher CPU efficiency and higher scalability.
- so we can think of it as the node js.. this webflux will brings the node js async type of functionality into spring for handling the non blocking.
  - and to handle this reactive approach the spring framework also has the reactive db drivers such as mongodb-reactive, cassandra-reactive, redis-reactive, spring-jdbc-reactive etc.
- for using the non blocking we can use the **CompletableFuture<T>** as the return type in our controller path. means we re saying that we don't ve the val rn and we will add it in the future obj.
- in spring webflux the name for this CompletableFuture<T> feature like entity is called **Mono<T>** and **Flux<T>**
- these are the 2 types of futures in spring webflux, the Mono<T> return type we can use when we ve 0 or 1 element. or single val
- and if we ve more than one elem we can use the **Flux<T>** return type.. for the list of vals or the live List of vals(stream of events like the rxjs) - so every time the record is inserted into the db it will emit the event then it will stream the live data to the client w/o having the client to continously pooling the data.

  - this similar approach we can also use it in the req as well ex - in our controller - public void getLivePrice(@RequestBody Flux<T> prices){price.subscribe()} // and the annotation for this method will look like @PostMapping(value="/save/price", consumes = "application/stream+json" ) //
  - here this consumes param will keep the connection b/w the user and web server open, and the client can keep pushing new vals at any point in time.

- there are also other features of the webflux such as fuctional Api, combinators, webclient, Backpressure, scheduler.

- **Full reactive Stack**
  - we can add this approach to our existing application, in spring MVC we can impl this webflux to make it reactive non blocking i/o (reactive request handling in @Controller)
  - the web client came in jdk 8, which has the fuctional fluent api, async non blocking io, reactive/ declarative by design, Streaming.
  - just like the rxjs the Mono or the Flux<T> future vals we ve to **Subscribe()** for getting the streams/ non blocking io
  - just like the observables, the Stream class has the of() ex - Stream.of(), and the Mono.when(), .take() methods etc
  - the reactive out of the box only runs 4 threads, and to achieve the non blocking it relies on the callbacks.
  - when we work with the reactive and making the nested asyc call (which itself returns the flux or mono) we ve to use the **FlatMap** instead of map() ex - Flux.range(1,3).flatMap(i -> client.get().uri("person/{id}", i).retrieve.bodyToMono(Person.class).collect(Collectors.toList()).blockLast()). // .blockLast() is same as the .then().block().
  - these are all from the **webclient** however also in the spring MVC web supports as well
  - the spring MVC supports reactive, the controller can return Mono, Flux and **Observables**. It decouples from container thread and built on top of servlet 3.0 asnyc
  - and the reactive type handling are for Mono - DefferedResult, for Flux/non-Streaming - DefferedResult<List<T>> and for Flux/streaming - ResponseBodyEmitter with back pressure
  - the reactive handling is not just the web client, it also supports spring Data Repositories, websocket client in WebFlux, CF java client, RabbitMQ Http client etc.
  - web test client - we can use it for the integration testing of the web client (in streaming scenarios)

### linked hash map and linked hash set

- linkedHashMap - is a combination of link list and hash map extends the hashMap<T> and impl the map interface.
- this maintains the order of the insertion (kvp).. so our entry (record) will ve key, value, hash(of k and v), next(by default it is null) val, before(node), after(node)
- the linked hash map also has the head and tail - (the first node and the last node) which is not available in map.
- all these six vals in the Entry class..
- when we create/enter a first record the default vals of the next, before and after are null, and it will store/contain the val of the head and tail(which are record itself since it is only one record ).
- and when we start adding the subsequent record it will update the tail of the first record and also the after val of the first record. and the head, before of the second record. lly for all the subsequent records. it will keep track.
- these all are for the put/post operation.. lly for getting the records, it will hash() our key and search for the key (hash already exists) and finds the val of the hashed key.
- lly there is **linke hash set** instead of storing kvp, it will only store the val.

- **HashMap** - ex HashMap<String, Integer> employee = new HashMap<>(); // will hold the empl name as the key and the id as the val. and it won't accept the **primitive types** ex int, or long
- and to add the recordes - employee.put("jon", 1234);
- the hashMap doesn't care about the order.
- and to get the val we can use the get() ex - employee.get("Jon")

### Spring Boot web service using SOAP (Simple Object Access Protocol)

- SOAP is a messaging protocol. Messages (requests and responses) are XML documents over HTTP. The XML contract is defined by the WSDL (Web Services Description Language). It provides a set of rules to define the messages, bindings, operations, and location of the service.
- we can use the web service spring boot starter package(which has the soap) to use the SOAP, and additionally we need to ve the **WSDL**(wsdl4j) dependency, if we re creating the soap based service we need to ve the WSDL, stands for web services descriptive lang. its sim to json.
- there are 2 ways to develop the soap based web services, 1. contract first (wsdl to java) and 2. contract last (java to wsdl).
- in the spring framework we don't need to write the wsdl file only we need to write the xsd file (xml schema definition), spring boot will generate the wsdl file for us.
- in the xsd file we only write the cust req (like name, age, income) and in the response we write like (is eligible, approved amt)
- also for the soap req we can create a xml file <soapenv:Envelope > and lly like the soap env header and the body
- once we created our xsd file we need to gen the binding class for that we ve to use the **xjc** plugin,
- now if we build our app, we can see the sb creates a package that we mentioned name in the xsd file and inside there is a class with all the fields that we mentioned .
- it creates bunch of class files like one for the req, one for the res, and obj factory class etc.
- and then we can add the controller, config nd service impl class by ourselves.

- **Consuming a Soap web service**
- just like the REST templates(for consuming the restfull app), sb provides the template for consuming the soap based web service
- just create a new sb project (client for consuming the soap based web service)
- we ve to create the binding class (from the wsdl file) for that we can use the plugin called "jaxb2" is from the jaxb2 marshaller
- once we done with the soap client we can use the REST controller to invoke our soap client to get the loan status of the client

### AVRO (write a kafka avro producer)

- producing avro msgs on kafka using sb.it uses **confluent** platform and uses schema registry to service to deal with the avro schema files.
- the dependencies are 1. schema registry clients 2. avro serializers (to use the avro msgs we need to serialize them in order to push the topic) 3. AVRO (specific to create the schemas.avsc files) 4. Avro maven plugin (to gen the java classes out of the schema) we also ve to specify the src and the destn in the pom file under the plugin section
- in the resource dir we can create a package "avro_schema" and inside we will ve our avsc file which is nothing but a json file. and this we can use to publish the msgs on the topic
- now if we build the app, then it will gen the AVRO file out of the avsc file, and when we push the msgs we will use this avro java class file.
- then in the yml file while configuring the kafka producer, under the producer section we use the "key serializer (from string Seriralizer class)" and the value serializer, the classes that we installed from the avro serializer package.
  - and in the properties section just mention the schema registry url (default port name of thhe registy server is 8081)
- and also mention our **topic** to publish the msgs

- then we can craate the producer package and create a service class for the kafka producer(DI kafkaTemplate) and it will ve send() and inside this fn we will use the send() from the kafka template(which takes the key and val and key must be in the type string so use the **String.valueOf()** to type cast)
- then create a REST Controller,
- then he started 3 services from the confluent platform, 1. kafka 2. zookeeper 3. schema registry(since we re gon to produce the avro msgs, we re gon to talk with the schema and avsc file.)
- then finally use the post man to send the req (with the json body)

### spring boot avro Kafka consume msgs from the topic

- lly like the producer, just in the yml file define our consumer properties - and here sim to key and val serializer we will be using the "key Deserializer(from string Deseriralizer class)" and the "val Deserializer" for the consumer.
- along with them there is also one more prop **autoOffsetReset** - which ve 2 vals/options tells when we want to read the data 1. earliest(first - reads the msgs from the kafka topic from the 1st offset) or 2. latest (read the msgs from the kafka topic from the latest offset)
- then create a consumer cofig class with the we ve to craete couple of beans 1. consumer factory and 2. kafka listener container factory.
- then create a consumer service class with the @Listener(topic ="", consumerFactory ="KafkaListenerConsumerFactory") annnote with our read()

- refer the code [here](https://github.com/vishaluplanchwar/KafkaTutorials.git)

- **Topic** is the stream of data, is sim to the table in the db.
- **Partitions** the topics are split into partitions, the partition starts from 0, each partition is ordered, and each msg with in the partition gets incremental id called **offset**
- data is kept in kafka only for a limited amount of time (default 1 week), but the offset will keep increasing (it doesn't go backwards).
- once the data is written to the partition it can't be changed (immutable)
- **kafka consumer and consumer group** lets say if our consumer wants to read msgs from the partitions in parallel(read 2 msgs) - here with in each partition the consumer will read msg 0 then msg 1, means with in the partition the msgs are read in order.
- but they are read in parallel across partitions.
- **consumer groups** the consumers can be organized into consumer groups to leverage the parallelism.
  - we can't ve more consumer than the partition otherwise the consumer just does nothing(inactive).
  - lets say if we ve 3 partitions then in our consumer group we will ve 3 consumer reads from each partion to make sure the parallelism.
- **consumer Offsets** - now how do the consumer knows where to read from - thru the consumer "offsets"

  - kafka stores the offsets at which the consumer group has been reading.
  - the offset commits live in kafka topic named "**consumer**Offsets"
  - when a consumer has processed the data receieved some kafka, it should be commiting the offsets.
  - if a consumer process dies it should be able to read back from where it left off (by using the offsets)

- **Kafka Brokers** - the kafka cluster is composed of multiple brokers (aka servers)

  - each broker is identified by "ID" (int)
  - each broker contains certain topic partition
  - after connection to any broker(called a bootstrap broker) we will bee connected to the entire cluster.
  - a good no. to start with the brokers is 3, but some cluster ve 100s of brokers on em.
  - each broker consist of partitions and topics (some brokers may not contain the topics), if we connected to the broker that doesn't ve any topic it will redirect us to the broker that has our requseted topic.

- **Topic replication Factor**

  - the topic should ve the replication factor which is gt 1 ( usually b/w 2 and 3)
  - so this way even if a broker break down/crash the other broker will serve the data.

- **Leader of the broker**
  - any time a broker can be leader of the given partition and only that leader can receive and serve the data for a partition.
  - the other broker can synchronize the data.
  - there each partition has one leader and multiple ISR(In Sync Replica)

### Load testing (JMeter apache)

- **Performance Testing** - is a type of s/w testing to ensure the s/w apps that will perform under their expected work loads.
- there are 2 types of tests 1. stress testing nd 2. Load testing.
- **load test** is check to see if the app is perform/ under with the no. of user reqs (ex 1000 reqs)
  - this load testing determines the s/w's performance according to the real life condition. and it checks how much load a s/m can handle.
- **stress test** is check to see how well the app will perform with high load nd limited resources.
- the jmeter also supports stress testing, distributed testing, web service testing by creating **test plans**
- this also supports protocol such as http, jdbc, jms, soap and ftp.
- it can support reporting, can support dashboard report generation.

- **Elements in jmeter** there are 4 elems / components in the jmeter 1. Thread 2. sampler 3. listener 4. configuration
- 1. thread group - is a collection of threads, each thread represents one user using the app under test. (in other words each thread simulates one user req to the server.)
- 2. Sampler - since the jmeter supports multiple protocol and how does the thread knows which protocol to use for the req, this is where the sampler comes in, and it will ensure to use the correct protocol for the user req to the server.
- 3. Listeners - shows the result of the test execution, table / graph / tree / log etc..
- 4. configuration - it sets the default vars for later use by the samplers - this also have http cookie manager, csv data set config, login config, http request default. etc

- jmeter also provides "Recording and playback"

### Quarkus and JPA Streamer(fcc)

- quarkus is a open source java development platform. just like we sb we can write our app normally and the build process is bit different.
- some of the advantages are we can be able to restart our app with in split seconds, and when we re ready for the deployment, the native build capabilities of qurkus will boost our performance significantly. both in terms of start up time and reduce the mem footprint

- **JPA Streamer** is an extension for the hibernate that doesn't impact the hibernate how we'd use normally but it does extend the api with the capability for **java Streams**, means the stream api as the way of expressing our queries and upon execution it will automatically translate our queries into sql.
  - aueries are - intuitive to read and write, type-safe, translated to efficient hql queries,
- its a cloud native framework. low mem footprint, higher throughput, and it integrates wide range of java libraries.
- less boilerplate, live reload, more action, continous, targeted, testing

- querkus.io is the starter just like the spring io starter we can select the build tool, artifacts and the starter plugins we need.
- for the db she uses the oracles 'sakila' db in the docker, which comes with the actor (imdb) model. `docker run --platform linux/amd64 -d --publish 3306:3306 --name sakila restsql/mysql-sakila`

  - the db's user name - root and the p/w - sakila. and the url - jdbc:mysql://localhost:3306/sakila
  - and then sim to the sb, put all the db props in the application.properties file
  - then only for the actors and the films table she generated the entity class.

- the streamer will translate our stream pipeline into sql queries and in order to do that it need some extra meta information about the columns we wanna query, (JPA MetaModel)
- rigth click our app and select build module - it will gen the meta model and the target dir..
- and the generated source dir we ve to mark / assign as the "generated sources root".

- then create the endpoints to serve the films to the clients. REST controller
- **Quarkus Dev mode** - to run the app in dev mode the cmd is (also we can find in the readme file) `./mvnw compile quarkus:dev`
- the quarkus also provides swagger ui in our rest controller endpoint ("/").. and for the every sub sequent endpoints or the changes we make in our app, and we save it it will replicate in the running app, just like the HMR hot reloading (front end) w/o having to restart our app manually.
- and to make our class singleton make sure to use the **@ApplicationScoped** annotation
- also in the controller we can use the **@Produces(MediaType.TEXT_PLAIN)** annotation
- sim to nest or the ng, to do the DI we can use @Inject annotation. and our streams we can use the post fix $ ex - Film$ - is the stream.
- while making any updates / put method (in our service class) we ve to use the **@Transactional** annotation to persist any updates that we make on the entities.
- quarkus has the cool feature built into the dev that allow us to create tests that runs every time our app restarts.
- **continuous JUnit test** - quarkus comes with the default jUnit 5. - we can ues the **@QuarkusTest** annotation, we can also use the plugins called **rest-assured** to test our rest controller ..
- lly we can also perform the unit tests
- **Debugging in the dev mode** - `./mvnw compile quarkus:dev -Ddebug` just add the Ddebug flag in the run command.
- then edit our intellij configuration - and select remote jvm options.

- **Native compilation** - `quarkus build` to build our app (creates a std jar file) - the target dir will be created and inside we can find our app.jar file and to run the jar file using the regular java command `java -jar target/quarkus-app/quarkus-run.jar`

---

### Pyspark - apache spark(fcc)

- spark is a unified analytics engine for large datasets - this also provides the data frames sim to the pandas DF
- this also provides spark "M Lib" which allow us to perform ML task like regression, classification, and clustering etc.
- some of the features are its faster, easy to use(in multiple langs), generality etc.
- we can use the jupyter note book - `!pip install pyspark`
- when we work with the pyspark we always starts with the park session ex from pyspark.sql import SparkSession; then give it the session name ex - session = SparkSession.builder.appName("practice").getOrCreate();
- and sim to the pandas we can create df and read / write csv files and use head() , show(), tail() fns
- add / drop columns describe(sim to pandas)
- to check the data type of the column we can use df.printSchema(). and to see the data types of each coulumns df.dtypes();

- **handling missing vals / null values**

  - handling missing vals by mean, median or mode.
  - to drop the null val columns we can use df.na.drop() lly for filling the missing vals df.na.fill() fill takes 2 arguments 1 val and subset/column
  - import the Imputer and then we can use the mean, median for our missing vals.
  - the tilde ~ sign is the not operation !

- **Data bricks** - also helps us to impl ml flow / cicd pipeline
