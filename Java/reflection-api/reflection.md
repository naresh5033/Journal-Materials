### Java Reflection (udemy)

- the java reflection is the lang and the jvm feature that gives us the runtime access to info about our app class and objs
- is available in the java reflection api
- using the java reflection we can write flexible code that links diff s/w comps together at the runtime.
- creaetes new prog flow w/o any source code modification.
- it allow us to write general purpose algorithms that dynamically adapt and change their behaviour based on the type of the objs or the classes that they are working on.
- general prog w/o reflection takes the data and the i/p and process and gives the o/p, in contrast with the reflection which takes data, i/p analysize and perform ops on both and produces the output.
- now with the ability to to analyze app's obj and classes at the runtime, we can create very powerful **libs**, **frameworks**, **software Designs** that'd be impossible to create.
  - some of the libs that are built with the reflection are **JUnit** and in the **DI** framework such as **Spring Boot** and **google Juice**
  - when we re using DI in the spring boot like the @Autowired in our di, which is based on the reflection and understands that those dependencies needs to be injected in the runtime
  - **JSON Serialization and deserialization** - like the Jackson or GSON - uses reflection.
- and another popular use cases of reflections are **ORM**, **Logging Frameworks**, **WEB frameworks**, **dev tools** and many many more..
- now the reflection are not only for the libs we can use them to architect our own app, and libs, augment em with unique functionality, and features

- **challenges**
  - if we don't use the reflection properly then we may end up with the code that is
    - making the code ie harder to maintain.
    - slower to run, dangerous to execute (since it has the potential to crash our app unrecoverably).. ie why the reflection is reserved for the devs with the strong java knowledge.
- **Entry Point**
  - the obj of Class<?> is the entry point for us to reflect on our app's code
  - this Class<?> contains all the information on the give obj's runtime type / a class in our app
  - that info contains what methods and the fields it contains, what class it extends or what interfaces it implements or much more..
- 3 ways to obtain the obj of the Class<?>

  - 1. Object.getClass() {this wont work for the primitive types}
  - 2. ".class()" suffix to the type itself - this will works with the primitive as well ex- class booleanTypes = boolean.class;
  - 3. Class.forName(), by using this static method we can dynamically lookup any class in our class path using its fully qualified name..
    - we can also get the info about the inner classes ex - Class<?> engineType = Class.forName("vehicles.Car$Engine");
    - just like the first ex we can use this in the primitive types.
    - this is the least safest method amongst the 3, but there are some use cases we can use only this as our option.

- **Java Generics wild cards** - the abstract class Number extends the integer class, interface CharSequence implements the String class but the abstract class List<Number> doesn't extends the List<Integer> and the class List<CharSequence>doesn't extends the List<String> class
- however the List<?> (generic wild card) is the super type for the list of any generic types, aka List<?> is the super class of List<T> for any generic param type T

- **Introduction to Constructor<?>** in the java reflection A ctor of the class is represented by an instance of the Constructor<?> class
- since the class can ve multiple overloads of ctors we ve few ways to get em ex -
- 1. Class.getDeclaredConstructors() returns all the constructors of the class regardless of the public or not
- 2. Class.getConstructors() returns only the public constructor.
- if we re looking for the particular ctor and if we know the params of the constructor we re looking for then we can use Class.getConstructors(Class<?> ...ParamTypes) or sim to the getDeclaredConstructors and pass in the params.. in these it returns particular ctor based on the param or if its not there it will throw an exception.
- if there is no constructor in the class then java will creates a default constructor(no arg ctors)

- **Goal and motivation** - ex - our goal is to create a single factory method, that can create a obj of any class

  - depending on the obj we passed to the method, it will find the right constructor.
  - create the given class obj by calling the right constructor.
  - w/o the reflection it is impossible ( the code can be found in the constructor.zip)

- **Restricted ctor access** - as we know the private ctors are not accessible outside of the class, but by using the reflection we can ve full access to the private, protected and package private ctors as well.
- and we can also use the Constructor.newInstance() method to create a new instance of the class using the restricted constructor.
- the only modification we ve to make the ctor accessible is by setting the setAccessible property to true, ex - constructor.setAccessible(true);
- the ex - http server config - refer the code webserver.zip file.
- this advance feature should be reserved to specific use cases and should not be abused to invoked internal or delibrately restricted ctors on classes that do not belong to us.

**8.Restricted classes instantiation automatic DI implementation**

- package private class instantiation using the ctor class.
- we ve to set the accessible method to true like earlier, before instantiating the obj
- 2. DI - the other scenario where the java reflection comes in handy is the instantiation of the package private classes from the another package.. auto creation the obj at the app start up.
- for this section refer the tictac toe.zip file
- in that ex - we can see all the class's ctor are priv and to create the instance/ to instantiate them. we re gon to create by implementing an autmatic DI engine using the reflection
- a recursive algorithm is the great fit, for each dep we visit we keep going deep into the graph, until we find a class with no dependency we can instantiate.

<h4>Section 3, inspection of fields and arrays </h4>
  - **Introduction to field class** - as we know the fields are class mem vars.
  - **Synthetic fields** java compiler gen artificial fields for the internal usage, we don't see em unless we use the reflection to discover em at runtime.
    - in most cases we don't want to touch/ modify or rely on em. - Field.isSynthetic() // to find the synthetic props of the field.
    - lly  we can be able to read the val of the given field and set a given instance of the class using the reflection

- so with the reflection in the field class, we can write the generic algorithms and libs that can operate on objs
  - whose type is unknown in the runtime.. or don't even exist until the prog is executed.
  - for the code refer (fields.zip)
