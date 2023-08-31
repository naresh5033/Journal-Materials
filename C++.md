# C++ (FCC)

- some of the online compilers out there are Wandbox, compiler explorer, coliru
- Reading the data with the spaces, if the user types the name with spaces as we know the space will break the exe flow and it breaks/end to fix this - std::getline(std::cin, full_name).. its like fgets() in c.
- sim to the std::cout, cin, clog, cerr..

- execution and the memory model - just how the prog follows the mem addr, for exec the var, fns or statement in the flow
- core lang vs std lib, vs STL .. the core is the core of the lang like how to ( defn vars, fns, etc ), the std is the std library features we can use ex - stdlib, stdio, string, etc
- STL are the collection of container types, that will allow us to store the collections, are the specialized part of the std library..
- the mathematical formula for digits and the data range is n = 0~2^n-1 .. ex 2 = 0~2^2-1
- the hexa decimal id the group of 4 binary bits - so in total 16 bits and it starts with 0x
- if the num doesn't fit in the group of 4, we gon end up with the group on the left and then starts the next group with the missing piece as 0s - this is called **padding**
- **octoSystem** divides our binary nums into the group of 3,

- **numbers/m** to use the num s/m in cpp.. 10 - decimal, 015; octal; 0x10.. hexa; 0b00010 binary;
- the signed int range is [0~2^n-1] and the unsigned int range is [-2^n-1 ~ 2^n-1 - 1] so the vals are range of [0 to 4 billion] and [-2.147b to 2billion]
- lly short - 2bytes, long - 4 or 6 bytes, and the long long is - 8 bytes
- and the modifiers kw are **signed int, unsigned int**

- **fractional nums or floating points**

  - float - size 4, precision 7 ,
  - double - size 8 , precision - 15,
  - long double - size 12, precision > double,

- std::setPrecision(15), to control the precision. and this is from the **iomanip** lib
- if the float num is divided by 0 - is infinity;
- lly 0.0/0.0 = NaN - 2 float points
- and where we re using float, long its good to use the suffix ex - float num = 1.2223f; (if not with the suffix then the num will be consider as the double/ saved as double in the mem)

- "boolean" occupies 8bits in mem, use the sizeof() we can see.
- the bool will returns the 0 or 1 by default 1 - true, 0 - false; and to see the bool vals we can use - std::boolalpha;
- the char takes 1 bytes in mem. and if we save a numeric val in char it will gives the asci val of the 65 ex - char num = 65; the asci val is A

- **auto** kw - allows the compiler to deduce the type..comes in handy when we ve really hard type names, that are hard to type and let the compiler decides ex auto var {12.0f} or auto var {123u} - unsigned int
- "arithmetic ops" with the div we won't be getting the fractional val ex - 31/10 - 3 the missing fr val..
- "precedence" - tells us which operation we ve to do first
- "assosiativity" - tells us which direction or which order. (left / right) ..there is a cpp precedence table we can refer - its mostly like Bodmas
- **compound operators** ex - value +=3; or value %= 3;
- refer the io/manipulator lib and we can see the list of available fns in the .. std::flush - when the buff is full then the data will go to the terminal, setw() set the width for the text. setfill('-') will fill the specified chars in the empty fields, showpos() to show the +ve sign, showbase() to show the base in our data
- the std cmath lib has some math operations we can perform.. ex ceil(), floor(), abs()

- **weird integral types**

  - the integral types less that 1 byte are not support the arithmetic ops. ex char (1 byte) and short int - 2 bytes, but if we do the arithmetic operations on these the compiler will convert em in to the int type and do the opertions
  - the **switch** statements doesn't work with the string types, it works with enum or integral types
  - **size_t** kw used to represent some unsigned int for positive nums [sizes] - is 8 byte

- range based for loops - we can use this with arrays, when using this we don't ve to use the iterater, test and increment ex - for(auto value : num){} - or for (int elements : num){}
- if we the array size is gt the elems then the remaining places in the array will be occupied by 0s

- to get the size of the array int count{std::size(nums)}, this can array size can be helpful in loop thru the array of dynamic size.

  - b4 that size() they use the sizeof() to find the array size and then the sizeof() single elem in the array then divided them both..
  - **null terminator \0** if we ve the char array then its good prctice to use the null terminator at the end of the array ex char num[]{'e', 'r', '\0'}; otherwise some garbage vals will store in the unalloc mem.
  - also if we ve the spcified size array char num[4]{'e', 'r'} then the compiler will add the null terminator at the end of the array automatically,
  - if the char array is null terminated then it is called c string (since its comming from c lang)

- using **string literals** when we intiliaze the char array its best practice to initialize it with the double quotes like the str, so the compiler will add the null terminator at the end of the array, ex - char num[]{"Hello"};
  -note: and we can only print(the vals of) the char array directly, if we tryna print the int array directly then instead of val/elem we will get the mem addr of the array.
- **array bounds** when we tryna access the elem out of the array (outside the bounds of the array), but if we try to access/assign the elem of some crazy big num lets say 10million th idx the compiler will still allow but our prog may **crash**

### pointers

- with the pointers we can store mem addr, or in other words the pointers are special vars to store the mem addr.. we can't store anything other than address in the pointer variable.
- all the pointers are of same size(8 bytes), since everything saves only the address.
- and pointers only stores the type which it was declared. ex - int \*ptr; now this can only store the type of the addr which hold the int vals
- the preferred way to initialize a ptr is int \*ptr {}; this is called pointer with null ptr 0r initialized a ptr with null ptr. it is knwn as implicit null pointer
- we can also explicitly initialized with null pointer ex - int\* ptr{nullptr};
- note : the location of the pointer is does not matter we can put it on left right or middle. ex int _ptr or int_ ptr or int \* ptr

