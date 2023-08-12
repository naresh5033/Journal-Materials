## Closures in rust - |\_|{}

- As we know the clousures are like the anonymous fns.
- just like the fns we can assign the closures to the vars and we can can pass the closures as the fn params
- and when using the closures we don't need to pass in the i/p params types and the return type, the compiler is able to determine the types (we can explicitly define the types if we want)
- if we ve only a return val in the body and we don't ve to enclose the val with the brackets ex - |s|s
- All the closures was implemented by one of the standard libraries Fn - traits -- 1. Fn traits 2. FnMute 3. FnOnce
- note that regular fns also implement these fn traits

- "Caching" vals is generally a useful behaviour, but we the thing which has to be taken concern is cacher has couple of problems, 1. when calling the val it will return same val regradless of the i/p args, (inside the match block with the some and none), the first val is always gon return none
- so instead of caching one val no matter what we passed in the arg, we need to cache one val for each arg passed in.
- we can use the HashMap instead of storing the single value. the keys will be the args of the val() and the val will be the closure (with the args of the val())

- Unlike fns the closures can ve the access to the vars that are defined with in the scope in which the closure is defined.
- because the closures are able to capture the environment theey ve to use extra mem to store the context, bcoz fns don't capture the environment,(only access to the vals inside the scope)
- the closures can capture the environment in three ways 1. by taking the ownership 2. by borrowing mutably 3. by borrowing immutably
- and these 3 ways are encoded in those Fn traits
- FnOnce - takes the ownership of the vars inside the environment - Once mean the closure can take the ownership of the vars more than once . we can call it only once.
- FnMut - mutably borrow the vals, can be called multiple times(but once at a time), and may mutable state
- Fn - immutably borrow the vals, (can call it multiple times and at a same time)
- we could also force the closure to take the ownership of the environment vals, by passing the move kw, this could be useful when we re passing the closure to the one thread to the other thread.
- so we can also pass the ownership of the var from one thread to the other.
- once closures takes the ownership then we can't use the val in any other place in the scope, after the closure

# crust of rust (fns, closures and their traits)

- The fn items and the fn pointers are different from each other.
- but fn items are coercable into the fn pointer (the fn pointer is like something like the arg that points to the fn ex - fn bar(f : fn())
- the take away is the fn item uniquely identifies the particular instance of the fn, where as the fn pointer is the pointer to the fn with the given signature.
- The fn pointer implements all 3 of those above traits. (or we can think of like the fn pointer imps the Fn traits )
- anything that impls Fn that will also impls FnMut and FnOnce
- but anything that imps FnOnce but not Fn , (bcoz if we given the shared ref we can't produce the exclusive reference)
- if anything that impls FnOnce that only impls FnOnce.

- in general the closures are called closure coz they re closed over the env, or they capture the vars from the env and they generate the unique fn

- there are non caputuring closures that just coarcable to the fn pointers ex - let f = || (); baz(f)..
- in this the non capture closure does not capture anything so it coarceable to the baz(f) (fn pointer)
- but if we pass the vars (that the closure can capture) then it won't be coarcable to the fn pointers anymore, we can't pass the fn pointers after the closure
- just like the other traits we can also use the Fn with the dyn to get the dynamic dispatch
- ex fn hello(f: Box<dyn Fn()>){} and we can also call the other 2 trait fns as well
- but this Box<dyn Fn()> didn't impl the Fn() and lly for the other 2 traits
- the dyn in general is unsized (not sized), and that's y we always ve to use the dyn with & or &mut or <Box> we can't ve the free standing dyn coz it can't ve the size.
- so the compiler won't know how to arrange in the stack
- in order to call the FnMut() we need to stick the dyn with the exclusive ref ex let f: &mut dyn FnMut().
- in order to call the Fn() then we need to stick the dyn with the shared ref ex let f: &dyn Fn()
- in order to call the FnOnce(), then we need to stick the dyn with the wide pointer type (like Box) that allows us to take the ownership.. let f: Box< dyn FnOnce()> = Box::new(f)
- and this wider pointer like the Box we can use with the other 2 traits

" const Fn Traits" - there is a const traits for the Fn .. ex const fn foo<F: ~const FnOnce()>(f:F){} -> the ~ means the foo will be const if F is const, if F is not const then FnOnce() is not const - if we ve the ~const Fn() its callable at the run time or the compile time

## For bounds

- when we ve a trait impl like -- ex - fn quox<F>(f: F){ where F: Fn(&str) -> &str}, if we trait to fill in the lifetime for the trait bound
- so we wana tell the F to reuse the lifetime that it gets in and the nd its o/p, and this is where we get this special for syntax
- ex - where F: for<'a> Fn(&'a str) -> &'a str; -- this is actually the desugaring of the above syntax.
- now we can read this a the F is the ref to the fn with `a to another ref with the same lifetime of a.
- its kind of saying that this need to hold for any lifetime.
-

# Cli app in Rust

- we will be building the popular cli tool "grep" - searching for file and str we can create the vec<string> type and pass in the env::args().collect()
- the agrs() will give us the iterators over the arg that we passed in.
- the collect() will turn that iterators into a collections (in our case its a Vec of Str)
- if we don't pass in any arguments it will give us the path to the binaries. which is 0th index
- if we use expect() on the result type, it will give us the val wrapped in the Ok variant or if there is error (it will prints out the error)

- the rust community has developed the pattern - when the main.rs (aka bin crate) gets too large create a lib crate (lib.rs) and then ve our bin crates call fns in the lib crate.
- clone() --> when we don't wanna take the ownership of the variables.

- when we create the struct for our fns, then our fn will be closely coupled to the struct.
- to fix this we can add the impl for the struct.
- then we can pass our fn into the impl block.
- when we wrap our fn with the new() its like calling the ctor. ex - fn (new(args: str) -> Config) and to call this we can use Config::new()
- instead of retuning the Config type we can return the Result type
- ex - -> Result<Config, String> in our it will return the Config type if it is Ok() type (in which we can wrap our Config ) or string for the error type
- now that we ve this types as the result.
- we can use unwrap_or_else() in our calling fn - ex Config::new().unwrap_or_else(). and inside we can paas in the closure to handle the error
- As we know anytime we return the ref to the fn, we ve to tie the lifetime of the ref to the lifetime of the one of the i/p parameters.

## Iterators (:Iter<i32>)

- this pattern allows us to iterate over the sequence of elements regardless of how they are stored(what kind of data structures they are).
- we can call iterator by using the .iter().. and the iterators are part of rust std library.
- all iterator in rust implements the iterator trait.
- as we know in iterators we ve to implement the only one method ie .next() which is the next element in the sequence.
- note if we wana call the .next() then make sure that our var is mutable (the next() needs a mutable reference to the iterator)
- if we want our iterator to return immutable references then we ve to use .iter_mut()
- and if we wanted to own types, ve to use .into_iter()

- "Adapter methods" - one such method is map() takes in the closure and calls the iterator, .zip() is one such method.
- remember that the iterators are lazy, and for this adapter with the closure don't do anything until we pass in the consumer()
- we can pass in the collect() consumer method which will transform our iterator into a collection.
- static lifetime, when our result type will just only live the duration of the program, so we can use the static lifetime. - `static

## Publising a rust crate

- the cargo has 2 main profiles 1. dev and 2. release
- the release profile allow us to configure how our code is compiled.
- the dev profile defines good defaults for development an the release profile difines good defaults for release
- if we run `cargo build` our code will build with the dev profile. its unoptimized and contains debug information
- for release profiles we ve to run `cargo build --release` its optimized
- cargo has default settings for these profiles, however if we want we can define custom settings in the cargo.toml file - and there we can set up the opt (optimization) level 0 to 3 (low to hign)
- for dev its default val is 0 level.
- for the public io the best practice to release our code is to write the comments /// for doc (3 slashes)
- in order to build the html documentation for our crate, `cargo doc --open` will open the browser with our documentation.
- besides the example section, there are few other commonly used sections 1. panic section 2. Errors 3. safety section
- //! (this two slashes and the ex mark) this documents the item containing the comment.

- "re exports " - if we export our modules then we other user use they vo to import the mod and then access the fn
- but we can make the fns or enum or any type to be exported as the top level so they can import just our module followed by the type/fns they wana use -
- for that we ve to use like -- pug use self::kinds::PrimaryColor
- - now when thy import they don't ve to specify kinds they can directly access the type followed by the module name ex - use mod art::PrimaryColor.
- just we need to login the crates.io (already did), and then get the token, now we can be able to publish our crate.
- then push it to the git and then publish the crate with the unique name `cargo build`
- and in the toml file some of the meta data that are madatory to fill in are name, version, author, edition, description, edition, license.
- once we publish our code, it will be there in forever, crates.io will put in archieve, we can't modify as well coz others may ve reilied on our code.
- although we can prevent others by downloading it, by specifying the "yank" cmd - `cargo yank --vers 0.0.1` and if we can undo this by `cargo yank --vers 0.0.1 --undo`

## Cargo workspaces

- as we know we can keep the bin crates and the lib crates, not what happens if our project keeps growing.
- in this case Cargo Workspaces will helps us to organize our project.
- it helps us to manage multiple related packages, the packages in workspace shares the common dependency resolution by having one cargo.lock file.
- the packages in workspace also shares one o/p directory and various settings such as profiles,
- in the cargo.toml file instead of creatting/having [package ] sections we need to ve [workspace] section
- next we can specify the packages, which are called members - members [path to our packages]
- now in the root dir we can run the command to create the new packages `cargo new package` and then build the packages `cargo build` but inside the package directory there won't be any cargo.lock and the target dir they will be only in the root directory (of our workspace)
- cargo uses this structure bcoz the packages in the workspace are meand to depend on each other
- and in the package's cargo.toml file we ve to define the path for the other packages in the dependency section (cargo doesn't assume that the relattion they ve to depend on each other so we ve to define)
- we can build the workspace `cargo build` in the root directory, and then to run the specific package we can use the `cargo run -p package-name`
- if we wana add the external package to our packages (ex we ve 2 packages) both of them must ve the same version of the dependency. (that can be maintained in the cargo.toml file in our root directory)
- running the test command in the root of the project will run the test in all our packages
- if we wana run test on specific packages then we can pass in the -p flag in our command.
- and if we wana publish the package then we ve to publish them individually in their respective package directory.