- **Reading field JSoN serializer** - using the field we re able to get the class's field, name, type and read the val of an obj.
- the ex of such lib is the data obj lib like JSON.
- and for the json serializer implementation, we re gon to implement a method that takes any obj and serializes it into a JSON object.
- we ve no prior knowledge about the data object we re gon to serialize, and java reflection is the perfect for that task.
- sim to the ctor to access the restricted fields we can use the Field.setAccessible(true)..
- refer the code "json-writer.zip".

- **Arrays** - Array inspection using java reflection api, such as reading vals from the arrays using java reflection.
- in java the arrays are the special classes we can get the basic information about the arrays using the Class<?> API
- we can confirm that the obj is an array using the Class.isArray() method and we can use the Class.getComponentType() to get the type of an array elements, if the obj is not an array this method will return null.
- in java the 2d array is just the array of an array objs ex - double[][] twoDimArr = {{1,2,3},{4,5,6}};
- we can use the reflection in the given array to access the arr obj at the runtime ex - Array.getLength() and Array.get(Obj, idx) to get the idx of the given arr obj.
- in the zip file (arrays.zip) we can see we ve all the methods to inspect and iterate all the elem in the 2d array regardless of the type and dimensions.

- **REading arrays JSON Serializer** - in that ex - code(JSON-writter.zip) we can see we implement a practical real life algorithm converting the data objs to JSON strings
- using recursion we traversed the fields that contains the obj or the arr of obj and w/o reflection we couldn't be able to do such a generic solution whcih is perfect to be packaged as a library.

<h4> setting field vals, generic configuration File parser</h4> - as we know in many cases the field name and the types are  unknown at the AOT(ahead of time), since our reflection code is so generic it can work on the unknown AOT type and field names and we can write algos that can work with the infinite variety of classes.
- Use cases - Obj Deserializers, whcih takes data in a predefined protocol and translates it into a POJO
- ex - N/w protocol Deserializer and ORMs, App config file parser. (refer the code config-parser.zip)
- **Array Creation and initialization** refer the code config-parser2.zip
- in that code we can see we re using the reflection's Array.newInstance() method this is useful when the type of array is determined dynamically at the runtime.

<h4> Methods discovery and invocation </h4> - refer the code ("api-test-getters.zip")
  - the Class.getMethods() method returns all the methods including the ones inherited in the super class and implemented interfaces.
  - Class.getDeclaredMethods() returns all the methods declared in the class.
  - some of the other methods we can call on the Method's properties are Method.getName() and Method, Method.getReturnType(), Method.getParameters() and getParametersCount(), getExceptionTypes().
  - (in the ex code) its a kind of like a test framework, where each field in the data class has the correct getter exposing its val to the api users
  - and thanks to the reflection this testing framework is of type generic and can be used with the any type..

- lly there is code of the api-test-setters.zip (part 2) - in this ex in our framework sim to the getters we will use the setter to the testing framework.

- **Method Invocation (polymorphism)** refer the code polymorphism.zip file.. as we know the polymorphism is allowing the obj to takes multiple form.
  - as we know we can invoke the method using Method.Invoke(Object instance, Object ... args); // if the method is static then we can pass null as the instance.
  - the args of the method must be of correct type and order are the method signature.
  - **return type** if the return type is void then the invoke() will return null, if the return type is primitive then the invoke() method the return val will wrapped in an object . for ex the int val is returned as the wrapped in Integer object
  - this method.invoke() can throw some exceptions, and the most important exception is the InvocationTargetException - is thrown if the method itself we ve invoked threw an exception.
  - the method.invocation is one of the most widely used features in the java reflection, it allows the decoupling of the business/ test logic from its execution logic.
  - we can group and exec classes that ve nothing in common.
  - in that ex if we see we grouped objs of diff classes that don't ve any common interface or belongs to different classes, we re able to execute all those methods in a generic way.

<h4> Java Modifiers Discovery and Analysis </h4> 
- as we know the modifiers are added to the class, ctor, fields, methods and the modifiers are of type access modifiers and non access modifiers.
- **Abstract** makes our class abstract class that can't be instantiated 
- **final** finalizes the implementation of the class and makes it non inheritable
- **static** makes the inner class instantiable w/o instantiating its outer class
- **Interface** is also the modifier.. the interface is not only the modifier but also abstract modifier, just like the abstract class the interface can't be instanstiate w/o the concrete implementation. 
- **synchronized** method can be accessed by only one thread at a time.
- **Native** typically the method implemented in diff lang (typically C).
- **transient** marks the field not to be serialized.
- **volatile** makes the read/writes to long/double atomic and prevents the data races
- to get the modifiers we can use .getModifiers() in the class, ctor, field, method, etc. and those modifiers are packed with the integer vals and all those methods returns int. 
- the modifiers are encoded as **bitmap** so each modifier is represented a single bit.
- refer the code ("modifiers-example.zip")

<h4> Annotations with java Reflections </h4>

- Annotations can instruct and direct reflection code on what target to process and what to do with those targets
- we can decouple our app code from the reflection code.
- **custom annotation** as we know to create our own custom annotation we can use the @interface and every annotation in java extends the annotation interface
- not all the annotations are visible at the runtime, by default the annotations are retained by the compiler at the compile time and ignored by the jvm and not visible at the runtime.
- to make it visible we ve to use the @Meta annotation. one of the most important meta annotation is the @Retention meta annotation. this has 3 enum vals
  - ex - 1. RetentioPolicy.SOURCE - source annotation are discarded by the compiler ex @Override, @SurppressWarning annotation.
  - 2. RetentioPolicy.CLASS - class annotation are recorded in the class file by the compiler but still ignored by the jvm in runtime ex - @AutoValue
  - 3. RetentioPolicy.RUNTIME - annotation are recorded in the class file by the compiler also retained by the jvm in runtime

