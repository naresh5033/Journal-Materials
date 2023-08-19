# C

- to exec the d.c file in the gcc compiler `gcc -o d d.c` o/p the d.c file into d bin format and to run the bin exec file `./d`
- the wierd qyirk in c what i found is they are declaring the fn then defining the fn and then calling the fn inside the main fn
  - ex ` int add_two(int x, int y); -- fn declarations
     int main(void){
        int x = 2, y=3;
        add_two(2,3)
     }
     int add_two(int x, int y){ - fn definitions
        return x + y;
     }`

### Pass by reference (aka pass by pointer)

- this is like passing the pointer as the param then when calling the fn we can assign a mem addr to the pointer(in the below ex we just assign(ar) the mem of the b to the pointer (param) while calling it), since pointers can only save the memory address, and then we can def the pointer to change the val of whatever addrthe pointer is pointing to.

- ex ` int main (void){
      int b = 5;
      add one(&b);
      printf("%d\n", b);
      }
      int add_one(int *a)
      {
          *a = *a +1
      }`

- as far as i know there are 4 types of pointers in c 1. null ptr (int *ptr = NULL) 2. void ptr (void *ptr) and to print the val of it we need to typecast ex - printf("After Typecasting, a = %d", _(int _)ptr);
- 3. wild pointer - A wild pointer is only declared but not assigned an address of any variable. They are very tricky, and they may cause segmentation errors
- ex int \*ptr; this is not assigned to any mem addr

4. Dangling ptr - Suppose there is a pointer p pointing at a variable at memory 1004. If you deallocate this memory, then this p is called a dangling pointer.
   You can deallocate a memory using a free() function.