- "installing crates" from the crates.io
  - we can only install the packages with the binary target ( not the library target)
  - all the installed packages will be in the root bin directory, $HOME/ .cargo/bin
  - to install the packages ```cargo install package-name`

## Box Pointers

- is one of the pointers. ex - let b: Box<i32> = Box::new(5);
- as we know in general the pointer is var that stores the memory address .
- the most common pointer in rust is the reference, ref is they borrow the val they point to, they don't ve the ownership of the
- references don't ve special capabilities and overhead, unlike smart pointers
- the most common pointers are data structures, that act like a pointer, but they ve metadata and extra capabilities to act on.
- one ex is the reference counting smart pointer they allow us to ve multiple owners by keeping track of the owners once there are no more owners, it will clean up the data.
- in many cases the smart pointers own the data that they pointed to.
- "stings and Vectors" are some smart pointers, they own some data and allow us to manipulate it.
- smart pointers are usually implemented special structs they implement the deref and drop traits
- the deref allow us to treat the instance of the smart pointer as ref, so we can write the code that works for either ref or smart pointers.
- the drop traits allow us to customize the code that runs when an instance of the smart pointer goes out of scope.
- "Box pointers" as we know that the smart pointer allocate the val to the heap.
- and in the stack we store the addr to the heap.

- we will use em in the following situations, when we ve a type whose size can't be known in the compile time and we wana use the val of the size in the context which requires konwing the exact size
- when we ve the large amount of data and we wana transfer the ownership of the data, and we wana make sure the data isn't copied.
- lastly when we own a val and only care the val impl the specific traits rather than being the specific type
- at the end of the scope it will be deallocated meaning that the box smart pointer on the stack will deallocated and the underlying val on the heap will be deallocated as well.
- "indirection" means instead of storing the List vals directly we can store them in the Box pointers
- lets understand how rust computes the size of the non-recursive enums - it will go through each variant inside the enum and it will figure out which var takes more space.
- coz we can ony use one variant at a time from the enum.
- the box smart pointer will ve the fixed size in the stack although it will point some arbitrary amount of space in the heap.

## Deref Trait smart pointers (\*)

- this will allow us to treat the pointers like the regular references
- if we impl our custom pointer, we need also to impl the deref trait - use std::ops::Deref;
- look the doc for the ex.
- the deref trait allows the rust compiler to take any val that implements the Deref and calls the deref() method (since it takes the &self as argument) to get the reference and the compiler knows how to deref
- the deref just returns the reference instead of the val itself, this has to do with the ownership
- if the deref() returns the val directly then rust will move the owner of the val outside of the our smart pointer, and we don't want this
- "deref coercions" - will converts the reference to one type to the other type.
- the String is also impl the Deref trait, if we call the dref in string it will return the sliced string &str (rust will do the automaticall coercion(deref) to the str)

- similar to the Dref trait to override the deref operator for the immutable reference, we can also use the DereMut trait
- that will override the deref operator for the mutable reference
- rust does the deref coercion in following cases 1. when going from the mut ref to the mut reference 2. when going from the immut ref to the immutable reference 3. when going from the mutable ref to the immutable reference
- Rust cannot perform the deref coercion when going from the immutable ref to the mutable reference, coz of the borrowing rules (state that we can only ve one mut ref to the specific val within a scope)

## The Drop Trait

- the drop trait is almost always used when implementing the smart pointers.
- it allows us to customize when the val goes out of the scope.
- for ex in the Box when the val goes out of the scope, we wanna deallocate the mem
- unlike some other programming languages, this clean up happens automatically in rust with this Drop trait
- the clean up behavior is the last added scope will drop first.
- if we want to customize the behavior,
- rust doesn't allow us to directly call the .drop() method. coz when our vars will go out of the scope rust will still call the drop() method automatically.(its become double free, its tryna free the memory thats already been freed).
- instead of calling the drop() method we can use the drop fn that provides by rust std library.
- we can pass in the val to the fn - drop(val), this fn is included in the prelude.

## Reference counting Smart pointer (Rc<T>)

- from std::rc::Rc

- this will allow us to share the ownership of some data.
- in some cases single val may have multiple owners, ex - when we ve graph with edges that points to the same node. conceptually that node is owned by all the edges.
- in this case, the node does not cleaned up unless it does not have any edges pointing to it.
- so this reference counting smart pointer will keep track of the ownership of the val, and when there are no more owner references the val will be cleaned up.
- the reference counting smart pointer will be used when we have val allocate in the heap and multiple parts of our program read that val and we don't know which part of prog is finish using the val last at compile time.
- if we knew which part will finish last we can assign the ownership to that part.
- in the ex - from the doc
- we can construct the a - val with the Rc::new() method
- then to shard the ownership we ve to use the Rc::clone(&a) -- this Rc's clone does not make the deep copy of the data unlike the most .clone() method
- as it will increase the reference count.. to get the reference we can use the Rc::strong_count(&a)
- note - with this pointer it will increase the reference count and different part of our program can read the val not modifying it.
- allows only immutable borrow check at the compile time.

## Interior Mutability

- is a design pattern that will allow us to mutate the data even there are immutable references to the data. (which is typically disallowed by the borrowing rules)
- to mutate the data this pattern uses unsafe code inside the Data structure to bypass the typical rules around mutation and borrowing
- unsafe code is the code that will not checked at the compile time for the memory safety.
- but we can still enforce that rule in run time.

**RefCell (std::cell)**