-**Target Meta Annotation** - is another meta annotation, available for the FIELD, LOCAL_VARIABLE, PARAMETER, METHOD ElementType

- to find the target annotation, we can use boolean isAnnotationPresent() method, will be present in the class, field, method, ctor, param targets.
- refer the code annotation example .zip file where we re finding the packages (the file name ends with the extn of .class) based on the URI
- if we dont want to include any of the method or the class in the run time then simply remove the annotations then the particular method or the class will be excluded.
- **Parameter Annotation** - the param annotations on ctors and methods, if the param vals are supplied using the reflection w/o the annotation, then we don't ve the way to tell which one is which.. we can use the param annotations on the ctors, methods alike.
- code as a graph - usually if we ve a bigger problem we will break that into smaller tasks and extablish the relation between em. generally we represent task as a method and implement prev task as the method's param..

  - the final step which is the sequential reperesentation and the execution of the tasks can be easily automated using reflection.
  - for the code refer the graph-example.zip file .. in that ex we can see our algo will keep recursively go through the entire graph of operation all the way to the method that don't ve dependencies and will be able to get the vals immediately.
  - so using these annotations we uniquely identify the methods or parameters based on their annotation val rather than the java types or names

- **Repeatable Annotation**
  - automatically scheduling - to allow annotation to be applied repeatedly we need to explicitly declare it as a repeatable annotation
  - isAnnotationPresent() or getAnnotation() methods will not work for the repeatable annotation.
  - refer the code repeatable-annotation.zip

<h4> Dynamic Proxies</h4>
  - lets see the proxy design pattern and the implementation - in the oop the proxy design pattern - the proxy is an obj that holds an another object(real obj).. the client will communicate with the real obj thru the proxy
  - some of the examples of the proxy design pattern are 1. security proxy (protection proxy)
  - the jdk collection class - Collection<T>, Map<T>, List<T>, Set<T> wrap the data 
  - another example for the proxy is the resource management - Lazy initialization proxy
  - another use case is for in mem caching proxy - performance 
  - then the another example is Remote Pxoxy - for making remote procedure calls
  - **Limitations w/o reflection** if we ve M methods in each interfaces then for N interface we ve to implement M*N methods in total which is tedious and code conumption - so this could be solved by the reflection's dynamic proxy

- **Dynamic Proxy Implementation**
  - the dynamic Proxy is a class generated by reflection at the runtime. its name usually starts with $proxy.example
  - an obj of the dynamic proxy class intercepts any method invocation and delegates it to an instance of the Invocation handler

<h4>Final Section -  Performance, safety and Best practises</h4>
  - Reflection - performance - Typically operations involving java reflection are slower and less efficient than their statically combined compiled equivalent operations
  - in reflection everything the compiler will do for us ahead of time compilation, we now ve to do it at runtime.
  - runtime vs compile time checks - the compiler can't make any access or type safety validations. At runtme the JVM has to validate - this is what makes the entire ops a bit longer
  - **Exception Handling** - in production we ve catch/ handle the exception at every edge case with care to make our reflection code more safe and reliable.
  - **3 Rule of Thumb** (when to use Reflection)
    - 1. use reflection only when we must - if we can achieve the same task with regular java code w/o reflection then we don't ve to use reflection.
    - but there are so many use cases where reflection can do, and the regular code doesn't.. and there are so many use cases where reflection can make our code more safe, cleaner, shorter and easy to work with.
    - 2. Test and Handle Edge Cases - when we use reflection the compile time code exception will be runtime exceptions. To keep our code safe and correct we need to perform those checks ourselves.
    - handle and prevent each exception by writing defensive code. write many automated (unit tests) to test any code that uses reflection.
    - 3. keep the reflection based code loosely coupled (with the rest of the code base as possible)
------------------------------------------------------------------------------

## the sample code and the solutions

### chap 1.

Exercise
Some Java IDEs use Reflection to inspect the code we, the developers are typing, and provide us with additional information about this code.

For example if we hover over a JDK based class we would see a popup with information about this class:

In this exercise we want to help develop a different, smaller plugin which will provide a different popup window that looks like this:

For example:

If we hover over the class List we will see this popup:

if we hover over our custom Product class we would see a popup window that looks like this:

And if we hover over a int type we would see a popup window that looks like this:

Inherited Classes

If the input is an interface the inherited classes are all the names of the directly extended interfaces.

If the input is NOT an interface, the inherited class is either the name of the direct class it extends or null

JDK vs Custom Types

To determine if a type is a JDK type or a custom type we need to inspect the package of the type.

Classes that belong to a package that starts with one of the following prefixes:

"com.sun.", "java", "javax", "jdk", "org.w3c", "org.xml"

is considered a JDK class

Primitive types do NOT belong to any package, and are also considered JDK types

Helpful links: Class

Solution
import java.util.\*;

public class Exercise {
private static final List<String> JDK_PACKAGE_PREFIXES =
Arrays.asList("com.sun.", "java", "javax", "jdk", "org.w3c", "org.xml");

    public static PopupTypeInfo createPopupTypeInfoFromClass(Class<?> inputClass) {
        PopupTypeInfo popupTypeInfo = new PopupTypeInfo();

        popupTypeInfo.setPrimitive(inputClass.isPrimitive())
                .setInterface(inputClass.isInterface())
                .setEnum(inputClass.isEnum())
                .setName(inputClass.getSimpleName())
                .setJdk(isJdkClass(inputClass))
                .addAllInheritedClassNames(getAllInheritedClassNames(inputClass));

        return popupTypeInfo;
    }