ex - ` int _ptr=(int _)malloc(sizeof(int));

         int a=5;

        ptr=&a;

        free(ptr); // now the the ptr is dangling ptr, its just points the mem addr which has no vals in it

- **Uses Cases of pointers**
- Pointer to pointer - In this situation, a pointer will indirectly point to a variable via another pointer.
- Pointer arithmetic - (You can use this operator to jump from one index to the next index in an array.)
- Array of pointers - An array of the pointer is an array whose every element is a pointer.
- Call by value - In ‘call by value’, you must copy the variable's values and pass them in the function call as a parameter. If you modify these parameters, then it doesn't change the value of the actual variable.
- Call by reference - In call by reference, you must take the variable's address and pass it in the function call as a parameter. If you modify these parameters, then it will change the value of the actual variable as well.

- **Pointer notation vs array notation**
  - we can often treat arrays like pointers and pointers like arrays.
  - ex - int add(int[]); int a[] = {1,2}; and while calling add(a) --> whats happening here is we re only passing the mem addrs of the a not the array ,
    - when we use like this, just the a (not with the []), means the **array decays to the ptr**
- to avoid this what we can do is while defining and declaring the fn - we vo to use the ptr (for the array ) ex - int add(int \*array) a (just treating the ptr like the array)

- **array notation**
- ex - int a[] = {1,2,3}; int \*p; p = a; now we ve assgined the ptr to the a, now the with the ptr we can access the array ex - printf(%d, p[1]) the val is 2, this is called array notation, just accessing the vals(of array) in the mem by shifting the idx

**pointer notation**

- are way of getting the vals, that are shifted over from the given mem addr, ex - printf(%d, \*(p + 1)) - here we re defer the pointer p and then add + 1 to the mem add arithmetically we shift +1 to the mem addr and get the val of the next mem addr,
- in other words p + 1 says get the next mem slot, and then the \* says get the val of the mem slot..
- this is also called pointer arithmetic, since we re adding 1 to the pointer
- we can also set the a instead of p ex - printf(%d, \*(a +1)) will gives the same val, since the a has the same mem addr as the p.

- at this point we can think pointers and the arrays are same and we can use em interchangeably, but we can't ex - we can't assign val to a, ex - a = p; we can't change what mem addr a is storing..

### dynamically allocated memory (malloc, calloc, realloc, free)

- as we know the reg vars like int, float etc are stored in the stack (next to each other)
- and bcoz of the fact the vars in stack are stored next to each other we can't store the [], bcoz if the size of the [] changes then it can't take the next addr (mah be a int var occcupied),
- and that's where the dynamic mem comes in, we can alloc a block of mem, and later we can access.
- and this dynamically allocated memory is stored on **Heap** is a special place in mem.. we can think of as the bottom of the big mem space that our prog has.. and the stack is at the top of the big block(heap)
- so the stack grows **downwards** and the heap grows **upwards**
- the dynamic mem alloc fns are included in the **stdlib**
- ex - int _a; a = malloc(sizeof(int) _ 5); - allocate mem for the int (of 5 vals), and the size of will returns the no.of bytes that stakes to stored in the int val.
- now the malloc will return the mem address, ie y we stored in the ptr.
- once we allocate the space, we can use the array notation to access/ assign the val ex - a[0] = 1;
- **calloc** when we use malloc there could be val stored in the mem addr already, (some garbage vals), when we use malloc it doesn't do anything to that mem (it doesn't clears the val) it just goes and alloc the mem.
- now that's where the calloc comes in, it takes 2 args ex - int \*a; a = calloc(5, sizeof(int)) - no of things we wanna allocate for and the size of the thing.
- now when a calloc returns the ptr to the block of mem it assigns all val to 0 (just clears the val, as the name stands)
- the advantage here is that we re basically deals with the clean mem..
- but the calloc comes with cost, becoz of the fact it does as same as the malloc does plus inaddition it has to go and assign 0 to all the block, so it is **time consuming** in comparision with malloc

- **free()**
- whenever we use malloc or calloc, we must use free(), to free up the mem that has been allocated, and it makes that mem to available again.
- it doesn't delete the content, it will just makes a space in mem and makes it available again.
- if don't free() up the mem, then it could leads to the mem leak. where we don't ve the access to the mem to free up in the future, maybe we change the ptr or something like or we loss the track of original mem addr, original ptr to that mem space.
- and that point we ve mem leak where the mem that's been allocated its set aside for storing data, but it can nolonger free it either.
- another thing thta could happen is **run out of mem** we keep on adding more and more data in the mem and we forget to free it.

- **realloc**
- when the allocated memory size is increased then we can use the realloc to reallocate the mem size ex - realloc(sizeof(int) \* 2)
- sometimes it can't make it bigger coz of next things already stored in the mem, in that case it takes the malloc and stores it in some where else. (copies the existing data and stores in the new block of mem)

- **type def and struct** (making a new name for the type )
- these allows us to define and use new types. ex - typedef int inch; it creates a new type synonym for the word inch, we can now use the inch as the type.
- one thing to use the type def is it helps the readabilty of the code,

- the typedef often used in the conjuction with struct.. ex - typedef strct {}Point; now we can just use the only the struct name to assign the var ex - Point point = 3; as opposed of Struc Point point = 3; which is tedious..

**struct**

- allow us to group related information together. the group related information often called as the record of information.
- when using inside the fn (as an arg) the structs are **actually passed by val**.. pretty sim to other vars like int..
- so when we define the struct it will alloc in the mem, and then when we pass in as the param/var the stuct's val will copy and duplicated on the stack in mem.. so if we ve huge data in the struct then it will be a mem concern.
- so one thing we can do is we can use the **dynamic allocation** lets say we ve Students struct and we wanna dynamically allocate the mem for the Student..
- ex - int main(void) {Stundent \*s1; s1 = calloc(1, sizeof(Student))} - allocate memory space to store 1 student
  - lly we can deref the fields and set the vals - ex - (\*s1).age = 20;
  - instead of using this syntx we can use the **arrow syntax** ex - s1->age = 20; (the arrow says at the block of mem pointed to the mem add s1, go get the age member var in that block of var)
- we can use the struct pass by ref and often times ie how the struct are used ex - void age_studen(Student \*s){s->age +=1;}

  - note when we re doing this way its not like duplicating the entire thing, instead we re just passing the pointer to the same struct (Student) that already exists in mem

- structs can also contains the pointers to things as well ex - typede struct {int data; int \*array;} Info;
- then we can define like Info a ; a.data = 7; Info b; b = a ; - when we re doing this we re just copying the struct a for b.. so whatever the val the data field, \*array field(holds the mem addrr) holds will gets copied

### 2D Array

- the other way of organizing the data..ex - int data [2][3] = {{1,2,3}, {1,2,3}}-- this is the 2d array with 2 rows and 3 cols
- we can also define the 2d array like this int data [2][3] = {1,2,3,4,5,6} - based on the vals there it will still get assigned to the vals of their corresponding index, but its bit difficuilt/ confusing for the code readablity.
- **constants** ex - #define ROW 3; can be change during the compile time but not during the runtime

### main fn and the return val

- the main fn's - int main(void){return 0;} - whats happening here is .. if we exec the prog now it will exec with the status code 0 ie the exit status of the code. anything other that 0 is considered as some kind of error/ failure 0 is success .. and to see it on the shell `echo $?` to see the status

### command line Args

- our prog can also interact with the shell by recieving the command line args
- ex - int main (int argc, char \*argv[]) - the argc is the no. of args including the actual command itself.. and the 2nd arg is the arr of str we can print out ex - printf(%d, argc) , it will show the 1 ie our command to run the bin also we can pass the 2nd arg while exec the bin ex - ./d 1 2

  - and this will prints 3, the command itself and the other 2 vals we passed in

- there is method in the stdlib - that converts the str to the int - atoi()

### Type casting and type conversion

- the way it works is **(type) expression** the type we wana convert to and the expression
- ex - int a = 3, int b = 4; double c = (double) a/b ..
- makesure if our expression has the bracket (a/b) then the a gets converted to the double 1st.
- because the (double) has to happens first..

### File Io

- FILE \*fh_write; -- the FILE is kw .. and to read the open the file fopen("write.txt", "w") - with the write permission/ mode
- now we can assign that to fh_write.. and to write the file fprintf(fh_write, "hello world"); then fclose(fh_write); .. when done with the file
- we can also open the file in the **append** mode "a" it will append the content to the existing content in the file
- lly we can use read the file, just open the file(with the read mode "r")
- and to read the line and save the line as buff - ex - char buffer[100]; fgets(buffer, 100, fh_read );
- it will read in the line of the file and store in the buffer, but the fgets will stops when it encounters the 1st new line .
- and to read all our 100 chars that we mentioned, we wanna read until it gets no more data to read, the fgets will return null when there is no more data to read.

  - ex ` while(fgets(buffer, 100, fh_read)!= NULL; {
            printf() )`}

- so its actually a common thing we can see often the fgets will work along the while loop and execs until it gets null

### const with define and const vars

- if we wanna use the const across our prog then define is best option, or we can just use the const int x = 2;
- when dealing with the const it belongs to that fn scope
- we can define a global var outside of the main fn
- now the one thing with the global scope var is, it can be dangerous to use, what can happen is if we call the global scope var in the fn and returns some val that fn could actually change the val of the global scope,
  - and this type of fns are called side effects
- also the local scope var (with the same name as the global scope) will takes the precedence

### accepting user i/p strings with spaces

- the scanf will breaks if the user i/ps a space, intead of using scanf, we can use fgets
  - ex - fgets(buffer, 100, stdin); this stdin takes the i/p and then we save em in the buffer