- "**virtual memory** " - each prog is abstract to the processor and each processor has access to the mem range of 0 ~(2^N) -1, where N is the os bit 32 or 64, so for 64 bit the access to the 0 to (2^64) - 1; t
- the mem map is a std format defined by the os. all the prog written for that os must control to it

- **stack** - mem is finite. the dev isn't control of the mem lifetime. life time is controlled by the scope mechanism
- **Heap** - mem is finit the dev is in full control of when mem is allocated and released. lifetime is controlled explicitly thru the new and delete operations.

- **releasing and resetting** - we use the delete kw in c++. to delete the pointer val. ex delete ptr;

  - and it is really bad to call the delet kw on a pointer twice. otherwise we will get crashed..

- when initializing the pointer w/o the null terminator, it will ve junk mem, so if we tryna assign val to that ptr then the pro will crash during runtime, ex int *ptr; *ptr = 5; //could be any junk vals in that mem addr and .

  - and lly we can't assign to the ptr with the null terminator ex int *ptr {}; *ptr = 5; //could be any junk vals in that mem addr. and it will crash during runtime.

- and the best way to this is **dynamically** assign the heap mem and then assign the pointer val

  - ex - int *ptr{}; p = new int; *ptr = 5; -- this way we assign the val new int (instances) in the heap mem, and then we can assign the val to the pointer.
  - and then finally we can use the **delete** kw to release the memory..
  - and again once we release the memory, again if we try to assign any val to the ptr the pro will crash.
  - **note** the important thing is once we delete the pointer we need to assign it to the null ptr e - delete ptr; ptr = nullptr; // by doing this way our code is much safer to use

- once we done deleting we can **reuse the pointer** - ex - ptr = new int(34);

### Dangling pointers

- the ptrs point to the invalid mem addr .. there are 3 ways the dangling ptrs can happens

  1.  uninitialized pointer
  2.  deledted pointer
  3.  multiple pointers pointing to the same mem addr.