    /*********** Helper Methods ***************/

    public static boolean isJdkClass(Class<?> inputClass) {
       return JDK_PACKAGE_PREFIXES.stream()
                .anyMatch(packagePrefix -> inputClass.getPackage() == null
                            || inputClass.getPackage().getName().startsWith(packagePrefix));
    }

    public static String[] getAllInheritedClassNames(Class<?> inputClass) {
       String[] inheritedClasses;
        if (inputClass.isInterface()) {
            inheritedClasses = Arrays.stream(inputClass.getInterfaces())
                    .map(Class::getSimpleName)
                    .toArray(String[]::new);
        } else {
            Class<?> inheritedClass = inputClass.getSuperclass();
            inheritedClasses = inheritedClass != null ?
                    new String[]{inputClass.getSuperclass().getSimpleName()}
                    : null;
        }
        return inheritedClasses;
    }

}

import java.util.\*;
import java.util.stream.Collectors;
public class PopupTypeInfo {
private boolean isPrimitive;
private boolean isInterface;
private boolean isEnum;

    private String name;
    private boolean isJdk;

    private final List<String> inheritedClassNames = new ArrayList<>();

    public PopupTypeInfo setPrimitive(boolean isPrimitive) {
        this.isPrimitive = isPrimitive;
        return this;
    }

    public PopupTypeInfo setInterface(boolean isInterface) {
        this.isInterface = isInterface;
        return this;
    }

    public PopupTypeInfo setEnum(boolean isEnum) {
        this.isEnum = isEnum;
        return this;
    }

    public PopupTypeInfo setName(String name) {
        this.name = name;
        return this;
    }

    public PopupTypeInfo setJdk(boolean isJdkType) {
        this.isJdk = isJdkType;
        return this;
    }

     public PopupTypeInfo addAllInheritedClassNames(String[] inheritedClassNames) {
        if (inheritedClassNames != null) {
            this.inheritedClassNames.addAll(Arrays.stream(inheritedClassNames)
                    .collect(Collectors.toList()));
        }
        return this;
    }

    public boolean isPrimitive() {
        return isPrimitive;
    }

    public boolean isInterface() {
        return isInterface;
    }

    public boolean isEnum() {
        return isEnum;
    }

    public String getName() {
        return name;
    }

    public boolean isJdk() {
        return isJdk;
    }

    public List<String> getInheritedClassNames() {
        return Collections.unmodifiableList(inheritedClassNames);
    }

    @Override
    public String toString() {
        return "PopupTypeInfo{" +
                "isPrimitive=" + isPrimitive +
                ", isInterface=" + isInterface +
                ", isEnum=" + isEnum +
                ", name='" + name + '\'' +
                ", isJdk=" + isJdk +
                ", inheritedClassNames=" + inheritedClassNames +
                '}';
    }

}

**Exercise 2**

Recursion occurs when a something is defined in terms of itself.

For example the famous Fibonacci sequence is defined recursively like this:

F(n) = F(n-1) + F(n-2)

where:

F(0) = 0, F(1) = 1.

Recursion is a very useful way to solve problems in computer science and write algorithms in a compact and elegant way.

In this exercise we will use Recursion and Java Reflection to solve the problem of "Finding all Implemented Interfaces of a class".

Example:

(B, C, D, E, F, G are interfaces)

The class A implements the interfaces B, C and D.

However because:

Interface B extends E

Interface C extends F

Interface D extends F and G

"All the implemented interfaces of the class A" are:

{B, C, D, E, F, G}

Using Java Reflection and Recursion, implement the following method to return all the implemented interfaces of a given class:

Solution
import java.util.\*;
public class Exercise {

    /**
     * Returns all the interfaces that the current input class implements
     * Note: If the input is an interface, the method returns all the interfaces the
     * input interfaces extends
     */
    public static Set<Class<?>> findAllImplementedInterfaces(Class<?> input) {
        Set<Class<?>> allImplementedInterfaces = new HashSet<>();

        Class<?>[] inputInterfaces = input.getInterfaces();
        for (Class<?> currentInterface : inputInterfaces) {
            allImplementedInterfaces.add(currentInterface);
            allImplementedInterfaces.addAll(findAllImplementedInterfaces(currentInterface));
        }

        return allImplementedInterfaces;
    }

}

**Object Size Calculator**

There are many ways to measure/estimate the size of an object in Java, such as building an instrumentation agent, calculating the free JVM memory before and after forcing a garbage collection and others.

In this exercise we are going to use Java Reflection to estimate the size of an object of any class.

Input : An object of some class

Output: An estimate in bytes of the amount of memory the object is taking in the JVM.

Formula

Size Of(Object) = {header size} + {object reference} +{sum of the sizes of all its fields}

Assumptions:

header size = 12 bytes

Object reference = 4 bytes (that is correct for JVM with heap size smaller than 32 GB which is good enough for our estimation)

To keep things simple the only types of fields our class can have are:

int

byte

long

double

float

short

String

(Others can be easily added later)

Also we can assume that the class does not inherit any fields from super classes

For example given this class:

public class AccountSummary {
private final String firstName;
private final String lastName;
private final short address;
private final int salary

    public AccountSummary(String firstName, String lastName, short age, int salary) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.age = age;
        this.salary = salary
    }

}
And this object as input:

AccountSummary accountSummary = new AccountSummary("John", "Smith", 20, 100_000);
The estimate for size of the object will be

SizeOf(accountSummary) = {header} + {reference} + SizeOf{firstName} +SizeOf{lastName} + SizeOf{address} + SizeOf{salary} =

