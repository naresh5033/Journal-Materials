# Go (continuations from the notes ) FCC

### Maps

- map is unordered we re just mapping key to the vals.
- some of the fns we can use in the map.. we can insert, del, get an elem in the map and we can check to see if the key exists in the map
  - ex - elem, ok := m[key] // the 2nd val is the bool

**key types** - the map keys are any type that is comparable. ex - bool, str, number, pointer, channel, interface, structs or array are of those types. these can be compared for the quality - what can't be compared for the quality is the slices, map, fn bcoz these are just pointing the addr to the mem, ex - if we re comparing one slice to the another we re not actually comparin the vals we re comparing the addr - so we can't use slices, map and fns as map keys

- which is simpler ? to use the struct directly as the key or to nest the map ?

  - ans is to use the struct directly as the key

- map can ve at most
- if we attempt to get the val from the map where the key doen't exist it will returns the 0 val
- if our map itself doesn't exist then our code will panics.
- a fn can mutate the val stored in the map and those changes will affect the caller.

**nested Maps**

- we ve seen the maps can contain another map as the val not as key,
- in go the char is often called as **runes**

### Advanced functions

- **first class and Higher order fns**
  - the first class fns is, if it allows us to pass around fn (just like pass around vars). the fn that treated like any other var
  - and then the fns that uses that fn, aka a fn that accepts the fn as a params and returns the fns is called higher order fn.
  - the first class and the Higher order fns are often used in the backend of the stack such as
    - Http/api handler
    - pub/sub handler
    - onClick callback

**fn Currying** - is also a kind of High order function, it accepts the other fn as an param but it also returns the new fn as a o/p. - again this can be useful in the backend when particularly in the mw.

**Defer Kw**

- is a unique feature in go, it allows the fn to be executed automatically just before its enclosing fn returns.
- the defered fns are often used in the db to close the connection, file handlers like that.. for the clean up stuffs.
- this defer kw allow us to exec some fn in the end of the current fn. or the current fn exits.

**Closures**

- just like the rust, the closure is a fn that references the vars outside from its own fn body,
- but there is no specific syntax for the closure..unlike (rust)
- the closure can mut a var outside of the body.
- when a var is enclosed in a closure, the enclosed fns has access to the mut ref of the original val.

**anonymous fns**

- as the name suggest the fns don't ve a name . no name..
- is more useful when we re creating like one off, may be creating a closure or returning new type of fn , if not defining the fn for use across the entire program. but using as a val or first class fn. thats when we use the anonymous fn.

### Pointers

- the pointer is a specific type in go.
- the * syntax defines the a pointer ex - var p *int
- **so here is my big confussion the _ is used as the pointer type and again _ is used to deref** - when this * is used along with the type like the one on the above ex then its a pointer type but if its used along with the var then its a deref.. ex - *y = 5;
- pointing the mem addr of the var in the stack
- the deref operator * -- if we wana mut the val of var x w/o access to the x then we can use the deref operator ex - x := 5; y := &x ; *y = 6; -- so now the val of x is 6.. this is how we use the deref.
- the pointers are useful it allow us to manipulate the data in the mem w/o making copies or duplicating the data

- **NIl pointer**

  - if the pointer points to nothing (then its 0 val of the pointer type), then deref it will cause the runtime error or panics..
  - so pretty much every time b4 we deref we need to check whether we re ref anything, then only we ve to deref it.

- **pointer with the methods**
  - a reciever type on a method can be a pointer.
  - ex - func (c \*car)
  - and its more common to see the fns with pointers than a fns with non pointer recievers.

### Local Development

- as we know every go prog is made of packages, ex - package main - on top of code , we re writing code on the main package.
  - this is a special type of package that run a standalone prog.
  - and the main package always has the main fn which serve as the entry point to our program.