- these are the 3 problems in the dangling pointers, and to solve em

  1. initialize the pointer (if we don't konw what to initialize with put the null pointer and then later put the mem)
  2. reset the pointers after deletion (put null ptr)
  3. for multiple pointers to the same addr, make sure the owner pointer is clear.

- "**when new fails**" - what happens when assign the dynamic mem fails ex - int \*ptr{new int(3)};
  - in some case the new operator will fails to dynamically allocate the mem, in that case there is no mechanism to handle the failure .. prog crash (although it is very rare case)
- we can handle this by **thru exception mechanism** (by try/ catch block)
- the std::nothrow setting - this will not throw an exception, and it will return the null pointer. ex - int \*ptr {new(std::nothrow) int[10000]};

- **null pointer safety**

  - make sure when working with the pointer it contains the valid addr. we can check this by some condn ex - if (!(ptr === nullptr)) {}

- **Memory Leaks**

  - when we loose access to the memory, ie dynamically allocate.. (basically we loose the pointer that was allocated to the dynamic mem)
  - ex - int \*ptr{new int(3)}; int num{5}; ptr = &num; as soon as we asssing the ptr to the num var we lost the control to the dynamically allocated mem(new int(3)).. leads to the mem leaks..

    - another ex - in a nested scope - { int \*ptr {new int(3)}; } - this dynamically allocated mem belongs to that scope .. and we can't access the ptr outside of the scope..hence the mem is leaked . so its better to delete the ptr when the scope ends.

### Dynamic arrays

- the normal way of defining arrays are called static arrays (and they are store in the stack).
- same like the int.. initialize the arrays with the null ptr . ex - int *ptr{int [size]{}} or we can also create a dynamica array with nothrow ex - int *ptr{new std::nothrow int [size]{}};
- **releasing the mem** - in version ex - delete[] ptr; ptr = nullptr;
- some of the take away are pointers initialized with the arrays are diff, ex - std::size won't work, and they don't support range based for loop.

### References (&) aka the address

- is an alias var that we can use to reference an original var.. ex - int num1{5}; int &num2 {int num} // this is thru initialization, we can also make it thru assignment ex int &num2 = int num1;
- now that we ve the ref, if we make the changes then it will be reflected in the original vars.

- **Comparing Ref to pointers**
- lets see some diff b/w em

  | References                         | Pointers                                            |
  | ---------------------------------- | --------------------------------------------------- |
  | don't use deref for reading        | must go thru deref for reading/write to pointed val |
  | can't be change to ref of          | can be changing to point somewhere else             |
  | something else                     |
  | must be initialized at declaration | can be declared uninitialized initialized           |

- the **const** ptrs are can't be used to point somewhere else once we assigned ex - const _ptr {&num}; ptr = &num2; // can't do this throws error.. or even we can't assign val ex const int_ const ptr{&num}; \*const ptr = 5; // error
- and the **const ref** we can't modify once we assign the const ref ex - int age(4); const int &ageref {age}; now we can't assign a val to this ref - ex ageref = 3; // throws error..

### character or string manipulation

- **c strings** are not safe or convenient to work with cpp.
- when we re using this std::sting to create val then it will assign in the mem/ initial mem(as usual), but unlike the char the initial mem will return to the os, so even if we reassign any str vars it will be in a new mem,
- so by using this std::sting the mem allocated will not be wasted out.
- instead we must use std::string and iostream, cctype.h (this header was the part of null terminated byte str libs) - refer the doc for the fns the lib has.
- **sizeof()** when we re using the sizeof() to find the size of the arr string, note that it also includes the null chars. so instead we can use strlen(), it won't include the null chars.

- **<cstring>** - lib contains many useful fn
- **one definition Rule**
  - the defn can't show up more than once in our entire prog or translation units ..
  - and the context are 1. free standing vars 2, fns 3. classes 4, class mem fns 5, class static mem vars
  - ex - vars with same name and type, lly for the fns or class with same nanes

### Functions

- reusable code, takes i/p gives o/p..
- and the **fn signature** is the fn name and the params. and this fn signature must be unique throughout our program. (return type doesnt matters)
- the fns must be declared b4 the main()..

- **fn declration and fn defn**

  - sometimes its more flexible to split the fn into header and body and keep the code for each in different places. and the "declaration" is the header part of our body.
  - or the other way of doing is just like the modules put all the declaration in one file ex "dec.h" and then include this in our main prog. this is called include directive..
  - the fn declaration is what we call in front of the main fn. and some times it is also called as **prototype**
  - the fn declaration doesn't care about the params (as far as the params type is there it still works)
  - and the defining/imp is on below the main
  - if we put in front we don't need to use the fn declaration, bts cpp will create a copy (for the fn declaration), but it is hard to readable for the others / tedious

- **compilation model** - has 3 stages, 1. prreprocessing - the source file, fetch the imports and replace the contents are called as **translation units**

  - 2. compilation - the translation units(TU) will be compiled by the compiler, and gen the **object files**, binary representation of the content in our TU.
  - 3. Linking - then the obj files will be processed by **linkers** and it will gen one sigle Binary flie (out of all the obj files)
  - we will get the linker error when the compiler can't find our fn definition file (ex "ld" in gcc linker error)
  - and finally this bin file we can run on our OS.

- for include the std lib use angle bracket for the other imports use "" quotes

### Pass by val, ref, ptr

- **Pass by val** when we call the fn inside the main() we assign the var and we use em as args this is diff from the param that we define in the fn, and it will ve scope only inside the main().. and what ever var we used in the fn body will be just the copy of the vars in the main().
- **pass by pointer**

  - this is allow us to avoid the copies that we re experiencing in the passing the params by val..

  - just declare the fn param as the pointer and while calling the fn inside the main() we assign the mem addr to the vars ex - int main(){ int age{22}; my_age(&age)} // now the var was passed as the ref to that pointer (param)

- **pass by reference**

  - this is another way to avoid the pass by ref (avoid the copies)
  - and this is much cleaner than the pass by pointer
  - ex - void say_age(int &age){}; then in the main() -- int main(){int age{20}; say_age(age)}

- so in summary we should not pass by val or lly return by value. better to use the other approaches..

### function overloading

- multiple fns with the same names but with diff params.
  - but the return type doesn't matter. the cpp will not allow us to create fn with same name and same params, as long as the params are diff, (the return type doesn't matters)
- the fn overloading is a cool way in cpp to make our fn easier.. (so in that way we re letting the compiler to pick which one is suited based on the args we passed)
  - params differences - we can make a param dff based on the 1. order 2. number 3. types

### Lambda functions

- are anonymous functions.. and the fn signature looks is - [capturelist] (parameters) -> return type {fn bdy}; ... the return type is optional
- [] () -> {}(); and to call this fn use paranthesis after the body. we can call it this way if we don't wanna call it again..
  - if we wanna use it multiple times then we can assign it to a var ex - var func = [] () {}; then we can call the func() many times..
  - the capture list is for the vars that re declared outside of the lambda fn.. since the var dec outside the lamda we can't access em directly inside the fn body.
- "capture mechanism" - there are diff ways we can use to capture ..

  1. capturing by val - ex int c{3}; [c] () {}; (and this has the copy issue just like the pass by val)
  2. capturing by ref - ex [&c] () {}

- **capturing all in the context**
  - capture everything outside the context of the lambda function.. and the syntax is **[=]** this equal sign inside the capture list will capture everything.. but this syntax is only for **capturing all by val**
  - we can also **capture all by ref** and the syntax is **[&]**

### Function Templates (aka Generics)

- are blueprint for fns.. this will solve the code repeatation.. ex- when we ve multiple overload fns
- the syntax is template<typename T> T maximum(T a, T b) {return (a > b) ? a : b };

  - return type and the params (are both of type T)

- by using this template the compiler will decide the param and the return type based on the args type we passed in..
- just like the regular fns we can also declare and define the fn template
- the templates are not real cpp code, they are just blueprint the real code will generated during the compile time...(we can see that in the Cpp playground)

- **template type deduction and explicit args**

  - is the mechanism that the compiler uses to deduce the type, it would use to set up our template instance from the args we passed to our fn call..
  - means the compiler will change/deduce the vars types based on the args we passed ..in other words it simply changes the three T's in our blueprint(temp params) with our args type

- and we can explicitly use the type (return type) while calling the fn (regardles of the param types) ex - maximum <double> (x, y);

**Template Type params by ref**

