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
              sayHi(count  -1);}

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
    - the simplest solution is the ***Base Case*** the base case of the recusive fn is the only i/p where we provide an explicit answer, all other solutions to the problem are build on the base case
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

- 


