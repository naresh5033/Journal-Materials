## Rust examples create a chat s/m (coding tech)

- create a chat server with async tokio. the tokio has ton of features such as file io, n/w, subprocesses, time etc. refer the lib
- lets see, we will ve multiple tcp clients connected to the chat server when one client sends the message to the server the message will be broadcast to all the clients connected.
- we will set the tcp echo server and wait for the client (listener)to connect and once the client has connected, it will take any message the client sends and bounce it rigth back to the client.
- then we can destruct the tuple of socket and the addr from the listener.accept()
- ex - let (mut socket, addr) = listener.accept().await().unwrap()
- then create a buffered and then read it from the socket - let bytes_read= socket.read(&mut buffer).await().unwrap()
- then writes the buff..  socket.write_all(&buffer[..bytes_read]).await().unwrap()
- the wirte_all() doesn't mean we re writing to all the clients, it simply means write every single byte into the i/p buffer and out to the o/p buffer.
- for this read and write use the use AsyncReadExt and AsyncWriteExt from tokio io crate
- to make this support multiple messages at a time loop that right after the tcp client
- we can make use of the tokio's BufReader - let mut reader = tokio::BufReader::new(socket) -- the bufreader is very sim to the std lib's buf reader. this will allow us to do operations like read the entire line. so we can call the read_line() method in the socket.
- and we can split the read and write from the bufreader - let (reader, mut writer) = socket.split()
- now instead of passing socket to the bufreader we can pass reader and then for writer to write_all
 - after writing clear the line line.clear(). otherwise we will be appending the existing strin/line to the new line.


**Multiple client Independently**

- to handle multiple client independently, the std way in tokio is just like how we do the read and the write in the looop we can listen for the  client in the loop too. ex - loop {listener.accept().await.unwrap()}
- but even after this we can't do the async op we re still blocking (synchronously).
- coz we din't implement the "task" yet (currently we ve only one task (kinda like the main thread situation))  the task is the unit of work in the async world, we can think of it as a lightweight thread. and these are not OS threads 
- tokio::spawn(task: async move {}) -- rust has this async block which is basically wrappig up the one piece of code into its own future, into its own unit async work,

- rn, this is not great impl for fostering the community, its not a chat server/
- so basically we need to communicate the line which is currently read by one client, and make that out to read by every single client that is connected. so to do that we can use the channel (broad cast channel from tokio)
- so each client task will send lines that it reads (from its own client) and also it will receive lines that are read from the other client as soon as they written the lines.
- ex - let (tx, rx) = broadcast::channel::<String>(10) ; the 10 is the no.of items it can keep in its internal queues for the reciever.
- now to listen our channel after we read the line, - tx.send(line.clone()).unwrap() - before calling clone() we re tryna move the val tx into the loop we need to clone tx... let tx = tx.clone();
- and now we can receive items on the channel, create a rx by subscribe to the tx.. ex - let mut rx = tx.subscribe(); and now lets recieve items - let msg = rx.recv().await.unwrap();
  

**tokio::select!**

- is just like the go select, which basically allow us to run multiple things concurrently at the same time and act on the first one that comes back with result.
- the way it works is we give it identifier and the future, and the block of code. ex - tokeio::select! { _ = reader.read_line(&mut line) => {tx.send(line.clone()).unwrap; line.clear()}} .. literally move all the code into this select()


- rn we re echo the client msg and give it back to them

- so we now we re only sending the msg instead we can also send the client address that it was read from.
- and we need to pull back off of the n/w channel in our receiver - ex - result = rx.recv() =>{let (addr, other_addr) = result.unwrap();}

- turbo fish ::<> is a special kind of syntax/operator that's unique to rust, that tells the compiler what kind of generic type it expects to return from a fn. (so turbo fish is the disambiguating generic types that the compiler is not smart enough to figure out by itself)