- sytax - template <typename T> const T& maximum(const T& a, const T& b);
- same like the regular fn the pass by val on the template fns will be a copy (and its an issue as we know), so here comes the pass by ref in template..
- when we ve the 2 templates with same name but one with pass by val, and the other with pass by reference, then we will get the compiler error.

**Template Specialization**

- by this feature we can bypass the default mechanism of how templates works in cpp.
- its way of telling don't replace the type of the args, instead use/impl the type that we will be giving.
- syntax - template <> const char _maximum<const char_>(const char *a, const char *b); // that template <> is the template specialization followed by the impl

-**concepts**

- from #include <context>
- concepts are mechanism to place constraints on our template type parameters..
- for ex we can set this and say we want our fns only to be called with int. and if we call it with some thing that isn't int then we will get the compiler error..
- the concepts are one of the big 4 features in **c++20**
- there are 2 types of concepts 1. std built in 2. custom concepts.
- some of the built-in concepts are same_as, derived_from, integral, floating_point etc ..
- syntax 1. template<typename T> requires std::integral<T> .. the **requires** kw is the key here..
  - 2. template<std::integral T> .. this is same as the above.
  - 3. auto add(auto a, auto b){}; // this syntax will gen the template bts.. we can also pass concepts inside ex - auto add(std::integral a, std::integral b){};
  - 4, template<typename T> T add(T a, T b) requires std::integral<T>{}; //
- the concepts ve much cleaner syntax than template specialization

- **custom Comcepts**
- import the concept and the type_trait libs

  - there are diff ways to implement the custom concepts. lets see the syntax
  - syntax 1. template<typename T> concept my_int = std::integral<T>; .. with the concept kw
  - 2. template<typenae T> concept my_int = requires(T a, T b){};

- **requires clause**

  - lets zoom in our requires clause, it takes in 4 kinds of requirements
  - 1. simple requirements(is the one that we used so far, with the requires kw) 2, nested (nested requires stmt) 3. coumpound(ex - concept add = requires(T a, T b){{a + b} -> std::convertible_to<int>;}) 4. type requirements

- **combining concepts**

  - with the logical operators, ex - template<typename T> requires std::integral<T> || std::floating_point<T>; .. now this concpt will either work with the integral or floating point
  - lly we can use && and other logical operators

- **concepts auto** - concepts and auto kw .. we can combine them both
  - ex - std::integral auto add(auto x, auto y){}; now this concepts will hold only the integral..

### Classes

- are the blueprints for our objs..
- members of the class are private by default.
- and inside the class we ve mem vars, and fns (behavior)
- the mem vars can **never be refs** (coz they can never be left unitialized), since the mem vars can left uniitialized.
- and then we can initiate the objs (objs are run time data)

- **constructors**

  - A special kind of method ie called when an instance of the class is created.
  - no return types, same name as class, can ve params, usually used to initialize the mem vars

- **Default Constructor**

  - ex - Cylinder() = default; -- now the compiler will generate a default empty constructor for us.

- **setters and getters**

  - ways to read / write the mem vars in our class

- **#ifndef** - when we already ve the import and to avoid it importing (in the other files), ex - we ve a file called constant.h and we wanna use in "cylinder.h", in the constant.h we can use #ifndef CYLINDER_H and then #define CYLINDER_H then our cont double PI =3.14; #endif
- it is the way of telling the preprocessor if the name is not defined then we re gon define that but if its already defined then we re gon skip this all and not gon include..
- now that we ve this guard we can include the file more than once w/o having any error..

- **Manage Class Objects thru pointers**

  - in most cases we wanna mamage our class objs thru pointers, if we re using some form of dynamic memory allocation
  - ex - Cyclinder \*c1 = new Cylinder(2,1); // create obj on the heap
  - then we must ve to delete/release mem from the heap delete c1;

- and then to call the method we ve to use the deref and then call - ex \*(c1).volume() or we can use another syntax ex - c1->volume(); // (aka deref)with this pointer access notation we can access the stuff directly allocated to the pointer in the heap

### Destructors

- are special methods that are called when the **obj dies**
- they are need when the obj needs to be release dynamic mem or other kind of clean ups.
- syntax is **~Cat()** ~ is the destructor

  - when we waana del particular field (mem vars) then the syntax is Dog::~Dog(){delet dog_age;}

- when they are called ?
- 1. when an obj is passed by val, 2. when a local var is returned from a fn 3. when a local stack goes out of scope(dies) 4. when a heap obj is released with "delete kw"

- **ctor pass by val** when we pass the ctor by val its gon to be copy val. not the real that we declared in the ctor..

  - but if we ve a pointer mem var inside the class then we re copying the mem addr not the val.
  - and when we ve the class obj pass by val, then the copy inside will cause the destructor to be called when the fn exits, since the copy is scoped inside the fn.

- **ctor and destructor call order**
  - in which order they need to called ?
  - the order is the obj created last will be destroyed first. and the obj created first will be deleted last..

### this pointer

- is a special pointer maintained in cpp. **each class mem fns** contains this hidden pointer.
- if we use this kw in our print statement then we can see the mem addr of our current obj
- the pointer contains the mem addr of the current obj for which the method is being exec.
- this also applies to ctors and destructors.

