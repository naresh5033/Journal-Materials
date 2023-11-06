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
  - and we can assign a type (alias to the fn) ex - type mathFunc =(a: number, b: number) => number; // now we can use this fn type to assign to any fn. ex - let mul: mathFunc = function(c, d) { return c \* d;} // it infers the math fn to the mul.
  - note: we can't assign a default parameter to the type alias fn. or also with the interface, the default vals won't work
  - ex - we can assign the type like this type num = string | number; and we can use this type (num) to assign any var but we can't do it with the interface.
- **Enums** unlike the most of ts features, enums are not type level addition to the js but something added to the lang and the runtime. - enums are enumerated and has the "idx" but we can assign the default idx position ex - enum { a = 1, b, c } // now the idx starts with 1. also b and c will adjust their idx position accordingly

- we can assign a optional param to the fn ex - let num = function(a:number, b:number, c?:number): number => { return a + b + c; } // it will throw error we ve to use the **type guard** since the c is type of num or undefined ex - if(typeof c !== 'undefined' ) then the return statement works.

- **Rest params** just like the spread operator, means the **rest of the params** ex let num = (...a:number[]): number => { return a.reduce((prev, curr) => prev + curr)}// now we can pass in any nums we want ex num(1,2,3,4); and the result is 10.

- **never type** - this is for the fns that explicitly throw an exception/error. ex - let err = (a:string) => { throws new Error("")}// now ts will infer this fn as the never type if we want we can explicitly assign the return type as "never" for that fn
  - and also if the fn has infinite or endless loop then it will be of never type. ts will infer automatically.
  - this should also be useful in ex - let func =(val: number | string) => { if (typeof val === "number")return 'number'; if (typeof val === "string") return 'string'; // we can do it since this fn needs to return str, so what we can do is return createError('') // now this will work since we re returnig error ts will infer this as the **never type**
  - and inside the switch when we re exhausting the type we can use this never as the default type.
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
    - ex - const monthlyIncome : Incomes = { salary: 500. bons: 200, short: 21 }; for(const i in monthlyIncome){ console.log(montlyIcome[i as keyof Incomes]); } // so if we re using the record utility type then we ve to use the assertion ..

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

- **Return Type** this is useful with the fns specially from some external libraries, ex - type NewAssign = ReturnType<typeof createAssingn> // now if we use this as the return type in any fn and the val will updated as the create assgin changes..

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

---

### Type narrowing

- the **in narrow operator** could be more useful for the interface scenarios where we inherit from multiple interfaces and check to see any particular prop is available in which interface.

  - ex - function isAdmin(account: Admin|User){ if ("isAdmin" in Admin) { return IsAdmin} // here we re check to see if the isAdmin prop is in the Admin intterface.

- **Instance Of** with this narrowing we can find the whether its an instance or not, or anything with the **new kw**

  - ex - function logVal(x: Date | string) { if (x instanceof Date) { return console.log(x); } // x is the Date which is like let x:Date = new Date; //so this instance of type narrowing work on x .

- **Type predicate**
  - the kw **is** ex - function isFish(pet: Fish|Bird): pet is Fish{} // here we re strongly telling that pet is the type fish.

---

# React hooks

### useState

- the hooks can be used only in the function component .. not in the class component
- when we use the state hook with the fn declaration then it runs only once every time our comp loads ex const [state, setState] = useState(() => { console.log("only once"); }); // so it is useful when we re really using some complex or computational task..
- and one thing to keep in mind if we use the object in the useState hook as the default val ex const [count, setCount] = useState({count: 4, theme: blue}) // then to update just one field in the obj it will entirely override the obj so we will loose the second field to make it in righ way we ve to spread and then update the field we wanted
  - ex - function decrementCount() {setState(prevState => { return { ...prevState, count: prevState.count -1 }; }); // so this way our theme field will still be intact..
- so this way our entire object will get updated, not will merge with the fields..
- or we can use separate state hooks for the fields of our obj ex - one for the count and the other for the theme..

### UseEffect

- with this hook we re telling that we want some sort of side effect when some thing happens / changes in our comp.
- and that changes(or resources) is what we put in the dependency array..
- or just leave it empty and it will loads only once during the component initialization or **onMount**
- most of the time we will use this hook for some event listener and then we can remove the event listener inside the hook (in the return statement)..

### UseMemo

- to memoize the state or vals.. or caching the val so we don't ve to re computed every single time...
- when we use the state hook / to update the states in react it will render the comp which causes some delay since it renders the comp from top to bottom.. ex when we ve slow() (the simulated fns which loops for ex millions of times) which causes delay in loading our comp..
- so we can cache the slow fns return val and memoize em .. so we don't ve to recalculate the slow() over and over again..
- ex - useMemo(() => { return slowFunction(number)}, [number])

- one caveat with this hook is it causes some performance overhead.. since its saving the val in some mem location .. so we can't use this hook for everything instead we ve to use some really computational task for caching..

- there is second use case for this useMemo is **referential Equality** means when we compare 2 diff vars in js it will compares the ref in case of objs and arrays. whenever we want to make sure the ref of the object or the array are the exact same as it was the last time we rendered .. and we only want to update the reference of the obj/ array.
- so we can use when our obj exacts same ref we wanna cache in the memo hook, and then update the changes in the useEffect .ex in the themes obj.. // the useEffect will compare the old theme style and the new theme style and they are exact same object.

- these are the 2 most common use cases for the useMemo..

### useRef

- ex - if we wanna use the i/p and to get the val that we type .. we can can think of using a using useSate but it will render the comp for every single keystroke we type in ..
- and if we set in the useEffect for the state changes it rerender and re render and causes infinite loop..
- so the state is not the right way to do.. instead we ve to use the useRef..
- the ref is just sim to the state but the diff here is it will not cause our component to **re-render** ..
- and this hook return the obj..the obj with the current prop.. and we update the current prop and that is wat get persisted diff renders..
- ex - const renderCount = useRef(1); useEffect(() => { renderCount.current = renderCount.current + 1; },);

- and the another important use case the useRef will be used is **to reference the elem** inside the html.. and as we know each elem in the html has the **ref attr**
  - ex -const inputRef = useRef(); <input ref={inputRef} // and if we use this var inside a fn ex - fucntion foucus() { inputRef.current.focus() } // this i/p.current referred the entire i/p el.. so we can access the val from the i/p attr.. ex - inputRef.current.value; //
  - but it will update the ref of the val not the state (so we don't wanna do this) and lly append child we don't want to append with the use ref instead with the jsx..
-
- another use case of the ref is to store the prev val of our state ex - const prevName = useRef(''); useEffect(()=> prevName.current = name},[name]) // where name is the i/p el val attr
- so in fn comp we need to use refs in order to persist states if we re not using useStates..

### UseContext

- the context hook itself is the context api.. to share / pass the data b/w the child and the parent w/o always depend of the prop.
- when we re using context hook in the **class comp** there is 2 sections 1. **context provider** this is where we wrap all our code in to .. ex - <ThemeContextProvider value={darkTheme} /> .. has the val prop..
- and then the **context **

- if we using it in a **function component** then we ve to use the hook not the context provider..
- the context is to store the data and having accessible in 2 comp in any where in the dom..w/o passing to props.
- the way is to create a custom context and it must ve one default val ex - export default DashBoardContext = createContext<User | undefined>(undefined);
- and to use this we ve to use it with the provider ex - <DashBoardContext.Provider value={user}> <DashBoard> <DashBoardContext.Provider/>//
- now we can get the user directly from the context is by **consuming**(consumer.. initially we provide the provider now the consumer) the context with the useContext hook - ex - const user = useContext(DashBoardContext);
- if by any case if we don't wrap the comp(or if we forget to add the content provider) with the provider then the user will be undefined .. and won't render anything.
- to bypass this we can create our custom hook that will handle all of the logic for us..
- ex - export default useUserContext() { const user = useContext(DashBoardContext); if (user === undefined) { throw new Error ("user must ve to be definded") } return user; } // by this way if we ve the user undefined then we will get the error... which is easier for us to debug.
  and now we can use our custom hook instead of the useContext() inside the comp.

### UseReducer

- just like the use state.. the use reducer will also help us to manage the state and re renders the comp whenever the state changes.
- its just like the **redux** and takes most of the boiler plate from the redux.
- generally when working with the use reducer we will pass in the obj as the initial state instead of the val ex - function reducer(state, action) {return {count: state.count +1} }; const [state, dispatch] = useReducer(reducer, {count: 1}) // and the dispatch will update our state it will call our reducer() function .. the dispatch will dispatch the action..
- now we can call the dispatch ex - function increment() { dispatch() }
  and the code we can find [here](https://blog.webdevsimplified.com/2020-06/use-reducer/)
- the good thing about the reducer() we just pass down our stuff to the one dispatch() .. that hanldes all our use cases.. and we no longer ve to pass in handle click() handle complete(), handle New() or delete() we just pass in one dispatch which will do all the things and makes our code cleaner w/o the bunch of props..

### UseCallback

- this hook is just as similar as the useMemo ..
- lets ve an ex if we ve a fn (getItems) and then we use it in another fn List() and we use this getItems function inside the app comp useEffect's dep array .. this fn will recreated every single time in the app comp (when we change the list())..
- and we we pass this fn into the list() getItem() a brand new fn.. and its recreated every time.. this is where we wanna use the useCallback .. ex - const getItems = useCallback(() => { return [number, number +1, number +2]}, [number]) // now this will render only when the number changes.
- the one difference between the useMemo and the useCallback is useMem will take fn and will return the returned val of the fn..
- but useCallback is diff it will take a fn and that is what it will return.
- so in the above ex if we use useMemo .. the getItem is set to the array (in return), instead of setting to the entire fn..
- since its(useCallback) returning the fn we can pass in param for the fn inside the hook..
- the only reason we ever want to use the useCallback is **referential equality**
- and the other reason we can think of using the useCallback hook is.. when the fn is actually really slow..

### Custom Hook

- how to create our own hook.. to store the vars in the local storage..
- make our fn with the prefix of use so react will do all the linting and pre check for us..
- ex - export default function useLocalStorage(key, initialValue) { const [value, setValue] = useState(''); return [value, setValue]; } //now we can use this hook as the same as the useState hook..
- and to get the stored val .. function getSavedValue(key, initialValue) { JSON.parse(localStorage.getItem(key)) ..// the key is what our vars is (that we store in the local storage.)
- refer the code [here](https://blog.webdevsimplified.com/2019-11/how-to-write-custom-hooks/)..
  - and we can save our local storage inside the useEffect ex - useEffect(() => { localStorage.setItem(key, JSON.stringify(value))} , [value]) return [value, setValue] ..// we need to stringify the val b4 saving to the local storage..

### UseLayoutEffect

- this hook is almost similar to useEffect .. we can use this hook when we need to set the css prop to the comp itself..
- the useEffect is asynchronous, they are not gon block the dom .. now the difference between useEffect and the useLayoutEffect is not asynchronous..
- when we use useLayoutEffect it runs synchronously b/w when react calculates our dom and when it actually prints to the screen..which means useLayoutEffect are perfect when we need to do something on the layout of our dom..ex - measure the dom el and move things into the dom that are visible to the user, we can use this hook..
- but its not performance since it is synchronous.
- so that's the main difference between useEffect and the useLayoutEffect .. whenever we re manipulating the dom in a way the user can directly see based on the measurements or other things we need to use the useLayoutEffect otherwise we may ve the slight flash/flicker that we see in the useEffect hook
- we can do the measurements of the dom with hte getBoundingClientRect() .. see the ex code [here](https://blog.webdevsimplified.com/2020-04/use-effect/)
- the useEffect runs after the dom is rendered unlike the useLayoutEffect.

### useTransition

- is meant to speed up the app and feel more responsive.. event there is lot going on.
- in that ex he simulated a list with lot of data in it which is time computation.. complex task
- and in that ex the setInput(), and the setList(which is more computation) // the react will try to combine all the diff state changes in one call and makes it all it once b4 re render our app... which is why its slow..
- instead we want to make the setInput() as the higher priority it runs ahead of time.. than our setList() with the lower priority..
- so that is what the transition hook will do it will allow us to make 2 diff state changes at the time and **rank em with** the priority.
- this hook has 2 fns (we can destruct) 1. isPending and the 2. startTransition .. ex - const [isPending, startTransition] = useTransition(); // now we can put all the computational related stuff(of the List()) inside the startTransition()..
  - ex startTransition(() => { const l=[]; for(let i=0; i<20,000; i++) { l.push(i); }); setList(l) }); // by doing this we re telling this setList() is less priority..
  - so now our high priority stuff setInput() will render first in the dom. and the low priority when it finishes the computation bts./
- refer the code [here](https://blog.webdevsimplified.com/2022-04/use-transition/)
- there is one thing we want to know about this hook is we ve to use this only when it absolutely needed .. since when we use this hook we make our app more render than the normal.. even in this above ex we ve this hook renders our app 2 times.

### UseDeferredValue hook

- the way this hooks works is similar to the debounce or throttling..
- this when we set the deffered value ex const deferred = useDeferredValue(input); and use this var means we re telling that it low priority than our input.. and computationally expense to do and we gon let it happen later..
- and this is better than the debounce or throttling since its not gon force to wait for the 200ms gap b/w the keystrokes. instead if nothing's gon happen it will work right away.
- source code and the course [here](https://courses.webdevsimplified.com/view/courses/react-hooks-simplified/1327246-custom-hooks/4121583-15-custom-hooks-1-5-usetoggle-usetimeout-usedebounce-useupdateeffect-usearray)

### useImperativeHandle hook

- we can just pass in the ref the prof of teh comp and then pass in tto the i/p attr ex - function CustInput({style, ...props}, ref) { return <input ref={ref} >
- but what if we want to completely change how the ref works and we want our own cust ref.. (not the one related to the comp but the actual cust ref) .. that is where the useImperativeHandle comes in takes 3 props.. 1. ref 2. fn (which will return a single val and the return val is the new val for our ref..) 3. dep array
- ex - fn CustInput({style, ...props}, ref) { useImperativeHandle(ref, () => { return { alert:() => alert("hi") }}, [props.value]) // now our ref is gon to the return obj (alertHi)..
- this hook we don't very often use em.. there is very specific use cases where we use it..

### UseDebugValue hook

- this makes the working with and writing the cust hooks lot easier..
- with this hook we can write some thing essential next to our hook.. some thing to know whats going on inside our hook when we re debugging it..
- **note:** this hook **only works** inside of the cust hooks.
- this hook takes in 2 params 1. the param of the fn and 2.the fn itself ex - useDebugValue(value, v => getValueSlowly(v)) // the value tis the param of the getValueSlowly(value) fn..
- by using this fn version(as the 2nd arg) we re telling the react only run this hook if we re in dev and 2. if we ve the react dev tools installed and see what the result is.. otherwise don't run this at all .. when we ve slow code..

### UseId hook

- came in react 18..
- as we know in our html the el must ve its own id .. if two or more el ve the same id then its a prob..
- we can use the math.random() function to assign new id for each of the el ex - const id = Math.random() ..// <input id={id} > but this could be cumbersome..
- that's where the useId comes instead of the Math rand we can use this hook .. ex const id = useId(); // now this id is gon to be unique for el and each time we render the comp.
- the way this hook the el will get same id as every time the page renders .. this is really important when we re doing CSR and SSR..
- on the other hand one of the prob with the math.rand is our server may render diff id and then our client may render diff id for the same el.. then the id mismatch..
- and the ids are start with the colon : ex - ":r1:" and the thing is now we can select this id using the doc.querySelector(":r1:") // will throw the error..
- instead we ve to use the ref ex - const ref = useRef(); // and use this in our el (attr - ref={ref}).. (the reason they re doing is in react we don't wanna use the doc.querySelector instead we wanna use the ref so they are forcing us to follow the good practise

### UseEffectEvent Hook

- this will makes working with the useEffect hook so much easier and dealing with the side effects much faster. 6months ago this hook was in experimental version..
- this hook is still experimental phase so if we wanna use according to the doc we ve to import it like import {experimental_useEffectEvent as useEffectEvent} from 'react';
- refer the react doc or kyle's blog for the code..
- the idea behind this hook is essentially we click on the btn or emitting event (which is not tied to the react)
- in our case when the room connects we want to ve some kinda logging and we don't want our use effect to be affected by all the things in the events.. - bcoz its entirely different and its encapsulated in its own thing (in the useEffectEvent hook)
- so now our useEffect hook doesn't ve to depend or rely on any of those data and it doesn't even need to know what it is

- the main usage of this hook is we wanna use the useEffect that only runs when certain dep changes.. and the other thing ex logging or something we wanna keep in the useEffectEvent hook .. ex our useEffect only ve to run when the url changes but it also need to access the other data such as logging..
- refer the code [here](https://blog.webdevsimplified.com/)

### UseHook (experimental version)

- this hook ignores every single rules that the other hooks ve.. and it also makes the boiler plate code when dealing with the async code ..
- this hooks takes the promise ex - export default function Data({url}) { use(fetch(url).then(res => res.json()); } // this use is just like the await .. if it resolves it will give us the data back and if it fails it will give us the error.
- and to handle the loading and the error state.. inside our app comp we can use the <Suspense fallback=<div>Loading..</div> ><Data url={url}> elem.. the fallback is gon to be rendered any time we re in the loading state.. this Suspense comp is all about doing the async stuff..
- when we re dealing with the error we need to use the **error Boundary** which is really common when using the suspense.. now we can wrap our suspense and the data comp with the <ErrorBoundary fallback={<div Loading...} </div> > comp

- now this hook goes one step further .. we can pass the shouldFetch as the 2nd arg in our fn and we can make a condn statement saying only fetch the if the state is shouldFetch.. which is not possible in other hooks we can use the if / codn statement .. but with this use hook we can do
- ex - export default fn Data({url, shouldFethc}) { if(shouldFetch) {const data = use(fetch(url).then(res => res.json()) // this hook can be used in anywhere we want it can be wrapped in the for loop if statement..and conditional return .. pretty much anywhere we want..
- and we can pass in the <Data url={url} shouldFetch/>

### React suspense and error boundaries..

- from dave gray.. source code [here](https://github.com/gitdagray/react-suspense)
- `npm react-error-boundary` .. <ErrorBoundary fallback={<p classname="error> error </p> <Suspense fallback/> <PostList/>
- from this package doc we can see the error boundary do not catch certain errors such as the 1. event handlers 2. async code 3. ssr 4. errors thrown in the error boundary itself..
- - when using the suspense comp this comp shows the fallback whenever our comp returns/throws promise

- this suspense comp is useful when using the motion div in our cust comps and the suspense will fallback into the loading state until our comps are ready..

### useFormStatus (from react-dom)

- this hook is also in the experimental version..
- this is simplifies the form update loading state so easily (which is originally so annoying to deal with the form update loading state)
- if we ve i/p el we basically enter the val and to submit or create (button) we basically set the state of the button setLoading to false and then keep track of the state and in the button el we disable it if its in loading state..
- this useFormStatus hook has 4 states( or obj) 1. action 2. data 3. method 4. pending.. ex function SubmitButton(){ const data = useFormStatus(); const isLoading = data.pending; .. // and to make this pending status work we need to set the **action** to our form el ex - <form action={onSubmit} // and this fn looks like
- async function onSubmit(data: FormData) { const title = data.get("title"); } // lly for all the form i/p fields (like the title, age, email etc) ..
- now that we re using the action instead of the onSubmit attr we will get the form loading state.. and also we will get the other state as well.
- the way the pending state works is as long as we re waiting for something .. or as long as the promise that is waiting to resolve .. that pending is set to be true.

### useOptimistic Hook (for OptimisticUpdates)

- in the experimental version
- the idea behind this hook no matter how long it takes for our api to update..we make it so whatever the user does shows up immediately for em and then it rectifies if there is error or something..
- but it will feel super responsive.. since everything we do will update immediately on our screen..this is how most of the social media works when we hit the like it will increment immediately and if there is error it will revert back after.
- the way this hook works is very similar to the useSate hook.. ex - const [optimisticTodod, setOptimisticTodos] = useOptimistic(todos);
- the other use case is we can pass in the 2nd param as **reducer** and it will work exactly same as the useReducer.. ex const [optimistTodos, dispatch] = useOptimistic<OptimisticTodo[]>(todos, reducer);
- so in the ex - he whatever we type in the i/p field and hit the create todo button the text will appears immediately on the screen, (due to this hook) but once the real val is fetched out of the server it will replaced the old one with the fetched val..
- since its mostly useful in the server side app, and this hook is meant to be used with the next js..

---

# Elastic search

- refer the pip package elasticsearchquerygenerator
- the elk search or elastic stack - **ELK** elastic search, logging, Kibana
- he uses the netflix data set we can find in the kaggle. and in this ex - he is using kibana to run all the queries but we can also use postman and write all the queries ..
- he uses the kibana server runs in the port 9200 and upload the doc in the server ..
-
- `GET _cat/idices` - will gives all the indices..
- `GET learn/_search` - learn is the name of the index and search..
- `GET learn/_doc/doc-id` to get the specific doc. with the id.
- we can write some complex queries like `GET learn/_search { "source":['title'], "size: 10", }` and inside this obj we can define some other attr like the min_score query: which can be bool and set to some attr/props to must, filter, should, must_not etc...
- the must - **and** operator, filter - is like the normal filter, should - **or** operator, **must_not** - not operator
- in the size prop the max limit is 10k and if its more then we ve to use pagination.
- for the exact match in our search then we ve to use **match_phrase**
- we can also write nested bool queries ex "query": {"bool": { "must": [], } and other 3 operators inside
- its nothing but we write query inside the query ex - must_not: [ { bool: { should:[{ match: {}]] .. and it goes like that
- so in the above ex inside the must_not we ve the should and looking for the matching title.. this is what the nested query is .. in nutshell the nested query is some thing like - ex - title is (A or B) NOT (C or D)
- to ve the scroll option `GET learn/_search?scroll=1m` and in the next doc `POST /\_search/scroll { scroll: 1m, scroll_id: "paste the scroll id"}
- but its better to avoid this scroll method.. with this scroll approach elastic search will store the cache and if its more poeple then all the results will be accumulated in the cache..

- we can use his lib (pip/ py) to gen the queries.. which is easier than writing the more complex queries..
- PUT my-first-index/doc/4 {"name": "any name"} // to create an doc..
- **Aggregation** - ex - helper.add_aggregation(aggregate_name="Duration", field="duration", type='terms, sort='desc', size=5)
- to Delete the index - DELETE abc .. the name of the index ..

- **elastic search geo mapping** we can do the geo based mapping.. means we can do geo based queries ex search for the lat, long, dist (with in 2k or etc..)

- **Alias** this will be useful for the db migration we can rename the old data and save it as new and delete the old one..

  - and we can filter out the query (ex logs ) for the start date to end date and save it as alias (new data)..
  - and we can use **routing** and we can use it for the shards operation and make it routing: 1 and then 2 for the next document.

- the elastic search is **NoSql** db.. some of the use case of the ES is app search, web search, logging and log analytics, geo spatial data analysis and visualization, business analytics and much more..
- raw data will flows into the ES from diff sources and then the data ingestion (process of data is parsed and normalized and enriched) b4 its indexed in ES

## basic concepts of ES

- cluster - for the rdb node is db instance there can be N nodes with the same cluster name.
- - **Near Real time** Es is the NRT platform
- - **index** is a collection of docs that ve sim characteristics.
- **node** is a single server that holds some data and participates on the cluster's indexing and querying.. and node is the one ES instance
- **shards** is subset of the docs of an idx .. an idx can be divided into many shards
- - **installation** for the installation refer the doc .. just extract the zip and run the bin/elastic search -- ./elasticsearch .. then the server will run on the local port 9200..
  - lly for the kibana.. it will run on the port 5601.. and in the kibana dashboard in the **dev tool** we will run our curl commands all the above commands we ve seen so far..

0 its a wierd the **put and post** are both used to post the doc .. (as opposed to update)..
**REST APIs** there are 4 types of rest api in ES 1. index api(PUT/POST and it helps to add / update the json doc in an idx when the req is made to that respective idx) 2. GET 3. DELETE 4. UPDATE api

**LogStash** in the ELK stack the l stands for the logstash.. - the logstash is a tool based on the filter/pipes patterns for gathering, processing and gen the logs or event .. it helps in centralizing and making the real time analysis of logs and events from different sources.. - and it can collect the logs from diff sources like iot, social media, ecommerce, financial etc

- it is a plugin based data collection and processing engine..
- it can collect the data from different sources ,, can also handle http req and res data.. can handle all types of logging like apcahe logs, n/w protocols, event logs and many more.. and it provides variety of filters which helps the user to find more meaning in the data by parsing and transforming it.
- **Event obj** - it uses this obj to store the i/p and add extra fields created during the filter stage. logStash offers event api to devs to manipulate the events.
- **pipeline** the data flow stages in logstash from the i/p to the o/p.. the data entered in the pipeline is processed in the form of an event. then sends to o/p destn in the user or end s/m's desirable format.
  - **i/p** logstash offers various plugins to get data from diff platforms.. some of the most commonly used plugins are file, syslog, redis and beats.
  - **Filter** middle layer of logstash where actual processing takes place..like we can do some regex pattern etc.
  - **o/p** is the last stage of the logstash where the o/p events can be formatted structure required by the destination s/ms. it sends the o/p event after completing processing to the destn by plugins.. some of the most commonly used plugins are elastic search, file, graphite, statsd. etc..
- **Advantages of logstash** offers regex pattern, supports variety of web servers and data sources for extracting logging data, provides multiple plugins to transform data..logstash is centralized and makes it easy to process and collect data from diff servers, uses http protocol enables user to upgrade elastic search versions.
- indexing docs in bulk .. if we ve like 100 files we can use the bulk api to divide the files into batches and send em ..
- refer the doc for the (indexing docs in bulk)..
- **bool** to construct more complex or nested queries we can use/ combine bool with the queries section..

- **Relevance scores** by default ES sorts matching search results by relevance score.. which measures how well each doc matches the query.. the higher the score the more relevant the doc is..
- **query context** - a query clause answer the question how well does this doc match this query clause..
- **Filter context** - a query clause answer the question does this doc match this query clause. and this context is mostly used for filtering the structured data

- **compound queries** - this queries wrap other compound or leaf queries, eithe to combine their result and scores to change their behavior or to switch from the query to filter context. the queries in this group are 1. bool query 2. boosting query 3. constant score query 4. dis_match query. 5. function_score query.

- **full text queries** - it enables us to analyzed text fields such as the body of the email. this qs is processed using the same analyzer that was applied to the field during indexing.
  - the queries in this group are 1. match query 2. intervals query 3. match bool prefix query 4. match phrase query 5. multi match query 6. common terms query 7. qs query
- there is a framework called **elastic Search-py** in python .. its a low level client lib wiht more limited scope.. it also provides an optional persistence layer for working with docs as py objs in an ORM like fashion. `pip insatll elasticsearch`
- **ES DSL** is a high level client aims to help with writing and running queries against ES. its build on top of official low level client ES.. (refer the doc ES DSL for more infos..)

  - `pip install elasticsearch-dsl`

- **ES Analyzer** - there are diff types of analyzers such as standard, simple, whitespace, stop, kw, pattern, language, and fingerprint analyzer.

---

# Advanced JS

- **Scope** as we know there are 3 types of scopes 1. block scopes 2. fn scope 3. global scope
- **Nested fn scope** - in the ex he has 1 global var and 2 fns.. 1st outer fn has var b and inner fn has var c ex - let a =10; fn outer(){ let b = 20; fn inner { let c = 30; console.log(a,b,c) } inner(); } outer(); // it will still prints the vars accordingly
- - first c var is in its own scope and then it will look for b which is not so it will go one step up and found in the outer fn scope lly finds the a in the global scope 2 step up.. this is the ex of **lexical scoping** which describes the how js resolves the vars names and the fns are nested..
- - the main take is that in the nested fn the var declared in the inner fn has access to their own scope and also to the outer or the global scope..

- **Closures** acc to mdn the closure is the combination of a fn bundled together with the refs to its surrounding state. closures are created every time a fn is created, at fn creation time.
- - with the same ex - fn outer() { let counter = 0; fn inner() { counter ++; console.log (counter); } inner() } outer(); outer(); // if we call this twice (outer fn ) the o/p is 1 and 1 since every time we invoke the fn new temp mem is allocated..and we ve new counter var is established with 0
- - in the same ex if we return the inner fn ex - fn outer() { let counter = 0; fn inner() { counter ++; console.log (counter); } return inner; } const fn = outer(); fn(); fn(); // by returning the inner fn .. we can assign the outer fn to the var called fn.. // now if we call the fn() twice the o/p is 1 and 2 .. by returning and assigning and then invoking the fn..
- - this is bcoz of the closure .. in js when we re **returning the fn** from the another fn we re effectively returning the val of the fn defining along with the fn's scope.. this'd let the fn defn ve an associated persistent mem, which could hold on to live data b/w the execs. that combination of the fn and its scope chain is what is called a closure in the js.
- - means when returning the fn the js also return the fns scope.. and in such situation the val will be persisted in the mem (in our case its 1), and ie y for the 1st fn invoke call its 1 and then 2 for the next fn call.
- - and the key point is with the closures the inner fn has access to the var of the outer fn even after the outer fn has finish the execution.

**Fn Currying** - currying is a process in functional programming in which we transform a fn with the multiple args into a seq of nesting fns that take one arg at a time... the currying doesn't calls the fns it simply transforms it
ex- fn f(a,b,c) is transformed into f(a)(b)(c) ..
ex - fn sum(a,b,c) { return a + b; } console.log(sum(1,2,3))// and lets transform this to sum(2)(3)(4) and the way we do that is by **nesting fns** where each fn takes one arg at a time .. lets see an ex
`fn curry(fn) { 
` return fn(a){
return fn(b) {
return fn(c) {
return fn(a,b,c) }}}}
const curriedSum = curry(sum); console.log(curriedSum(2)(3)(4)); and the o/p is 10;`

- the currying is used to compose the reusable fns for ex we can create fns like log info, log header etc.. where one or more args are set and we get to set choose the remaining args .. currying makess the composing new fn very easy..

**this** kw

- the js this kw is used in a fn, refers to the obj it belongs to ..it makes the fns reusable by letting us decide the obj val.. this val is determined by entirely by ho a fn is called .
- there are 4 ways to invoke a fn and determine the val of **this** kw.. 1. implicit 2. explicit 3. new 4. default binding .. are the 4 types of bindings.
- 1. implicit binding- denotes that when the fn is invoked with dot notation the obj to the left of the dot is wat this kw is referencing ex - person.sayMyName() // the left side of the dot is person which is "this" or its equivalent to this.sayMyName()

2. explicit binding- denotes that when the .. in js every fn has a built in method called **call** which allows the context in which the fn is invoked.. ex - sayMyName.call(person) .. and inside the fn sayMyName() { console.log(${this.name}) // this is referring person obj.. and this is explicit binding
3. new binding .. in js we can invoke the fn with new kw and in such scenario the fn with this kw is referencing an empty obj..ex -fn person(nmae) { this.name = name } // now we can initiate the fn with diff args ex const p1 = new Person('naresh') // this fn is known as ctor fn..here the this = {} // empty obj..
4. fallback binding - if none of the other 3 rules isn't matched.. ex - sayMyName(); if none of this rule is matched js will default to the global scope and set this kw to the global obj..in the global scope it will find this.name since it is not there and it will be "undefined". . and to set the global scope "globalThis.name ='kumar"; so the 4th binding is the global scope..

**order of precedence** - when multiple rules is applied to figure out the this kw then the order of precedence is New, explicit, implicit and default binding..

### Prototype

- ex - fn Person(fName. lName) { this.fistName = fName; this.lastName = lName; } const p1 = new Person ('clark', 'kent'); p1.getFullName = fn() { return this.firstname + "" + this.lastname } console.log(p1. getFullName()).. if we try to assign the var p2 and init new person obj and console log it will throw error since the we rer calling p1.getFullName() and if we want to make the generic fn.. the fn to assign on every instance..
- and this is where **prototype** comes in.. in js every fn has the prop called prototype that point to an obj.. we can make use of the prototype to determine all or sharable prop..
- ex - Person.prototype.getFullName = fn() {} // now this fn is generic and can be used by any instance to call this fn. .. the important use case of this prototype is **inheritance** .. and it is called prototyphal inheritance.
- ex from the above ex .. plus lest ve a fn ex - fn superhero(fName, lName) { Person.call(this, fName, lName); this.superHero = true; } superHero.prototype.fightCrime = fn() { console.log("fighting crime") } // to inherit the fullName(); we can use the obj.create which will delegate on another obj in field look ups..ex - SuperHero.prototype = Object.create(Person.prototype) // this Person.prototype has the fullName() and js will exec.. this is how the method will inherited from the prototype.. hence the name prototyphil method..
- and lastly Superhero.prototype.constructor = SuperHero; // otherwise js will thinks the SuperHero was created from the Person.. which is incorrect

- **class kw** introduced in 2015..
- in js the classes are primarily syntactical sugar for the existing "prototyphal inheritance"..
- ex lets rewrite the above ex - class Person { constructor(fName, lName) { this.firstName = fName; this.lastName = lName; }// then all the fn with in the prototype obj are re written as fn with in the class ex - sayMyName() { this.fistName + this.LastName = this.; }} const cp1= new Person('jon', 'doe') console.log('cp1.sayMyName());
- now there are 2 kws are take care of the entire inheritance **1. extends**, **2. super** ex - class SuperHero extends Person { constructor(fName, lName) { super(fName, lName) } // this super will call the person class's ctor .. which is the parent..

### iterables and iterators

- if we wanna loop over the str then we use the for loop and the str.length to loop over the str and lly if its array then arr.length.. to loop over .. but there are couple of difficulties in this approach ..
  - 1. difficult in accessing the elem 2. difficult to manage the iteration on the data for various types of data structures ..
- there is need to iterate over various ds in a new way that abstract away the complexity of accessing elems one by one and was at the same time uniform across diff ds. makes our code more readable and less confusing
- iterable and iterator allow us to access data in a collection one at a time that makes us what to do with the data rather than how to access the data .
- some of the ds are implemented this 2 protocols (iteators and iterables) by default... they are str, arr, set, map..now they ve new **for of loop** ex const str = 'naresh'; for(const char of str) { conole.log(char) } lly for the array .. for(const item of arr) { console.log(item) }
- the obj which impls the iterable protocol is called an iterable..
- and for an obj to be an iterable it must impls a method at the key[symbol.iterator].. that method should not accepts any arg and should return an obj which conforms to the iterator protocol.. the iterator protocol decides whether an obj is an iterator..
- if an obj is an iterator then it must satisfy the following rule .. the obj must ve **next()** that returns an obj with 2 props. **value** which is the current elem and second **done** which is bool val indicating whether or not there are any more elems that could be iterated upon..
- ex - with an empty obj and make it iterable.. to impl the obj must ve the [symbol.iterator] as a key..

  - ex - `const obj = { [Symbol.iterator]: fn() {
            let step = 0;
            const iterator = { 
            next: fn() {
              step++;
              if (step ===1) {
                  return {value = "hello", done: false}
              } else if (step === 2){
  return {value = "world", done: false}
  }
 return {value = undefined, done: true}
  }, }
  return iterator
},} 
for (const word of obj) { console.log(word) } `

- this similar approach is what js is impl for the str, arr, set and map..

### Generators

- Generators are the special class of fns that simplifies the task of writing iterators.
- we write the generator with the syntax "_" asterisk.. ex - function_ generatorFunction() {}..
- in comparison with the normal fn.. the normal fn won't stop the execution unless it returns something or throws an error.. which in contrast with the generator fn which **can stops** in the midway and then continue execution from where it stops..
- or in other words the generator functions can stop the execution. to achieve this behavior it uses the **yield kw** and yield is an operator where the generator can pause itself.. everytime the generator encounters yields it yields the val after yield ..
- ex - function\* generatorFunction() { yield 'hello'; yield 'world'; } // const generatorObj = generatorFunction(); // when we invoke the fn unlike the normal fn the gen fn returns the generator Object.. and the generator obj is an iterator..since its an iterator it can be used in **for of** loop
- ex - for(const word of generatorObject) { console.log(word) }
- now we ve achieve the same iterator behavior with the less code with the help of generator fns.

## Async Js

- to achieve the async in js we need new pieces from outside of the js which is where the web browsers comes into play..and this browsers define fns and apis that allow us to register fns that should not be executed synchronously and should be instead invoked asynchronously when some kind of event occurs.

### TimeOuts nd Intervals

- **setTimeOut()** ex setTimeOut(fn, duration, param1, param2, ..) // and to clear the set time our .. we can use clearTimeOut() passing in the identifier returned by the setTimeOut as a param.. ex - const timeOUtId = setTimeOut(greet, 1000, 'naresh'); clearTimeOut(timeOutId); .. and more practical scenario is clearing the timeOuts when the comp is unmounted to free up the resources..
- **setInterval()** to run the code repeatedly with the regular interval of time.. the fn signature remains same as the setTimeOut() .. and to clear the interval **clearInterval()**..
- this fns are not part of the js.. these are browser fns .. implemented by the browser..
- it is possible to achieve the same effect as the setInterval() with a recursive setTimeOut() ex setTimeOut(fn, run()) { console.log('hello'); setTimeOut(run, 100)}, 100) // the run() will calling itself every 100ms..

### Callbacks

- as we know in js the fns are the first class objs..
- any fn that is passed in as the **arg** to another fn is called cb fn in js...and the fn which accepts the fn as an arg or returns a fn is called **higher order fn**..
- why is cb ? there are 2 types of cb fns synchronous cb fn 2. Async Cb.. the cb fns which execs immediately is called synch cb fn..
- an async cb is a cb that is often used to continue or resume code exec after an async operation has completed..
- the problem with the cb is if we ve multiple cb fns where each level depends on the result obtained from the prev level the nesting fns becomes so deep and the code become difficult to read and maintain.. this is called **Callback hell** ..to tackle this prob the promises were introduce in es6

### Promises

- to overcome the cb hell.. as we know the promise ve resolve and reject // when the promise is fulfilled its in resolve state else in rejected state..or the **return** value is fulfilled then the promise is resolved or else the promise is said to be rejected.. and finally the **success cb** will exec if the promise resolves successfully..
- and if the promise is rejected then the **failure cb** will exec.
- according to the mdn the promise is a **proxy for the value**
- in simpler term the promise is an **obj** in js .. and the promise is always in one of these 3 states .. 1. **pending** or initial state..2. fulfilled 3. rejected (in op failed)..
- promises helps us to deal with the asynchronous code in a far more simpler way.. compared to the cb..
- these resolve and reject both are the **fns** which are called changes when the status of the promise is either fulfilled or rejected.. thru this fns only we can update the status of the promise.. we can't directly mutate the state of the promise..
- the **2 callback** fns (that occurs in case of success or failure) are **.then** and **catch** these cb fns we can call on the promise instance..
- we can create a promise obj by ex - const promise = new Promise((resolve, reject) => { setTimeOut(()=> { resolve('bringing food')}, 1000)});// and we can pass in cbs... promise.then(onFulfillment) // inside this cb pass in the fn as arg.
- note: this .then() can be able to accept both the success and error fns as args .. ex - promise.then(onFulfillment, onRejection) ..// but it is not recommended way tho..better to use the catch for the rejection.
- then we can use **chaining promises** ex - we can use the .then and catch() in the same line(can be chained) .. ex promise.then(onFulfillment).catch(onRejection);// and this chaining can be done as many times as we want to and this can **solves the cb hell**
- **static methods** in promise 1. **promise.all()** query multiple apis and perform some actions but only after all the apis ve finished loading.. {refer the mdn doc ex }.
- the promise.all() method takes an iterable of promises as an i/p and returns a single promise that resolves to an array of results of the i/p promises.
- the returned promise will resolve when all of the i/p's promises ve resolved, or if the i/p iterable contains no promises.
- however it rejects immediately if any of the i/p promises rejects or the non promises throws an error and will reject with the first rejection message/error..

2. Promise.allSettled() .. it waits for all i/p promises to complete regardless of whether or not one of em is rejected..
3. Promise.race() method returns a promise that fulfills or rejects as soon as one of the i/p promises fulfills or rejects, with the val or reason from the promise.

### async await

- there is still a way to improve it further and ie by async await introduces in ES8 (es2017)..
- the async kw is used to declare the async fns.. the async fns are the fns that are instances of the async fn ctor..
- unlike the normal fn the async fn always return a promise .. ex of async fn .. async fn greet() { return 'hello' } greet(); in the console o/p we can see the promise fulfilled and the str hello..
- the above fn is just same as writing like async fn greet() { return Promise.resolve('hello') } greet();

- **await** the await kw can be put infront of any async promise based fn to pause our code until that promise settles and returns its result... in simple terms the await operator makes the js wait until the promise settles and returns the result.

- **Sequential vs concurrent vs parallel execution** lets ve the ex fn hello returns str hello and set time out 2 sec and lly world fn set time out 1 sec if we exec this fns sequentially then the total time takes is 3 sec since it has to wait the firs fn to finish.. then in the concurrency exec this takes only 2 sec to finish ex console.log(await hello) lly for the world fn since we re using the await this is concurrent exec...
- then the parallel execution .. ex - fn parallel() { Promise.all([(async () => console.log(await hello()))(),]) parallel(); // inside the array lly for the world fn.. and in this case wat ever result will be resolved first and the time taken overall is 2 sec.

### Event Loop

- as we know the js RTE consist of an JS engine which has the **memory heap** and **call Stack** .. whenever we declare a var or fn the mem is allocated in the heap .. and whenever the fn is exec the code is moved to the callStack .. and when the fn exec is completed then the code will be removed from the callstack.. **Last in First out** implementation of the ds.
- the popular ex of the js engine is chrome's **V8** engine..
- then the second part is the **web Api** the browser's part for ex timeout, promise, req, dom etc .. this apis are not implemented by the js this apis are browsers that js has access to..
- and the third part is **callback queue or task queue** this queue is FIFO DS..
- and the Fourth part is the **Event Loop** has only one task .. it checks if the call stack is empty if it is then it will push the item from the cb/task queue into the call stack.
- ex - console.log('first') // lly we ve two more console.log for the second and third .. first in the callstack **global()** the thread of exec always starts in the global scope .. then the first console log will be added and exec and pop out then the other 2 as well .. if no more fn to exec then the global() will be pop out of the call stack.
- as we know the setTimeOut() are the web Api fns .. so even if we use em in our js code this fns will be added to the call stack then js will handed this fn to the web apis (and js will simple pops out the time out fn since from call stack) ex - our timeout fn is 2 sec then after 2 sec the fn finishes then the web api will push the cb to the cb queue (since it can't directly pushes to the call stack or no access) .. now the Event Loop will take care of the callbacks in the task queue.. it will check if the call stack is empty or not if its empty then it will push the cb (timeout) to the call stack..
- now what happens when the timeout duration is 0 ms.. it doesn't make any change the exec flow will be like the prev ex .. the cb has to wait for its turn to get the call stack empty.. since the set time out duration is the **min delay** not the guaranteed delay..

- now lets ve a scenario with the asyc promise code .. here comes 2 more components in the picture the "heap memory" and the **micro task queue** ex - console.log('first'); const promise = fetch('www.udemy.com'); promise.then(value => { console.log ('promise val is, value) }); console.log('second') // here the fetch is also the web api.. but it is promise .. so unlike set time out fetch live behind js obj in heap's memory.. once the fetch(url) retrieves the data which are courses .. the web api will set that as the promise val to the mem(in js) when the promise val is updated the js will automatically execute all the fn listed in the fulfillment array (in heap).. now this fulfillment fn needs to be executed in the call stack (since the js can't directly push the cb on to the stack) instead the cb fn will be passed into the micro task queue along with the promise value..
- again the event loop will checks if the call stack is empty if it is then it will push the cb + value into the callstack

- why do we ve task queues ? and micro task queues ? to understand this lets take the prev ex code along with the while loop while( let i = 0; i < 10000000; i++) {} this block of code is simply there for the thread blocking maybe takes like 3 secs. and in this ex we also ve the set time out fn now the set time out cb will finishes the exec in web api and moves into the task queue and the lly the fetch(url) will moves into the micro task queue along with the value (cb + val) in the mean while the call stack is still executing the while loop.. once there is no more code to run in the call stack (emptied) .. now which one will be pushed to the call stack
- the js will give priority to the **micro task queue** over the task queue .. so in our ex the fetch() (cb + val) will push into the stack then again if its pop out and the call stack is empty then finally the task queue will be pushed ..

# Bun Js

- written in zig..
- alternative RTE for node.. besides that it also includes bundler for the frontend, testing tool kit, built in sql lite, HMR support, built in FS, websocket (no need to import ).. its like all in one toolkit..
- includes native bundler, transpiler, task runner and npm client ..fully compatible with node Apis..
- instead of v8 engine, it is actual built on top of **JS core engine** which is the performance minded js engine built for "Safari"..

3 major Design Goals

1. Speed - faster than node and deno, extends the js core engine, little deps..
2. Elegant Api - minimal set of highly optimized APis for performing common tasks.
3. Cohesive DX - complete toolkit for both server side and front end.

**Features and Advantages** - speed and performance, Ts out of the box, Drop in node compatibility, JSX, works with "node_modules", watch mode (HMR), Native NPM client (we can install all the npm package but with bun command which is like **30 times faster than npm**)
Environment vars support (no need dotenv or other package to read env files), No module madness, integrated bundler (this is huge bcoz bun has the built in bundler which is faster than webpacks, parcel and many others, it will bundle our source file and bundle em together into a single file that runs on the browser) , Web standard Apis, built in Sql lite..

- it also supports express, koa, and "alicia js" (may be experimental phase) which is supposed to be 20x faster than express js and its built to be used with bun..
- `npm i -g bun` (already installed) .. to init the project `bun init`
- lly to node to run the file (node index.ts) -- `bun run index.ts`

- to create a very simple web server - const server = Bun.serve({ port: 5000, fetch(req) { return new Response('Hello world')}, }) console.log('listening on port ${server.port}') // this is very few lines of code and our web browser .. which is comparable to using http module in node js..
  - to make run our server in the watch mode `bun --watch index.ts` .. and if we re working with the front end then for the hot reload `bun hot index.ts`
- to read the env files there are 2 ways 1. process.env.PORT (to read the port num) 2. **Bun.env.PORT**

- **bunx** which is similar to **npx** which will allow us to run a package w/o installing it. ex `bunx cowsay Hello Bun` .. so if we wanna use something like npx create react app , which can be replaced with bunx create react app
- we can use the import syntax (es6 syntax) and the node syntax - require syntax in the same file...
- we can use the "path", "fs" .. but instead of fs there is new optimized api (file I/O api) - ex - const data = ' i love js'; await Bun.write('output.txt', data); // its that simple 2 lines of code .. and to read file .. ex - const file = await Bun.file('output.txt'); console.log(await file.text()); // lly we can use other fns in the console.log such as file.size, await file.stream(), await file.arrayBuffer()

**Test** - supports integrated testing no need additional package ex "index.test.ts" (file name should include test).. this test is very similar to what we do with jest .. ex import { describe, expect, test, beforeAll } from 'bun:test' .. to run the test `bun test`

- **Bundler** - to bundle our file `bun build ./src/index.ts --outfile=./dist/bundle.js` and lly we can add "--watch" at the end to enable the watch mode

- **React and JSX** lets see how we can actually transpile into jsx `bun install react react-dom
-

### build a todo app with bun

- Bunx & Htmx ..
- `bun create next ./app` to create a next js app.
- to build and compile `bun build ./cli.ts --outfile mycli --compile` .. will create an bin file and we can exec by `./mycli`
- we can make the https server by providing the tls prop to the server ex tls : { key: Bun.file('./key.pem'), cert: Bun.file('./cert.pem') }..
- **websocket** we can create a websocket very similar to http server.. ex - Bun.serve ({ fetch(req, server){ if(server.upgrade(req) { return; } return new Response(""") ); websocket: { open(){ console.log('msg received')}, message(ws, message){ console.log('messge'); ws.sendText('hello from bun ws')}}});

- **File ops** - to read the file ex - const file = Bun.file('package.json'); const contents = async file.json(); console.log(content) .. lly to write the file ex - if(content.scripts) { contents.scripts.start = "bun run src/files.ts"; Bun.write('package.json JSON.stringify(contents, null, 2));
  - we can also use stdout in the file op e Bun.write(Bun.stdout, "some content to stdout"); // can see the msg in the console
- **import.meta** we can get the meta data about file, path dir, main, url objs and we can destruct and console.log those objs ex const { file, path, url, dir, main } = import.meta; console.log ({ file, path, url, dir, main});
- **sql lite** we can use it like import { Database } from 'bun.sqlite'; const db = new Database("db.sqlite");//will create a file called "db.sqlite" const query = db.query('say hello'); const result = query.get(); console.log(result); ..

- **file s/m router** - to create a one - const router = new Bun.FileSystemRouter({style: 'nextjs', dir: './pages'}); const theMatch = router.match('/'); console.log(theMatch);
- **Testing** we can make use of the node compatible packages ex import { readFileSync } from 'node:fs';
  - ` for the testing we ve already seen ex - import { expect, test } from "bun:test"; test("1 + 1", () => { expect(1 + 1).toBe(2); }) // is a simple test ..
- **Bunx and Htmx** todo app.. check out the **Elysia js** which claimed to be 20x faster than express and its a bun web framework.
  - the repo for this app is [here](https://github.com/Eckhardt-D/bun-htmx-example) .. but its just a post method only.. just for creating the todos there is no update,, or delete .. very simple app.
- we can also use the add cmd like yarn in bun ex `bun add -D react react-dom @types/react @types/react-dom`

# JS functions (fcc)

- that's great starts with concept >>> syntax // concept is much much gt syntax ..good author impression..
- **Default param** - we can use the default val as the safe guard in case if we or any one misses to pass the any of the args .. ex fn calc (a=2, b=4){} ..
- **Rest params** - this rest params will allows the fns to accept any no.of args as array..infinite no.of args ex fn calc(x, ...y){} // some important things to keep in mind. the fn must ve **only one rest param** and it must be the last param in the fn.. as the name suggest rest (means rest of it or whatever left over)
  ex - fn calc(x, ...y){ console.log(x); console.log(y);} calc(1,2,3,4,5,6,7,8,9); // here in the o/p one the 1 - belongs to the x and the rest of the vals are the y (rest param) from 2 to 9 in an array..
  **Higher Order fn** - as we know the HoF are the fn that takes one or more fn as an arg and return the fn as the val ..
  - ex - fn calcVal(calc) { calc(); } // is an HoF
- all the **array related** that we ve been using are the HoF ex map, filter, reduce, slice ..

- **Pure fn** - in js the pure fn is a fn that produces same o/p for the same i/p.. ex - fn sayGreet(name) { return 'hello${name}'; } ; sayGreet('naresh') // whatever the i/p we pass in as the arg we will get the same o/p..
- there is also the concept called impure fn which is quite opposite to the pure fn .. the fn will not produce the same o/p for the given i/p..
- the side effects are the vars that are outside the scope of the fn.. that fn will not ve any control over the var and any one can change the var which cause the side effect.

- **IIFE** Immediately Invoked Fn Expression.. means the code inside the fn gets exec immediately after its been defined..
- to do that we don't ve to give the fn a name ex (fn () {console.log('IIFE') })() ..// note this nameless fn can run only inside the parenthesis .. and that fn is the str representation of the f () {}..
- this iife exists bcoz b4 es6 where we use the var for the global var which gets polluted and we ve to use the iffe to protect em..another use case is if we re using the fn name and (that exists in the global context) and there are chances that somebody can use the same name for the var and the chance of getting it polluted .. so for that not be happened the iffe can be used.

**Recursion** - the fn that calls itself until the max call stack size exceeded.. ex fn foo() { console.log('foo'); foo(); }//
as we know when using the recursion we need to ve the **base Condition** means under which condition we ve to stop the recursion ex - fn recursion() { if(base_condition) //do something; return; } recurse() } .

- another ex fn fetchWater(count) { if(count === 0) { console.log ('no more water'); return; } console.log ('fetching water...'); fetchWater(count -1); } fetchWater(5); // now the recursion will stop exec when the base condn meets (after 5 iteration)
- - we can also achieve this same thing with the for loop .. so there are few scenarios where we might wanna use the recursion ex **factorial** where recursion makes our code more readable..

# TS (Vox)

- **void type** that returns nothing just the opposite of the any type. and in js if a fn returns nothing then its "undefined" so basically the void fns are undefined. and also the void accepts only undefined ex - let movieName: void = undefined;
- **never type** the val will never occur .. mostly we should avoid this, if we set the prop of allowUnreachableCode to false in our ts config then ts will not allow to use the never type for our fns.. throws warning
- **type aliases** allow us to create custom types in ts. .. to provide the name alias .. ex - type courseDiscount = 24 | 35 | 10 | 1 ; // now these are the type literals and we can assign this cust type to var/fn ex - let course: courseDiscount = 25; .. we can also use this for the string but when we use this for the string it will no longer the val it is a type (although the str literals holds val but when we use in the type alias it is just the type) ex - let course = "TS" ; is a type and not the val.. and the type alias do not contains vals they **holds only type**
  - so that's why they are literal types we re literally creating the type that resembles the str or num but not the vals..and it literally can't accepts any other types.
  - they are very well known for creating an obj.
- **Recursive types** - the types that references itself.. we can use this recursive types to create nested types ex array of nested numbers or types.. ex - type NestedType = number | NestedType[]; now with this type we can create the recursive array of array/nested array ex let newArr: NestedType = [1, [2,3.[4,5]]] // we can create this nested array as deep as we want .. the types/ type aliases don't ve run time elems they are just used for compilation. and in the runtime we re not gon see any traces of the types .
- **Type Assertions** aka type casting **as** there are 2 ways of doing the type casting 1. old way with the parenthesis and cast ex - (<Employee>JSON.parse(employeeObject)) // now its of the type Employee.. 2. way is the new way **as** by using the as kw ex - JSON.parse(employeeObject) as Employee;
- **typeof** is the guard that basically guards our type from the other types so the other types can't penetrate the only type we want ex - if(typeof employee === 'number'){now inside the declaration we can see all the number related js options we can choose from and in the else state we can see all the str related js options we can pick} .. and this is called **narrowing** from the broad types we narrow it to the specific type of str.
- **Literal Types** - the literal types are extremely strict our vals, or the vals the fn param or var can accepts.it can look like a numb, string, boolean.. ex - let number: 45; // the 45 is no longer the number, since we assigned it, it is a literal type...its literally a type. and the var can't accepts anything except 45..this will avoid the typos..

- **intersection Types** - just opposite to the union types not just picking one or other type instead just pick all the type **&&** ex - type product = { name: string, price: number } & productSubscription; .. it combines multiple types together.
- **fn type** - we can assing the function as the type ex - let productName: (product: string) => string; productName =(product) => {return product;}

- **optional params** as we know the optional params are thee params whose vals are necessarily required. we create the optional params with the **?** .. just mark our param followed by the ? ..
- **literal object type** are the kind of types we directly pass into the function. ex - fn studentName(student: {fistName: string}): string { return student.firstName; } ..//the literal object type is when we pass in the obj literally into the fn
  - note we can de-structure the obj as the param ex fn student({firstName}: {fistName: string}): string{return firstName } // this type of de structuring the param we often seen in the params ( get params of http) and got confused.
- note: if we re assigning the type make sure the left var/val is broader than the right ex fn name(): string; let id: number | string = name(); // will works fine but the other way around won't work ex - fn name(): number | string; let id: string = name() // won't work since the left side of the var is smaller types scope than the right side..
- if we ve a prop inside the obj ex - meta-data we need to define this property in a double quote since its more than one word and also while calling the prop we ve to put that into the square bracket ex type Course = { name: string, "meta-data": { published: boolean } } // now to access this published prop(is a nested obj) from the course we ve 2 option ex - 1. course["meta-data"]["published"] // both the prop and the nested prop in a square bracket 2. with the . (dot)notation ex - course["meta-data"].published
-

### Generics

- **generics array** we can create a nested generics array of array ex -let arr: Array<Array<number>> = [[1,2,],[3,4],]
- when we create a generics fn we ve to specify the fn nature (generics ), the params (may be generics) and the return val (also generics) ex - fn getArrays**<T>**(num: Array<T>): T {}
- **generic fn type** to create a generic fn type ex type FunType<T> = (arg: Array<T>) => number; and to use this type let getArray: FunType<number> =(arg) => {return arg.length();}
- **generic sets** as we know the sets are diff data collection in ts, just like the str, numb, arr.. set is generic type in ts ex let mySet = new Set([1,2,3]); the o/p will be set(3){1,2,3}; // although its like arr but the vals are obj. the set has 6 fns we can call for the number collection ex Set<number>: add, clear, delete has, and 2 more .. and in total it had forEach, keys, size, values, entries .. to get the length of the set, we can use the size() (just like the length() in array).

### Type narrowing

- type narrowing allow us to narrow down the type of the vars .. using the conditions and logics we can narrow down their types (of vars with multiple / wide types) into one single type
- **conditional narrowing** - allow us to narrow the type of the param, lets say the fn has the 2 types of arg either an obj or array if its an obj then it will return the property or if its an array return the len of the array ex - type Product = { name: string }; fn itemOrItem(item: Product | Product[]) { if(Array.isArray(product){return product.length} else { return product.name; }); ..// this is type narrowing
  - this is also the type guard we ve created, bcoz with in that if statement no other type (except the array) can penetrate, means we re guarding our type from other types. and in the else clause we only ve the obj and that is also an another type guard..
- **Object narrowing** - refers to we get the objs parse parts or only the obj's properties(parts of the prop ) that we re interested in.
- **type guards** - there are different types of types guard and different ways to create them, **typeof** - will gives us the type of the specific var/val..we can use this for the type narrwing ex fn getProd(name: string | number): number{ if(typeof name=== "string") { inside this declaration we can ve the option to use all the str related methods }
  - **2.instanceof** is the another type guard, this tells whether the obj is an instance of the class or an obj ctor, ex - let date = new Date() { if (date instanceof Date) {}
  - **3.specific value check** is the 3rd type guard, ex - let value = null; if (value === null) {console.log("success")}else{ console.log ("failure")};
  - **4. truthy and falsy check**(bool) ex - let value = true; if (!value){ console.log("success")}else{ console.log}
  - **5. built in check** we ve already seen it, for the array check .. ex - let array = [1,2,3]; if(Array.isArray(array)){}
  - **6. property presence check** - let someObj = {price: string}; if("price" in someObj){ console.log("success")} else{}

### Type widening

- is the another name for inference, to stop the ts to widening the type we can use **as const** lets say we ve product type which has name: "ts" .. this product has the name prop of type ts . then the course obj has the name of "ts" {but this is string) the ts inferred the type .. and to stop the ts to narrowing the type we can make use of the "as const kw" ex course = { name : "ts" as const } // now this ts is of the type "ts" from the product type (even though we haven't impl or assign the type product to the course) just by making it as const will switch from the string to the type "ts".

### Interfaces

- are use to create the shape/blueprint for the object, the diff b/w the type alias and the interface is that the type alias has bit more capability than interface. the interface can only provide the type for an object, that obj could be in obj literal or instance of the class.. where as the type alias in the other hand they could provide type for anything.
- **extending interfaces** allow us to **extends** one interface into another, extending the props of one interface to another.
- **augmenting interface** - when creating the interface, we can ve 2 interfaces with same name, (unlike the 2 type aliases or one type alias and one interface with same name is not allowed), interfaces can ve same name, and its called augmenting interface or **interface merging**. when we ve interface with same name the 1st interface merges with the 2nd one and augmented or enhancing the capabilities of the 1st interface.. so when we assign this interface to a var, we ve to declare all the props in both of the interfaces. and in those interfaces we can ve same props(name) must ve the same type. same prop name with diff type will not be allowed..
  - in this type of augmenting interface the order matters whatever the interface we declared first will ve the higher hierarchical order, and that is gon be the main interface and the second one is the sub interface(just the part of the first interface).

### intermediate types section

- **unknown type** this type can be assign to any val/var its almost like any type, ex let discount: unknown = 25; this unknown type is more useful in type narrowing, ex - we in the type narrowing we can conditionally assign the val to the var - let otherDiscount: number = typeof discount === "number" ? discount : 10;
- **any type** in this we can assign var to any type ex let courseName: any = "TS"; let coursePrice: number = courseName; // this works even-though the course name has any type (that inferred to str) and the course price has the type of number and still we can assign (ts will allow).. but this will catch in the unknown type (we can do like this in unknown type) .. and we can assign the any type var to the unknown type and vice versa..

  - in the case of the **union** the unknown type absorbs the union type and in the case of **intersection** the intersection absorbs the unknown type ex - let courseName: unknown | string = "TS"; let coursePrice: string = courseName; // (this is not allowed since the unknown absorb the union type) .. and it is reversed when using intersection(the unknown will be absorbed by the intersection), ex - let courseName: unknown & string = "TS"; let coursePrice: string = courseName; (this time it works since the unknown type was absorbed by the intersection string type)
  - and when we re trying to assign the any type to unknown make sure the any type var comes first and then assign it to the unknown type var in this way it will ensure the type safety and we can get rid off the above problem.. ex let courseName: any = "TS"; let coursePrice: unknown = courseName; now if try to assign this to a number type we will get error ex let coursePublish: number = coursePrice;// (earlier just with the any type it will accept but now with the unknown type it will be error) hence the unknown type provides type safety than the any type.

- **index Signatures** - defines the entire signature of an object using the indices or the keys of an obj, the **keys** of the objs are the important and integral part of the index signature.. defining the shape of an obj is completely diff from the defining the signature of an obj..the index signature is something that provides the signature for the keys of an obj. and we can ve many keys as we want, and the name the literal key name can be whatever we want. (but the key type and the value type we need to define b4 hand)
  - ex - in the product type the name(which is a key) prop itself is a string and that holds the val (of str).. so the index signature will looks like ex - type Product = { [name: string]: string } .// so in a nutshell the index signature is when we assign the type for the key itself to our prop. or assigning the type for both the key and the val.
  - now when we re assigning this type(index signature) the key prop needs to be str but of any name we want doesn't need to be the same name as we declared ex- let course: Product = { TS: 'advanced course'; JS: "intermediate"; } // like that we can add as many prop we want as long as the types matched.. which is not possible with the types / interface.
  - lly we can ve the key and val of diff types ex - type Product = { [isPriced: string]: boolean, released: boolean } ..// and we can assign.. let course: Product = { TS: true, released: false }
- **Dictionary with index signature** we can also create dictionary with index signature to create that our val has to be an obj ex - type Product = { [k: string]: {name: string, price: number}; let course: Product = { TS : { name: "TS", price: 5 }, JS : { name: "JS", price: 15 } } // like wise we can specify many obj with the diff keys and the vals (has to be the same that we declared).

- **Indexed Access types** - with this we can access some part of a type with the another type much like when we access the keys of an obj .. as we know we can access the prop of an obj with either the dot notation or with the brackets ex let course = Product.name or Product["name']; (**Note:** that dot notation won't work on the types ex - type course = Product.name; it won't work, but the bracket notation works)// now indexed access type is the same thing but for the types(it accessing some part of the type with the another type) . lets assign the name prop of the product to the course type .. ex - type Course = Product["name"]; const courseName: Course = "TS";// (so we re indexing into this product type using the course type amd ie index access type)

  - the more common use case of this indexed access type is lets say in our product type we ve nested(obj) prop moreInfo and while assigning this props as the param to the fn how we can assign the nested obj as the param..ex - type Product = { name: string, price: number, moreInfo: { release: boolean, level: string } }; fn promoteProduct(name: string, price: number, moreInfo: Product["moreInfo"]): Product { return name, price, moreInfo }
  - so far we ve seen the indexed access types for the type alias and lly we can do with the interfaces as well. ex - to assign the prop from the nested obj - let course: Product["moreInfo"]["level"] = "intermediate";
  - **Indexed access types with union** we can do union with it, ex let coursePrice: Product["name" | "price"] = 10; // note with in the bracket notation we can only access the prop like a str (allows only the string)

- **Partial Types** - is a flexible way to create an obj, that could contain some or all of the prop, but none of the prop are required to be existent with in the obj, as we aware the the optional prop with the ? followed by the prop, and ts has syntax for this.. and that is let course: Partial<Product> = {name: "TS:}; let courseName: string = course.name; //here we can't assign the name prop to the string since the name is of type str or undefined (which is a broader type scope can't assign to the str) ..// now using the partial is as same as marking our props with the optional ? syntax.. so just by making the Partial<> syntax avoid us to mark our props with the ? syntax,
- **Mapped types** are extremely important concept in ts which leverages the concept of index signature,
- **Readonly Type** just like the partial the read only is the another type of mapped type. as the name says we can't edit the prop ex - let course: Readonly<Product> = {} ;/ we can declare the ReadOnly like this or in the type alias itself ex type Product = Readonly<{name: string}>; // lly for the partial type we can declare like this.
- **Readonly Array** is an array that doesn't support any method, change and mutate the array.. as we know the primitive types are **stored by the value** and for the more complex data types like the arrays and objs they are **stored by reference** (more details in the c++ pointers and in rust) .. and in storing by val if we assign the var to another var it will points to diff mem addr where as in the store by ref it will still point the same addr/ ref to the addr .. the syntax is let numbers: ReadonlyArray<number> = [1,1,3]; .. // lly we ve **ReadonlySet** and **ReadonlyMap**..

- **shared fields** - lets say if we ve 2 types aliases and they ve couple of prop fields as same in both of em, ex name and price prop in both type alias .. in the shared fields we can put the common props in one type alias and then use intersection & to use in our types instead of repeating the fields for every single type ex - type course = Product & { released: boolean, date: string }..// now the name and price prop is added to the course .. this is shared fields concept.
  - and the other syntax is to add the additional field in the product type itself ex - type Product = { name: string, price: number } & ({released: boolean, category: string} | {availability: string, rating: string}) ..// after the shared fields (intersection) we can add as many unique prop fields as we want..
- **Optional chaining** - when we ve prop field undefined, and when calling we can use the ? optional chaining on the prop.. ex - const courseInfo = course.map((course) => course.info ? course.info?.noOfStudent : undefined ; ..// note in the arrow fn if we re using using the {} curly braces we ve to use the return kw in our fn declaration.. if we don't wanna use the braces like this in our ex we can directly return the val w/o using the return kw...so for the implicit return we can get rid off the braces.(and directly return the val)

- **Nullish Coalescing operator ??** - in the and operator if both vals are truthy, then ts returns the **second val** ex - console.log("hi" && "hello"); // the o/p is hello.. when any one of the val is false then it returns falsy .. ex - console.log("" && 5); // its a falsy val and it returns the empty str(the left side val)..lly for the console.log(undefined && 5) // returns undefined (since its a falsy val) .. if both are falsy then it returns the 1st falsy val ex - console.log(undefined && 0); // returns undefined (since both of em are falsy it returns the 1st falsy val).
  - now for the or operator its opposite of the and if both vals are truthy, then ts returns the **first val** ex - console.log("hi || "hello"); // returns "hi" (the first val) ..lly if we ve one falsy and truthy val it will return the truthy val ex - console.log(undefined || 5); returns 5,.. and if both vals are falsy then it returns the second val ex - console.log(undefined || 0); returns 0
  - Note: the logical AND(&&) and OR(||) operators evaluates 0 as the falsy value. ex Boolean(0); is false and lly for the empty string it will treat as falsy val.. but what if we wanna pass in the number 0 and """; essentially they shouldn't be falsy its a number and empty str..it has to be true.. and that is what the nullish coalescing operator fix for us.
  - the Nullish coalescing operator always returns the **left side** of the operation, as long as the left side is not null and not undefined (in those cases it returns the right side).. it just care about the null and undefined not anything els ex NaN or false, it doesn't care
    - ex - console.log("hi" ?? "hey") returns "hi"..// console.log(NaN ?? 5) returns NaN.. // console.log(undefined ?? 5) returns 5 ..// and console.log(undefined ?? null) returns undefined (lly if null is in 1st then it returns null)

### Enums

- as we know the enums doesn't ve the run time element to it .. enums provide us with the way to create named numeric constants that we can use in our app.. the entire idea of the enum is to provide us with the systematic way of keep tracking the state of our app..
- ex - enum Crops { Barley, Rice, Wheat } ..// if we see this js created file.. its a js created obj ..its like the fn and has the indexed access to every prop we defined in idx wise order ex - var Crops; (fn(Crops){ Crops[Crops["Barley"] = 0] = "Barley"})(Crops || (Crops = {})) // inside the function declaration the key(barley) of crops returned 0 (idx) - and assigned it to "Barley" ..
- we can also use the enums like an expression that produces the const val and we can use that as the val for our enum as well (instead of idx with the nominal val) ex - enum Crops = { Barley = "corn".length } // assigning the len of the str corn to our enum field.. note if we re doing this type of expression assignment to the 1st field (or any field) then we need to assign the initializer for the next field ex - enum Crops = { Barley = "corn".length, Rice = 1, Wheat} // now the initializer will start from 1 and the 2 will be automatically assign to the wheat by ts..

- **constant and computed enums** - there might be some situation where we don't want the js to create the runtime objs/fn for our enum, in such scenario we can do inline enum, so there won't be any js objs produced in the runtime.. ex - const enum Crops = {}..// now by adding the const kw we can see the js o/p fn has no more the enum obj/fn..
  - now the limitations of the const enums is we can't provide the val of expression to the props, .. the vals/props of the const enum is also only the constants... since the expressions like "Barley".length are computed expressions and the computed expressions are not work with the const enums. and what the computed val create for us is the computed enums and are not work with the const enums.
  - but there are some cases where const enums can accepts the expressions that are very simple or primitive ex - const enum Crops = {Barley : 5 + 2 } // this type of simple expressions will work.
  - we can however add the const enum to the js fn / obj during the runtime just add the "preserveConstEnum: true" property in the ts.config file.
- **String Enums** - along with the numeric, const the enums can be string as well.. ex - enum Employee = { name: "jon", job: "dev" }

- **exhaustiveness checking** - means check all the members of the enums at least once during the compile time .. its nothing but using the switch inside the fn to check all the props in the enum ex fn setCoursePrice(course: Courses) {switch(course) case courses.js: return "4.0" default:} // but if we ve cases for only couple of our props and then later if we add one more prop to the enum then while calling the fn with the newly added prop (which by the way doesn't ve any case declared) will cause an error..
  - to avoid this we can use the never type .. ex - fn setCoursePrice(course: never) {} // the never type can be assignable to any type, however no other type can be assign to "never" except itself.. this means we can use for narrowing and rely on never to do an exhaustive checks for us.
  - so in our ex - the never will take a look at all the members of the enums and pin points which ever prop doesn't ve the price.. in other words the never type will do an **exhaustiveness checking** for us on our enum b4 our enum is compiled into js.
    -ex - fn courseWarning(course: never): never { throw new Error ("Course is not priced") }
  -

### OOP in ts

- **instanceof** just like the type of we can use the instance of operator to find if the obj is the instance of the class ex - console.log(robot instanceof Robot) // true of false.
- **this** the common usage of the this kw is in 3 methods 1. call 2. apply 3. bind method .. when we ve an obj and inside there is a fn then the this kw will refer to the obj .. if we don't ve obj but if we use this kw inside the fn (w/o obj) it will throw error but disable the strict mode prop in the ts config now we can see the this kw in the fn refers to the **window obj**.. so when we don't ve obj this kw will bind to the window obj.

  - lly in the context of the class this kw refers to the class.. **call method** the job of the call method is to bind the this kw.. just pass in the ref of the fn (means w/o parenthesis) ex fn addition(){console.log(this)} // addition.call({name: "john"}) // now this kw inside the fn refers to the obj that we pass in the call() .. now this call() also takes additional args(besides the obj) that we passed in the fn's params.
  - lly the **apply()** method just as same as the call() but the additional args we ve to pass in like a array ex - additional.apply({name: "jon"}, [10,20]) .. // here even though in our fn we doesn't pass in the param like an array but while using the apply() we ve to call the arg like an array.
  - **bind()** - lly the bind method binds the val of the "this" kw to the fn, but the diff is it returns a new fn. and we need to invoke the fn.. ex let someFn = addition.bind({name: "jon"}, 20, 10) // now we need to exec the someFn ex - someFn();

- **Scope** - as we know the scope is where the var is accessible.

  - the var kw is a locally/fn scoped(inside the fn block), where as the let and const kw are blocked scoped. is not inside the fn (we ve seen this in the dotnet plenty of times) ex - { var num: number = 1000; let num2: number = 2000;} console.log(num, num2) // now only var can be accessible outside of the block (num), not the let and const (they are scoped to the block)

- **Lexical Scope and arrow fn** - lexical scope is where we ve the access to the var from the enclosing scope with in the enclosed scope.. basically if we ve fn (parent) and the inner fn (child) the inner fn will ve access to the var of the parent fn .. this is called lexical scope.. but the other way around is not possible we can't access the child fn var in the parent fn ..
- arrow fn - in the child fn lets say we ve set timeout as inner fn if we use the normal fn declaration for the set time out and console log the **this** kw it will refer to the window obj not the class .. but if we use the arrow fn (instead) in the set time out and console log the this kw, now it will refer the class.
  - the reason is for the normal fn declaration the this kw was executed in the context of the parent fn.. and the arrow fn doesn't ve the this kw binding and it doesn't know what this kw is and ie why when we console log this kw passes thru arrow fn and finds the class
- in the class or constructor sometimes we see the prop prefix with underscore ex private \_price: number; // is like the naming convention for the private prop.
- as we know we can modify the private prop but we can use the getters and setters on the private prop and access the val ex - get price() { return this.\_price;} // and to read the val ex - console.log (product.price) // call the fn by ref..now lly for the setters starts with the set kw ex - set price(price: number): number { this.\_price = price; } // now lets assign the val product.price = 30;

- **Index Signatures** - allow us to dynamically add properties to our obj, which is something that ts will not allow us to do by default.

  - ex lets ve an obj .. const robot = {}; // since its an empty object we can assign any val from the outside ex - robot.hasHead = true; // this won't work and the same applies for the class as well the ts is very strict with the shape of an obj/class.
  - but what index signature prop will allow us to do is to add **dynamic properties** to our class/ obj.
  - this index signature are the same as we used in the types alias and the interfaces .. ex - class Robot { [bodyParts: string]: boolean } // now we can dynamically assign the prop ex let robot = new Robot(); robot.hasHands = true; //

- **inheritance and abstract classes** - when we ve classes that has same props or methods then we can consider using the inheritance,.. by using the **extends** kw.. and also in the derived class ve to put all the parent class member vars inside the super() (like ctor) ex class Toy extends Robot { constructor(pubic name: string, public price: number){ super(name: string, price: number)}//

  - **abstract class** says that when we ve the base class, the base class should not be instantiate on its own. ex - abstract class Robot { name: string } // now we can't create the instance of this class .. the idea is the abstract class is just only to ve the shared/ common props / fns that we extends in the derived or child class and this abstract classes are not meant to be for instantiate or create an instance.

- **protected members** as we know the private access modifier works only in the parent class / class of its own not in the sub class.. and the protected modifier works /allow us to access the mem vars in the derived / subclass

- **interfaces and implements** as we know the interfaces provides the shape for an object, and we can ve the same feature for the class as well, but the limitations for the interfaces is that we can use private or protected for the props and we can ve the setters and getters..

  - just define all the shared / common properties in the interface and then implement the interface for the class.

- **Static prop and static method** - the static member is a kind of member that only exists in the class, and it can't be accessed in the obj instantiation of the class, normally we instantiate the obj and then from the obj we access the class prop . but when we using the static kw for the prop then its the other way around we can't now access the prop with the obj (that has instantiated) instead we ve to access the prop by the class name itself

  - ex -class Product { static name: string }; const product = new Product(); console.log(Product.name) // not can't do with product.name (with the instance it won't work) // lly for the method as well create a static method and access it by the class itself.. by doing this now we don't even ve to instantiate the obj,

- **Generic classes** the generic classes can accept multiple types as data

- **discriminated union** this discriminates against the types based on which or according to which it is supposed to work.. ex - type User1 = { online: true, status: true } type User2 = { online: false }; type User = User1 | User2; const userInfo: User = { online: true, status: "something" } // here based on the prop we passed the ts picks the type User1 and discriminated the type User2.

- **typeof type query** - as we know this typeof operator give us the type of the specific data val(types)..and this operator also has other type of functionality, it has the ability to query any type/data and extract the types for us.

  - ex const course = { name: string }; and now we can't apply this course to the type alias since the type can't accept the val (and course is actually the obj val) and it accepts only the type ex - type Product = course ; // can't do that .. but we can use the typeof in front the the course obj, ex - type Product = typeof course; // now this is not the typeof operator we know so far.. this is the **type query** now this will take whatever types in front of it (course obj) and extract its type for us, and ie why it is type query, it queries the type of a val.

- **keyof type query** - After we extract the type of certain vals/types we can also extract the **keys** as well .. ex - type Product = typeof course; type ProductKeys = keyof Product; .. and also further we can extract the types of the vals that our keys holding with the bracket notation ex - type ProductValue = Product[ProductKeys]; //(this is an idx accessed type) now this will extract all the types of our keys in our course obj .. and all these 3 constitute something called **lookup types** so we can do this many levels deep and extract every thing that the obj has..
  - lets take the above ex and create a fn ..ex - fn getCourseName <T, K extends T>(course: T, key: K){return course[key];} // in this the T is the course obj, k is the key which extends from the obj T or k is the keys of the course obj, now we got all the keys in our obj and we can aceess em ex - let courseName = getCourseName(course, "name"); // for the other prop/key (ex - price) or even we can pass in other types of obj in the course param, and access the keys.
- these operators (keyof and typeof) are extremely useful when it comes to making the type completely optional, and also the keyof plays important role when comes to understanding the source code of TS.. and with these we can create our own types, our own conditional types, or our own map types we can create em by ourself.

### Conditional types

- its same like the conditional rendering the val (or ternary operator) but instead of the val its for the **types** .. and the syntax is simple compare to the conditionally render the val
- ex - type Name = string; type Price = number; type Product<T> = T extends "ProductName" ? Name : Price; // note here the ProductName is a type(literal type) not a string.. let productName: Product<"ProductName">; // now this is(ProductName) of the type string and the remaking/ whatever type is there in product are of type number ex - let unknownProduct: Product<"whatever type">; // this is of type number. since its the else clause in the conditionally types rendering.
- under the hood what ever we ve done in those ex for the conditional types they implement **keyof** type query
- lets see some ex of conditional types ex - undefined extends never ? true : false; // is "false" (since never is never gon achieve like an empty box, and undefined is something, something can't be part of nothing but the nothing can be part of something) ex - undefined extends unknown ? true : false; // is "true" (as we don't know both of the types) .. null extends never ? true : false; // is false .. unknown extends never ? true : false; // is false.. string[] extends any ? true : false; // is true..

- **Extract** utility types.. the utility types are the built in ts types.. that on fundamental level implements conditional types. some of the common utility types are **extract** and **exclude**

  - this extract utility type is used to extract the sub part of it type based on a certain criteria, and the part we actually extract is the part we need. an in the case of exclude its reversed.
  - ex lets say we ve an type Product of mixed types with the strings and numbers but we wanna extract only the string type ex - type Strings = Extract<Product, string>; now this will extract all the string types in our product obj, if we found those types we re looking for, we return those types otherwise returns never..
  - lly we can also extract the types of the product ex - type ProductItem = Extract<Product, {name: "TS"}> // the name "TS" here is of the type (literal type) and lly for the arrays ex - type ProductItem = Extract<Product, string[]>; returns all the string array in our obj if we ve, otherwise never/nothing

- **Exclude** utility type, this allow us to get the sub part of a type by specifying what we do not need, by excluding the type that we don't need we will end up with the type we want.. and its the opposite of the extract type.
  - ex - type NotStrings = Exclude<Product, string>; // will gives all the props that are **not** of/except type string.. lly with all the above examples with opposite.
  - its quite handy ex - in our product object we ve 100 types and we want to extract 90 of em, to extract 90 of em is difficult than exclude 10 of em. and its simply the other way around.

### Mapped types

- as we know the map refers to transform one thing into another, specifically it refers to changing similar items into different list of transformed items ex - console.log([1,2.3].map((item) => item.toString()));
- **index accessed types** as we know with this we can access the key of the interface or the type alias ex - type ProductName = Product["name"];
- **index signatures** as we know this is useful to create the blue print of the interface or the types ex - type Product = { [k: string]: string } // const product: Product = { name: "js", desc: "good" } // with the index signature, for the arbitrary key we can ve as many KVPs as we want..

- **Mapped Types vs Index signatures** lets see an ex with the index signature ex - type Course = { name: string }; type Product<T> = {[k: string]: T } //(T of type generic) function getCourseInfo<T>(course: Product<T>){return course; } // lets make the fn call .. let courses = getProductInfo<Course>({course_1: {name: "js"}, {course_2: {name: "Ts"}}}) // since we re dealing with the idx signature we can ve as many keys as we want (and assign the val (of obj) for the keys)

- **mapped types** - with the mapped type we re sort of dealing with the behavior of for loops but for types. means we effectively loop over the keys of the type with the mapped types. lets ve an ex in the above code, lets limit the no of course we want to ve. we wanna limit to specific no of course with the specific no of keys.

  - lets modify our prev ex - remove the generic types and .. type Course = {name: string }; type Product = {[course in "JS" | "TS"]: Course};// now with this mapped type we wanna use the specific prop key(type) either JS or TS.. function getProductInfo(course: Product){return course;} .. let courses = getProductInfo({TS: {name: "ts"}, JS: {name: "js"}}}) // that's it, as opposed to the index type in mapped we can specify the only mapped idx type(in our case its either js or ts).
  - note: if we try to convert this mapped type to index signature ex - type Product = {[course: "TS" | "JS"]: Course} we will get an error saying the idx signature param type can't be a literal type or generic type, consider using the mapped obj type instead.
  - now with our index signature ex - what if we convert that to a mapped type, yes we can do that ex - type ProductClone = {[course in string]: Course} ; // this seems like loop over all the possible strings and one of em could be "js" and "ts"

- **Record** Mapped type, as we know the record is the kvp of types. ex - type Course = { name: string }; type Product<keyType extends string, valueType> = {[key in KeyType]: valueType }; // this is equivalent to type Product<T extends string, U> = {[key in T]: U}; or the official record mapped types look like this.. type Record<k extends keyof any, T> = { [P in K]: T // and here also the extends str means find all the available possibilities of str combination in ts .. the keyof any(type) means it is of union types of (str | num | symbol)

  - the record mapped type constructs a type with set of properties, which are key type of type val type..

- **Omit Type** - ex - type Product = Omit<Course, "price"> // the omit type helps us to remove the props from the the obj, in that ex we want to get rid off the price prop (the 2nd arg in the Course obj) lly we can remove multiple props by using the union types in the 2nd arg.
- **pick type** lly the pick type(which is quite opposite of the omit) will allow us to pick the certain properties we want ex we only want to pick the name and the price props, ex - type Product = Pick<Course, "name" | "price""> ; and the implementation of the code looks like this ex - type Product<Type, keyType extends keyof Type> = {[key in keyType]: Type[key]} // its the indexed accessed type we re indexing the key of the type. which is the only way to index into the types and access the key we wanted.

- **Mapping modifiers** the readonly, required, partials are the mapping modifiers.. these are also the utility types
- **Readonly modifiers** consider we ve 50 plus properties with readonly access modifier instead of adding it one by one we can use this utility type. this will loops thru our types and for every key it grabs make it read only for that key. ex - type Product<Type> = {readonly [key in keyof Type]: Type[key]} // here we just loop thru all the keys in our type(obj) and make (map) it read only.
- **Required mapped type** as the name says if we ve the obj with bunch of optional properties and the required prop, we just loop the all the props with required mapped. event if we ve ? optional props it will make em all required prop type ex -type Product<Type> = {[key in keyof Type]-?: Type[key]} // this negative **-?** means the opposite of the optional which is required .. so it maps every keys in the obj as required type

- **partial Mapped type** makes any type optional, it makes it part of the original type and the syntax is same as the required type here instead of negative optional it applies positive optional to every keys ex type Product<Types> = {[key in keyof Type]+?: Types[key]} ..//now it makes every keys (props) optional

### Creating TS project

- `npm iniit` and `npm i typescript --save-dev` and `tsc --init` to create a TS config file.. `npm i esmodules-dev-server --save-dev` this is a minimal dev server runs on node js. (optional - he made the script : serve : "node node_modules/esmodules-dev-server") `npm run serve` to serve the dev server
- when we talk about the regular ts we can only export **declarations**. the declaration is any kind of var that uses var, const, let, also includes, fns, async fns and classes
  - to export multiple things in a file w/o needing to add export kw in every fns or vars instead we can make the export statement and add all the stuff we wanna export ex - export { getterFn, Product } //
  - **barrel files** the Barrel files simplifies the process of imports, and they are nothing but **index.ts** files
- **Default exports and imports** default export is used when we wanna export a single declaration or expression, and we can ve **only one** default export in a module. with the default export we can't export the declaration directly.. ex - const course = { name: string } // we can use the default export on the course (which is a declaration) .. // instead we wanna remove the expression and make the default export ex - export default { name: string } // just exporting the obj .. if we do wanna export the obj with the name declaration then instead of defining the default export in the obj lets define the obj and then make the export ex - export default course;

  - there is a one cool thing about the default export is, when we use the named export the thing we export will ve the same name when we import, but here in default export that's not the case, we can give whatever name we want while importing the default exports w/o using the aliases. (as far as the export file path is correct)
  - and for the expressions ex - export default 1 + 1; //this will work

  - **Export Assignment and the require method** for this we need to change the module prop(in our ts config file) to commonJs or "AMD" not compatilble with the es2015,. this is the other syntax for exporting and importing.
    - when we use the export assignment there are couple of limitations, when we ve the export assignment in a module we can't ve any other exports, but when we talk about the named exports we can ve several name exports, and we can ve one default export and several named exports that is not an issue. and the 2nd limitation is that it can't be re exported
    - ex - export = "cool"; // in order to import this we need to use the require method ex - import cool = require("./index.js") // the require() is globally available method and the only arg it accepts is the path name.

- **conditionally loading modules** - the import statements and the named imports can't be nested into conditionally loaded.. and they should be in the first line of the file..it supports in "esNext" and "commonJs" modules. ex - export const release = true; export const course = "ts" // if the course is released we wanna show the name of the course in the browser ex - import {released, course} from "./index.js"; if (released){ import('./index.js').then(() => console.log(`course name is ${course}`))} // with this promise we can conditionally load modules.
- **importing json file** ex - {name: "js"} and to import { \* as config } from "./config.json" and add the prop in the ts config file "resolveJsonModules": true;
- **creating ambient modules** - are also called **declaration files** is the feature in TS that allows us to import the 3rd party js libs in ts as if they were written in ts and use em safely. they are created with the ambient declaration file that describes the modules types but it doesn't contain any kind of implementation. first make couple of settings in the ts config file ex - "allowJs": true; declaration: false; ex - export addition(a, b){ return a + b; } and to import { addition } from "./ambient.js" // now we can see by default the addition fn is marked as the type any and so the params and returns type as well, and to fix and provide the correct signature for this fn as we re importing in the ts. we need to create the declaration files.

  - the declaration files can also be created using the ts config file, and also manually we can create (the declaration file), the declaration file has to ve the same name as the js file, ex - in our case "ambient.d.ts" now we can declare the module ex - declare module "ambient" { export function addition(a: number, b: number): number; } ../ we can import this file w/o the path just with the module name ("ambient")
  - and to work with these type of ambient files (since they are 3td party libraries) we can't work with the browser, we need to ve the 3rd party bundler like the "webpacks" .
  - the second way is to config in the ts config file set the prop "declaration": true;

- **augmenting ambient modules** - as we ve already seen the augmentation in interface, here the idea is same. in the ts-imports.ts file lets augment our declaration file ex - import {addition } from "ambient"; addition(10, 20); now to augment this - declare module "ambient" { export function addition(): string; } ..// now if we see this addition() has an overload which means we can now use this with str return type.
- **Module resolution and tracing** - there are 2 ways in which ts locates and loads the modules and these are called module resolution strategies, 1. the first way is classic(mainly used for backward compatibility) 2. the second way is "node" moduleResolution: node (is sim to how node js resolves the modules) and this node js modules are import using the require()
  - for adding the tracing set the "traceResolution": true and then `tsc --traceResolution` we can see how it try to resolve the modules(external libs) with the traces. if it can't find the module (which actually exists, declared in the d.ts file) then we can enable another settings ex - "types": ["./src/ambient"] .. // now this time it will resolve the modules declaration, this is very useful when working with larger project/ external libs that module can't resolve.

### Webpack

- the bundler for the ts projects. which will compile our ts files into one js file which is easy to load in the browser
- **extend the recommended ts config** - go to the github.com/ts-config/ bases or we can add `npm i --save-dev @tsconfig/recommended` .. and also ve bunch of other cofig files for create react app, cypress, ESM, Next js, Node js, Nuxt etc, and we can extend our current ts-config file with this recommended ts config file. by using this we can ve one universal config file and community support if anything goes wrong.
- **single file compilation using Webpack** - means we compile all our ts files into one js file, the advantage is we ve only one js file so less http req and the load time is required and our app speed will increase significantly.
  - however the drawback of this single file compilation is that maintainability since its a single file the changes are difficult to make and if there is any single change it reflects the entire app and the entire file has to push again(in the prod)
  - to install webpack `npm i --save-dev webpack webpack-cli webpack-dev-server ts-loader` and in the webpack documentation we can grab the "webpack.config.js" file (under the ts section) .. and we can add the couple of scripts in ts config to run the webpack and the webpack server.
  - then in our index.html file just add the src for the script section as the "bundle.js" (which is the dist dir)

---

# Microservices Node Js (udemy)

### Authentication

- one thing to keep in mind is that handling authentication in the microservices is unsolved issue. There are many ways to do it but not an one right way.
- we can go thru couple of solutions that works but still has a downsides in it.
- 1. fundamental option - where the individual service rely on the Auth service (sending a sync/ direct req) and the auth service has the logic to inspect the JWT or Cookies to decide if the user is authenticated in or not.
  - and the downside here is that sync service if the auth goes down then its a problem.
- fundamental option 1.1 - here the individual service rely on the auth service as a gateway.
- 2. option 2 is each individual service knows how to authenticate the user ..
  - and the down here is we will end up duplicating the authentication in each service.
  - and the other downside is if a existing user is banned in our app still he can be able to access the service (since he has the valid Jwt, cookie etc).
- regardless of these downsides in the option 2 he still chose to go with this approach with some tweaks.
- and that tweak is whatever is there JWt or cookie etc give em validity of time interval or expiration time (15 mins), aka some expiration mechanism
- now this token refresh logic is gon to be handle by our auth service, although our individual services will hold the logic to verify the token if it expires then the client will send a direct req to the auth service.
- and in the auth service user management logic if there is a banned user it will emit the event for that and in our event bus will listen for that event and we can persist/cache in the mem) this event in the service only for like 15 mins or same duration of time as the Jwt.
- **Cookie vs JWT** as we know the res header has set-cookie method and we set the cookie(of some string) will stored inside the browser and now the browser send the follow up reqs with the same domain and same port and the browser will append its req's header the cookie info (any arbitrary piece of info we want to store)
  - and the jwt is where we will take that piece of arbitrary information from the cookie as a payload and use the jwt creation algorithm that will spit out the jwt (encrypted format) and we can throw that into some decryption algorithm and access that arbitrary string
    - | cookies | JWT |
      | ------- | --- |
    - are transport mechanism | - are authentication mechanism
    - moves any kind of data b/w | - stores any data we want
      browser and server |
    - | automatically managed by svr | - we ve to manage manually |
      | ---------------------------- | -------------------------- |
- now to store the info we can go with couple of approach ex - 1. we can save the info about the user (implies the auth mechanism tells us about the info about the person)
  - the auth mechanism must be handle auth info.. must ve some built in tamper resist way to expire or invalidate itself..
- and implies auth mechanism should be understood to diff services(written in diff langs) and implies auth mechanism shouldn't require some kind of backing data to store on the server.
- and these all leads to JWT and all the above mentioned things can be implemented by the JWT.

- issue with the SSR and JWT .. with the SSR we need to know the auth info in the very firs req from the client. (before rendering the app/ html to the client) but the big prob here is during the 1st req we can't customize or run our js on the client. means we can't intercept the initial req and try to append the auth header in the req.
- now this initial req can be only possibly store in the cookie. which is the only possible way for us to communicate from the backend to our server during the initial page loads (and we can do it with service workers).. so we gon be store the **JWT inside the Cookie**.

- **Cookie and Encryption** lets install the cookie session manager `npm i cookie-session @types/cookie-session` this cookie session will allow us to store bunch of info in the cookie itself. and here we re not gon to encrypt the cookie coz cookie session lib uses diff encryption algo which can't be understand by many langs (that our services running on.since JWT are naturally tamper resist).
- **Generate JWT** lets gen the JWT and store it in the cookie (session obj) `npm i jsonwebtoken @types/jsonwebtoken` .. now in the cookie session we can see the cookie obj contains jwt we can copy that and go to base64 encoder and encode it to get the jwt obj.
- and grab that obj go to jwt .io and with our sign in key (in our code) we can verify the token and if it is verified then our jwt is valid token. but the payload we can see even w/o the verify key.(ex 'asdf')
- now all our other services gon to need that sign key to verify the jwt. so we need to securely encrypt the sign in key and store it and share with all the services. and for this we can make use of the k8s **Secret service** to store our sign in key (encrypted ). And now this secrets are gon to be exposed to our services as env vars.
- to create the secret in k8s `kubectl create secret generic jwt-secret --from-literal=JWT_KEY=asdf` (in k8s we can create diff kind of secrets but here we re creating generic type / all purpose secrets). since we re not using the config file to create the secrets so every time we create new cluster we gon remember all the diff secrets we ve created.
- and to get all list of secrets `kubectl get secrets` and in our container deply config file we can save this secret under the env prop. note: if there is any error in the env prop of our secret key then the pod will not create / spin up.
- { note: in the res from the server, to exclude the p/w field he just used diff approach - in the user model under the (mongoose schema) add the toJSON() as a prop and inside ex - toJSON: { transform(doc, ret) { delete ret.password; ret.id = ret.\_id; delete ret.\_id; }} // this transform( takes doc and the return val).. and lly we can use this approach to modify our fields(key) as well ex in the id (user Id) field the mongoose by default save the user id as **id (instead of id) we can take our the double underscore. and also mongoose added version key to the res automatically (**v) we can remove this as well...// but this approach is really view level logic, so if we re following the MVC approach it better to put this logic in the view layer instead of the model.
- **SignUp route handler** - in the sign in route handler we can use the import { body, validationResult } from express-validator; // this body fn to validate the body incoming req. and this same logic we will be using across all our service for the validation of our reqs, so we can extract the logic put it in a separate file (ex MW/validation-req.ts). and in this handler we also included the **JWT** and store it in the session obj.
  - and for this signIn logic just the usual checking / conditions like the user already exists or he provided the correct p/w like that. and also in this route handler we again added the **JWT** and assign it to the session obj..
  - **Current User route Handler** the current user route handler includes logic like whether or not the user is logged in an who the user is ? and that req will ve the cookie if the user exist . and if the user not logged in there will be no cookie. - we gon check like does the user ve "req.session.jwt" set ?
  - and also in this route we will be verify the jwt token by using the jwt.verify( method takes session obj and verify key) from the json web token package.
- **SignOut route handler** - essentially we gon send back a header that's gon tell the user's browser to dump all the information inside the cookie (just emptify the cookie) from the cookie session manage we can destroy the session ex - req.session = null; then send the empty res ex - ree.send({});
- **current user MW** we can extract the logic of those above 2 route handlers into the middlewares, the reason is as we the mw are the ones our req will first pass thru b4 even reaching the route handler.. in this he set it up 2 mws 1. mw to extract the jwt payload and set it up on 'req.currentUser' and 2. the mw to reject the user if the user is not logged in.
  - and in the current user mw we will be augmenting the type definition (for add the currentUser prop to the req obj) we gon add in the additional prop to the type defn of what a req obj is. for that create a interface of what our payload is (email and p/w) then we can assign the interface to the declare global ex - declare global { namespace Express { interface Request { currentUser?: IUserPayload; }}} // this is how we modify the existing type definition.
  - 2. mw that rejects if the user is not logged in - requiring the auth for route access. (require-auth.ts)
  - now our current user route ('current-user.ts') will look like this ex - route.get('/api/users/currentuser', currentUser, requireAuth (req, res) => {}); // with those 2 mw (in order wise)

---

### .Net and Ng jwt authentication

- ve to install some of the packages like ms.aspnetCore.Authentication.JWTBearer, then Identity.EntityFrameworkCore, ms.aspnetcore.identity.ui, ms.EntityFrameworkCore.Tools
- for the db migrations - `dotnet ef migrations add "initial-migrations"` then to create a actual db out of this migration `dotnet ef database update` - after this our app will go to the config file and get the connection string then it gon check if there is any migrations file then it gon exec all of em.
- in this we can will create id for each jwt we ve created so we can track the jwt
- `ng generate environments` will generate evn.dev.ts and env.prod.ts under the env directory.

### Spring boot OAuth2 keycloak authentication(prog techie)

- the source code [here](https://github.com/SaiUpadhyayula/spring-security-oauth2-keycloak-demo.git)
- the oauth2 is the standard way of providing authorization credentials to our services (providing authorization b/w the services hence the name OAuth)
- some of the oauth2 terminology are 1. protected resources 2. resource owner 3. resource server 4. client (can be confidential client or public client) 5. Authorization server (gen and validate access token for clients) - some of the auth server are aws cognito, google identity platform, ms AD, **keycloak**, spring boot auth server.
- **OIDC** open id connect protocol build on top of oauth2 that act as identity layer - this will provide the user info (**UserId token)** - so when the user req a token, it will provide the **access token** and the **userId token**
- access token to verify the user is authorized or not and the user id token to verify the user information.
- **Keycloak**
  - once we install the keycloak server we can see the **Realm** is like the we can manage clients users and rules we can create a realm and add users and rules. and also in the realm settings we can find the open id endpoint config - can see all the url belongs to our realm. and we can grab and use the issuer url which will provide all the endpoints
  - and we can gen the client secret as well.. then we can login, once the user logged in we can see the id token with the **new claim** of user info
- **PKCE authorization code flow** spelled as picksee stands for **Proof key code enhanced**.. created for public facing client ex - web app, considered as best practice to follow

  - if we store the client secret in the client to make a post call for the token endpoint, this is very risky coz anyone can view the source code of the java app and find the client secrets if it store it in the client.
  - for this reason this PKCE was created, by this flow we don't ve to store the client secret in the client. in the get url we gon send 2 additional query param 1. code challenge (code verifier) 2. code challenge method for more info {refer pkce auth flow doc }, finally this will send the access token
  - now if the client loggin, in the token (we can inspect and see ) our token obj and also there is a prop refresh_expires at prop, so if the access token expires it can req a new access token.
  - in the keycloak admin page we ve to add our ng app url as the valid url, access type - public, advanced settings - select s256 for the pkce for the hashing

- **ng app** in our ng app add the package `npm i angular-oauth2-oidc` - and if we see the client code its easy to understand how we re encapsulating the keycloak server for the auth.
- the clients make the post req to the token endpoint to retrieve the access token with the grant type as client credentials. usually the clients are cli app or shell scripts, or remote server of microservices. it will also include the client id and the client secrets to authenticate themselves with the authorization server.
- then the auth server will validate the credentials and if they are valid it will send the access token to the client. in the client's header will include the access token as the part of the req.
- in the microservice arch the req will pass through 100s of microservices, and to pass the token from one service to another we will follow the design pattern called **token relay** we can implement this in our spring by using **WebClient** or Template Rest api (which is deprecated)

- **Refresh token** - the auth server will usually respond with the access token and the refresh token. the access token is usually short lived and expired. and the refresh token will help us to generate a new access token (w/o asking the user to login again)
  - by setting the scope(in post man) - openid offline_token - we will get the refresh token of unlimited expiration, instead of 1800 secs
- **password grant flows** lets see how this flow works, username and password are used to authorize but not recommended to use OAuth, can only be used if we ve absolute trust on the client.
  - **single sign on functionality** provide the convenience way to use our existing social media account to auth any 3rd party app. and in this way we don't need to register each and every service we re gon use. in our keycloak admin page under the identity provider we can select the github or any thin for the SSO

---

### integrating the Auth with the RxJs and signals

- source code [here](https://github.com/joshuamorony/angularstart-chat.git) is a ng chit chat app.(for the firebase backend)
- rxjs for the events and the signals for the state,

---

### some types

- as we know if we wanna assign the fn's return type as the type for the val / create a type out of it then we can use the utility function ex - const func = () =>{const val: string; return val;} // type Return = ReturnType<typeof fumc>
- lly if we ve a async fn then we can simply wrap the return type utility fn with the Awaited ex - type Return = Awaited<Promise<typeof func>>

- **Prettify** utlity fn - this can be useful in the nested types scenario ex - interface MainType {name: string } ; and then type Some = MainType & {loading: boolean} // now if we hover over the Some type we can only be able to see the props of the Some type and not the interface props we ve to click and see the interface's props.
- so this prettify util fn makes it easy when works with the nested type with simple hover we can get to see all the properties of the types that are defined. and to impl this
- ex - type Prettify<T> = { [k in keyof T]: T[k]} & {} ; now if we use this ex - type IDK = Prettify<Some>; now if we hover over this Some type we can see all the type's props no matter how nested they are.
- lly we know there are some util types fns ex - omit (the fields that we want to omit from) and the exclude (same as the omit) and also pick and extract
- the thing about omit it can't work in the nested type scenario ex - type Shape = { kind: 'circle'} | {kind: 'square'}; now if we try to use the omit ex - type Omitted = Omit<Shape, {kind: 'circle'}> ; // it won't work..
- instead we can use the Exclude<T, U> in those scenarios when dealing with the nested type.

---

- **Generics with the react node** - we can't use the generics inside the react node ex - inside the <button/> element or any any react node we can't ues the generics, bcoz they are jsx elems, the jsx elems can be str or num and all sort of things and it does need to be a jsx elem,
- so for that we can use type assertion for the generics ex theme<T> but while using this inside the jsx elems we can assert like theme as string.
- or the better way / approach is to restrict the generic type ex - rn its of any type and it will infer the types based on the val we passed in but instead we can **restrict the generic type** ex - function convertToArray<T extends string | number>(input: string): T[]{ }
- lly when we wanna use the generic type inside the react node ex - function theme<T extends ReactNode>(){}

### tRpc 

- find the code here for the todo [app](https://github.com/jherr/trpc-on-the-app-router) 
- also find the code of kyle's [here](https://github.com/WebDevSimplified/trpc-express-comparison.git)
- `npm i @trpc/server` lly for the client - const client = createTRPCProxyClient({links: [httpBatchLinks({url: ""})]}) \\ this batch links will batch all our reqs into one req, the default and recommended one.
- so any time we send multiple reqs at the exact same time, it will batch em all and send as all the data together.. we can also create our own cust links 
- inside the http batch links (along with the url) we can also pass in the headers (may be for the auth)
- procedures.query() - the procedures are what we re gon do with our routes/endpoints. or we can make generic procedures that can ve root procedure inside em. 
- by default we don't need to define the return type since trpc / ts is smart enough to infer the return type, if we want we can also define the o/p() b4 the mutation(), and declare the type.
- **middleware** trpc has the built in mw which makes consuming "ctx"  and doing things like authentication easier.

- **Websockets and Subscription** - built in with trpc, `npm i ws` and `npm i --save dev @types/ws` - then - applyWSSHandler({wss: new ws.Server({server})}) // this handler from the trpc and then pass in our express server. . then also pass in the router and the ctx to the handler
- and then from the procedure we can call the .subscription() method .. we can also import the observable and then use it inside the subscription(), ex - onUpdate: t.procedure.subscription(()=> {return observable<string>(emit => {emit.next()})}).. 
- then finally we can close the connection
- in our impl we can see, one link (the normal http to update and quries)  and also we ve Websocket, this is where the **split links** comes in, is a another type of link which will allow us to connect one link or another based on the condition.
  - so this splitLink() will takes in bunch of props, like condition, true, and false .. refer the code. and our condn seems like if its a subscription use the websocket otherwise use the normal connection,
- the key about ws (as we know) if one user updates the val, since we re connected to the server and subscribed/ listend for the changes it will update all our browser and update the changes

### interfaces vs types 

- which one to go for its really a debatable topic, to answer this when we want to define a obj go for the interface otherwise always pick the types. 
- although ts wiki suggest to use the interface over types due to the performance, interface performs well in the scenarios like a very large complex project.
- with interface we can only describe objs but with the types we can describe objs and also everything.
- types can support union typs where as interfaces wont ex type User = string | string[];
- types can also easily used with the utility types, interfaces can also used with the utility types but with the ugly syntax ex - types User = Omit<UserProps, "name" | "age">; 
  - the sim thing with the interface approach is that interface User extends Omit<UserProps, "name" | "age> {}
- lly when working with the tuples the syntax is much cleaner in types ex type Address = [string, number]; // the same with the interface is interface Address extends Array<string, number>{0: string, 1 : number} // ve to also define the idx position of the typle inside the obj declaration.
- there is a really good reason to prefer types over interface is **Extracting types** over something ex - const specs = {title: "some name", size: "some size"}; type SuperSpecs = typeof specs; // lly we can use the [brackets] to pull out the nested obj types as well ex - type SuperSpecs = typeof specs["nestedSpecs"]
- however there is a benefit with the interface tho, with the interface we can ve multiple interfaces with the same name, which is not possible with types. 
  - and also if there is two or more interfaces with the same name the props / types will **merge**, so this is why the interfaces are open, since we can redeclare em (using same name) and it merges and the types are closed (can't redeclared em) 


### GraphQl (fcc net ninja)

- source code [here]()
- as we know its a ql for req or mutate the data in the server, alternative to the REST api
- some of the drawbacks of the rest api are 1. **overFetching** the data that we didn't ask for, 2. **underfetching** leads to ve us send multiple requests until we get the data that we wanted.
- graphql could solve both of these problems that the rest api ve. unlike REST api (which has seperate/ multiple endpoints for every request) the graphql has only one/ **Single endpoint**. ex  - mygraphql.com/graphql
- to send a req to the server the syntax is - Query {Courser{id, name, title} } // ew treat the query itself an obj inside we send the obj details that we re looking for.
- then we can send nested query inside single query(instead of sending multiple queries)
- for this project we will be using the nodejs server (for the graphql) using the package called **Appolo server** - this server will be responsible for handling all the queries and the mutations we will send.
- this server has the graphical (localhost) ui called explorer - used to send query, just like the post man.
- unlike rest api which fetch us the entire req obj, here in graphql we can fetch the specific part or the field we want in out req.
- installation find in the apollo server docs
- in the graphql we can use the 5 built in scalar type 1. int 2. boolean 3. ID 4. string 5. float
- the flow will go like this - first we will write the schema.js (with the type defs) then the **resolver** fns -  it should be like this ex - const resolvers = { Query: { games() {retur db.games}}};
- then in apollo server pass in the type defs and the resolvers ex - const server = new ApolloServer({typeDefs, resolvers}); 

- **Query Variable** - what if a user wants to send a review for single, lets say if we wanna pass in the id var to our query ex Query SomeQuery($id: ID){ } // with the $ - sign
- **related data** - its like the one to many or many to many, (relationships b/w the table).. also need to mention the relationship in the schema.js file with every obj(and their relationship like prisma)
- **Mutations** - we can do mutation query like - mutation DeleteMultiple($id: ID){}, for adding instead of using type to define the type def our our obj, **input** kw to add the input we want ex - input AddGame{}

------------------------------------------------------------------------------------------------

