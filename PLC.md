# PLC

## TwinCat - 3 (Beckhoff)

- The windows Control and automated technology - is pc based control.
- the twincat kernel ve direct access to all the h/w for all the real time critical tasks and using windows for all the user-space applications.
- in 2012 twincat 3 was released and they integrated both the developement env and the functionality of s/m mgr into visual studio
- to be able to do plc s/w in the visual studio, beckhoff relies on the compiler made by **CODESYS** - the codesys even provided the runtime for the Raspberry pi.
- in the IEC standard there are 5 programming languages defined.

1. Sequential functional chart (SFC)
2. Ladder Diagram (LD)
3. Functional Blcok Diagram ( FBD)
4. Instructional List (IL)
5. Structured Text (ST)

- this tutorial will only cover the Structured Text (ST), High Level language.
- b4 the plc the industrial automation uses relays to control the s/m, which require large amounts of ralays and space, a small change in the s/m will require rewiring and single wiring breakage could cause problems in the whole s/m.

- Twincat consist of 2 main parts
- 1. XAE and XAR
- **XAE** - extended Automation Engineering - which is the dev env for the twincat includes many libraries and compiler itself. (always runs in only windows)
- **XAR** - Extended Automation Runtime - provides twincat realtime kernel, real time n/w drivers.
- these XAR and XAR communicates each other with the protocol called **ADS**
- through the field bus interfaces twincat can communicate with various sensors such as temperature, position, and pressure sensors, and it can also actuate actuators such as relays, valves, and electrical motors.
- through the XAE we can create the .exe file and then transfer it to the XAR and run/exec it. so it can run in the real time env of twincat 3.
- just download the XAE which also includes the XAR
- **Scope SErver** - have a look into this, - we can plot vars in the time in X and Y graph and we can see how the data is developing over the period of time. can be useful especially in a s/m where we want to understand some behaviors with regards to motion.
- since the XAR is included with the XAE means we can control the Twincat state in our local s/m

### Part 3

- since twincat is running in the kernel mode, it can ve the access to the i/o, fieldbusses (EtherCAT, EtherNet/IP, profinet IRT), motion control.
- and also allow us to scheduling and parallel execution of the tasks.
- the task can do things like run the plc programs, do motion controls, or interface with the EtherCAT i/o bus.
- in the case of shared core, twincat utilize double-tick method, in where switching to real time mode - the tick and then switching back to user mode - double-tick is triggered by an interrupt.
- **Base Time** - is the time frame for the constant interrupt, where the twincat scheduler pauses windows and provide the time twin cat real time tasks.
- ex - if the base time is set to 1ms and the cycle time is set to 10ms, then the twincat task will only be exec once every 10 ticks.
  - the twincat runtime wakes up every 1ms and checks if needs to execute any tasks, the cycle time is always the multiplication of the base time.
- if set the upper limit to 90% then the real time task will get up to 90% of the time and the os will get the remaining 10% of the base time.

- if we use the twincat dedicated core which is also called as the isolated, which don't ve the double tick which gives back to the os, we only ve the first tick. if we ve 2 + we can do core isolation.
- Running twincat on the isolated core is the key advantage of the twincat.

- **ADSLOGSTR()** - this fn has 3 args msgcontrolmask, msg formatter str, str arg. it can be used to display the error, log, warining.
- once we complete the code we can select the activate config, which will compile it and put it on the target device. in our case the target is just the localhost.

### part 4

- **Dtypes** - bool, int, uint -2b, sint(short int) - 1b, usint, Dint(double int) - 4b, UDint, Lint - 8b, ULint, Byte, word - 2b, Dword - 4b, Lword - 8b, REAL (float) - 4b, LREAL (long real) - 8b, string - 80b (for data) + 1b for null terminator, string(300), Wstring (wide string) - 80 (+1b) words and 160 (+2b) bytes - this wide str uses unicode(utf -8) as opposed to ascii characters in the string, WString (300) - 300(+1b) words and 600(+2b) bytes
- Time(for the date and time) - 4b, TIME_OF_DAY(TOD) - 4b, Date - 4b, DATE_AND_TIME(DT) - 4b, LTIME - 8b (also support ns)
  - for the string we ve to declare the val with single quote (like char)and for the wstring we ve to declare the val with double quote (like the string)
  - the diff between the unicode and the ascii characters is that unicode has the much wider variety of char selection.
  - the wstring can be used in the front end hmi, and its not really used in the plc business logic.
