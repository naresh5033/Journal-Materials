# TS

- all the codes can be found in in this [Repository](https://github.com/gitdagray/typescript-course.git)
  
- is a Js with the syntax for types.. was created by Ms and the person who created c# is also developed ts.
- `tsc main.ts -w` to watch for any changes in the file and this will gen main.js.. bts.. or `tsc -w` to watch all the files in the root directory.
- `tsc --init` for initializing the tsconfig.json file .. in the ts config file we can specify the target prop es2015 or es05 for the older browser compatible.
- as we know ts is strongly typed and the strongly typed has 2 type dynamic and static .. ts is statically typed means types are checked at **compile time.** in contrast js is dynamic typed in which the types are checked at runtime.
- by default if we don't specify the type we want to assign the var, the ts will assume of it as **any** type.
- there is also **RegEx** type - /\w+/g; when we use some val like this ts will automatically infers the type as regex.
- ex - in a array of type num | string we can add or push those two types regardless of the position, as long as they are either of the type (they are not locked in the position or idx )

**tuple**
- its like array except with the position ex - const tup : [string, number, boolean] = ["a", 2, true]; the idx position matters
- in the array its like - [string | number | boolean] so we can pass the data regardless of the position.
- we can assign the array to a tuple ex - let tup = arr; but we can't assign the tuple to the array ex let tup = arr; // can't assign coz there is a possibility that array may or may not ve the len of the tuple // the tuple is the **fixed** len that we specified.

- **object types** - ex - let num: object; and we can assign an array..
  - ex - type num = { a: (string | number)[] } // this array supports any vals as long as the type is string or number.
- **Interface** is same as the type but when to use interface and when to use types ? 
  - the type is more like the **alias** but interface is more like the class or obj 
  - and we can assign a type (alias to the fn) ex - type mathFunc =(a: number, b: number) => number; // now we can use this fn type to assign to any fn. ex - let mul: mathFunc = function(c, d) { return c * d;} // it infers the math fn to the mul.
  - note: we can't assign a default parameter to the type alias fn. or also with the interface, the default vals won't work
  - ex - we can assign the type like this type num = string | number; and we can use this type  (num) to assign any var but we can't do it with the interface.
- **Enums** unlike the most of ts features, enums are not type level addition to the js but something added to the lang and the runtime.
      - enums are enumerated and has the "idx" but we can assign the default idx position ex - enum { a = 1, b, c } // now the idx starts with 1. also b and c will adjust their idx position accordingly    

- we can assign a optional param to the fn ex - let num = function(a:number, b:number, c?:number): number => { return a + b + c; } // it will throw error we ve to use the **type guard** since the c is type of num or undefined ex - if(typeof c !== 'undefined' ) then the return statement works.

- **Rest params** just like the spread operator, means the **rest of the params** ex let num = (...a:number[]): number => { return a.reduce((prev, curr) => prev + curr)}// now we can pass in any nums we want ex num(1,2,3,4); and the result is 10.

- **never type** - this is for the fns that explicitly throw an exception/error. ex - let err = (a:string) => { throws new Error("")}// now ts will infer this fn as the never type if we want we can explicitly assign the return type as "never" for that fn
  - and also if the fn has infinite or endless loop then it will be of never type. ts will infer automatically. 
  - this should also be useful in ex - let func =(val: number | string) => { if (typeof val === "number")return 'number'; if (typeof val === "string") return 'string'; // we can do it since this fn needs to return str, so what we can do is return createError('') // now this will work since we re returnig error ts will infer this as the **never type**
  - and inside the switch when we  re exhausting the type we can use this never as the default type. 
    - means we can use narrowing and rely on never turning up to do exhaustive checking in switch statements.
- 
### chap 5 type assertion and type casting.

- type assertion - convert to more or less specific type. we can do it with the **as** kw ex let b = a as Two; refer code // less specific type
- we can also use with the angle brackets ex let d =<number>12; but we can't use this angle bracket syntax with ts in react.

- **unknown** is also referred to forced casting or double casting, it will use double assertion. unknown type is something we can't really use unless
  - ex - we can't cast this 10 as string; but we can do.. (10 as unknown) as string. // double casting
  - in the dom if we can use assertion ex - const img = document.querySelector('img') as HTMLImageElement; // since ts knows its only the html elem or null so we ve to assert as html img elem.
    - or we can do **non null assertion** by putting ! at the end ex - const img = document.querySelector('img')**!**; // now its non null.
    - or we can also do const img = <HTMLImageElement>document.querySelector('img') // but this will not work in **tsx** files in react.
  - note : when our obj or var is null then we can use the type guard (if condn) ex if(year){} // with this type guard now our obj with null will works.
  - refer the code for the couple of variations we can use with the assertion.
- 
### chap 6. classes

  - as we know the class mem vars are public by default, and if we want if we don't wanna specify the mems at the class level scope but in the ctor's scope then we ve to use implicitly use the public kw for the vars
  - ex - class mem { constructor(public a,public b,public c){ this. a = a, this. b = b, this. c = c} // lly we can use differenct modifier such as Readonly, private, protected to the params of the ctor, so this way we don't ve to set the mem vars at the class level
    - by doing this even the assignments inside the constructor are not needed or not necessary
    - and if we wanna use the constructor assignment then use the super() and pass all the params (that doesn't ve any access specifier in the ctor if there is one then no need to pass) inside then assign the vals.
    - 
  - "implementing interface to the class." - 
  - **getters and setters** if we ve a private mem var inside the class, then we can use the getters and setters to read and write the mem vars regardless of the access specifier.
    - but when we set the setters it should not return any vals, we can leave it empty if we want. 
    - 
### index signatures and key of assertion
  - we can make a inx signature in the interface ex - interface txObj { [index: string]: number } // its saying like the all of the keys will be str and all of the vals will be number.
  - note : the keys can be of any types except boolean, but the vals can be of any types. 
  - when we ve the indx signature, then we can pass in any props we want while inheriting the interface, refer the code.
  - if we don't ve the indx signature then we can use the **keyof** kw ex - for (cost i in student){console.log(`${student[i as keyof[Student]]}`)}; //this will now this will work since we don't ve any index signature in the student obj.
  - the keyof will creates a union type of string literals which will allow us to loop thru the obj(mem vars)

- the alternate impl of this is ex - object.keys(student).map(key => {student[key as keyof typeof Student ])}; // keyof and we don't know the typeof so we re retrieving the type of by referencing the obj(student) itself.

- lets see one more ex with the function. ex - const logStudent = (student:Student, key: keyof Student):void => {console.log(`${student[key]}) // as we defined the key as key of student, once again it defines the key as str literals. and that are made up of diff names inside the student obj.

- lets see one more ex of index signature with types ex - type Streams = 'salary' | 'bonuses' | 'short'; type Income = Record<Stream, number>
  - but with this approach while looping we ve to use the assertion even though we ve defined everything (in the Record type) 
    - ex - const monthlyIncome : Incomes  = { salary: 500. bons: 200, short: 21 }; for(const i in monthlyIncome){ console.log(montlyIcome[i as keyof Incomes]); } // so if we re using the record utility type then we ve to use the assertion ..
### Generics

- lets create a generic fn that works with any type - const echo = <T>(arg: T): T => arg; // its useful for the utility fn when we re not sure what type to pass in and let the compiler decides based on the args that we passed.
- **!! double bang operator** - this will takes 0 essentially and then flip it and flip it back, that'd makes true or false instead of saying like 0 or 1, it will takes any thing and returns bool.
- check out the code for the diff implementation, "specially the one with the return type as the obj"

### Utility Types (chap 9)

- ts offers many utility types that are helpful for common type transformations.

- **partials** - when we ve many props in a interface where we don't wanna use every one of em, but instead wanna use some of them, and we can use the **partial** kw. 
- **Required and readOnly** - this required all of the properties that we specified. and lly the ReadOnly property we can't override.
- **Record Type** has the kvp
- **pick and Omit** - we can use the pick to pick the props we wanna use ex - type stud = Pick<IAssignment, "id" | "grade"> // we re picking only those 2 props from the assignment interface.
  - lly the omit will do the same thing except it will omit the prop we want..
  - **Exclude and extract** aren't gon work with the interface, they will work with the string literals. ex - type num = Exclude<Grades, "u">; // will exclude the str literal u from the grades union.
    - lly the extract will extract only the str literal we want to add and omit all the other ex - type high = Exclude<Grades, "A" | "B">; // now our high will ve only the A and B and the other vals are not included.
- **Non Nullable** - if we ve the union type of which also has undefined and null then we can use the non nullable to exclude the null or undefined .. ex type grade = NonNullable<Grades>;

- **Return Type** this is useful with the fns specially from some external libraries,  ex - type NewAssign = ReturnType<typeof createAssingn> // now if we use this as the return type in any fn and the val will updated as the create assgin changes..

- **Parameter type** we can derive the type from a fn param. ex - type num = Parameter<typeof Assignment> ; 

- **Awaited type** helps us with the return type of a promise.. ex - type FetchUserReturnType = Awaited<ReturnType<typeof fetchUser>> ; // here this fetchUser has the return type of promise, so we ve to use the Awaited. to resolve the promise.

### chap 11 project with vite

- its faster and easy to work with several lib projects..
- `npm create vite@latest` and select the ts project. then `cd ./ && npm i`
- this is great tool to init our project..
 - in the project inside the model class we re creating **singleton instance** by using static.. ex - static instance: ListTemplate = new ListTemplate();
   - now we can use this singleton in our project main.ts initApp() - ex - const fullList = FullList.instance;
### chap 12 ts with react 

- when we re using the generics inside the jsx comp, the ts will not recognize, so we ve to use it like const list = **<T extends {}>**({items, render}: ListProps<T>) => {}// this T extends empty obj will solve the prob.
- now the ts will recognize. or the easier way'd be just add the comma after the T ex - **<T,>** this will do the same..

### chap 13 react proj / hooks

- the use effect hook has some wierd behaviour, when we re in the dev mode(strict mode) it will mounts, unmounts and remount so it will mounts twice.
- useCallback - will memoize the fn, so its not recreate it.. ex if we don't want to rerender the fn we can put em in the usecallbacks and it always delivers the same comp.
- useMemo() as we know it will memoize the fn, for ex if the fn is costly/expensive to call we can memoize it. and the hook will memoize the result.
### chap 14 useReducer hook
- useReducer is a alternative to the useState when we ve complex logic that involves complex sub vals.
- the reducer will actually ve the switch case 
- "null coleskine operator" **??** this can be used in like if its undefined then its string ex - action.payload?? ""

### chap 15 useContext hook
- every context needs a provider - and the convention is to end with the Provider..  

### chap 16 react + ts project (cart)

- to launch the json server `npx json-server -w data/product.json -p 3500`
### chap 17 proj part 2 

--------------------------------

### Type narrowing

- the **in narrow operator** could be more useful for the interface scenarios where we inherit from multiple interfaces and check to see any particular prop is available in which interface.
  - ex - function isAdmin(account: Admin|User){ if ("isAdmin" in Admin) { return IsAdmin} // here we re check to see if the isAdmin prop is in the Admin intterface.

- **Instance Of** with this narrowing we can find the whether its an instance or not, or anything with the **new kw**
  - ex - function logVal(x: Date | string) { if (x instanceof Date) { return console.log(x); } // x is the Date which is like let x:Date = new Date; //so this instance of type narrowing work on x .

- **Type predicate**
  - the kw **is** ex - function isFish(pet: Fish|Bird): pet is Fish{} // here we re strongly telling that pet is the type fish.
- 
- 