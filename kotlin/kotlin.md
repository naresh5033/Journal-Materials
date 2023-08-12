# KOTLIN (.kt)

- kotlin is a statically typed language.
- the kotlin and the java are interoperable, means we can ve java code in kotlin and it will just work as fine.
- the jdk will take the kotlin code and convert into java byte code then it will run in the JVM.


### variables 

- kws, lets see the variable keywords we can use, 
  - var, val(const).. var is mutable, val is immutable
    - Int (min- -2.14b, max- +2.14b) is the default type inference (for the numbers)
    - short (min- -32768, max- 32767)
    -  long(min- , max- ) - is much longer amongst all its like 9,223,372,036,854,775,807
    - ~byte (min- -128, max- 127)
    - float - val number = 2.5f
    - double - val number = 2.5 default is double(infer)
    - and along with these bool, and char and all these 8 types are primitive data types,
- it is also **type Inference** even if don't specify the type it will infer the type based on the var vals

- note: kotlin supports the prefix and the post fix operation (increments or decrement) ex we can use ++num; or num++

### when Statement

- its something similar to the match or switch state 
  - ex - val x = 5; 
    when (x) {
      5 -> println() 
      6 -> println()
      else -> println()  ... its like the default stmt
- }
- we can also combine the val inside the when(x) {  5, 6, 7 -> println(${x) } .. now this will check which of the val is true or declared. this is like or operator.
- we can also use **in** kw to mention the range ex - when (x){in 1 .. 10 -> println()}.. the range 1 to 10. and we can use the not operator ex - !in 1..10 - not in the range..

### nullability

- by default we can't assign the null val to the var except if we use the optional ? at the type then we can use the null val ex .- var num: String? = null .. (w/o the optional ? we can't assign the null val)k
- we can use like .. var text: String? = null; println (text!!.length); -- !! is non null asserted 
- **elvis operator** is like the teritiary - ?: ex - var text: String = text ?: "some text"; if the condn is true assign the val on the rhs of the ? operator else the lhs of the operator

### functions

- just like the normal fns (like ts), nothing unusual/special here
- and the return type is mentioned as colon : -- ex fun add() : Int { return x}  and the return is the max reachable inside the scope, anything after the return won't be exec.
  - there is single expression fn- fun add(a, b): Int = if(a>b) a else b;