12 + 4 + (12 + 4 + 4) + (12 + 4 + 5) + 2 + 4 = 63

A method to calculate the size of each of the supported types is provided for your convenience

Solution
import java.lang.reflect.\*;

public class ObjectSizeCalculator {
private static final long HEADER_SIZE = 12;
private static final long REFERENCE_SIZE = 4;

     public long sizeOfObject(Object object) throws IllegalAccessException {
        long size = HEADER_SIZE + REFERENCE_SIZE;

        for (Field field : object.getClass().getDeclaredFields()) {
            field.setAccessible(true);
            if (field.getType().isPrimitive()) {
                size += sizeOfPrimitiveType(field.getType());
            } else {
                size += sizeOfString((String) field.get(object));
            }
        }
        return size;
    }

/******\*\*\******* Helper methods **************\*\*\*\***************/  
 private long sizeOfPrimitiveType(Class<?> primitiveType) {
if (primitiveType.equals(int.class)) {
return 4;
} else if (primitiveType.equals(long.class)) {
return 8;
} else if (primitiveType.equals(float.class)) {
return 4;
} else if (primitiveType.equals(double.class)) {
return 8;
} else if (primitiveType.equals(byte.class)) {
return 1;
} else if (primitiveType.equals(short.class)) {
return 2;
}
throw new IllegalArgumentException(String.format("Type: %s is not supported", primitiveType));
}

    private long sizeOfString(String inputString) {
        int stringBytesSize = inputString.getBytes().length;
        return HEADER_SIZE + REFERENCE_SIZE + stringBytesSize;
    }

}

**Reading Array**

In this exercise, we are going to implement a method to read an array from both the beginning and the end.

Input:

An object of type array

Index of the element we want to read. The index can be both positive, negative, and zero.

Output:

If the index is non-negative, the method returns the element at the given index, counting from the beginning of the array.

If the index is negative, the method will return the element at the given index from the end of the array.

For example for the inputs:

int [] input = new int[] {0, 10, 20, 30, 40};

and the index = 3.

The output is 30

For the inputs:

String[] names = new String[] {"John", "Michael", "Joe", "David"};

and index = -1;

The output is "David".

Solution
import java.lang.reflect.\*;
public class Exercise {

    public Object getArrayElement(Object array, int index) {
        if (index >= 0) {
            return Array.get(array, index);
        }
        int arrayLength = Array.getLength(array);
        return Array.get(array, arrayLength + index);
    }

}

**Smart Array Concatenation**

In this exercise we are going to implement a method that performs "smart concatenation" of elements.

Input:

The type T representing the the type of elements the method should return.

A variable number of arguments.

The arguments can be of:

Some type T

An array of type T

A combination of arrays of type T and elements of type T.

Output : A flattened array containing all the input elements of type T.

Example 1:

Integer [] result = concat(Integer.class, 1,2,3,4,5);

Result will be an array 5 integers containing the following elements: [1,2,3,4,5]

Example 2:

int [] result = contact(int.class, 1, 2, 3, new int[] {4, 5, 6}, 7);

Result will be an array of 7 integers, containing the elements: [1, 2, 3, 4, ,5, 6, 7];

Example 3:

String [] result = contact(String.class, new String[]{"a", "b"}, "c", new String[] {"d", "e"});

Result will be an array of 5 Strings, containing the elements : ["a", "b", "c", "d", "e"]

Note: You can always assume that the given type as the first argument is assignable from any element given as an input to the method.

Useful links : java.lang.reflect.Array

Solution
import java.util._;
import java.lang.reflect._;

public class ArrayFlattening {

public <T> T concat(Class<?> type, Object... arguments) {
if (arguments.length == 0) {
return null;
}

        List<Object> elements = new ArrayList<>();
        for (Object argument : arguments) {
            if (argument.getClass().isArray()) {
                int length = Array.getLength(argument);

                for (int i = 0 ; i < length ; i++) {
                    elements.add(Array.get(argument, i));
                }
            } else {
                elements.add(argument);
            }
        }

        Object flattenedArray = Array.newInstance(type, elements.size());

        for (int i = 0; i <elements.size() ; i++) {
            Array.set(flattenedArray, i, elements.get(i));
        }

        return (T)flattenedArray;
    }

}

**Chap 5 methods and discovery** (simple Testing Framework)

In this exercise, we are going to build a simple, general-purpose testing framework.

The input to our framework is a class containing test cases, where each method is an isolated test.

An example of an inputs test class:

/\*\*

- Represents a test suite for testing the PaymentService
  \*/
  public class PaymentServiceTest {
  private PaymentService service;
      public static void beforeClass() {
          // Called in the beginning of the test suite only once
          // Used for all tests need to share computationally expensive setup
      }

      public void setupTest() {
          // Called before every test
          // Used for setting up resource before every test
      }

      public void testCreditCardPayment() {
          // Test case 1
      }

      public void testWireTransfer() {
          // Test case 2
      }

      public void testInsufficientFunds() {
          // Test case 3
      }

      public static void afterClass() {
          // Called once in the end of the entire test suite
          // Used for closing and cleaning up common resources
      }
  }

Input to our framework is a class object

We need to:

If a method called beforeClass() is present, it needs to be called once, at the beginning of the test suite.

If a method with the name setupTest() is present it needs to be called before every test.

Every method that starts with the name test is considered a test case. We need to run each of those tests one after another. A separate object of the test class should be created for each test invocation. The order does not matter.

If a method called afterClass() is present, it needs to be run at the end of the test suite, only once.

Any other methods are considered helper methods and should be ignored.

Note: A proper beforeClass(), afterClass(), setupTest() and test\*() method has to be