- like the box it will represents the single ownership to the data/
- the difference between em is the box will enforce the borrowing rule at the compile time but the RefCell will enforce the borrow rule at the run time
- checking borrowing rules at compile time is the default behavior in rust. (except for the RefCell)
- we can use this RefCell only in the single threaded program.
- because the RefCell allows multiple borrow check at the run time, we can mutate the val inside the RefCell even though the RefCell itself is immutable.
- when we re using the RefCell pointer(which is immutable), but we can get the mutable reference to the val that stored inside the RefCell by calling .borrow_mut().. or to get the immutable reference we can just call .borrow()
- but with the refCell we can't borrow the mut reference more than once, it will be ok in the compile time but throws an exception in the runtime.
- but we can use the interior mutable reference to fix (borrow mut reference more than once)

**Combining RC with the RefCell**

- Rc<RefCell<i32>> - to get the multiple owner of the mut reference.
- and to assign/ create a instance.. let val = Rc::new(RefCell::new(5));
- the we can use \*val.borrow_mut()+= 10; -- here we re dereferencing the val which automatically dereference Rc into RefCell. then we call .borrow_mut() from the ref cell to mutate the value.

## Reference Cycle Smart pointer

- as we know rust is good for the memory safe, however it doesn't provide the same guarantee for the memory leaks, rust makes it difficult but its not possible to create a memory that is never cleaned up.
- we can create the memory leak by using the Rc, RefCell. by using these two we can create the reference where the item reference each other in a cycle, that will create the memory leak
- if we re mixing the Rc with the interior mutability, we need to make sure that reference cycle aren't being created.
- creatin the Ref cycle is logical bug in our code, which should be prevented using automated tests, code reviews and other software eng best practices.

**Weak pointer**

- also from std::rc::weak

- the pointers that points the data that they are not owned to.
- then we also prevent the ref cycle
- the weak pointer is a version of the Rc smart pointer, that holds the non reference.
- in the ex - from the doc we pass the weak pointer to the parent field, and it will expects the weak pointer.
- in order to transform a Rc pointer into weak pointer, we can use Rc::downgrade(); .. and pass in our rc pointer inside that.
- and similarly the .upgrade() method will used to convert the weak pointer into a Rc pointer.
- note that, whenever we want to see or mutate the val inside the weak pointer we ve to call the upgrade() to upgrade into Rc pointer.. because the weak pointer has no idea if the value is being dropped or not.

**Strong count vs Wwak count**

- now internally the Rc smart pointer holds 2 counts
- the strong count represents the number of references which has the ownership of the data
- now the weak count represents the number of references which don't ve the ownership of the data.

## concurrency

- diff part of our prog executes independently.
- one of the goal of the rust lang is to hadling the concurrent programming safely and efficiently.
- lot of concurrent errors are able to be caught at the runtime.
- the thread also increases complexity, because threads run concurrently we don't ve the order of the threads which executes our program. this leads to diff challenges.
- as we know one such challenge is the race condition.
- and other challenge is deadlock. (where 2 threads are waiting for the resource that the other thread has, as a result both thread waiting indefinitely)

- "Thread Types" there are 2 types of threads

1. One to one thread, Os thread or native thread or s/m thread etc
2. green thread, also called user thread or program threads etc. (we can ve 20 green threads that mapping to 5 os threads)
   - this is what we called green thread model or m to n model
   - but the trad off of this is runtime support.
   - as the rust ve very low runtime, (if they include more features out of the box the larger the runtime would be)
   - because the green thread would require larger runtime, rust will comes only with the one to one thread out of the box/ its std library.

- by default every program ve one main thread.
- and when the main thread finishes the execution, the spawn thread will also finish the execution, no matter if its finished execution or not.
- to avoid this we can assign our spawn thread to JoinHandle<()> and at the end of our main thread we can call handle.join(), which will wait for the thread to finish execution.
- and also we need to unwrap() coz the join will return result type.. handle.join().unwrap()
- calling join() will currently block the thread, until the thread associated with the handle
- if we call this join(), before the main thread execution, it will wait for the spawn thread to finish and then it will start. so the position of the join will affect the thread exection order.
- if we use closure inside the main thread and passes the val of the scope, the will not know how long our thread will run for, rust doesn't allow us to take the ref of the val(in the scope) inside the closure.
- so to take the ownership of the val we can use the move kw in front of the closure.

## Message passing

- passing data b/w threads
- one popular approach to ensuring the safe concurrency is message passing
- where we ve threads / actors passing messages between each other, which is data.
- the Go Language has the slogn that summarises do not communicate by sharing memory instead share memory by communicating.
- just like Go, the rust also enabless message passing concurrency is **channel** included in the std library
- channels are anology to the water like river or stream
- as we know that channel has 2 things 1. transmitter and 2.receiver
- the transmitter is the upstream and the receiver is the downstream.
- one part of the code will calls methods on the transmitter(passing in the data we wana send)
- and the other part of our code will listening to the receiver to get the data.
- the channel will be closed if either the receiver or the transmitter halves are closed

- once we re familiar with the channels we can implement things such as chat s/m or a program which performs part of the calculation and one thread aggregates the results.

- first bring the mpsc mod into the scope --> std::sync::mpsc;
- Multi Producer Single Communication, FIFO queue communication primitives. in rust we ve multiple producers but only single consumer.
- mpsc::channel() - contains tuples which has the sender and the receiver.
- the channel (tx)has the send() which returns the result type and we can unwrap, coz our if the receiving end has dropped for some reason, in that case send() will return an error.
- then we can use the .recv() method to receive the data and then unwrap it.
- the rx(aka reciever) also has .try_recv() method
- the recv() will blocks the execution of the main thread, while it awaits for the val / message to be received. in the channel
- the try_recv() will not block the execution of the main thread instead it will return the result type immediately.
- this method is useful when we want our thread to do the other things, for ex we can have a loop for every so often we call the try_recv() method to see if there are any new messages. otherwise we let the thread do the other things.
- ownership rules helps us prevent the errors in oue concurrnect program.
- when we re using the rx in the loop we don't ve to specify .recv() method, instead we re treating rx as a iterator, every iterator will ve the val that passed down the channel, when the channel closes the iteration will end.
- when we re creating multiple producers in the diff threads, we will get an error cause when we pass the tx to the 1st thread it alrady taken the ownership of tx, so instead we can do
- create a new tx var and ref to the 1st tx - let tx2 = tx.clone()
- now this will give a new sending handle which we can use in our second spawn thread.

## Shared state

- transfering data using shared state
- we can think of it as a one way flow, where one thread passes data to the other thread, now the receiving thread owns the data.
- with shared state concurrency, we ve some pice of data in the memory that multiple threads can read and write to

**Mutex**

- from std::sync::Mutex

- mutual execution means we ve some data and only one thread can access it at any given time.
- to achieve this mutex use locking s/m.
- the lock is a data structure, that keeps track of which thread has the exclusive access to the piece of data,
- once a thread has acquired a lock on a particular piece of data and no other thread can access it.
- once the thread is done with the data it will unlock and allow other threads to access the data.
- so when using the mutex(are difficult) these are the 2 rules we ve to keep in mind, the lock() and the release().
- but forturantely rust type s/m and ownership s/m rules guarantees that we can't get locking and unlocking wrong.
- Bts we will get the mutex smart pointer which deref traits poits to the inner data of the mutex
- also mutex guard impls the .drop trait such that when mutex guard goes out of scope it releases the lock to the data, means release the lock will be done automatically, we don't ve to remember.
- if we want to ve a multiple owner of the val inside the closure, we can think of Rc smarter pointer.
- but they are not thread safely, and even if we try to use we wiil get the compile time error.
- Fortunately the rust std library has the **Atomeic Arc** Rc smart pointer, which we can use in this case. std::sync::Arc;
- atomics are concurrency primitives, we can learn more from the documentation.
- in our case we will use the Atomic and they could be shared across threads.
- The mutex uses interior Mutability, that will allow us to change/mut the val inside the Arc smart pointer.
- the RefCell smart pointer comes with the risk of creating circular dependencies.
- and the mutex smart pointer comes with the risk of creating deadlocks.