- **function overloading**
- fn default val or param ex - fun add (a: String ="hello") {} now hello will be the default value
- lly we can also assign a fn as the default value for the params
- **varArg kw**
  - the vararg kw allow us to pass multiple arguments in a fn (while calling) regardless of how many params that has been defined in the fn. ex - fun add(vararg nums: Int): Int{nums.forEach {println(it}}; add(1,2,3,4,5,).. for each will iterate thru all the nums in our arg

- **the range** as we know we can use .. for the range, for(i in 1..10){println(i)}. lly we can also use for(i in 1 **until** 10){println(i)}.. but the diff here is the range is included 10, where the until kw will exclude the 10 (til 9)
  - and to reverse the loop itration we can use the for(i in 10 **downTo** 1){println(i)} .. this will prints 10 to 1 (reverse iteration)
  - we can also use the step to skip the iteration ex - for(i in 10 **downTo** 1 step 2){println(i)}.. will skip the every 2nd iteration

### while and do while

- it supports while and do while loops (as long as the condn is true) ex - var num =0; while (num < 10) println(num++)
- the do while loop - atleast execute the code once(irrespective of our condition), ex - do {println(num)}while (num >10) 
- **continue and break** the continue kw is used to bypass the condn/ code.. and the break kw is used to stop the loop.

- **label** is like a decorator or annotations.. ex in the nested while loop if we use the break kw in the inner while loop then we can able to stop on the inner while loop, to stop/exit the outer while loop, then we can use the label kw.
  - ex - @outer while{..while(){..break@outer}  }, so with this @outer kw in the outer loop and the inner loop's break@outer -- now this time around the break kw will break the outer while loop(not the inner loop).
  
### Arrays 

- ex - var num: Array<String> = arrayOf("ram","jon"); .. here since its type inference we dont ve to mention the type (Array<>)
- **is** the is kw is used to see if the var is of certain types ex - for (i in nums) { if (i is Int) {println(i)}}

### oop

- yes, just like java it has obj, and classes 
  - **primary constructor** - we can directly assign  the val to the constructor instead of the parms ex - class Car (model:String){ var model = model}.. instead we can assign like class Car(var model: String); both are the same
  - this is like moving the prop to the params, we re directly storing the var in the prop
  - kotlin also support the **this** kw

### initializer block 
   - lets see how to exec more lines of code when an instance is created. ex - class Car(model:String){ var model = model; **init** { }} 

### secondary constructor 

- besides the primary ctor we can define multiple ctors and they are called as secondary ctors, and we can pass only the params and can't define the props unlike the prim ctor.
  - ex class User(var name: String){ constructor(name: String): this(name)

### getters and setters

- in kotlin the getters and setters are assigned automatically, every time we call the val of the prop and assign the a new val to the prop, we are calling them (Bts)
- and therefore we never access the vals of the prop directly, that ensures the encapsulation. in other words we never allow any one from outside of our class to access the props directly.
- they are implicit (declared automatically), every time we declare the prop either inside the class or primary constructor.
- however we can also modify the default getters and setters, (if we wana perform some additional code). and we need to define them immediately right after we assign the prop
- ex - class User(name: String) { var name = name ; get(){return "name" $field} set(value) { field = value}}

### late init kw (oop)

- sometimes we wanna declare val/prop to the class but we don't want to assign it right away/ immediately, we wanna assign it later. and to do that we ve to use the lateInit kw.. 
  - ex - class User(var name: String) { lateinit var favmovie: String; } .. lateInit - initialize later
  - in that ex since we already directly assign the prop as param (inside the class) so even if we try to assign a prop outside (w/o the lateinit kw), we will get the compile time error, and we need to add the lateinit to fix it.
  - and finally make sure to call the prop (lateinit) ex - User.favmovie = "interstellar"
- and this kw is only work with the string or other class type not with the primitive types(since the primitive types are not the classes) ex - lateinit var num: Int --> this won't work.

### Companion Object

- if we ve a class and the class has a fn and to call the fn we will first create the obj/instance of the class and then call the fn. so everytime we need to use the fn we ve to create the obj of the class.
- so wouldn't be great if we can directly call the fn w/o declaring the obj every single time.
  - ex - class Cal(){companion object {fun sum(){}}}.. now this fn isn't belongs to the obj/instance instead it belongs to the class itself. so when calling the fn we can call it directly.
  - ex - Cal.sum() --  
- there is one more good ex of the companion object is Int.MAX_VALUE -- here we re using the Int's class's companion object ie MAX_VALUE

### singleton

- as we know we wanna create a single instance and use it thru out the program ex - DB instance.
- note : when use the access modifier with the primary ctor we need to use the constructor kw ex - class DB private constructor(){} since this class is **private we can't create a instance** of the class and we can't ve the access to the fns or props we can use the companion object inside. 
- so by that way we can access the props or fns outside of the class w/o creating the instance 
- there is also **object kw** there is also object kw with that we can create a singleton (instead of using class) 
  - ex - object DB { init { println() } }

### lazy initialization (oop)

- is used when creating an instance / obj is expensive.. means in prog terms it takes time and consumes mem.
- to create the lazy initialization we ve to use the kw **by lazy** ex - class `user(){} and while creating the instance ex - val user:User by lazy { User() }
- although we ve created the lazy obj, it will get initialized only when we use the obj (otherwise it won't init)

### enum (oop)

- enum classes are used only when we want some fixed set of vars / constants, and they are powerful we can use props or fns .
- ex - enum class Direction() { SOUTH, NORTH, WEST } and to print the vals in our enum[] .. for (direction in Direction.values()) {println(direction)
- lly to the values(), there is also **name** ie built in to the enum class ex - println(Direction.NORTH.name) ..we don't ve to define the name prop its built in to the enum class.
- use  the enum class with **when** kw
- ex - val direction = Direction.EAST ; when(direction) { Direction.EAST -> println() } -- its something like or similar to **match** statement
- val direction = Direction.valueOf("east".uppercase())

### inner classes

- are declared an another class, are generally used when there is a close relationship between 2 classes. and the inner classes will ve the access to the props / vals of the outer class.
- to define the inner class we use the kw **inner class** ex - class ListView(){ inner class ListViewItem() {}}

- note : similar to the arrayOf("","",""), we also ve the another kw ex - var tx = **mutableListOf<Int>()**

### inheritance (oop)

- the class which we re inheriting is called the base class, super class, or parent class, it will reduce the code repeatation and less coding.
- **open kw** to inherit from a class the parent class must be marked with the kw open,coz by default its final. ex - open class Vehicle() {}.. now we can inherit ex - class Car(): Vehicle(){}
- and in the parent class we can allow a fn to override in the child class, and to allow the override again we need to mark the fn with the open kw ex - open fun stop(){}.
  - now in our child we can override the fn (from the parent) ex - override fun stop(){super.stop()}
  
### Sealed classes

- .often we wana cosider the fixed set of possibilities, we can use the enum classes but enum classes have some limitations.. ex - enum Error{SUCCESS, FAIL } .. here we can't defined the non resolver error ex - FAIL("some error") can't do that,
- for that case we ve to consider using the sealed classes ex - sealed class Result(val msg: String) { class Error(msg: String): Result(msg)} lly create a Success class inside the sealed class and it also inherits from the Result (parent) class.
- now when we use this inside a **when** codn which is sim as the match stmt the results must ve to exhausted ex - fun getData() { when (Result) { is Result.Error -> prinntln(); is Result.Success -> println()
- with the sealed class, the compiler knows the exhaustive and it knows all the possibility cases so it can fill for us
- so this is why sealed classes are more powerful than the enum classes, it can ve sub classes it can ve more info (error), exception passes and so on.

### Abstract classes
- the abstract classes are similar to the interfaces the difference here is we can declare the props which can ve the vals.
- with the abstract classes **we can't create the instance, we can only inherit** them (they are only created to be inherited)
- to create an abstract class ex - abstract class Car () { abstract fun stop(); } .. we can also use the abstract kw to create the fn (abstract fn) but it should not contain the fn body, that fn should be impl inside the class which inherits this abstract class.
  - ex - class Tesla(): Car(){ override fun stop() } .. now here, when we override the abstract class's fn we can ve the fn body (and the logic defined)

### Data Classes

- 1st  lets see the diff b/w the structure equality and the referential equality, 
- the structure equality is when we check to see if the 2 vals are same or not ex - print(name1 == name2) .. returns bool
- lly the referential equality is check to see if the 2 vals are ex same - println(name1 === name2) this also returns bool.
- in kotlin by default every class that we create is implicitly inherits from **any class** that has some abstract fns we can use such as toString(), hashCode(), equals() and so on...
- and we can override those fns as well ex - class User() { override fun equals(other: Any?): Boolean { if (this === other) {return true}} -- .. here we re checking like if this (current obj/ current instance) is eq to the 
- here the rule is whenever we override the equals we also need to override the **HashCode** when the 2 objs are equal, they also ve to ve the smae hash code.(although) the hashcode is only for the performance.
- with the use of this **data** kw in our class we don't ve to override the fns (the equals or the hashcode), all the code will be generated automatically for us.
- ex - data class Car() {}

### interfaces

- are commonly used behavior shared among diff classes.. interfaces can't ve constructors, since we aren't gon create instance or obj for an interface.
- they are created only to be implemented by the classes, sim to the abstract class we can ve a fn inside that needs to be overrided while impl the interface.
  - ex - interface Engine {fun start() } .. (the fun w/o the body)and to impl this class Car(): Engine {override fun start() {} } 
  - so rule(or contract b/w the interface and the class) is like if we impl the interface we must override the fn

### object expressions(oop)

- we can use the object expressions (or also called as anonymous classes), ex - val button = Button(id: 123, object: onClickListener{override fun onClick(){}}).
- this is how we assign/impl the interface to the object
- by using this object is like the anonymous class since we don't ve a name or reference for the class,

### Delegation

- means giving power or the authority from one instance from one class to the another class, the delegation is used in the scenarios where inheritance starts to break, something like if wanna inherit from two or more classes (since we can inherit from only one class)
- now with the delegation we can plug in multiple implementations of our own class,
- ex - interface A { fun print() } lly interface B, now.. open class DelA(): A { override fun print(){} } and lly for the DelB extends from the interface B, now with this impl lets create a class and make use of delegate to impl/inherits our both the interfaces
- ex - class App: A **by** DelA(), B by DelB() {now inside this class we can override both the interface's fns} 
- so by using the delegation we can implement/inherits multiple interfaces we want.

### Collections

- are the groups of objects stored together in a single variable, (they are similar to the [] but are not.)
- **lists sets Maps** 
  - they are both mutable and immutable lists, sets and maps .. to create a immutable list - var names = listOf<String>("1", "2");
  - lly to create a mutable list -  var names = mutableListOf<String>("1", "2");
  - in this list we can use bunch of fns like .add(), .remove(), removeAt(), .forEach() etc..

- **Set** as we know the set we can't ve duplicate vals, or no repeated vals 
  - similar to the array we can create mutable and immutable sets 
- **map** is for storing the kvp.
  - and to create a map ex - var user = mapOf<Int, String>(1 to "jon", 2 to "ram") ... map the key to the val.
  - and to get the val we can use the key to call - ex - println(user[1]) -- is jon the key 1 belongs to the val jon
  - lly we can create the mutable map. ex - var user = mutableMapOf<Int, String>(1 to "jon", 2 to "ram")

### Mapping (collections operations) Transformations

- the transformations are some functions which we can change a specific collection. those transformations fns are lambda fns.
- ex - var users = setOf(1,2,3,4); println (users.map{ it * 10}) .. inside teh curly brace whatever thing we specify it applies to all the elems in the set
-  so this transformation fn changes every element in our set and multiply by 10 (at every iteration)
- we can also target specific elem in the set ex - println(users.map{ if ( it==3 ) it*100 else it * 10 })  })
- lly we can use the transformation fn on the map, when ues it on the map we can either use it on the key or the val.
- ex - println(users.mapKeys{it.key.uppercase()}) ... or to use with the vals .. ex - println(users.mapValues(it.value + it.key.length))
- then println(users.mapIndexedNotNull{ index, value -> if (index == 0) null else index + value })

- **zipping and association**

- zipping is the another transformation function, this will allow us to create a pairs of elements in the same position in both collections.
- ex - var colors = listOf("red", "green", "blue"); var animals = listOf("fox", "bear", "cat"); println(colors.zip(animals)).. this will prints all the pairs of colors and animals - [(red, fox)] ..and so on (like the array of tuples)
- lly we can also use the lamba fn inside the zip
- we can also use the unzip() to unzip the pairs - ex- var pairs = listOf ("one" to 1, "two" to 2); println(pairs.unzip); this will unzip and creates ([],[]) 2 arrays inside the tuple

- **Association**
  - allow us to build maps from the collection elems and certain vals associate with em.
  - and diff association types the elems will be either keys or vals in the association map..the basic fn is "associate with"-
    - ex - var nums = listOf ("one", "two", "three"); println(nums.associateWith{it.length}) ; .. the result is the tuple of (one=3, two=4..) ..so it creates the map with the keys (which are the elems in the collection,) and the vals are produced by the expression we defined
    - lly println(nums.associateBy {it.first().uppercase()}) .. now this will produce the keys with the uppercase of the first elem in the collectioin and the vals are just the elems in the collection. ex - {O=one, T=two..}
    - or we can use the key selector and the valTransformer function ex - println(nums.associateBy {keySelector = {it.first().uppercase}, valueTransformer = {it.length})
  - 
- **Flatten**
  - is another transformation fn
      - lets ve an ex if we ve a multidimensional array and to loop thru that is not easy.. ex var numset = arrayOf(arrayOf(1,2,3), arrayOf(4,5,6)) and to access this println (numset[1][2]) .. and the result val is 6
      - with the flatten we can convert the 2 dimensional array into a single array. ex - var single = numset.flatten() and it will give us the single array and its easy for us to loop thru it.
      - 
      - 
### String Representation

- if we need to retrieve the collection content in a readable format, we can use the fn that transforms the collection into string. there are couple of fns we can use **joinTo and joinToString()**
- ex - .joinToString() will return just the string (if we pass the list) and the .joinTo() - will add two list and then return as a single string representation
- if we want to specify a **custom** string representation then specify its params(inside the .joinTo()) with the prefix, separator and postfix .. so it will return the string starts with the prefix (that we mention), separated by the separator and ends with the postfix. that we mentioned
- and we can also specify limit and truncated ex - var nums = 1 .. 100; println(nums.joinToString(limit = 10, truncated = <..>)).. so it will return only the first 10 elems (limit) and then the remaining will be truncated with <..>
- 
### Filtering 

- is one of the most popular task in collection processing , in kotlin filtering conditions are defined by predicates. the lambda fn takes the collection and returns the bool
- ex - nums.filter {it key endsWith("1") && it.value >100 } so this will finds the key that ends with 1 in our collections and the val which is gt 100 in our collection
- lly we can use .filterIndexed { index , value -> (index !=0) && (value.length>5) }  so by this way we can filter our collection by both the index and the val
- the .filterNot() returns the elems where the predicates yields false. ex - .filterNot {it.length <=3 ) .. although we mentioned like the val must be lt or eq to 3 it will return the val that is gt 3.
- and .filterInstance<char>().forEach() {it} will takes the type and returns the filter type that we pass in (in this case it will returns only the chars.)
- 
- **partitions** 
  - is another filtering fn and it filters the collection by predicate, and it keeps the elems that don't match in a separate list
  - ex - var (one, rest) = numbers.partition { it.length > 3}; println (one); - > this prints the elems in the collection that matches our condn and then.. println (rest) -- will prints the rest of the elems in our collections (that doesn't match)
  - 

- **Test Predicates**

- are the fns that simply tests the predicates against the collection elements. there are 3 fns 1. **any** (that returns true if at least one of the elems matches the given predicate) 2. **none** (none of the elems mathces the given predicate) 3. all (all of the elems matches the given predicate)
  - ex -- numbers.any { it.endsWith("a") } lly for the other 2 fns (none and all) and these all returns the bool

### Grouping 

- kotlin provides the extn fns for grouping of the collection elems.. the basic groupBy fn takes the lambda fn and returns the map and in this map each key is the lambda result.
- we can also use the groupBy in the 2nd lambda argument a val transform fn, the result map of the groupBy is 2 lambdas
- ex - 1. println(numbers.groupBy {it.first().uppercase()}); .. 2. println(numbers.groupBy(keySelector = {it.first()}, valueTransform {it.uppercase()}); -- in this 2nd stmt the key are the elems with just the 1st letter in the collection and the vals are the elems (and all are uppercase transformed)
- 
### Retrieving collection parts 

- .slice(), .take(), .takeLast(), drop(), dropLast()
- we can also use predicates - and those fns takeWhile(), takeLastWhile(), dropWhile(), dropLastWhile() - 
  - ex - nums.takeWhile { !it.startsWith("f") }; and num takeLastWhile { it != "three" } ; the takeWhile applies for all the elems in the collections, and the takeLastWhile applies for last elems
  -
- .chunked(3) - will chunk our collection (here it will get chunked 3 elems of list in a list)
- .windowed(3) - is sim to chunk but it gives more flexibility, 
- 
### Retrieving single elems

- .elementAt(3) will retrieve the 3rd elem in the collection lly we can use .first(), .last(), .random()
- 
### aggregate operations

- sum(), .count(), average(), max(), min(), maxOrNone(), minOrNone()..sorted()
  - sunOf {it * 2} - returns the collection with sum of all the elements multiplied by 2
- "comparable"   - is an interface we can use for the comparable operation such as sorting with specify(requirement), we can make our own fn or override the compareTo() fn.
- the **comparator**- is also an interface and we can impl and override the compare() fn
- this both interfaces can give more flexibility, and we can override the fns as we want, but still this lead to few lines of  code impl, if we want to keep it concise we can use **lambda**
- ex - laptops.sortedWith(comparedBy {it.price}).forEach {println(it)} lly we can use with the compare() as well..
  - or even concise way is - laptops.sortedBy { it.price }.forEach {println(it)} - its syntatic sugar for sortedWith and comparedBy 
  - we can also use .thenBy {} kw inside the lambda for the sorting

### Binary search

- as we know if we have a collection/list of elems (lets say 100) and we can find the 50th element, the search will loop  thru all the elems in the list until it findes the 50th.
  - now this is what called **linear Search** this is not so good for huge data sets lets say 1b records
- and that's where the binary search comes in, .. the way it works is find the mid elem in the list and comp if the search item is lt (lhs) or gt (rhs) in the list 
- if its less then it will eliminate all the lhs elem in the search and then again find the mid elem and repeat the cycle again until it finds the elem we wanted 
  - and to use the binary search we can use - numbers.binarySearch(27) - it will find the 27th elem in the list
  - note : binary search only works with ordered collection of elems, our elems needs to be sorted in order for the binary search to work.

### Generics

- generic upbounds - the generic extends from the particular ex - class Team <T: Player>()

### coroutine (threads)

- in order to the coroutine we need to add the dependencies.
- the coroutines are just like the threads and they are even more efficient, but they are executed inside of the thread.
1. ex - we can execute many coroutines inside a single thread.
2. the coroutines aer **suspendable** means, we can execute some expressions and pause the coroutine in the middle of the execution and then continue when we wanted to.(that is something the threads can't do)
3. They can easily switch the context . means the coroutines that started in one thread can easily switch the thread that running in

- every coroutine has to start in its own scope, ex - GlobalScope.launch {} this global scope means the coroutine will live as long as the app does
- we can suspend the coroutine by using the kw delay (sim to the sleep) ex - delay(1000) -- delay by 1000 ms
- "suspend fn" - ex - suspend fun networkCall(): String { delay(1000L); println("this is a n/w call") } and this fn we can only call inside the coroutine ex GlobalScope.lunch { networkCall() }

-**coroutine Context** 
    - in general coroutine are always starts in a specific context, and the context will specify which thread the coroutine are started in
    - we can pass the dispatch as the params inside the .launch() and the dispatch has Io, Default, Main, unconfined,  etc ex - GlobalScope.launch(Dispatchers.Io) {}
    - or we can also create our own threads ex GlobalScope.launch(newSingleThreadContext(name: "my Thread")) {}
- lets switch the context ex GlobalScope.launch(Dispatch.IO) { n/ewCall(); withContext(Dispatchers.Main) {}} .. with the withContext() we can switch the threads, now the dispatcher is running in the main thread

- **Run Blocking**
- as we seen previously even if we call the delay() it won't block the thread, but there is a fn in coroutine that can start the coroutine in the main thread and also  can block the thread
-  ie runBlocking - can be useful if we don't need the coroutine behavior but still want to call the suspend() in our main thread. ex - runBlocking { delay(1000L) }
- another useful case of runBlocking is when testing with jUnit, we can actually access the span fn 
- this  runBlocking {} is actually a coroutine scope, so we can create another coroutine inside ex - runBlocking { launch() {} } and this coroutine will run async to the main thread..inside the runBlocking we can create as many coroutine we want and each of them will be run asynchronously.

- **Jobs, Waiting, Cancellation**
  - lets see about the coroutine jobs and how we can wait for them and cancel em
  - whenever we launch the coroutine, it creates a job and we can save it in a variable. and then we can wait for this job to finish (by using the .join())
    - ex - var job = GlobalScope.launch(Dispatcher.Default) {}, then we can wait - job.join()  note : this .join() is a suspend fn so we can't exec in our main thread, so we can use it inside our runBlocking Scope.- ex - runBlocking { job.join() }
    - this join() will block our thread until the GlobalScope's coroutine finishes
    - we can use the repeat(3) {} and it will exec what ever code inside the scope for 3 times, and we can call cancel() on the job to cancel the job.
    - we can use the **withTimeout(2000L)** {}  and if we ve any loop / code inside then it will cancel automatically as the time is expired ex - in our case its 2000ms, 
- **Async await**
  - yes we can create an async function in the kotlin .. let see an ex - lets create an async scope inside the global scope.- val ans = async { n/wCall() } and to get the result - println(${ans.await()}) 
  - this async is of type Deferred<String>.. 
  - in summary we ve to always use async await when we ve to return some kind of result
  

- **lifecycle scope and View model Scope**
  - first ve to use some dependencies 1. android view model lifecycle dep and 2. android lifecycle runtime dep
  - so now we can use the lifeCycleScope.launch {} instead of the global scope.. the advantage of using this over the global scope is it will stick to the lifecycle of the activity coroutine, so if the activity is destroyed then all the coroutine that launched in this lifecycle scope will also destroyed.
  - lly as the view model scope is just as same as the lifecycle scope, and it will only alive as long as our view model is alive.

- **coroutine with firestore and firebase**
  - need the dep for the firestore, and the dep for the ktx .. the kotlin version (optimized) dep
  - ex - var doc = Firebase.firestore.collection("coroutine").document(pathname)  here we can assign the val for the person.. then inside the global scope we can set and get the doc

**the Retrofit with coroutine**
- lets see how we can use the retrofit combinations with coroutine
- .enqueue() will create a new thread, means it will start the req in a separate thread, but if we use this is not very efficient, instead we can create coroutine (which is more efficient)

- **exception handling and coroutine cancellation**
  - 

- 