Public

Take zero parameters

Return void.

If either of those conditions are not met, the method should be ignored.

Assumptions

The test class is guaranteed to have a single declared empty constructor.

Solution

import java.util._;
import java.lang.reflect._;

public class TestingFramework {
public void runTestSuite(Class<?> testClass) throws Throwable {
Method[] methods = testClass.getMethods();

        Method beforeClassMethod = findMethodByName(methods, "beforeClass");

        if (beforeClassMethod != null) {
            beforeClassMethod.invoke(null);
        }

        Method beforeEachTestMethod = findMethodByName(methods, "setupTest");

        List<Method> testMethods = findMethodsByPrefix(methods, "test");

        for (Method test: testMethods) {
            Object testObject  = testClass.getDeclaredConstructor().newInstance();

            if (beforeEachTestMethod != null) {
                beforeEachTestMethod.invoke(testObject);
            }

            test.invoke(testObject);
        }

        Method afterClassMethod = findMethodByName(methods, "afterClass");
        if (afterClassMethod != null) {
            afterClassMethod.invoke(null);
        }
    }

    private Method findMethodByName(Method[] methods, String name) {
        for (Method method : methods) {
            if (method.getName().equals(name)
                && method.getParameterCount() == 0
                &&  method.getReturnType() == void.class) {

                return method;
            }
        }
        return null;
    }

    private List<Method> findMethodsByPrefix(Method[] methods, String prefix) {
        List<Method> matchedMethods = new ArrayList<>();

        for (Method method : methods){
            if (method.getName().startsWith(prefix)
                && method.getParameterCount() == 0
                &&  method.getReturnType() == void.class) {

                matchedMethods.add(method);
            }
        }
        return matchedMethods;
    }

}

**chap 6 java modifier and discovery analysis**

In this exercise, we are going to improve our JSON serializer from a previous lecture.

Input: An object of some class that contains data fields.
Output: A JSON representation of the given object.

Example:

Given this class:

public class Address {
private final String street;
private final short apartment;

    public Address(String street, short apartment) {
        this.street = street;
        this.apartment = apartment;
    }

}
And an object of this class:

Address address = new Address("Main Street", (short) 1);
Our JSON serializer will return the following JSON string representation of the address object.

{"street":"Main Street","apartment":1}

However this time, our class or object may contain fields that we DO NOT want to serialize.

static fields belong to the class and not the object so they should not be serialized

Fields marked as transient should be ignored in the serialization process as well

For example

public class Address {
private static final int ZIP_CODE_MAX_DIGITS = 5;
private final transient String zipCode;
private final String street;
private final short apartment;

    public Address(String street, short apartment, String zipCode) {
        this.street = street;
        this.apartment = apartment;
        this.zipCode = zipCode;
    }

}
And the following object

Address address = new Address("Main Street", (short) 1, "12345");
Our JSON serializer will return the following JSON string representation of the address object.

{"street":"Main Street","apartment":1}

Please update the following code to ignore fields that are:

static

transient

Note: For simplicity, the class of the provided object can contain only the following types:

String

int

boolean

long

short

double

float

No indentations are required

Solution
import java.lang.reflect.\*;

public class JsonWriter {

    public static String objectToJson(Object instance) throws Throwable {
        Field[] fields = instance.getClass().getDeclaredFields();
        StringBuilder stringBuilder = new StringBuilder();

        stringBuilder.append("{");

        for (int i = 0; i < fields.length; i++) {
            Field field = fields[i];
            field.setAccessible(true);

            if (field.isSynthetic()) {
                continue;
            }

            int modifiers = field.getModifiers();

            if (Modifier.isStatic(modifiers) || Modifier.isTransient(modifiers)) {
                continue;
            }

            stringBuilder.append(formatStringValue(field.getName()));

            stringBuilder.append(":");

            if (field.getType().isPrimitive()) {
                stringBuilder.append(formatPrimitiveValue(field, instance));
            } else if (field.getType().equals(String.class)) {
                stringBuilder.append(formatStringValue(field.get(instance).toString()));
            }

            if (i != fields.length - 1) {
                stringBuilder.append(",");
            }
        }

        stringBuilder.append("}");
        return stringBuilder.toString();
    }

    private static String formatPrimitiveValue(Field field, Object parentInstance) throws IllegalAccessException {
        if (field.getType().equals(boolean.class)
                || field.getType().equals(int.class)
                || field.getType().equals(long.class)
                || field.getType().equals(short.class)) {
            return field.get(parentInstance).toString();
        } else if (field.getType().equals(double.class) || field.getType().equals(float.class)) {
            return String.format("%.02f", field.get(parentInstance));
        }

        throw new RuntimeException(String.format("Type : %s is unsupported", field.getType().getName()));
    }

    private static String formatStringValue(String value) {
        return String.format("\"%s\"", value);
    }

}

**Chap 7 annotations with java reflection**(Annotations discovery)

In this exercise, we are going to practice creating annotations and discovering them using Java Reflection.

Task 1

You have an incomplete annotation declaration called @OpenResources.

Complete the annotation declaration so that it is

Discoverable at runtime by Java Reflection

Can be applied to methods only.

Notes: Do not change the annotation name, location or access modifier

Task 2

You are provided with an object of some class. Your task is to find all the declared methods, annotated with @OpenResources annotation, and return them from the method.

Solution
import java.lang.annotation._;
import java.lang.reflect._;
import java.util.\*;

public class Exercise {

    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.METHOD)
    public @interface OpenResources {
    }

    public Set<Method> getAllAnnotatedMethods(Object input) {
        Set<Method> annotatedMethods = new HashSet<>();

        for(Method method : input.getClass().getDeclaredMethods()) {
            if (method.isAnnotationPresent(OpenResources.class)) {
                annotatedMethods.add(method);
            }
        }

        return annotatedMethods;
    }

}