- this can also be useful in name **conflicts** when we ve param and the mem vars that are named in same way. ex - this->dogname = dogname; .. and this dogname is the mem var.
- and to set the ptr of the mem var to the ctor param .. \*(this-> age) = age;
- when we set the ptr ctor to the fn ex - Dog\* set_age(int age); this->age = age; return this; .. here return this kw will return the mem addr.
- **chained call using ptr**
  - and when we ve multiple setter fns like the above then we can use **chained** ptr. ex dog1.set_age(4)->set_name("alb")->set_breed("aa");.. since these setters are all ptr ctr and they are all returning the mem addr(this).. so we can chain em.
  - **chained call using refs**
    - use the class ref in our mem fn ex Dog& set*age(int age); {retun \_this} ..since we re returning the ref we need to use deref * (for "this")
    - and now we can chain ex - dog1.set_age(4)->set_name("alb")->set_breed("aa");..

### Struct

- is a kw to create classes in cpp.. the diff b/w em is, in classes the mems re gon be private by default, but in struct they re public by default..
- when don't wana dec any fns inside the class then we can use the structs instead.

- **size of class obj**

  - inside the class the compiler will count the mem vars . if its pointer var , then the size of the ptr (not the val it pointed to)
    - ex - std::string which is a const char\* ptr
  - and the fns wont be taken in count. coz the fns are not affiliated with class obj,

- **boundary alignment**

### Inheritance

- to extends class from another class.. ex class Player : public Person{};
- and the inheritance tree have most fundamental class at the top and the derived class from the bottom

  - and we can't derive the priv var of the mem class from the base class.

- **protected** access specifier - some time we want our mem var of the base class to be assessible by the derived class, but not by the other class (outside class )

- we can also use the access specifier for the class level ex - class Player : protected Person {}; .. now any public member var in the base class will be protected var in the derived class.. (and the priv wil remain the same)

- lly we can also ues the private access specifier, then all the pub, priv, protected mem vars of the base class will be private in the derived class.
- and the public access specifier wont do anything .. so by default its don't specify anything then it will be public.

- **Resurrecting members back in scope**

  - if we ve 3 classes, base class, derived class and sibbling class, lets say derived class inherited the parent as private specifier (now it can ve the acces to protected and pub member var of the base)
  - but the sibling class inherits from the derived class won't ve any access to the parent
  - so in that case we can use the **using kw** to access the parent from the sibbling ex - using Person::get_name;
  - note this won't work with accessing the priv mem fns of the base class..

- **Default arg ctor with inheritance**
- **copy ctor with inheritance**
  - when we create a couple of ctor for the class the second one is the copy ctor, (cpp will create its copy bts)
  - we can access the parent class from sibbling like Engineer(const Engineer& source) : Person (source.m_full_name, source.m_age, source.get_addr());..like that we can access the mem vars
  - we can also set up our own copy ctor.. ex - Engineer eng1(10, 4); and then Engineer eng2(eng1);
  - now eng2 is the copy ctor of the eng1.
  - to create our own copy ctor - ex Engineer(const Enginee& source);
  - once we declare this and we can use this copy ctor.. ex - Engineer(const Engineer& source) :Person(source), name(source.name){}; ...now this will use the Person obj to initialize ctor(the Person(source) part will slice off the Engineer part and create the person obj in the copy ctor )) .. this is the optimal way to set up our copy ctor if we ve our inheritance hierarchy
  - note: the Engineer default ctor will contain no data or some gibberish

### Inheriting base ctor