**Sync And Send Traits** are the 2 concurences feature provides us the building block for concurrency

## OOP (in the context of Rust)

- as we know the oop has 3 main concepts, obj , encapsulation and inheritance.
- "objects" - enum and struct provides the impl blocks holds data in which we use implement block to provide methods on the struct and enums.
- so eventhough enums and structs are not objects, but yet they provide the same functionality.
- "encapsulation" - means the impl details of the object are hidden from the code using that object.
- instead of changing the internals directly, code outside of the object is limited to interact with the object through its public api.
- in rust we ve the fields set in the struct and then instead of manipulating the fields directly we can implement the fn for the struct, that will allow us to manipulate the fields.
- "inheritance" - ability to inherit an object from an another object.
- rust can't ve this ability, specifically in struct, we can't define a struct that inherits fields from other struct.
- however we can use other tools to available, depending on why we're reaching inheritance, there are 2 main reasons for using inheritance.

1. code sharing, we can impl the behavior of one type and then iherit that from other type.

   - we can achieve this by using the defult trait implementation.
   - but the limitations are we can only define methods but not fields, although there is proposal out there that will allow traits to update the fieds.

2. the other reason for using inheritance is polymorphism, allows substitute multiple objects for each other at run time, if they share certain characteristics.
   - ex parent (base class)
   - in rust we can use generics to abstract the away the concrete types and we can use the trait bounds to restrict the characteristics of those types.
   - in addition to generics rust also provides trait options, which are similar to generics, except they use dynamic dispatch, where as the generics use static dispatch.

## Using trait objects

- in rust we share the behaviour using trait.
- in the ex - from the doc we use trait object, instead of generics.
- but one difference is that when we use generics, we re limited to only one type.
- if our list is homogeneous then using generics will be good option.
- bit however if we need the flexibility to store any type that impls that trait, then we want to use trait object.

"**Static Vs Dynamic Dispatch**""

- static dispatch is when the compiler knows the concrete fn that we re calling at compile time.
- the dynamic dispatch means the compiler doesn't knows,the concrete method we re calling at the compile time. so instead it figures that out in runtime.
- when we use trait object the rust compiler must use the dynamic dispatch, thats coz the compiler doesn't knows all the concrete methods that will be used during the compile time.
- which means there is a runtime performance cost.
- but the upside is we can ve the flexibility to code that can accepts any objs that impls that trait.

**Object Safety**

- we can only make obj safe traits into trait bounds.
- what is meant to obj safe ? there are 2 rules we must follow.
- the trait is obj safe, when all of the method implemented on the trait ve these 2 properties

1. the return type is not self
2. and there are no generic parameters.

- if the trait doesn't ve these 2 properties then the rust compiler can't figure out the concrete type of that trait and doesn't know the correct method to call.

## State Design Pattern

- The state object oriented pattern using rust.
- in this pattern we ve some vals which are internal state, and that internal state is represented by state objects.
- each state object has its own behavior and to deciding when to change into another state. the val that holds the state object knows nothing about the different behavior of the state. or when to transition into another state.
- the benefit of this state design pattern is when the business requirement changes we don't need to change the code which uses the val, we simply need to change the code inside one of the state objects, or perhaps add new state objects.
- note in the ex - from docs - we use the state field as optional, ie bcoz inside our request_review() we returns the self.state.take() -- this take() returns the optional(some, none) the takee() takes the val out of the option and leave none in its place
- in our ex its removing the val inside of opttion (which is state) and outside of the post into state variable.
- in which it returns the self.stae -> which is now none, then we reassign the Some val in the state.