- and any packages that other than main is the lib package, ie imported from the other lib.
- in go the main packages live in the directory level rather than a file level. and one package per directory, (we can't ve multiple packages at the same directory)

**modules** - the module is a bigger idea than just a package, the module is a releasable collection of go packages, ex - go.mod - there is no central location for the 3rd party packages, like npmjs.com.. instead go tool works on top of git, nd uses the import path as the remote url. - the packages in the std lib doesn't ve a path prefixes.. - and to create a new go mod `go mod init github.com/naresh5033/hellogo`(the hellogo is the name of our repo ) and in the go.mod file we can see **remote url in the mod path** this is to simplify the remote downloading of packages. - `go build && ./hellogo` build and compile the bin flie - `go install` will compiles and installs the prog locally.

- **Lib package**

  - to create a lib level package , use the kw "package helloworld" and inside we don't ve to ve a main fn and the normal fn is enough, but the fn name should be starts with caps ex - func Add()
  - by conventions the lower case fns are not allowed to exported (can't export)
  - so when we run the `go build` cmd in the lib packages, it will just compile the package and saves to the local build cache..
  - Generally the go tool will look for the package to download from git, but if we ve package locally available then we can replace the file path in the go.mod file.

  **clean Package**

  - we shouldn't export the fns from the main package. since the main package isn't a lib, so there is no reason to export from the main;
  - when should we not export a fn, var or type ? when the end user doesn't need to know about it.
  - we shouldn't often change the package's exported api. instead try to keep changes to the internal functionality..

  ### Channels and concurrency

  - note : writing concurrent / parallel can't increase the performance of our prog.
  - we ve to write our code in a diff way. we need to expect some of the ix are gon to happens at the same time, and that's what gon to speed up our prog.
  - we need to exec one part of the code in a separate cpu core and the other in a separate. (taking advantage of the cpu core)
  - **go** is the kw to write the concurrent prog in golang, and it spawns a new GoRoutine,
  - ex - we can simply create a new GoRoutine in a anonymous function -- go func(){}()

- we can think how useful it is to call a fn in go routine, if we can't even get the return val. ?
  - the answer in go is we use **channels** typically to resynchronize our code..

**channels** - just like the maps and slices, the channel must be created b4 we use em. ex - ch := make(chan int)

**send data**

    - to send the data to the channel <- operator (is called as the channel operator) ex - ch <- 69  (here the val 69 is send into the channel)
    - Data flows in the direction of the arrow, this operation will block until the another go routine is ready to recieve the value.

**Receive Data**

    - to receive data from the channel ex - v := <-ch .. here this reads and removes the val from the
    - channel and saves into var v, this operation will block until there is a val in the chan to be read.

- one thing to remember both the sending and the receiving on a chan is a blocking operation, ex - if we re tyna send the val into the chan and there's no go routine on the other end to read it out, then our code will stop and wait on the go routine..until the reader is ready..

**Deadlock** - when a group of go routines are all blocking and none of them can continue, and this is a common bug, we need to be careful when working on a concurrent prog..

- **Tokens**

  - empty structs are refered as tokens, in this context the token is a unary val, we don't care what is passed in the channel, we only care when nd if it is passed..
  - we can block until something is sent on the channel and the syntax is <-ch.
  - this will block until it pops the single item off the channel and then continue, discarding the item..

- **Buffered channels**

  - channels can optionallay be buffered..
  - to create a chan with the buffer, we can provide the buff len as the 2nd arg to make() to create a buffered chan ex - ch := make(chan int, 100) .. here we defined the buff size of 100.. we can also pass in the len as the buff arg ex - ch := make(chan int, len(emails)) ..
  - sending on the buffered chan only blocks when the buff is full.
  - receiving blocks when the buff is empty.

- **Closing Channels**

  - channels in go can actually be closing ex - ch := make(chan int) .. close(ch)
  - the reason that we wanna close the channel is to indicate that we re done with it.
  - the channels should always be closed from the sending side, so the sending go routine will be the one to close the channel. and this will indicate the readers that this channel is closed and there is nothing else to read from it.

- **checking if a channel is closed**

  - now on the reading side we can actually check if the channel is closed, ex - v, ok := <-ch
  - with that optional(ok) bool if its true the chan is open and if its false the chan is closed.
  - if the channel is happens to be buffered then the ok will remains true until the chan is emptied out.

- Don't send to the closed channel.. if we do then the chan will cause panics

- **Range kw**
  - sim to the slice and the maps channel can be ranged over..
  - ex - for item := range ch {} -- this ex will receives the vals over the chan (blocking at each iteration if nothing new is there ) and will exit only when the chan is closed..
  - it will block until the val is ready..

**Select**

- the select is like a **switch** stmt..
- sometimes we ve a single go routine listening to multiple channels and want to process the data in the order it comes through each channel..
- a select is used to listen multiple channels at the same time, its similar to switch stmt but for channesl

  - ex - select { case i, ok := <-chInts: fmt.Print(i)
    case s, ok := <-chStr: fmt.Print(s)}

- the select also has the **default** case . this will stop the select stmt from blocking..
- we don't necessarily need to use em, basically it will only fires if we re interested in non blocking..

**Tickers**

- lets see some std lib fns..
- time.Tick() is a std lib fn that returns a chan that sends a val on a given time interval.
- time.After() sends a val once after the duration has passed.
- time.Sleep() blocks the current goroutine for the specific amt of time.

**Read only channel**

- the chan can be marked as read only by casting it from a chan to <-chan type ..for ex - func readChan(ch <-chan int){}
- then inside the main fn - ex func main(){ch:=make(chan int); readCh(ch)}

**write Only Channel**

- the same goes for the write only chan but the arrow position moves..

  - ex - func writeOnly(ch chan<- int){}

- so the **readers read out of the chan and the writers writes into the chan**

- **send to the nil chan blocks forever**

- if we don't use the make fn to the chan then the chan is nil. and if we try to send a val to the nil chan our code will block forever..
- lly **the receive nil chan will block forever**

- **A Send to the closed chan blocks panics** so we need to very careful, and makesure we re

- **Receives from a closed chan returns zero val immediately**

### Mutexes (Mutual exclusion) aka Mux

- are also allow us to communicate or share data b/w Goroutines,
- mutexes work by locking access to protected resources, so that only one go routine can access the resources at a time..
- go's std lib provides the built in impl of the mutexes with the **sync.Mutex** type and its 2 methods

  - Lock() and Unlock()
  - we can protect a block of code by wraping the lock() or unlock()
  - ex - func protected(){
    mux.Lock()
    defer mux.Unlock()} - here the defer can be used to ensure that we never forget to call the Unlock()

    - here when one go routine calls this protected() if its a 1st call then this mux.Lock() will lock the mutex for this go routine, and it will be able to move on and complete the rest of the fn and then call the Unlock()
    - every other goroutine that shares this mutex will actually block, since the mutex is block by other goroutine..
    - as the name stands it excludes every go routine except the one that holds the lock.
    - mutex are very powerful, like other powerful things, it can cause many bugs if they don't handle carefully..

  - **Maps are not thread safe**
    - Maps are not thread safe in go routines, if we ve multiple go routines working to the map, atleast one of them is writing to the map, and we must be lock our maps with mutex..
    - some times it will cause the race condn and go will panic..

- **Note: Wasm is Single Threaded**

  - which awkwardly means maps re thread safe in web assembly..

- the mux can locks **one thread** at a time.. if it locks any more at the same time it wouldn't be vary useful..
- we use the mux to safely access the data **concurrently**

- **RW Mutex**

  - the std lib also exposes a sync.RWMutex.. Read Write mutex
  - in addition to the Lock() and Unlock() the RW Mutex also has couple of methods - **RLock(), RUnlock()**
  - it will allow us to ve **multiple Readers** at the same time..so it can help with the performance if we ve read intensive ops..
  - so the way it works is if the mux is Lock(), then no other goroutine can Lock() or RLock() the mux
  - But if the mux is RLock() then other mux can RLock().. so we can get multiple readers but we can't get writers and writers or readers and writers..
  - so many go routines can safely read from the map at the same..(multiple RLocks() can happens simultaneouly). however only one go routine can hold the Lock() and all the RLocks() will be excluded.

- in summary There is only **one Writer** can access the RWMutex at same time..where as **infinite Readers** can access the RWMutex at the same time..
  - and even the reader can't access(at a time) if the mutex is access by the writer

### Generics (or Type Params)

- introduced in go in 1.18.0 .. one of most requested features.
- generics allow us to use the vars to refer to the specific type.. and it allow us to write the abstract fns that drastically reduce the code duplication
- the generics are oftern used in the libs and the packages

- **Constraints**

  - allow us to write generics that actually used for the subset of types..
  - in other words constraints are just the interfaces that allow us to write generics that operates with in the constraint of the given invterface type.
  - ex - func add[T any] (x []T) {} -- here the constraint of type T is Any
  - lly func add[T string] (x []T) {} -- here the constraint of type T is string
  - As we know **T** is just the naming convention could be anything we want..

- or we can create our own cust struct type or interface type and then use that as the constrait Type.

- **New Interface Types**

  - when generics was released, a new way of writing interface was also released at the same time.
  - we can now simply list a bunch of types to get the new interface/ constraints (type)
    - ex - type Ordered nterface {~int | ~int8 | ~int32 |} ..just like that
  - this ordered is a type constraint that matches any ordered type.
  - the ordered type is one that supports comparision operators

- **ParaMetric Constraint**

  - it basically means we can use the type params in the interface as well..
  - ex - type store[p product] interface {} .. here the store interface takes in the product interface as its type

- **Go Proverbs**
  - one of the creator of this lang created awesome set of **proverbs** that outlines some of the more philosophy behind the language.

    - Don't communicate by sharing memory, share memory by communicating. (its saying we should try to use chans more often than use the mux)
    - Concurrency is not parallelism.
    - Channels orchestrate; mutexes serialize.
    - The bigger the interface, the weaker the abstraction.
    - Make the zero value useful.
    - interface{} says nothing. (and sometimes this just leads to unsafe code)
    - Gofmt's style is no one's favorite, yet gofmt is everyone's favorite.
    - A little copying is better than a little dependency.
    - Syscall must always be guarded with build tags.
    - Cgo must always be guarded with build tags.
    - Cgo is not Go.
    - With the unsafe package there are no guarantees.
    - Clear is better than clever.
    - Reflection is never clear.
    - Errors are values.
    - Don't just check errors, handle them gracefully.
    - Design the architecture, name the components, document the details.
    - Documentation is for users.

### RSS Aggregator Project

- lets build a backend svr, the purpose of this svr will aggregate the RSS feed, the Rss is a protocol that makes the distributing things like podcast and blog post easily..
  - now our svr will users to add diff rss feed to the DB and then it will go automatically collect all of those post from the feeds and download em and save em in the DB..so we can view em later.
- and this svr we will be building is gon to be the json REST api.
- to get/ pull the package from the github `go get github.com/joho/godotenv` in this case we just need the dotenv package
- then `go mod vender` to copy that package/code into our vender dir -- kinda ve the local copy of that.
- for this project we will be using the **chi router** a 3rd party router, which is very light wight. just like the go std package `go get github.com/go-chi/chi` and also `go get github.com/go-chi/cors` the cors from the same package.

  - again to add the vender `go mod tidy` && `go mod vendor`

- the err code in the range with in 500 are the client side err so anything is gt that is the svr side error ex - err > 499

  - when we create our cust error struct we can mark it as the string type corresponding to the json, and this is called the "json reflex tag" ex - type errRes struct { err string `json:"error"`} -- so now for this json str type the error is the key and it looks like {"error": "some thing went wrong"}

- for the query we ve to install sqlc and goose packages, he uses the local postgresql db and pg admin to connect that (these packages allow us to write raw sql queries)
- the way sqlc works is it takes the sql statements and it generates types safe go code that matches sql

  - ex - --name: CreateUsesr : one .. create a user and it returns one record
  - and we can pass the args like VALUEs ($1, $2, $3) these are the parameters for the user and we can return with RETURNING \*;
  - to gen the query `sqlc generate` this will gen db dir which contains db.go and models.go and users.sql.go
  - and inside the schema dir where we create the migrations for the user and an if we wanted to add any new migrations then we ve to keep the order ex - 001_users.sql , 002_user_apikey.sql .. so by this way the goose will know which order the migrations will be done..
  - in this 002 aka the second migration we can alter the user table with add a new column to store the api key (which is of type VAR CHAR)

- when we use the error (the std package from go ), we should not use the caps ex - errors.New("must be case sensitive") .. otherwise it will throws the linting error..

- **Context()**

  - in go we ve the context package, basically it gives us the way to track something that happens across the multiple go routines.. ex - GetUserByApikey(r.Context(), apiKey)
  - the most important thing we can do with the context is we can cancel it..by cancelling the context it will effectively kill the http req.
  - and every httpreq ve the context, and we can use the context with any calls that we make within the handler that requires context..

- to grab the param id we can use the chi.URLParam(r, "feedfollowID") .. req and the id we wana grab..

- so far we ve been buit the crud part of our server..

**to fetch the Rss Feed**

- now the part of the svr goes out(periodically) and fetches the posts from the rss feed in our db..
- for that we can create a "scrapper.go" which is a long running job, so this will running in the BG, as our server runs..
- the fn will takes the no.of goroutines that we wanna run on, db, and the time b/w the reqs

**Wait Group**

- the wait Group frm the std lib - sync.WaitGroup ..
- and the way the wg works is anytime we spawn a new go routine with in the context of the wg we do like `wg := &sync.WaitGroup .. and we can use this wg inside our feed loop, and iterate as how much as feed we ve.. and for every feed it adds one wg..
  wg.Add(1)
  go AddScrap(wg)
  wg.Wait() ... then finally we can create a fn and inside we can defer the wg.Done()

- then the final feature we wanna add to our rss feed agg is, we wana able to get our users the list of newest post from the feeds they re follwing

- the project in the workplace dir - go/workplace/fcc