- we can inherit the base ctor and use them in the derived class
- by default it is not possible for the derived class to inherit the base ctor. but it is possible to tell the compilers to set up our own obj.
- ex - class Engineer : public Person{ using Person::Person}; .. with this **using** kw we can inherit the base class ctor
- this using kw will bring all the ctor that we define for the Person class(ex default, ctor with params..except the copy ctors which can't be inherited)
- bts the compiler will create like ex - Engireer (const std::string name, cont std::age) : Person(name, age){}; // this is called inheriting the base ctor.

**some facts** to keep in mind..

- the copy ctors are not inheritable..
- on top of derived ctors we can add our own ctor that possibly initialize the member vars of derived
- inheriting ctors will make our code confusing.

- **Inheritance with destructors**

  - lets see how inheritance works with destructors..
  - unlike the ctor inheritance, the Destructors will be called in the **reverse Order** sibbling, derived and base class.

- **Resused Symbols in inheritance**
  - lets see how we can reuse the names in inheritance hierarchy..
  - when we ve the same mem var (or the same fn signature) in the parent and the child.
  - by default if we inherit from the base the child will hide the mem vars of the base and will use its own..

### Polymorphism

- polymorphism means multiple forms .. means the base class pts or refs can take multiple forms
- is the set up we can do in cpp, to use the base ptrs to manage derived objs..
- ex - Shape\* shape1 = new Circle;(like this our base ptr can take multiple form/types) or we can also do with the refs ex - Shape &ref1 {&shape1};
- now why do we wanna do like this ? with the polymorphism we can set up a simple method and passing a base ptr or refs to the fns

  - ex - fn rect(Shape \*ptr){ptr->draw()}; this ptr is pointing to the draw mem fn in the Shape class. lly we can use the ref inplace of ptr.

- in cpp we won't get polymorphism by default instead, we will get **static binding**

- the polymorphism is about using base class ptrs or refs to manage derived objs in our inheritance hierarcy
- the other benefits of polymorphism is it will allow us to store **diff kinds of objs** in the single collection

  - ex in array we can only store the obj/elem of same type but with polymorphism we can solve this limitation.
  - ex - Shape \*shape[] {&oval, &circle};

- ex - Shape shap1("name"); Shape \*ptr = shape1; ptr->draw(); this will call the draw() from the base class(shpe class) lly if we assign the ptr to other ctors ex - ptr = &oval; ptr->draw(); ...lly for the refs... this will still call the draw() from the base class (shape) .. but we don't want this behavior..
- and this is the default behaviour, and its called **static binding**.
- to be able to call that methods from its own obj/ctor ..or we want only one method and want our compiler to resolves which one is called at runtime based on the obj we passed
- and another thing is we want to ve only one collection that can be able to store diff types (of objs)

**dynamic binding** or Late binding

- lets see how we can achieve dynamic binding in polymorphism with **virtual** fns..
- for this in our prev ex lets mark our draw() to be virtual ex - virtual void draw() in shape class and lly for the other classes
- the moment we use the virtual kw in our fns we will get the dynamic binding behavior in our inheritance
- no if we call the draw() ex - Shape \*ptr = shape1; ptr->draw() calls in shape class .. ptr = &oval: ptr->draw() .. calls in the oval class .. "and this is behavior" we wanted
- just like with the ptrs, we can also achieve this dynamic binding by the base "refs"

- now with this dynamic binding we can set one method and it will draw any shape (based on the shape that we passed in )

  - ex - void draw(Shape \*ptr){s->draw();} ..now if we passin draw(&oval); lly draw(&circle); it will takes the types that we passed in, lly we can also use the ref ex - void draw(Shape &s){s.draw();} and now we can pass draw(oval);

  - and we can also use it with the raw pointers ex - ptr = &oval; ptr->draw();

- in summary the dynamic binding or polymorphism works only if our methods are virtual..

- **Size of Polymorphic Objects and Slicing**

- the size of the objs that use dynamic binding or polymorphic objs
- with dynamic binding our objs are gon to be much larger, coz our cpp will keeps track of the info that allow us to resolve the fn calls dynamically..

  - and those infos are stored in the **virtual tables**..

- lets ve an ex - we ve 3 classes shape(base), oval, circle - the oval extends base and the circle extends oval (so now it has shap and oval )
- but if we assing the shape = circle; then the compiler will stripe out the space for the oval and the circle (since there is not enough mem to save so it will stripe off )
- this is called **Slicing**
- we can see the size of our obj with and w/o the virtual kw in our fn..

- note: if we re not using the base ptrs or refs, and we hoped to get dynamic polymorphism results, but we won't coz the compiler will slice off the derived classes's info and left with the base class info

  - ex - Circle circle1("a", 2); Shape shape = circle1; shape.draw();// we won't be having any info about the circle() class excepte the base class ie shape.

- **Polymorphic Objs Stored in Collections**
- - what happens if we stored Polymorphic Objs Stored in Collections
  - if we ve some collection of objs and try to store em in an array directly w/o the refs then we will get the copy not the actual objs.
  - ex Shape shape [] {circle, oval, line}; here this will slice of the circle, oval and line's info excepte the shape info will be stored in this array here..(since all of them inherited from base shape)
  - the moment our data is sliced off, we will never get our data back its lost permanantly.. even if we tryna take the obj and assign it to a ptr or ref..
  - so if we store our derived objs in an array or directly assign it to the derived objs.. we will get slice off..

- storing the collections with the ptr will work ex - Shape \*shape[] {&circle, &line, &oval} and also works with the smart ptrs **but not with the refs** ex - Shape &shape[] {circle,line, oval} // will throw error.. since the refs are **not left assignable**

### Override

- to override the methods in the base class use the **override kw** in our derived class.. ex - virtual void draw() const override{}; this will override the draw method in the base class
- this is the recommended way to use the override kw in our inheritance hierarchy if we happen to be using the virtual fns..

<h3> overloading, overriding, and hiding</h3>
- if we override one virtual fn in our base class, and all the others are gon be hidden, and we ve no choice but explicitly override each one of em..
  
- **Inheritance and polymorphism at different levels**
  - we can do polymorphism in different levels in our inheritance hierarchy..
  - the idea is the by default the polymorphism will work  only on the top level / base class.
  - but we can also make virtual fns in the derived or the sibbling class, and we can ve the method overriden in the downstream inherited class..
  - when we wana do this we ve to add the virtual kw to our overridden function ex - virtual void run() const override{}; // by doing this now this fn can be available to be overriden in the next level inheriting class..
  - in the practical app we need to ve our destructor virtual if we ve any virtual fns in our class ex - virtual ~Shape();

- **inheritance and polymorphism with static members**

  - as we know the static vars are the member vars which associates with the types itself.. and its not associated with any objs that we created..
  - in our inheritance hierarchy, if the classes ve the static vars with the same name they each maintain their own var..
  - but if the derived class doesn't ve any static vars then it will use its base class var

- **Final Specifier**

  - the final specifier (kw) used in the inheritance hierarchy.. this will allow us to restrict one of these 2 classes
    - 1. Restricts how we override method is the base classes .. or
    - 2. Restricts how we can derive from the base class..

- 1. ex - void run () const override final; by doing this we re stopping the downstream inheritance or no further inherited class will be allowed to be override.. (no virtual kw even if we ve one but still no use with it the final has high precedence)

- 2. ex - to restrict the class .. ex - class Plane final{}; now no one will be able to inherit our class..

  - if we ve a override method from the base class and the derived class is final ex - class Cat final : public feline {void run() const override {}} // since this override fn is from the base even if we inherit this cat final class we can still be able to override the run() method ..

- **Virtual fns with the default args**

  - default args are handled at the compile time.. while the virtula fns are handle at the runtime with polymorphism..
  - if we use derault args with virtual fns we might get error/ wierd result with polymorphism. coz the default args are decided by using static binding, and the fn we get called is decided by the dynamic binding..

  - ex - if we ve the virtual fns with the args in both the base and the derived/child class, when we call the virtual fn in the child it will use the child's override fn, but the compiler won't use the child's args instead it uses the base class's virtula fn's args ..

- so the recommended way is to avoid the virtual fns with the default args..or else its gon be hard for others to read and maintain our code..

- **virtual Destructors**
  - these are the destructor method we want to call using dynamic binding or polymorphic behavior.
  - as we know the destructor order they will be deleted in the reverse order first come last out or vice versa
  - if our destructor are not virtual, then the compiler is gon to use static binding to decide which destructor to call .. ex - Animal \*animal = new Dog() {delete animal; } // here the compiler will delete the destructor of the base class ie Animal not the animal
  - so now the dynamic mem we ve allocated for the animal is gon to be leaked ..
  - so to fix this we ve to use the virtual destruct. ex - virtual ~Dog; // lly mark the other class in our hierarchy as well
  - by doing this now the compiler will call the most specific destructor for our classes.

### Dynamic_Casts<>()

- Dynamic_casts are the facility in cpp to do downstream transformation b/w our polymorphic types..
- ex - Animal \*ptr = new Dog(); // the only thing we can call is polymorphic or virtual fns. with the Animal base ptr..
- but some times we wanna do just more than that ex - call non polymorphic fns it won't work.. since the base ptr ve no knowledge abt those fns
- but if we need to do that explicitly transform the base ptr to the derived ptr will give us that capability..
- couple of things we wanna achieve (thru dynamic casting)
  1. Transforming from base class ptr or refs to the derived ptr or refs in the runtime
  2. makes it possible to call non polymorphic fns on derived objects.

ex - Feline* ptr = dynamic_cast<Feline*>(animal1); // this animal1 is the i/p and inside the angle bracket is th output wat we wanna get out of this transformation

- if the cast succeeds we get the valid ptr back
- if it fails we get null ptr..
- and if its not a null ptr we can call the non polymorphic fns in it ..

- lly we can do for the derived refs ex - Feline& ptr {dynamic_cast<Feline&>(animal1)};
  - but with refs we don't ve a null pointer, so there is no way to directly check the return val like we did on the ptr..
  - so with the refs there is no way we can check and see if the transformation was successful, and this is the limitation..
  - but we can turn the ref into a ptr and do the casting.
  - this is not a recommended way of doing instead we can use the pass thru pointer

ex - Feline* ptr {dynamic_cast<Feline*>(&animal1)} // here we re changing the (i/p) base ref into the base ptr (Feline\* o/p)

- as we know by doing this now the ptr can store the null ptr, and we can use this to as adv to check and see if the transformation was successful or not..

- overusing downcasts is a bad design, if we use this alot to call the polymorphic fns may be we should make the fn as polymorphic in the first place..

- and finally this dynamic casting is useful if we re using the base ptr or the base ref in our fns..
- ex - void something(Animal* animal) {Fellin* ptr = dynamic_cast<Feline\*>(animal)}; // here we re takin the animal ptr and turn that in to the feline ptr(casting).. lly we can do with the refs ..
- so we can use this technique to get the non polymorphic fns ..
- note: that we ve to make sure to use the dynamic cast in the inheritance hierarchy that supports virtual fns. or in other word the i/p and the o/p ve to be in the same inheritance hierarchy.. ie the desing purpose of the dynamic cast..

  - if we use that outside of the context we will get the error..

- **Don't call polymorphic fns from constructors and destructors**

  - means never call the virtual fns from the ctors or destructors. ex - Base(){this->setup()}; lly for the dtor ex - virtual ~Base(){this->cleanup()};
  - as we know the calling order of the order of the ctor and dtor ex - Base \*b = new Derive; // in this ctor - order the Base class 1st and then the Derived class.. but dtro order the Derive class 1st and Base Class next..
  - so if we call the virtual fn from the derive the derive ctor wasn't set up yet .. and if we want the virtual polymorphic fn we want the most specified/recent part to be called (which is the derived Class). but it wasn't set up yet..and the compiler won't find the derived part and it will call the base class fn
  - ie y calling the virtual fns in ctors / dtor will result in the static binding..

- but if we really need we can call the virtual fns directly on the objs .. ex - Base \*p = new Derived; p->setup; // we can call the virtual fns like this b4 releasing the mem..

### Pure Virtual fns and abstract classes

- is a mechanism in which the method isn't been meant to implemented in the base classs.
- they are meant to be overriden and implemented in the inheritted classes.
- another thing is now the compiler won't gen the obj of the base class.
- syntax - virtual double run() **const = 0**; // by definin const = 0 to our virtual fns will make the pure virtual fn.. once create the pure virtual functions few things are gon to be happen,
- 1. this class is gon to become an abstract class.. in that **we won't be able to create an obj of the abstract class** anymore, even if we try we will get the compiler error.. so once we ve atleast one pure virtual function our class will become an abstract class..
  - ex - Shape shape = new Shape() // error...
- but we can still use the base ptr to manage it ex - const Shape \*shape = new Rect(3,"a"); shape->surface(); and this will still work.

- derived classes from the abstract class must explicitly override all the pure virtual functions from the abstract class, if they don't then the derived class itself will become the abstract class.

- pure virtual functions don't ve an implementation in the abstract class.. they are meant to be implemented by derived classes
- we can't pass the pure virtual functions in the ctor of the abstract class.
- the ctor of the abstract class is used by the deriving class to build up the base part of the obj..

### Abstract classes as interface

- an interface can be thought of as an abstract class, with only pure virtual function with no mem vars,

  - in short a abstract class is a class with only one virtual fn and no other fn or vars..

- but the wierd thing there is no kw for using the interface, just with the class name and one pure virtual fn is consider as the abstract interface

### smart Pointers

- As we know that there are 3 types of smart pointers in the cpp
- 1. shared 2. unique and 3. weak smart pointers // and they are from the lib **<memory>**

- the smart pointer is a continer or a wrapper for the raw pointer. and the advantage of these smart pointers are they **deAlloc** the mem automatically. so we don't ve to worry about the memory leaks in our programs

1. Unique Ptr:
   - there are few ways to create the unique ptr
   - ex - unique_ptr<int> uptr = make_unique<int>(23); // to deref \*uptr to access the val

- the unique thing abt the unique ptr is it won't share the mem to other ptrs once its assigned. but still we can **move the ownership** of the uptr ex - unique_ptr<int> uptr2 = move(uptr1); // now the uptr2 is the owner of the mem addr uptr 1.

- once we move the ownership now the prev pointer is gon be the null ptr and if tryna access the val we will get the **null ptr exception**
- and the unique ptr will dealloc/destroyed at the end of the scope..

2. Share ptr:
   unlike the unique ptr the shared ptr can share its mem addr to the other ptr..aka we can assign multiple owners .
   syntax - shared_ptr<Shape> sptr1 = make_shared<Shape>();

   - just like the **RC ptr in the rust** it has the **count of all the owners** and to access the no. of owners we can use the **use_count()** ex - sptr.use_count();
   - and to share this ptr ex - shared_ptr<Shape> sptr2 = sptr1; //that's it now the ownership count will be 2.
   - and the mem deallocation will be happen when there are **no more owner** on the scope. or when there are no more ptrs pointing to that location.

3. weak pointers:
   - unlike the shared ptr, the weak ptrs will not include the no.of owners, means we use the weak ptr to observe the obj in mem. but the weak ptr will **not keep the obj alive in mem** if nothing else needs it. where as the shared ptr will keep the obj alive in mem.
   - syntax -` weak_ptr<int> wpptr1;{
shared_ptr<int> sptr1 = shared_ptr<int>(25);
wptr1 = sptr1; }` // the wptr mem was deallocated when its last owner(sptr1) left its scope.. means it won't keep the obj alive..

### Function Pointer

- The pointer that stores the mem addr of the fns (instead of the var's mem addr)

  - as we konw if we ve the fn ex int get(){}; and if we print that w/o the paranthesis we will get the addr of the fn ex cout<< get; //now we can store this addr of the fn.
  - syntax - int(\*fptr)() = get; // just the return type ptr name and the param and assign it to the fn (w/o paranthesis) its just the mem addr
  - syntax 2 - for the fn with params ex- int(\*fptr)(int, int) = add;

- **why and when to use fn ptr**
  - we can use em to pass the arg of the fn ptr to the another fn.. in order to optimize the code.
  - first we assign them as a param to the fn, then ex - void custsort (vector<int>& numVect), bool(\*fptr)(int, int){} // now in this fn ptr we can assign to the assending sort or desending sort as long as the fns return same type and same type param (as mention in the fptr)
  - now in our main fn - ` vector<int> mynmber = {5,2,3, 4};
                       bool(*fptr)(int,int) = ascending;
                       custsort(mynumber, fptr); 
                       print(custsort);` // since the fptr is pointing to the ascending() it will sort em in ascending.. lly we can now change the ptr to points descendin()
  - so this way it solve the code repeatation, and its easier to use.

### Function ptr, Delegates and Callbacks

- delegates will give us the flexibility to pass the method as arg to other methods
- as we know in c# has this concept of delegates, but bts they implemented with fuction ptr..
- lets see how it works in cpp which makes it easier for us to understand in any other langs about the delegates.
- lets ve an ex of 2 forms with parent and child(gui) send msg to each other.
- syntax ex - void (**closure \*fptr)(int, int) = showmsg; //its same like the fn ptr but ve to add **closure for the method ptr
- note: \_\_closure is C++ Builder specific extension. With standard C++, the function pointer must be a "member function pointer"
- now we can use this fptr as the arg
- steps - 1. create a fptr on our parent form 2. pass that fn to the child form 3. save that fn ptr as global var in the child 4. invoke it when we need

### ctor Delegation

- allows one ctor to use another ctor with in the same class. which can reduce the code repeatation
- lets say we ve a class with 4 mem vars and we instantiate a couple of ctor one with 3 var as param and the other ctor with all the mem vars as the param.
- and we will let the compiler decide which one to use based on the args that we passed during the runtime.
- but instead we can extend from one ctor to another, ex -
- `Rectangle(int l, int w){lenth = l; width = w};
  Rectangle(int l, int w, string col) : Rectangle(l,w) {color = col;}

  //by delegating / extending the one ctor to another, we can reduce the code and still ve the same functionality

  - but now the if the ctor2 will call ctor1 during the runtime as it inherted..