- in the fn content(), we returns self.state.as_ref().unwrap().content(self) --> here we call as_ref() coz the state is optional, that owns the state obj, but we just want the ref to the state obj
- and in the fn content() (in the published impl), our return type &str, and we return &post.content so the life time of the our return val is tied to the return type, so we can use the `a and also update this fn inside the trait definition.

- one downside of this state design approach is that one state is couple with other. and another downside is the code repeatation, if we ve many repeatations, then we can consider using the macros.

**Encoding states and Behavior as types**

- By implementing this state pattern, as exactly as in oop means we re not taking the full advantage of the rust traits.
- Rather than encapsulating the state and transition completely so that outside of the code ve no knowledge of them, instead we will encode different states as different types.
- in this new implementation, now our structs don't need the state fields anymore thats because removing the encoding to the type of the struct.
- in this inside the impl DraftPost the fn request_review() this methd and the approve() method takes the ownership of self, means it can consume and invalidate and then rerurn the new state.
- so in summary this implementation doesn't follow the oop pattern, and the advantage of this are "invalid state are impossible due to the type s/m and the type checking.

## patterns and Matching

- patterns are special syntax that matching against the struct of types
- the pattern consists of the some combination of following

  - variables
  - literals
  - wildcards
  - placeholders
  - Destructed (Arrays, Enum, Structs, tuples)

- the vars defined inside the match block are shadow the vars outside of the match block.

- these components describe the shape of the data we re working with, which we can match then agaist the vals
- with the match kw and then the vals (list) we wana match on and the body is "match arms" - this match arms consists of the pattern which maps the expression.
- one thing about this match is every val of the pattern has to be exhaustive, meaning that every possible val of this variable has to be accounted for.
- if there is any pending val (that does not included in the pattern) we can fix by using catch all pattern which is an place holder \_ . its like the default case in the switch statement.
- note that this place holder (catch all) does not bind to the variable.
- if we did wanna bind then we can use any str and set the type equals to match that we re matching.

- another place we can use this match pattern is if else if conditions
- coz the down side of the if else statement is it doesn't exhaustivek
- another place we can use this match pattern is while loop, for loop, let statement, and fn parametes, and closures as well.

**Refutable and irrefutable patterns**

- "Refutable" pattern are always match no matter what.
- "irrefutable" pattern might not match,
- and note that - fn parameters, let statements, for loop only accepts the irrefutable pattern.
- if let and while let matches bot the irrefutable pattern and refutable pattern.

## Pattern Syntax

- lets see all the synatax we can use inside pattern and why we may wana use them
- 1. the matching against the literals pattern(is useful when we ve concrete vals) 2. the named vars 3. matching multiple pattern(ex- match x {1|2|3 => println("one two or three")}) 3. matching ranges of vals ex - match x {1..=3 => println()}.. but this range operator only works with the numeric val and the chars.
  2. pattern that matches the destruct val, when we re destructing a struct we can use the named vars and the literals.

- ignoring the vals in pattern,
- if we re prototyping to ve an unused variable, and we don't want the rust compiler to complain about, we can prefix the var with the placeholder ex - let \_x: i32 =10;
- note that prefixing the var with the placeholder and the regular placeholder (catch all) is different, coz we can still bind the val with the prefix \_ ,
- if we want to use certain part of the val and not care about the other we can use the range operator, ex - lets ve a struct Post which has 3 fields and we only care about the 1st so -- match Post{x:i32, .. => println();}, in this the range syntax just ignores the rest 2 fields (y and z)
- as we know the range syntax can expands to as many vals as it can be, ex - match numbers{(1,..,10) => println();} , in this the range syntax cares about the the 1 and the 10th val and ignores all the in between vals.

**Match Guards**

- is a additional if condition specified after the pattern, in a match arm.
- this is useful for expressing the complex ideas that the patterns can't express themselves. ex - match num {Some(x) if x>5 => println();}
- this match guard also solve the issues with shadowing inside the match block, ex - match number {Some(n) if n==y => println();}.
- we can also ve multiple patterns and ve the match guard apply for each pattern. ex - match number {1|2|3 if y => print()}, -- if any of the val matched the y that we defined.

**@ operator**

- this operator lets us create a var that holds the val at the same time testing the val, to see whether it matches the pattern, ex - match number { id: id_var:i32 @ 3,..=7 => println()} .. here we re checking if the id matches the range of 3 to 7, and also if it matches any we re also storing the var in the id_var.

## writing Unsafe in Rust

- so far all the code we seen has enforce to follow mem safety guarantees at the compile time.
- however if we wana opt out of mem safety then we can use unsafe rust .
- this exists for 2 reasons

  1. the static analysis is conservative by nature. (so rust will reject if it can't guarantee that the prog is mem safe, eventhough the developer knows that the prog is mem safe)
  2. the underlying computer H/W is inheritently unsafe, (since rust is low level s/m lang which allow us to do low level s/m prog which sometimes requires unsafe code)

- to use the unsafe rust we can use the unsafe kw and this will give us the 5 abilty that we don't ve in the safe rust world

  1. Dereference the raw pointers
  2. Call the unsafe fns or methods
  3. Access or modify the mut static var
  4. implement an unsafe trait
  5. Access fields of unions

- note that the unsafe doesn't turn off the borrow checker or disable the rust safety checks.
- eventhough we ve thesse 5 abilty we still get some degree of safety.

- writing the unsafe code may cause the memory issues but if we keep our unsafe code small and isolated, then it could be easier to debug.
- we can also enclose the unsafe code in a safe abstraction and then provide a safe api,

1. Dereference the raw pointers

- unsafe rust has 2 types of raw pointers that are similar to the references. the mutable raw pointer and the immutable raw pointer,

  - the "immutable raw pointer" are written as \*const i32; (means that the pointer can't be directly assigned after has been dereferenced )
  - the "mutable raw pointer" are written as \*mut i32;
  - these \* are not the deref operator, here this is how we declare the raw pointers.

    **Diff b/w the deref Smart pointer and the raw pointers**

    - the raw pointers are allow us to ignore the rust borrowing rules by having mut and immutable pointers or multiple mut pointer at same location in mem.
    - raw pointers are also not guaranteed to point the valid mem.
    - they re allowd to be null
    - and they don't impl any type of automatic cleanups.

- rust allow us to create the raw pointers and it doesn't allow us to deref the raw pointers, unless its in the unsafe block
- in order to deref the raw pointers we ve to use the unsafe kw / we ve to use the deref in the unsafe block... ex -- Unsafe { print(\*r)}, here we deref the r (raw pointer and prints the val)
- here in thix ex - we re pointin the immutable and the mut raw pointers at same location in the mem. if we re to create the mut and the immutable ref to the same location in the mem, then it'd break the rust ownership rules so it won't compile.
- the raw pointers may allow us to bypass the ownership rules but we ve to be careful, coz this may create a data race condition
- even with all these dangers the raw pointers are still useful specially when interfacing with the c code, or building up the safe abstractions that the borrow checker doesn't understand.

2. Call the unsafe fns or methods

- the fns or the methodes ve unsafe kw at the begining of the defn ex - unsafe fn number(){}
- when we call this unsafe fns we ve to give it the correct args if not it will lead to undefined behavior
- unsafe fns must be called inside the other unsafe fns or the unsafe blocks.

**Creating the Safe Abstraction over Unsafe code**

- we can wrap the unsafe fn inside the safe abstraction
- to get the raw pointer we can also use .as_mut_ptr() and then we can use the unsafe block -- unssafe {slice::from_raw_parts_mut(x,y)}.. this raw_parts_mut() will create a new slice. and this fn in unsafe we ve to use inside this unsafe block.
- even though this fn is wrapped in the unsafe block, we can still use this in the safe abstraction.

**extern Function to call external Code**

- some times we need to interact the code from diff language.
- for this rust has "extern" kw, which facilitates the creation and the use of a foreign function Interface (FFI)
- And to call the ffi we need to define the foreign fn in the extern block ex - extern "C" {fn abs() -> i32;} .. in this ex we call the abs fn from the C . calling this extern fn is always unsafe,
- so to call this we ve to use this inside the unsafe block.
- this C defines which application abi that the external function uses.
- the abi defines how to call the fn at the assembly level.
- when using the "extern" kw we ve to use the #[no_mangle] annotation, to let the rustc knows not to mangle the name of the function.
- "mangling" is the compiler changes the name of the function to give more information for the other parts of the compilation process.
-

3. Access or modify the mut static var

- in rust the global vars are called static vars, they are sim to constants and the convention is to use all caps. ex - static HELLO_WORLD: &str = "hi";
- the static vars ve the fixed addr in memory, means that we re always accessing the same data.
- constants are allowed to duplicate their data whenever they are used.
- for ex if we re referencing the constants thru out code then the compiler can replace all those instances with the concrete vals
- another diff is that static vars can mutable, but accessing the mut static vars are unsafe.
- ex - static mut COUNT:i32 = 2; unsafe{ COUNTER+= 5;} ..

4. implement an unsafe trait

- the trait is unsafe when atleast one of its method is unsafe.
- we ve to mark the unsafe kw to its defn nd also the impl block ex - unsafe trait foo{}; unsafe impl foo{}

5. Access fields of unions

- similar to struct but only one field is used for each instance.
- unions are primarily used to interface with the c unions and its unsafe to access the fields of the unions.

## Advance Traits

- "Associated Types " - they are the placeholder which we can add to our traits ex - pub trait num {type Item;} .. this type Item is the associated type. then the methods can use the place holder.
- then when we impl this trait we will specify the concrete type for the Item.
- so this way we can design the trait with the type unknown until we impl the trait.
- now the Generics and Associated Types both allow us to specify a type w/o specifying the concrete val, but the diff is with the associated type we can only ve one concrete type per implementation.
- where as in generics we can ve multiple concrete types per implementation.
- and the associated type can be implemented only once.
- where the generics can be implemented many times with the diff concrete type.

**Default Generic Type Parameter**

- generic type param can specify a default concrete type. this allows implementers to not ve to specify the concrete type, unless its diff than the defult.
- the good ex of this is when customizing the behavior of the operator aka operator overloading.
- lets say we wana overload Add operator
- ex - with the trait -- trait add<Rhs=Self> {type Output;} -- in this add trait has the generice (Rhs stands Right Hand Side and set it eq to self or the concrete type)

- In general we wana use the generic type param for 2 reasons. 1. to extend the type w/o breaking the existing code. 2. is to allow customization for the specific cases, (which most users wont need)

- "Calling methods with the same name"

  - rust allow us to ve the 2 traits with the same name, and impls both those traits on one type.
  - its also possible to implement the method on the type itself with the same name as the method inside the trait.
  - if we re in this situation as the smae method names we need to specify rust which one to call. the type of trait we wanna cal followed by the method name, ex - Point::fly(); -- call the fly() method from the Point trait.

**Super Traits**

- we might ve to depend on the functionality that impl in the other trait, the trait that we rely on is called super trait (like the parent)
- ex we wanna use the to_string() in our trait impl, but its from the std::fmt::Display Trait and to extend this trait we ve to specify like ex - trait Outline: fmt::Display {} - inside we can use the to_string() method in our fn body

**New Type Pattern**

- the orphan rules dictates that we can impl a trait on a type as long as the trait or the type is defined with in our crate.
- the New Type Pattern allows us to get around this restriction.
- we will do this by creating a tuple struct with one field which is gon be the type we re wrapping.
- this thin wrapper is local to our trait so we can impl any trait we need.
- however if we do want our new type to impl every method its holding then we can deref the new trait

## Advanced Types

**New Type Pattern**

- imagine we ve 2 parameters one fn took in age and the other fn took in id, and the type of both parameters is i32 and we want to prevent user to use the id instead of age, then we can create a new type that can wrap the i32.
- another useful implementation of the new type pattern is to abstract away the implementation details.
- and the code using this new patterns pub api will ve no idea of what data structure is used.
- in general the new type pattern is a light way to achieve encapsulation.

**Type Alias**

- in addition to the new type pattern rust allow us to create type aliases to the existing types a new name.
- the main use of the type alias is to reduce the repeatation

**Never Type**

- the never type is the special type, that denoted with the exlamation point ! . ex - fn bar() -> ! {} means that this fn never returns.
- ex - "continue" kw is never type, and can be useful inside the match block, (the type never returns)
- in the match block the continue will never return anything instead it will move back to the top of the loop and which has val.
- this never type can be useful in panic macro as well. ex match self {Some(val) => val, None => panic!}..the panic! is the never type, thus the return value of this function is val.
- And the loop is the never type.. since the loop never ends hence the val of this function is never type, however this wouldn't be true if we use the break statement which cause the loop statement to end.

**Dynamically Sized Types**

- Are some times called as DST or unsized types or the type whose size can be only known during the runtime.
- ex : str type, we can't store the val directly in the str type, so instead we can use the borrowd version &str.. now this will store 2 vals. 1. addr pointing to the mem location 2. and the len of the str. and both of those vals are usize and we know the vals in compile time
- the dst ve extra bit of meta data which stores the size of the dynamic information.
- "Traits" are also DST
- Rust has the special trait called the size trait, to determine whether the size can be known at compile time or not.
- size trait is automatically implemented to every type, whose size is known at compile time.
- also rust implicitly adds the trait bound to every generic fn, so by default the generic function will only work whose size is known at compile time.
- we can use a special syntax to relax this restriction, ex - fn generic<T: ?Sized> .. ?Sized means its sized or not. and this syntax is only available for the sized trait.

**Golden Rule for the DST**

- is we must always put em behind some sort of pointers, in the above ex - we put it behind the ref but we can also put it behind the Box or Rc Smart pointer.

## Advanced Functions and closures

1. Function Pointers

- previously we seen we can pass the closures to the fns, but we can also pass fns to the fn.
- its useful when we already defined the fn(and wanna use it) instead of creating a new closure. This is like the fn as an argument.
- to pass in the fn to the function we can specify the function pointer(fn), ex - fn add(x: fn(i32) -> i32)
- unlike closures the fn() is a type we can specify directly to our params, instead of using one of the traits as a trait bound.
- the function pointer implements all the three closure traits (Fn, FnMut, FnOnce), which is why its a best practice to write the fns that accepts closures.
- one use case where we wana use the fn() as arg type is with the external fns that doesn't accepts the closures.
- here is an another ex of pattern that exploits the implementation detail of tuple struct and the tuple struct enum variant. ex - enumt Status{Value(u32), Stop} - tuple struct uses paranthesis to initialize the val inside the tuple struct, which look like a fn call,
- infact this initializers are initialiazed as fns that takes arg and returns the instance of that tuple struct.
- this means we can use this tuple structs as fn pointer ex - let x:Vec<Status> = (0..10).map(Status::Value).collect().. here we re passing in the Value() tuple to the map(), now map is gon treat that like a fn pointer, where the arg is gon be the u32 and the Value is gon be the return val

2. Returning Clousures from functions.

- ex - fn add() -> { |x| x+1; } .. the closure are represented using traits, so we can't return a concrete type here.
- so instead what we can use here is a impl trait syntax. ex - fn add() -> impl Fn(i32) -> i32 {} .. here we re saying like the add() function returns the impl trait(Fn()) that takes i32 and returns i32.
- note we can use the impl trait only when it returns one type
- in the ex- from doc the 2 closures just look identical but they are of diff type (bcoz no two closure even if they re identical will ve the same type.)
- so each closure is a unique type even if it seem identical. so we can't use the impl traits.

- so in this case instead of using the impl traits we can use the trait object.
- and just like the other trait objects, rust doesn't know the size of the closure being returned, so we ve to wrap it in some sort of pointers.
- ex - fn add() -> Box<dyn Fn(i32)> -> i32 and we also need to wrap the closures with the Box.. ex- Box::new(move |b|b+a) function.

## Declarative Macros

- as we know the macros allow us to write code, which are written in other codes
- which is known as meta programming, we can think of it like a fns, where the i/p is code and the o/p is also code which is transformed in some way.
- we ve been using so far the println! or the vec! .. these macros tend to produce more code than the code we written manually,
- the macros will helps us to reduce the amount of code we ve to write and maintain
- which is similar to the fns however there are few key difference.
  - fns must declare the no.of params that'd accepts, where as the macros could accepts variable no.of params.
  - also fns are called at runtime, where as the macros are expanded before the program finishes compiling. so the macros are more powerful than the fns.
  - however the downside is we re increasing the complexity means we re writing the code that writes other code, meaning that the macros are harder to read, understand and maintain.
- with that caveat rust has 2 macros 1. Declarative macros and 2. Procedural macros.

1. Declarative macros

- these are most widely used macros, they allow us to write something similar to the match expression.
- to create a macro lets see the vec! implementation.. first it starts with #[macro_export] followed by macro_rules! vec {} ..and the remaining refer the documentation.
- ($($x:expr), _ ) - $ is sayin capture any vals that matches the $x:expr before use in the replacement code. and the coma means the code could match the , sign and then the _ means that our pattern could match the 0 or more types.
- refer the macros little book already book marked.. for the more detailed description.

## Procedural macros.

- the procedural macros are like fns they take the i/p code and operate on that and produce code as o/p.
- this is in contrast with the declarative macros, which match against the patterns and replace code with the other code.
- there are 3 types of procedural macros.

  1. custom derived
  2. attribute like and
  3. function like (and they all work in a similar pattern)

- the procedural macros must be defined in its own crate with the cust crate type -- use proc_macro;
- then the annotations of the macros type - ex - #[some_attribute] and the fn some_macro(i: TokenStream) -> TokenStream;
- the Tokens are the smallest individual elem of the prog.
- they can represent operator, literal, kw, identifiers, separators.
- procedural macros has to be defined in their own crate.
- since its a special type of crate so in the cargo.toml file under [lib] we ve to declare like proc-macro = true. and then add the syn and quote as the dependencies.
- the proc_maco is the crate that comes with rust so we don't ve to declare in the dependencies, however to import the crate we need to mention extern crate proc_macro; statement
- then we ve to ve 3 use statements we ve to brin use proc_macro::TokenStream; use sync; use quote::quote;
- the sync crate is short for syntax, allow us to create rust code, and turn it into a syntax tree data structure.
- and the quote crate can take the syntax tree data structure and turn it into a rust code.
- next we define our cust macro --> #[proc_macro_derive(HelloMacro)] indicating that thisi is cust derived macro with the name HelloMacro.
- the quote! has templating ability, and it will replace the #name with the actual name of our type,
- the stringify! macro will take an expression and w/o evaluating the expression ex - stringify!(#name)

2. Attributes like macro

- are similar to custom derived macros, except instead of generating the code for the derived attributes, we can create for the custom attributes.
- also custom derived macros only works with enums and structs where as the attribute macros work with other types such as functions.
- ex - route (attribute) -- #[route(GET, "/"")]

- we can define our attribute like macro with the function anoted with #[proc_macro_attribute] and our route fn will takes 2 args of TokenStream type. 1. the first arg will contain the context of the attr, the second arg contain the content of the item the arg is attached to (in our case its index fn.)

3. Function like macro

- they look like function calls however they are more flexible,
- firstly they can take variable no.of arguments and secondly they operate on rust code.
- ex - we wana create a sql function which will take sql! as the arg - let sql = sql!(SELECT \* FROM Posts WHERE id = 1);
- the fn impl of the function like are similar to the attribute macro, ex - #[proc_macro] and then pub fn sql(i/p: TokenStream) -> TokenStream {}

## Building a web server

- **listeningto Tcp connection**

  - we can ve to use the tcp listener struct from the std library, - use std::net::TcPListener;
  - to create a new listener let listener: TcpListener = TcpListener::bind(addr:"127.0.0.1:7878").unwrap();
  - the .bind() will return the result type so, if our result ve any error it will panic
  - then we re calling .incomming() over the listener which will iterate over the connection being received on our listener in the form of TCP stream.

- **Reading data from the TCP Connection**

  - Bring the Tcp Stream struct to the scope. use std::net::TcpStream; and also use std::io::prelude::\*;
  - also bring the io module preluded in the scope.
  - then create a fn handle_connection() takes the mut stream as arg of type TcpStream, the call this fn inside our listener. this stream is
  - then create a mut buffer that holds the data ie read.
  - then read(from io) the stream, this read takes the &mut this is bcoz when we read the stream some states will get modified.
  - then finally we print out our buffer by String::from_utf8_lousy(&buffer[..]) .. this from_utf8_lousy() - which converts a slice of bytes to a string , including invalid characters.

- **writing a Response**

  - our response must be in the following format

    - first we have the status line, which conssist of the HTTP version, StatusCode, ReasonPhrase, CRLF(Carriage Return Line Feed sequences) \r\n
    - next Header, CRLF(Carriage Return Line)
    - and finally the messsage body.

  - lets see the resp that contains no header and no message body.. ex - HTTP/1.1 200 OK\r\n\r\n (2 CRLF)
  - then we will save this response as &str. and then write the stram - steam.write(buff: respose.as_bytes()).unwrap().. this write() expects buffer of bytes as i/p so we re using on our buffer.
  - flush() will wait until all bytes are written to the connection, and it also returns the result type.

- **Returning valide Html**

  - create a valide html file in our root dir
  - now we can modify our handle fn to return this html page.
  - first we need to get the conten of the page and store it in a variable.
  - ex - let content = fs::read_to_string("index.html").unwrap();
  - then modify our response - let response = format! ("HTTP...", contents.len(), contents) -- here we re using the format! for string interpolation
  - the second arg content.len() header specifies the amt of bytes we re returning in the msg body.

**Validating the Request**

- let get = b"GET / HTTP/ .."; here we re prefixing our str with the b (which will give us the byte[] representing the string)
- next we will check if the buffer start with the expected string. if its trued then we wana return our page.
- else return the 404 html page.

## web server part 2

- previosly we ve seen single threaded server, this time we will see multi threaded server.
- the problem with the sigle threaded server is that, if it gets 2 requests, then it will process to the 1st request and then it will move on to the next request.
- there are multiple ways to solve the slow req backing up the server, we will look into the the 1st ex.

**Thread Pool**

- the way it works is it will ve a fixed no.of threads in the thread pool. then when the req will come into the server it will pickup the thread and start processing it.
- when the thread is finish processing the req, it will return back to the thread pool to pickup the new req.
- this means our server will able to process the multiple reqs concurrently, ex if the thread pool has 10 threads then our server will be able to process 10 reqs concurrently.
- the reason we re using only 10 threads is we don't wana someone to issuing ton of req (which would take ton of threads to spin up and go exhanustive) and take down our server resources.

- the way we will impl the thread pool is 1st thinking about its public api.
- and we want our thread pool interface to same name as the spawning the thread.
- ex - let pool = ThreadPool::new(4); and then the closure is like pool.execute(|| { handle_connection(stteam) })

**compiler Driven Threadpool**

- we will implement the thread pool in library crate.
- then we wana make the lib crate the primary crate, in order to do that, we can make a dir called bin in our src directory, and then move our main.rs to bin, now we ve lib.rs in our src directory.
- one thing is that we re not assigning anything for the size arg in the thread pool type impl fn, we are just implementing the enough functionality to resolve the given error, after resolving the error we might get new error, and then we will continue down the path of functionality until we don't have any error.
- this approach is compiler Driven Development, and its similar to test driven development.
- `static lifetime is the receiver can hold on the types as long as they need to and the type will be valid, until the receiver drops it.
- then our thread pool implementation, update the fn to ve thread size of size gt 0, coz it doesn't make any sense if its lt 0. so we can use the assert!(size > 0 )