- **Enum** TYPE myEnum : (1, a); END_TYPE

- **Pointers** - (are 8b) holds the address of the mem location to the var it holding to ex - pPointer: POINTER TO INT := ADR(nVariable), and to update the var (aka Using the **deref**) - nPointer^ := 30; using the the caps we can deref the pointer.
- **References** - is the ref to the obj, ex to assign the ref - nReferences : REFERENCE TO INT := nVariable;
- **Note -** when using the pointers we ve to be careful, since the twincat doesn't ve the smart pointers so its better to use the reference for most cases.
- **Arrays** - ex - nArrays: ARRAY[1..5] OF INT := [1,2,3,4,5]; then we can replace or access the array thru the index
  - for the 2d array - nArrays: ARRAY[1..5] OF Array[1..3] OF REAL; or alternatively we can also do nArrays: ARRAY[1..5, 1..3] OF INT;
- **Type Conversion** - we can do the type conv for every primitive,

### Part 4 functions and structures

- the structure is same as the struct to holding the data of diff types. to define the struct ex - Type st_event: STRUCT allvars: dtypes END_STRUCT END_TYPE;
- the struct is used to represent the records or logical groups. now to use our struct in the var ex - VAR allEvents: Array[1..100] of st_event; END_VAR

- Functions - ex - `Function celsiusToFar : REAL
                   VAR_INPUT: celsius: REAL; 
                   END_VAR
                   celsiusToFar :=  celsius * 1.8 +32;  ` - is the return stmt - inside that we can also define the var for the function scoped just like the var i/p param.
- Functions can also ve the i/p and the o/p vars (params and the return are of same type) - in that case we can use VAR_IN_OUT(inside we can declare our params) END_VAR_IN_OUT - means we can do both read and write from the same var.

- **Pass By val and pass by ref -** we can pass the var (params) either by reference or val.
  - just like the same concept as the c++ pass by val - copy problem.
  - and pass by ref avoids the copying problem, means we re dealing with the underlying data.
    - the pass by ref we can achieve by using the VAR_IN_OUT kw or we can also use the VAR_INPUT stEvent: REFERENCE TO ST_Event; END_VAR

### function block and interface