- FQA - how do we know when to use tokio::spawn and when to use tokio::select! as we know we can do concurrency in these 2 way. one good rule of thumb is we can use the select! when we ve things that we need to operate on the shared state and we ve finite no.of things. 
- in our case we ve the reader and the writer which borrows the same piece of mem, or shared state.
- so the vey common pattern when using the select is that has five or 6 branches on it that all just gathering msgs from different channels, and then altering some shared state,
- if we re using tasks for this kind of ops independent task has to read off all those channels, all that kind of shared state need to be behind the atomics or mutexes things like that. 
- but select allow us to ve them in very natural and sensible way. and also one good thing  about select is that we kinda ve free locking and concurrency guarantees just by virtue of way that select works.
- another good feature with the select is, because it all happens on the same task we can easily the references b/w the other branches of select, makes mem mgmt lot easier , so we don't need to worry abt everything need to be owned or static and sync etc.
  
- and anytime we use the tokio::spawn() the thing that we pass is to ve to be `static(no stack ref in it) + send + Future.

- there are some additional changes we can make to our current implementation, one of them is what is our broadcast channel is not available to us.


# How Rust used in the production environment

lets see what are the big companies they use runst
- **npm**   
  - the npm's first rust program has not been caused any issues for like 1.5 yrs.
  - they are satisfied with the memory safety, dependency management, and performance guarantees.

-**figma**

  - they got improvements  in the sections like mem usage(lesser), cpu usage(lesser), peak average file service time and paek worst case save time. all of them icreased drastically.

- **AWS**
    -   they increasingly use the rust for the criically IS like filecracker VMM (its the AWS Lambda and AWS Fargate)

- **Discord**
  - they first implemented with the Go, but for every few seconds they noted the large latency spike that were not good for the user experience
  - they found that the spikes are caused due to Go's Garbage collector.


- **Drop Box**

    - they rewrite the sync engine (which is the s/w that syncs file and the local s/m)
  

# Axum 

- ```cargo watch -q -c -w src/ -x run ``` cargo watch quite, clear b/w each compile, watch only the src dir, then exec, and run.. 
- and to exec/run our test ```cargo watch -q -c -w src/ -x run "test -q quick_dev -- --nocapture ``` .. (quick_dev fn) nocapture sees println in console
- note if we ve a tuple struct type(in fn param) then we can destruct dirctly in the fn parama ex - fn hello_handler(Query(param): Query<HelloParams>) -> thi is how we destruct the tuple struct directly in the function parameter.

- **composition of Routers**
  - another cool feature of axum.. create a fn that handles multiple routes and then in the main() where we create the router instance merge this router fn
    - ex - fn mai() { let routes_all = Router::new().merge(fn hello_handle())}  -- wiith this merge kw we can merge the routers.
  - we can set the static routing and nest the root url to the local file and then after merge add the fallback_service() so when the req comes in our root it will fall back to the local url that we mention.