**Storing the Threads**

- lets modify our thread pool struct to store a vec of threads (if we see the thread module the spawn thread fn returns the JoinHandle)
- lets ve a thread pool a vec of join handles, - threads: Vec<thread::joinHandle <()>> .. in this case our closure are not returning anything so specify the unit type <()>.
- we will use the for loop for creating the threads.
- the spawn() will take a closure which will exec immediately after the thread is created. this is not we want, we want to create thread that will wait for some code to execute later on.

**Worker Threads**

- instead of storing threads direclty we gon store thru worker(Struct).
- this worker will ve id and thread field(that listens for the jobs to be done).

- our execute method takes the closure as an arg, what we want to do is send that closure to one of our workers for processing, as we know channel are good for send info to one thread to another.
- this is the right case when execute method is called we will use the channels to send the closure to one of our workers.
- for this we ve our thread pool struct to hold onto the sending end of the channel, and each worker struct will hold on to the receiving end to the channel.

**sending req via closures**

- first we will create a job struct which will hold the closures we wana send down our channel.
- first destruct the tuple of sender and receiver - let (sender, receiver) = mpsc::channel()
- then we will store the sender inside the thread pool struct and then pass the receiver to each worker inside the for loop
- but with this iteration we will get error, in the 1st iteration of this loop we pass in receiver to the new fn of worker by val whcich moves the receiver into the worker, so in the 2nd iteration we will no longer pass in the receiver.
- what we want is our worker to have the shared ownership of the receiver, in addition listening the job requires mutating the receiver.
- we can get these behaviour by smart pointers, since we re working with threads, we want thread safe "smart pointers".

