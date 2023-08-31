# Ios Dev (Swift)

- the current important lib is uiKit and the SwiftUi library is emerging may be there will be paradigm shift in that watch out for.

- some of the selling points of swift is type safety, objs can't be  null, and optional ? and basically we will ve compile time errors not on the runtime errors.
- some of the other projects on swift  is tensor flow, and SSR is now possible
**types** - Int, Float, Double, String, Bool,.. and it is not a strongly typed lang so it does support type inference.
  - its a wierd let kw is used to declare a constant ex let myName ="jon"; can't mutate the myName, var is the for mut var.
**arrays** some of the methods we can use on the array ex - .first, .last, .count, .append, .insert(), sort, reverse(), shuffle()
-**set** is unordered and non duplicate, since its unordered so its performant and speed, it like a HashSet and even the syntax is also similar ex - var age: set<Int> = []; and we can also pass in the existing array in the set ex - var new = Set(arr) // but here the one thing is that arr shouldn't ve any duplicate val. and even if there is any and it will not be included.
  - if something is going into set it has to confirm **Hashable** the hash val is an identifier and its a direct point to the location. so it will be help in searching any particular in the set for ex if we search for num 34 in the set it will go to the hash val and find the num 34 where as in arr it will loop thru the entire array and then finds our the val.
  - some of the fns we can use on set are contains(), insert(), 

-**Dictionary** - have kvp ex - var dev: [String, String] = ["iphone" : "ios"]

- **functions**

- its wierd when we call the fn again we ve to pass the param ex func add(x:Int,to y:Int) -> Int { let sum: x + y; return sum } // then add (x: 5 to: 30) // is the call site the **to** is the arg label. in case if we don't ve the param label then we ve to call the fn like add(x:5, y:4);  // so the arg label is used at the call site.
  - we can assign any name we want for that parameter label.
  - 
- **range** operator is triple dots instead of double ex for item in 24...30 {} or two times for the conditional ex for i in 23..<32 {}
- we can use rand fn in Int ex - let rand = Int.random(in:0...100)
- **enum** group of val related together ex - enum phone { case iphone, pixel, google } and for the enum every val starts with the case kw. and to access the mem var of the enum use . dot ex - .iphone
  - string interpolation is little different ex print("it seems like \(num)" )
  - ?? - is nil coalescing operator
- **guard** kw is used ex - guard let age = ages.last else { return } // if the val of the age is nil it will stop exec the fn and it will not proceed further down the scope. and it has access to all the scopes inside the fn unlike the if let. 
- **force unwrap** - ! ex - let age = ages.last!; // its like saying if the ages has val or not, nil use it whatever. // this force unwrapping can lead to lot of errors in our code.


- **self** - refers to itself or instance of itself. whatever the instance of the class. its like the this kw in many other lang

### Class 

- is an instance of an object. here instead of ctor we ve to use the init() and pass in the mem var of our class ex class Person { var name: String; init(name: String){ self.name = name} } and to create the instance let sean = Person(name: "jon")// 
- the second way is to pass in our mem var as the optional so the initializer will be empty and we can instant the obj as empty ex let sean = Person(); //still we can access the prop of the person.

- **structs** are basically performance, and they are **value type** unlike the class which is **reference type**

- **extension** will add the custom prop to the existing type. ex - extension String { inside we can define a fn to remove the white space in the sting } or whatever with the extension kw.

 **auto layout** 

- are the constraint based s/m, that helps us to layout for all the different screen sizes,

### UiKit

- ex of some of the comps are UIButton, UIViewController,  etc for the building the ui.

- **color App** - nav controller is the container that holds all our views. 
  - UITableView 