- with the function block we can think of going to the procedural type programming to the object oriented programming
- as we know the objs ve the **states and the behavior**, the objs can talk to each other by sending messages via its methods.
  - we can think of data/**var** (are the states) and the behaviors (are the **methods**).
- the function block is the blueprint of the obj, and we ve to create the instance of the obj(just like creating any var just assign to it).
- once we created our function block we can select (right click) and create the method block
- The function block can hold/store the state over multiple plc cycles in their local var.
- while the function doesn't retain their state of local variables b/w 2 diff calls of it.
- according to him, he generally uses the body of the function block for the cyclic calls, that needs to be made in every plc cycle.
- and methods - when there are one shot call for a req or message to change the state.

- **Inheritance** - its possible to do multiple inheritance but the function block can only inherit from exactly one function block.
- **EtherCAt**- in twincat we generally work with ethercat, ie ethernet on steroids, is basically the deterministic version of ethernet.
  - every type of terminal to get the sensor data or actuate something, that we might want to communicate with something called **ethercat slave** controller inside of it.
  - these slaves provides some basic diagnostics.
- **Extends kw** - to extend the function block we can use this extends kw.
- as we know the interfaces are the one of the ways to achieve **polymorphism**.
- the interfaces are the blueprints of the function block. just we define the interface and then implements the interface in the function block.
- we can think of fn blocks as the class - it can inherit/extends only one fn block but many interfaces.
- **DI** - also uses the DI for the dependencies, can be achieved by using the **FB_INIT** method, can be called whenever an obj is instantiated.
  - so iniside the FB_init function block we can add our interface that we want to instantiate in the var.
  - and this fb_init method, we can think of like the ctor.

### part 7 instructions

- used to control the flow of our prog.
- if else, switch, for loop, while loop.
- in the for loop we can also optionally use the step size using the **By kw** ex - FOR nCounter := 1 TO 5 BY 2 DO;
- **State Machines** - are very common in the plc programming, we can achieve this by using the **Enums** as we know the enums are the named numerical constants that helps us to keep track of the app state.

### part 8 TC2_standard

- some of the TC2 standard functions are 1. TON - Timer On Delay, 2. TOF - Timer Off Delay, 3. R_TRIG - Detection of Rising Edge, 4. F_TRIG - Detection of falling edge, 5. Concat

1. TON - can be useful when we want something to happen during the period of time and wait b4 apply an action.
   - it has IN - timer with the rising edge, we also use the IN to reset the timer with the falling edge.
   - ET - elapsed time, display the amt of the time since the start of the timer.
   - Q - is o/p and it set to true after the timer has set to elapsed ET
   - pt - i/p(time to pass) which is also the time
2. TOF - its basically the inverse of TON, in TON the ET timer elapsing when the in is high, but here the Timer elapsing when the in is low, so in other words the falling edge is a trigger for the fnblock.
   - and the other diff is that once the timer has elapsed the O/p Q goes low as opposed to high in the TON.
3. R- Trig - when we want to detect the ricing edge, it has 2 params 1. clk(i/p) or signal to detect, and 2.Q(o/p) - or edge detect (both are bool).
   - Each time the fn block is called the q will return false until the clk has been **false** followed by a rising edge. in other words the val true.
4. F_Trig - is same as the R-Trig, but it detects the falling edge instead of the rising edge.
   - Each time the fn block is called the q will return false until the clk has been **true** followed by a **falling edge**. in other words the val false.
   - in both of them we just use the bool (clk) in the fn block to trigger.
5. Concat - the concat str only works with the str upto 255 chars, means that each i/p str can only be 255 chars and the resulting o/p str is also 255 chars.
6. Concat_To - can take the str of arbitrary len and concat em.

### Part 9 (twincat utility libs)

- its in the TC_2 utilities lib. ex of some of the fn in this lib are OS, PLC, convertion, S/m, str, others.
- lets see some of the fns.
- 1. FB_MemRingBuffer - which allows the data records of varying lens to be written into a ring buffer.
  - follows FIFO, the oldest entry/record in the ring buffer are the 1st one to read.
  - when we instantiate this fb, we will ve the read and the write ptr, both pointing to the 1st position in the buffer, these pointers moves f/w as the val comes in and once the read is done the val will be removed from the buff.

2.  Profiler (from plc) - which allows to measure the exec time of the plc code.

- it is useful when we want to understand the part of our code takes more time to exec. it has 2 i/p - start and reset (both are bool), and 2 o/p busy and data (profiler struct)
- the fb takes the last 10 measurements and calc the mean val and provides that in the o/p

3.  NT_startProcess (from OS) - can be useful to start the windows app from the plc.
    - can be useful to start the windows app in plc, for the params check the doc, there are like 9 params
    - such as NetId (aka ip addr) - we can exec the windows app on the another plc

### Part 10 (IO)

- IO - is about when interacting with the sensors - such as the gas, flow meter, leakage detector, temp, movement detector, distance sensor, humidity etc.
- and the actuators such as coveyor belt, motors in general, heaters, coolers, laser, pumps, robots.
- now the plc to interact with all those, it needs **field bus** - is a name for the industrial computer n/w used for the real time distributed control.
- **field busses** are diff types and can be implemented by many ways. some of the popular ones are Profinet, Ethercat, Ethernet/ip, Modbus, CAN open, ethernet powerlink, CC link.
- **Ethercat** is the defacto std for the beckhoff plc.
- the ethercat n/w consist of one Master and one or many slaves, the master is responsible for sending out the ethercat telegrams to each slaves, the slaves reads the data and also transmit its own data back to master
- the master can be implemented by any std pc, and the slave require a special h/w chip called **ethercat slave controller**
- **IO Link** - is the protocol to communicate with the sensors, this can be act as the ethercat slave and the io link master

- for defining the I/p, we ve to use VAR bInpu AT %I* : ARRAY[1..4] OF BOOL; lly for the output %Q*
- To run our prog in directly in the plc (instead of vm) we need to ve the **AMS connection** so we can talk ADS with the plc
- to add the plc -> router -> edit route and add the ip addr. and then select the plc as the target

### part 11 libs / Twincat function

- sometimes it requires us to extend the functionality of the Twincat, this is achieved thru beckhoff functions.
- beckhoff has the complete ecos/m of fns to solve various problems.. these fns are divided into couple of diff categories. and each category has bunch of fns.
- **TF1 series** - TC3 s/m - here we ve extension to the normal plc runtime. such as the extns to run models created in matlab or simlink or editors for creating plc s/w in uml.
- **TF2 Series HMI** - operator pannel of our tc s/w, with this we can use the frontend using modern tech such as html, js, css.
- **TF3 TC3 measurements** - includes tools for analytics, power measurements and **scope**. these tools are very handy when we use the both short and the long term analysis of our process.
- **TF4 Controller** this includes all the controllers that we might need such as the PID controller, and various filters.
  - in this we also have the TC speech which we will use the voice to send the to our plc.
- **TF5 Motion Controll** - is huge category, anything related to the motion we will find it here.
- **TF6 Connectivity** - whenever our plc wants to connect to the outside of the world.
- **TF 7 Vision** - with this we can integrate the camera into the TC when we want to use it for the Quality control or inspection.
- **TF 8 industry specific** - here we will find the fns that are specific for the industry, such as the building automation and the frameworks for the wind turbines.

---

**TF6 Connectivity** - uses the json format for the Connectivity, used when we want to communicate b/w the tc and the external app - **opc ua** also used for the data exchange b/w s/ms - **modbus Tcp** - - **TC3 Profinet RT device** - enable our plc to talk profinet(is a field bus developed by the siemens). - **TC3 FTP Client** enables our tc to talk to the ftp, to upload and download files.

- as we konw the plc(XAR) can be split into 2 parts the OS and the TC-3 kernel.
- **Modbus** - is extremely old protocol, we don't ve the luxury like the newer ones.

### part 13 version control, part 14 diff tc versions

- the version control has to be done for the every automation s/w.
- so far we ve seen the xae and the xar there is a 3rd comp that we can use from the beckoff, ie **tc3 remote manager**.
  - this remote mgr will include the libs and the compiler that we need to run the specific version of the tc.

### Part 15 ADS (Automation Device Specification)

- this is beckhoff's interface for communication b/w the s/w modules in tc based on the client and server arch.
- whenever we do the online login to read and write the vars, ADS is used in the background for the communication from the XAE to the xar.
- its also been used internally by tc to communicate b/w diff tc modules, so its also been used just by running the tc scheduler.
- whenever we use the xae to communicate with the target dev(plc) to read the vars thru online, ADS is used.
  - or even iot to plc(beckhoff)
- it can even exchange the data b/w 2 tc runtimes.
- due to this ADS TC has unlock the possibility to talk to and interact with beckhoff plc and TC runtime.
- its a client - server based protocol, where the connection is always initialized by the client.
- its both the cyclic and the event based (sub)
- ADS uses port 8016 and 48898 for tcp communication and port 48899 for the udp communication.
- when we re communicating the plc the data will pass thru the ADS msg router, its the msg router that we can define the communication to the diff channels to diff tc devices.
- it has AMS netId - looks like ip (but its longer 8 field), and it can be changed at any time. and it has the separate port for ex port 300 for I/O.
- it has the command id - think of it as a http status code. ex cmd id : 0x0009 for ADS read / write.
- it also has the error code and the invoke id.
- **pyads** a open source lib, its high performance.
- The better way to read the var is via Access Symbolic data by instance/symbol path (dotnet code refer the beckhoff documentation) - its something like the creating a new ads instance and then reading the val.
- and he even uses the c# program.cs in the xae to create the code

- **ADS with the linux using C++** - just clone the beckhoff ADS git lib
- he emphasize more about This ADS lib, and its fun to do stuff with it.

### Part 16 TC -3 Automation Interface

- some times we need to automate some process in order to save the time and the quality of the delivery.
- when working with the development of the tc s/w there are many different steps involved (even for the basic s/w and deploy it on to the target device). such as

  1.  Defining the Real Time Properties
  2.  writing the unit tests
  3.  writing of s/w ( creation of PoU and business logic)
  4.  I/O (creating io and linking em to the instances of the pou's)
  5.  lib management
  6.  Target configuration (including setting up ip addresses, setting the opca server etc.)
  7.  creating the AMS route to the target device.
  8.  selecting the target for the deployment of the s/w.
  9.  Activating the configuration on the target

- now if we re working on a large no.of plc and we don't ve to repeat these tasks manually on every single one of em.
- so we can make use of the tc automation.
- some of the things we will do in the tc Xae are

  - build and clean the project
  - activate the config
  - create AMS route
  - activate the broadcast route
  - config the real time settings
  - Add/remove the task
  - add/remove IO
  - running the static code analysis
  - selecting target device
  - management of POUs
  - management of libs
  - and much more

- we can do automation for em, the tc provides bindings for various prog langs, (so we can do the automation in various prog langs)
- for this Automation we need Visual Studio(DTE) and the tc automation interface.