- for thread safe and mutiple ownership we can use the **Arc Smart pointer**
- and for the thread safe mutability we can use the **Mutes smart pointer**.
- ex - let receiver = Arc::new(Mutex::new(receiver))

## Implementing the execute method

- first we will change our job struct to type alias ex- type Job = Box<dyn FnOnce() + Send + `static>
- then in the execute method we will wrap our closure in the Box smart pointer. then we will send the job down the channel... ex - self.sender.send(job).unwrap()
- the send() will fails if all of threads stop running
- now lets updating the worker struct, inside the spawn thread closure the first thing we wana get the job.
- ex - let job = receiver.lock().unwrap().recv().unwrap() -- first we re calling lock() to aquire the mutex, then unwrap(coz aquiring the mutex may fail), then recv() to receive the job from the channel.
- the closure may outlive the reciever so we can move the reciever into the closure

- finally we want our closure to execute infinite loop. because we waant our threads to constantly look for the job to execute.
- and to do that -- match || loop{receiver} and in our new() we call the job()

- in summary, lets say we ve 4 workers in our thread pool, one of the worker may acquire a lock to the reciever and then pick up the job to execute, as soon as the worker start executing the job, the reciever will be unlocked, and then the other worker will aquire the lock for the job and start executing the 2nd req. and so on
- if the 5th req will comes in where all the 4 workers are busy executing the job, then the 5th req has to wait.

- there are many ways to increase the thru put of the server, such as fork join model or single threaded asyc io model.

## web server part 3

- lets see about the graceful shutdown and cleanup.
- since we don't ve the shutdown in our current implementation, if we exit ctrl + c the server will abruptly shut down and any thread that currently workin will abruptly exit.
- lets improve this by letting the worker to finish before the server shutdown. to do that we can impl drop trait on the thread pool.
- ex - impl Drop for ThreadPool {fn drop()} -- inside this we will loop thru each one of the worker and call join() on the threads.
- but we can't use the .join() method on the worker (which is &mut ) since its a mut ref the .join() method will takes the ownership of the worker, to fix this instead of our workers having joinHandle directly, we can ve the optional containing the joinHandle or the None variant.
- then we can use the take() method to move the joinHandle out of the optional and replace it with None variant.
- so in worker struct - { thread: Option<thread::JoinHandle<()>} then we need to update the new() on worker.
- instead of passing the thread directly we will pass it in Some var - Worker {thread: Some(thread)}
- lastly we need to update our drop() impl.
- ex - if let Some(thread) = worker(thread).take(){thread.join().unwrap()} -- if take() returns the None variant then we know that the workers thread was already been cleaned up. so there is nothing left to do.
- now the join() will wait for our thread to finish however with our current implementation, the thread will loop inddfinitely
- so calling the join() method will block our main thread forever, waiting for the thread to finish.

**signaling the Threads**

- to make this change, currently our threads are only listening for the new jobs, we also want our threads to listen for the either job or the signal that they need to terminate.
- so for that we can create a new enum - enum Message {NewJob(job), Terminate}
- next we will update the sender to listen for the message instead of job. - self.sender.send(Message::NewJob(job)).. then also update in the worker impl, now our reciever now receives messages instead of job
- then we can match on the type of message we get if we get new job, then we will execute the job(), or if we get the terminate variant we will execute the exit the loop.
- now our thread are able to terminate, lets update the Drop impl on the thread pool to send the terminate message.

- so we re looping to thru the workers and sending the terminate message, so we will send no.of terminate message equivalent to the no.of worker in our thread pool, now we ve no control over the order in which our worker will pick up the terminate message. worker pick up the messge when they are done with the job.
- after sending out the terminate message we loop thru all our workers again and call the join(), which ensures each worker will ve enough time to recieve and process the terminate message.
- listener.incomming().task(n) -- this listener.incomming() returns the iterator on the incomming req, and we re calling the take() on the iterator which yields the first n elem

## Async/await

- in rust async is a special syntax that allow us to write fns, closures and blocks that can pause the execution and yield control back to the runtime allowing other code to make progress and then pick back up where they left off.
- typically those pauses are to wait for io. one advantage of the asynchronous code is that it allow us to write the asynchronous code that looks like the synchronous code.
- w/o the syntax sugar the async fns are just the fn that impl the Future Trait and the o/p associated type which represent the return type -- fn my_func() -> impl Futrure<Output=()>{}

- the future trait is a simple state machine which could be pulled to check up if its ready to return for the val. the Poll enum has 2 variant ready(whether the future is ready with the val) and pending (or the future is still expecting)
- the poll method also executes the cb method called wake.. if the calling poll method is pending then the poll() is continue making the progress, until its ready to get polled again.
- when its ready to get polled again the future will call the wake cb to notify its executer.
- fn Poll(&mut self, wake: fn()) -> Poll<self::Output>;

- this "futures" are similar to the promise in js, except they are lazy, they won't do anything unless driven by the completion by being pulled.
- this is what allows the future to be a zero cost abstraction. which means we won't incur the runtime cost unless we actually use the futures.
- and the another benefit of future being lazy is that they are easy to cancel. inorder to cancel the particular future all we ve to do is to stop polling the future.
- future could be drive to the completion by either awaiting the future or giving it to an executor.

- adding the await() to the async function call will attempt to run the future to completion. the await kw will also pause the execution of the current future yielding control back to the runtime.

- futures could be driven to completion, by 2 ways either calling await on its fn or manually polling the future until its complete.
- the await kw will work on other places but not in the top most future ex - we can't use it in the main().. to use the awaait kw in the top most future(manin()) we need to manually poll them to completion.

- and now that code is now called "Runtime or Executor", The runtime is responsible for pulling the top level future and running em util its completion. its also responsible for running multiple future in parallel.
- the rust std library does not provide the async runtime. so in order to async runtime we can use the community build async runtime, the most popular one is **tokio**
- add the tokio in our dependencies and make full future to be available.
- Now in our main() function use the macro - > #[tokio::main].. now with this we can await our main()

**tokio::tasks**

- to make our async code running concurrently we can use tokio tasks, a task is a light wight non- blocking unit of execution. a task is a green thread similar to **Go Routine**
- tasks allows top level futures to execute concurrently.
- to create a new task we can call the spawn on the tokio module .. ex- let mut handles = vec![]; for i:i32 in 0..2 {let handle = tokio::spawn(aysnc)} -- the tokio spawn takes future as an arg and returns joinHandle.

- this api is very similar to the api of spawning a thread. this is on purpose tokio makes it easy to switch from using threads to using tasks.
  -the tokio also has the sleep() is similar to the thread sleep(), except it will stop the current future from executing for a given duration, instead of an entire thread.

- by default tokio uses thread pool to execute tasks this allow us to execute tasks in parallel. we could however force tokio to execute on one thread by changing the flavor to current thread. ex - #[tokio::main(flavor = "current_thread")].. this would cause thread to be executed using time slicing instead of threads.

- note that, like threads in order to communicate b/w tasks we either need to use message passing message thru the channel or to use the shared state thru mutex.
- unlike threads async code uses cooperative scheduling instead of preemptive scheduling. if we ve 2 threaads the os can switch b/w 2 threads at any given time.
- when using the asyn code we need to tell the run time when a block of code is ready to yield so the other async code can run.(by using the await) finally we don't want to run the cpu intensive tasks on the async code..

## Modules mod

- to create a module we use the mod kw and to import the module we use the use mod kw
- the modules serve few usablity, for orgainize the code, readabilty, reuse and they also help us control the scope and privacy.
- in python, js and other langs the modudles are mapped to the file s/m but in rust the modules are not mapped to the file s/m
- a single file can ve multiple modules.
- if we wana run our code with the unused fns or vars, or types we can wrap our code with macro #![allow(dead_code, unused_variables)]
- **cargo Modules**

  - the cargo modules is a cargo plugins that allow us to visualize the crates, modules tree.
  - `cargo install cargo-modules` - and to visualize the tree `cargo modules generate tree` and we can also use the --with-type flag in this tree command to see the modules types

- the crate module is the root of our modules and it automatically created for every crate.. the content is either (bin crate or lib crate )
- even though we can create multiple modules in the same file, but still when we use the vals of the module in the other modules we need to specify the fully qualified relative pathe :: or the the absoulte path of the item in the module tree.
- for the submodules we can use the relative path, and to the vals in the other modules we ve to use the absolute path.

- but these fully qualified paths will make our code clutter, so to resolve this we can use the **use Declaration** to create a local name bindings to a given path.

  - in other way of saying(Rust way) this is we re bringing the items in the scope with this use declaration. in other way we re simply importing (although wchich is diff that importing external libs), coz with this use declaration we re bringing the mod with in the crate.
  - with this sub modules and modules in the same file (lib.rs) we can see the module tree, it clearly shows the crate has multiple modules and the submodules.
  - with this in mind, compare with the js and other languages(where the each and evey modules map to the one to one file s/m), where as the rust modules doesn't ve a one to one mapping with the file s/m.

- so in our ex - the modules we ve defined are in line, means the module and the content of the module declaration are in same file hower we can ve them in separate files.

  - in rust there are 2 ways to do this - **1**. if we ve a modules with no submodules then put the content in the separate files and sure the fields and fns are public, and then in the mod declaration file (ex lib.rs) just type use module_content; -- just the use kw and the content file name.
  - bcoz the module_content is not declared in the inline so rust will automatically look for the file.

- in rust submodules must be declared within the parent module, regardless of their contents is declared within the inline or separate file

- 2. the second appproach is to create a create a dir with the same name as our modules which contains the mod.rs - then we can place all our contents in there.
- so in this case the rust will look for the inline content, and then it will look for the same file name(as our module declaration) if no such file, then it will look for the file mod.rs (which does exists in our case)

- this approach of using mod.rs is analogous for using index.js file.
- this can sometime be difficult if we ve multiple files all opened at a time and ve the same name - mod.rs its difficult for us to keep track. so its better to stick with the 1st approach.

- note the use kw/ statements are also useful for reexporting, to do that we can use the pub kw b4 the use statement --> ex pub use module_content.
  - this can be usefule when our mod file only exporting one item but we also wana export the val from the top level module.
  - by this pub use kw we re bringing in our var into scope and reexporting it from the top module.