**security with annotation part-1**

In this two parts series of exercises, we are going to implement role-based protection against unauthorized access and restricted operations.

For example, we have an API class that allows access to different parts of a company database.

@Permissions(role = Role.CLERK, allowed = OperationType.READ)
@Permissions(role = Role.MANAGER, allowed = {OperationType.READ, OperationType.WRITE})
@Permissions(role = Role.SUPPORT_ENGINEER, allowed = {OperationType.READ, OperationType.WRITE, OperationType.DELETE})
public class CompanyDataStore {
private User user;

    public void CompanyDataStore(User user) {
        this.user = user;
    }

    // Different Database access operations

}

The class is annotated with the @Permissions annotation which indicates what kinds of operations each type of user can perform.

Example:

@Permissions(role = Role.CLERK, allowed = OperationType.READ)
means that a user whose role is Clerk can only perform read operations from the database.

@Permissions(role = Role.MANAGER, allowed = {OperationType.READ, OperationType.WRITE})
means that a user whose role is Manager can perform operations that read, write or both.

Task

Complete the implementation of the @Permissions annotation that annotates an API class and describes the operations which role can perform.

The annotation should be

Restricted to classes or interfaces only.

Discoverable by Reflection at runtime

Repeatable

Notes

Do not change the annotation name or access modifiers

Solution
import java.lang.annotation.\*;
public class Annotations {

    @Target(ElementType.TYPE)
    @Repeatable(PermissionsContainer.class)
    public @interface Permissions {
        Role role();
        OperationType[] allowed();
    }

    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    public @interface PermissionsContainer {
        Permissions [] value();
    }

}

**Part 2**

In these two parts series of exercises, we are going to implement role-based protection against unauthorized access and restricted operations.

In this second part, we are going to implement a PermissionsChecker class with a single checkPermissions() method that checks if the user has the right permissions to execute a particular method of an API class.

For example, we have an API class that allows access to different parts of a company database.

@Permissions(role = Role.CLERK, allowed = OperationType.READ)
@Permissions(role = Role.MANAGER, allowed = {OperationType.READ, OperationType.WRITE})
@Permissions(role = Role.SUPPORT_ENGINEER, allowed = {OperationType.READ, OperationType.WRITE, OperationType.DELETE})
public class CompanyDataStore {
private User user;

    public void CompanyDataStore(User user) {
        this.user = user;
    }

    @MethodOperations(OperationType.READ)
    public AccountRecord[] readAccounts(String [] accountIds) {
        PermissionsChecker.checkPermissions(this, "readAccounts");
        // ...
    }

    @MethodOperations({OperationType.READ, OperationType.WRITE})
    public void updateAccount(String accountId, AccountRecord record) {
        PermissionsChecker.checkPermissions(this, "updateAccount");
        // ...
    }

    @MethodOperations(OperationType.READ)
    public Summary readAccountSummary(Role callerRole, String accountId) {
        PermissionsChecker.checkPermissions(this, "readAccountSummary");
        // ...
    }

    @MethodOperations(OperationType.DELETE)
    public void deleteAccount(String accountId) {
        PermissionsChecker.checkPermissions(this, "deleteAccount");
        // ...
    }

}
A user is represented by an object of type User:

public class User {
private final String name;
private final Role role;

    public User(String name, Role role) {
        this.name = name;
        this.role = role;
    }

    public Role getRole() {
        return role;
    }

}
And is passed in the constructor of the CompanyDataStore for further access to the database.

Each method in an object of the CompanyDataStore is annotated with the @MethodOperations annotation which represents the types of operations the method performs against the database.

Example:

A method annotated with @MethodOperations(OperationType.DELETE) deletes record(s) from the database and requires the user to have permission to delete.

A method annotated with @MethodOperations({OperationType.READ, OperationType.WRITE}) both reads and writes from the database and requires that the current user has access to both read and write from/to the database.

Task

Implement the PermissionsChecker.checkPermissions(..) method.

The method should:

Check the operations the current method performs

Check that the logged-in user is allowed to perform those operations, based on the user's role.

If the user is allowed to perform the method operations, the method returns nothing.

If the user is not allowed to perform the method operations, a PermissionException should be thrown

Solution

import java.util._;
import java.lang.reflect._;
import internal._;
import static internal.Annotations._;

public class PermissionsChecker {

    /**
     * Checks that the logged-in user in the callerObject has the right permissions to perform the operations
     * in the caller method.
     * Throws PermissionException if the user is not authorized to perform those operations based on the user's
     * role
     */
    public static void checkPermissions(Object callerObject, String callerMethodName)
        throws Throwable {
        // DO NOT MODIFY THIS METHOD

        User user = getLoggedInUser(callerObject);
        Method callingMethod = getCallingMethod(callerObject, callerMethodName);
        Permissions[] allPermissions = getClassAnnotatedPermissions(callerObject);
        MethodOperations methodOperations = getCallerMethodOperations(callingMethod);

        OperationType[] methodOperationTypes = methodOperations.value();

        List<OperationType> userAllowedOperations = findUserAllowedOperations(allPermissions, user);

        for (OperationType methodOperationsTypes : methodOperationTypes) {
            if (!userAllowedOperations.contains(methodOperationsTypes)) {
                throw new PermissionException();
            }
        }
    }

