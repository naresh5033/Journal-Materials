### Protocol Buffer (protobuf)

- its a message format for the Grpc
- the protobuff are the repersentation of structured data (unlike the json), and the file extension is .proto
- the proto file is just a file, with the schema defn about our msgs and our structured data. 
- **ProtoC** is the compiler of the protobuf, we feed it the prot file and we feed it what lang the o/p has. and its gon o/p the equivalent language.
- we ve to download the protoc executable file and then in the cmd prmpt the downloaded file location and run this cmd ex - `/User/nareshxo/Downloads/protoc../protoc --js_out=import_style=commonjs,binary:. employees.proto` 
- this command will gives the output js file of our employees.proto file 
- and then we can install google proto buff `npm i google-protobuf` is the runtime google needs
- then we can write the js script file to this gen employeee_pb.js file and add the set fns to the fields for our employeee, and create multiple instances of the employees[]
- and to convert this into the binary/ to serialize this  ex - employees.serializeBinary().. and lly we can use the deserializeBinary() function
- and if we write this fs.writeFileSync() in our local disk the file size will be 52b as compared to the json file which is like 125b 

- one of the benefits of protobuf is having the schema. and its compact, smaller footprint.
- the cons is its a lota process, when sometimes we build a smaller app, we don't even need a schema, (no point of having it for the small app)

# GRPC 

- Google Remote Procedure Call, is a open source HTTP/2 RPC protocol. i
- it uses http2 as a underlying mechanism and uses proto buff for the message
- why is grpc ? 
- "client server communication"
    - SOAP, REST, GRAPHQL, SSE, WebSocket and Raw TcP
    - now the problem with the client lib is any communication protocol needs client lib for the lang of choice (for ex - if we wanna build a REST api our cliet lib is the HTTP lib)
    - Hard to maintain and patch the libs 
    - so in the client side everything is taking care of the browser, but what if we re not on the browser and on the backend.

- that's where grpc comes in, its a client lib and one lib for all the popular languages. (as we wanna unified the client library)
- since its uses the protocol buff as the message, and they are language agnostic, (we can use it with any popular languages) lang neutral.

### GRPC Modes

- There are different modes(of communication) for the Grpc such as 
  1. Unary RPC 2. Server Streaming Rpc 3. Client streaming Rpc 4. Bidirectional Streaming Rpc

1. Unary RPC 
   - where the client make one req to the server and then the server responds

2. Server Streaming Rpc
   - where the client makes one req to the server(and it expects alot of data from the svr) and the server res with the streaming of data. (ex - youtube )

3. Client streaming Rpc
    - it the opposite of server streaming, where the client is constantly sending the information, ex - if we re asynchronously uploading a huge file. or streaming a video or game.

4. Bidirectional Streaming Rpc
    - both side sends the streaming of data and receives the streaming.


- **Ex - ToDo App**

- write the schema, for the protobuf and then install the packages -- `npm i grpc @grpc/proto-loader`
- refer the code [here](https://github.com/hnasr/javascript_playground/tree/master/grpc-demo)


- **Pro and Cons**

- the pros are its Fast, compact, one client lib, progress feedback (upload), cancel req (Http2), http2 protobuf
- the cons are Schema, thick client, proxies, still young(support), error handling, no native browser support, timeout (pub/sub).
- 