- note we can ve only one Body extractor per route.. ex- pub fn api_handle(payload: Json<T>) --this type Json from axum is  body extractor
- and to create a json body with the json! macro -- > let body =Json(json! ({"result": {"success": true}})

- the way we add a layer or middleware into our router is - .layer(middleware::map_response()) -- this mw module from tower has the map_response() function and we can pass in our mw fn.
- **cookies** to add the auth token, use the cookies from tower .. most of the mw are from the tower..
- and to add this in our router .layer(CookieManagerLayer::new()); 
- one thing to notice is that the layer() will exec from the bottom to top, mean if we want the cookie layer data in the other layer/mw then then the cookie layer has to be in the top/first
- and to create a cookie ex- cookies.add(Cookie::new("auth-token", "auth-secret"))
  - in this we can impl the real auth token generation / signature..

- the "State" type is from the extractor from axum that returns the tuple -> axum::extract::State, but at the application level. 
- .with_state() - will make sure all our route handlers will get the states.
- axum also has way to add multiple states..just create the struct and all the states as the fields and we can use this struct. and wrap it with the macro #[derive(clone, FromRef)] .. and make sure we enable the macro feature in axum (.toml file)
  - this FromRef macro will make every sub prop a sub state. that we can inject.


- .nest() will merge all the routes under the one route path ex .nest ("/api", )

**Middleware**

- lets impl the first mw - mw_require_auth.. in our case this mw will be authenticating just by checking the cookie
  - create a mw fn that will ve 3 params -- cookie, req, next and returns Ressult type..
  - and we can add this mw by .route_layer(middleware::from_fn()) .. inside this from_fn() pass in our mw fn..  - now the reason y we re using route_layer() instead of layer() is we want the mw to be applicable for only that particular route. ex - we  can impl the auth/login routes with the mw.

**Token Parsing**

- lets parse the token of format - user-[userid].[expiration].[signature] which will returns the (userid, expiration, signaure)

- we can use the regex package, lazy-regex. - allow us to lazy compile regex once and reuse it again
- and to create one use the regex_capture!() macro
- and in our main.rs we can parse the token

**Our First Extractor Ctx**

- lets ve our first extractor in the form or context.
- create the ctx.rs in the root dir so it can be used by web and the modal layer
- the Ctx struct rn has the user id, which can't be mutated, but later we will add some mut hash specially for the access control.
- in the mw we will impl the extractor.. for that we need async - trait lib,..  #[async_trait]
- now our extractor will takes the info from the header and  url params etc. which is what we want, we want to take the cookie from the header.
- our extractor will return the result which is like Ok(Ctx::new(user_id))
- the couple of other crates we wanna use uuid, strum_macro - have some utilities for enum and serialize thenm as str #[derive(strum_macros::AsRefStr)]
- the another serde crate - serde_with #[skip_serializing_none] - such as all of the options doesn't get serialized
----------------------------------------------------------------

# Surrel DB (rust based DB)

- the surreal db supports TiKV as the persistent layer.
- so it can scale horizontally to accommodate massive volumes. so what kind of DB is the surreal DB ? ig a 
  - GraphQL database, Document Db, Relational Database, KVP..
  - surreal db takes the best aspects of each database paradigm and combines them into a surreal database.
  - and it also supports REST endpoints and enduser Authentication right out of the box.
  - the foreign key in the sql is called record link in surreal db.
  - and the thing about the record is they always ve an id, id consist of 2 parts and are separated by colon : --> tableName : Id of the record.
  - ex - to set a val to the field - CREATE armor:plate SET resistance = 20; tableName : record id and then set one field called resistance.
  - The other thing about the surreal db is the tables are "schemaless" so we can set whatever arbitrary field we want.
  - **record Links** we ve something called recordLinks, which is actually a direct reference to the foreign table, its a mechanism for the database to completely eliminate the joins.

- if we do wana create a schema full table, we need to explicitly define (like the fields and we want all the fields to ve) ex - DEFINE TABLE armor schemafull; and for the field - DEFINE FIELD resistance on armor type int;
- one of the cool things about the record link is it will pull data from multiple table at once.
- (if we do this in the postgres then we ve to join 2 fields on 2 diff tables ) ex -SELECT * FROM Player JOIN armor ON armor.id = player.id;
- but in surreal it will show the record link(id) to the armor table in the player and to fetcht the content of the armor table - `SELECT * FROM player fetch armor;` now this will fetch the content of the armor table.
- if we compare the both query this surreal query is more concise than the sql joins
- we can also use the where clause, aggregation fn, ex SELECT math::sum(strength) FROM player GROUP BY ALL;

- **Graph Connection**
  - what if we wana establish a relation b/w 2 diff records, and to be ve a 2 way relation and that can be queried in either direction.
  - for that that is **Graph edges** 
    - lets see an ex to establish a relationship b/w the player to the armor... `RELATE player:leroy->wants_to_buy_armor:dragon;`
    - so if wana list all the players along with the armor they wanna by, `SELECT id, ->wants_to_buy->armor as wtb FROM player;` .. this -> operator(arrow) all the records with ref to the edge table 
    - then we can query in other direction given the piece of armor what player wants to buy the armor .. `SELECT id, <-wants_to_buy<-player as players from armor:dragon;`

**Events**
  - the events are like the triggers from the traditinal db, but combined with the surreal db aim to support the websocket to give the consumers real time updates, they can be very powerful.
  - lets add a level to the player table - `UPDATE player SET level = 1;`
  - now lets say we wanna create a record every time the player changes the level ex . `DEFINE EVENT lvl_up on TABLE player WHEN $before.level < $after.level THEN (CREATE lvl_up SET time = time::now(), level = $after.level);`