    /**
     * Returns a List of all OperationTypes that the logged-in user is allowed to perform
     * If the user has no permissions in the allPermissions, an empty list should be returned
     */
    static List<OperationType> findUserAllowedOperations(Permissions[] allPermissions, User user) {
        for (Permissions currentPermissions : allPermissions) {
            if (user.getRole().equals(currentPermissions.role())) {
                return Arrays.asList(currentPermissions.allowed());
            }
        }
        return Collections.emptyList();
    }

    /**
     * Returns all the Permissions annotations the the callerObject's class is annotated with
     */
    static Permissions[] getClassAnnotatedPermissions(Object callerObject) {
        Class<?> callerClass = callerObject.getClass();

        return callerClass.getAnnotationsByType(Permissions.class);
    }

    /**
     * Returns the MethodOperations annotation that the callerMethod is annotated with
     */
    static MethodOperations getCallerMethodOperations(Method callerMethod) {
        return callerMethod.getAnnotation(MethodOperations.class);
    }

/**********\*********** Helper Methods **************\*\***************/

    /**
     * Returns the User object representing the logged-in user
     */
    private static User getLoggedInUser(Object callerObject)
        throws NoSuchFieldException, IllegalAccessException {

        Class<?> callerClass = callerObject.getClass();

        Field userField = callerClass.getDeclaredField("user");

        userField.setAccessible(true);

        if (!userField.getType().equals(User.class)) {
            throw new IllegalStateException("The caller object must have a user field of type User");
        }

        return (User) userField.get(callerObject);
    }

    /**
     * Returns the Method object of the callerObject's class corresponding to the methodName
     */
    private static Method getCallingMethod(Object callerObject, String methodName) {
        return Arrays.stream(callerObject.getClass().getDeclaredMethods())
                .filter(method -> method.getName().equals(methodName))
                .findFirst()
                .orElseThrow(() -> new IllegalStateException(String.format("The passed method name :%s does not exist", methodName)));
    }

}

**Chap 8 proxy (dynamic caching proxy)**

In this exercise, we are going to build a Caching Proxy.

This kind of proxy would allow us to cache the results of specific methods whose results don't change throughout the lifetime of the process.

We want to extend this caching functionality to any object of any interface in our application, so we are going to use a Dynamic Proxy for the implementation.

Given an interface, a result from any method annotated with the @Cacheable annotation should be:

Cached if it is not in the cache already

Read from the cache instead of invoking the method

As a result, every cacheable method should be invoked only once.

Example of an interface:

public interface DatabaseReader {

    void connectToDatabase();

    @Cacheable
    String readCustomerIdByName(String firstName, String lastName) throws IOException();

    int countRowsInCustomersTable();

    void addCustomer(String id, String firstName, String lastName) throws IOException();

    @Cacheable
    Date readCustomerBirthday(String id);

}
Since a customer's ID does not ever change in our system, we can mark the readCustomerIdByName(..) method Cacheable to save on consecutive database reads for the same (firstName, lastName) pairs.

Similarly, a customer's birthday does not change so we can also mark the readCustomerBirthday(..) method as Cacheable.

Cache details:

Every method has its own cache.

The key to our cache is a list of arguments passed into the method. (since different method arguments may produce a different result

The value of the cache is the result returned from the method

The arguments and the result may be of any type

Complete the following implementation of the Dynamic Caching Proxy InvocationHandler.

Note:

The dynamic proxy should throw the original target exceptions in case they are thrown.

Solution
import java.lang.annotation._;
import java.lang.reflect._;
import java.util.\*;

public class CachingInvocationHandler implements InvocationHandler {

    /**
     * Map that maps from a method name to a method cache
     * Each cache is a map from a list of arguments to a method result
     */
    private final Map<String, Map<List<Object>, Object>> cache = new HashMap<>();
    private final Object realObject;

    public CachingInvocationHandler(Object realObject) {
        this.realObject = realObject;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        try {
            if (!isMethodCacheable(method)) {
                return method.invoke(realObject, args);
            }

            if (isInCache(method, args)) {
                return getFromCache(method, args);
            }

            Object result = method.invoke(realObject, args);

            putInCache(method, args, result);

            return result;
        } catch (InvocationTargetException e) {
            throw e.getTargetException();
        }
    }

    boolean isMethodCacheable(Method method) {
        return method.isAnnotationPresent(Cacheable.class);
    }

/**************\*\*\*************** Helper Methods ************\*\*************/

    private boolean isInCache(Method method, Object[] args) {
        if (!cache.containsKey(method.getName())) {
            return false;
        }
        List<Object> argumentsList = Arrays.asList(args);

        Map<List<Object>, Object> argumentsToReturnValue = cache.get(method.getName());

        return argumentsToReturnValue.containsKey(argumentsList);
    }

    private void putInCache(Method method, Object[] args, Object result) {
        if (!cache.containsKey(method.getName())) {
            cache.put(method.getName(), new HashMap<>());
        }
        List<Object> argumentsList = Arrays.asList(args);

        Map<List<Object>, Object> argumentsToReturnValue = cache.get(method.getName());

        argumentsToReturnValue.put(argumentsList, result);
    }

    private Object getFromCache(Method method, Object[] args) {
        if (!cache.containsKey(method.getName())) {
            throw new IllegalStateException(
                String.format("Result of method: %s is not in the cache", method.getName()));
        }

        List<Object> argumentsList = Arrays.asList(args);

        Map<List<Object>, Object> argumentsToReturnValue = cache.get(method.getName());

        if (!argumentsToReturnValue.containsKey(argumentsList)) {
            throw new IllegalStateException(
                    String.format("Result of method: %s and arguments: %s is not in the cache",
                    method.getName(),
                    argumentsList));
        }

        return argumentsToReturnValue.get(argumentsList);
    }

}
