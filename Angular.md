## Angular

- the angular introduced by Google.
- is a front end framework build using the js by google

SPA - single Page App - that doesn't need to reload the page during its use and work with the browser - is all about the user exp and the speed - angular is for single page app

Angular Cli - we can update, scaffold, ,maintain and develop an app using the anular cli. - `npm i -g @angula/cli` - `ng new projectName` - `ng generate` gen comp, routes, services, and pipes with a single command - `ng serve` to run the dev server `ng serve -port 4330` to serve with the required port we want

# Angular Component

- in order to work with comp we ve to import the comp modul from @angular/core
- In order to transfer the ts class to an angular comp we ve to decorate the class with @Component({selector, template, style})
- or if we use the separate files not in the inline for the template and styling use the url -- ex: templateUrl prop, and styleUrl and just give the path of the files
- and then we ve to register this comp into the app.module.ts -- inside the @NgModule({})
- if we don't wana create the comp from the scratch by ourself then we can use the `ng generate` to create the comp with the boilerplate(including the spec.ts file for the unit testing) --> `ng g c componentName`
- by this way it will also automatically register our comp in the app.module.ts file

# NgOnInit() life cycle hook

- this onInit interface from the angular core is a life cycle hook
- ngOnInit() is a callback life cycle method, which will invoke after the comp is fully initialize (or fully loaded in the browser) inside the dom .

- "Angular Data binding"

  - to create a js inide the html file in ang we can use the jinja template {{}}
  - this is called the data binding in anular

- "Data sharing b/w the comps"
  - there are 3 ways we can share the data b/w the comps
  1. parent to child comp via @input (from ang core), use the deco inside the child comp to capture the var from the parent comp
  2. child to parent via @ViewChild() --> inside that pass the child comp that we wana get the data
  3. child to parent when there is a event via @output and Even Emitter

"After View init" life cycle hook from core ang - then impls this hook inside the class then - use the ngAterViewInit() deco this method will call once after wave initialized on the browser

"@Output and the event emitter" - this method is ideal for the events like form data, btn click and other user entries - the Output and the EventEmitter from the ang core - in plain js we ve onClick prop in btn but in ang we ve to use <button (click)='function name'>

# Display data and handle event

    - 1. to display the data we can use string interpolation {{}} jinja templ in anh or ts scope
    - 2. "property binding" - ex we can use the jinja temp to dynamically show the url var in the img tag <img src = {{url}}>
    - we can also use the prop binding to do the same w/o the jinja - <img [src]= "ur"l> that sq bracket in the src prop is the prop binding
    - "Class Binding" we can bind the clss in the html tags ex. <h1 [class.text.red]>
    - "style Binding" we can bind the inline style elem <h1 [style.backGroundClolor] >
    - refer the html dom style obj for more dom obj props of the style
    - "Event Binding" to handle the events, for event handling we ve to use normal braces ()
    - ex. <buttton (click)= "goNext()">
    - "Event Filtering" - in the keyboard event (in console we can see ) has so many props - one of em is keycode prop - for every keys that we pressed has a unique keycode , ex. for the s key the keyCode is 83, enter key is 13.
    - ex. <input (keyup.enter)= "enterKey()">, this ang event filter we re filtering this keyup event only when press the enter key

- we used to get the event vals by e.target.val but in ang has cleaner apprach using the template var # sign ex. <input (keyup.enter)= "enterKey(username.val)" #username>,

# Two way data binding

- ngModel (from the forms module) is the special syntax for ang 2 way data binding
- <input [(ngModel)]= "userName">
- this is how we use the 2 way data binding approach to get the vals of i/p fields

  "1 way vs 2 way data binding"

  - in the 1 way - can only binds the data from the comp to the view , its unidirectional
  - all the bindings str interpolation{{}}, prop binding [], class binding [class.any], style binding [style.domprop] are all one way binding
  - in 2 way its -- we can pass data from both bi directional comp to view and from view to the comp and the syntax is to use the [(ngModel)], and only this ngModel binding is the 2 way binding

  ## Angular Directive

  - the ang directive is the special kinda tech that can use to manipulate the dom obj
  - it can add/ remove the html elems from dom dynamaically
  - ang directives are comps w/o a view
  - and the comps in ang are directives with view
  - everything we can do with the directive we can do with the comp but we can't do everything we can do with the comp we can do with directive

- There are 4 types of directives
  1. structural directive - can change the dom layout by adding/ removing dom elems
  2. comp directive - with template view
  3. Attr directive - can change the appearance or behavior of an elem, comp or another dir
  4. custom directive - can create our own cust directives

"ngFor" - is structural directive - used to render an array inside the view - with this we can manipulate the dom (add/remove html elems to the dom) - ex. to show the list of our arr <li *ngFor= "our posts "> {{post}} - "Fetching obj array" - <li *ngFor= "our posts "> {{post | json}} --> inorder to fetch the obj in to the browser we ve to convert the obj into json using the pipe op.

    - "Change Direction"
    - "Array Index" -> to add the index of the array we can directly pass in the elem <li *ngfor= "post array"; let i = index> <button (click)="onDelete(i)">

- "\*ngIf directive"

  - for the condn view, and this is also structural directive
  - ex <div \*ngIf='postArray.length > 0' >

- "ngTemplate"

  - as we know in ang we can identify an elem using the # --> template var, and we can use this in our condns
  - but when we wana use the template var we ve to use it inside the ngTemplate tag otherwise it won't work
  - ex. <ng-template #templateVar> (now we can use this inside our condition)
    - <div *ngIf='postArray.length > 0 ; else templateVar ' >
  - we can't use this ng-temp tag as normal html tag, we can only use this for the structural directives

- "ngSwitchCases"

  - rather than using the ngIf several times to check the condns, we can use the ngSwitchCase
  - and there is also \*ngSwitchDefault for the default case

- "NgStyle" directive

  - we can do multiple style binding using this
  - when define the ngStyle condn we ve to define the else part as well.
  - ex. <h1 [ngStyle]="{color = isActive ? 'red' : 'black'}">

- "NgClass" directive

  - doing multiple inline style binding is not a good practise
  - so we can put the styles in css file and then make a class binding
  - ex. <h1 [ngClass]= "{classname : isActive }"> the key is class name (that we wana bind) and the val is condn/ var

- "custom Directive";

  - to create a cust directive `ng g d name` - with this cust directive we can avoid the code repeatation if we wanna use a fn multiple times.

- "Structural Vs Attr Directive"

  "Structural"

  - 1. the structural directive are responsible for the html layout
  - 2. shape or reshape the dom elem by add/ remove the elems
  - 3. can identify with the leading \* sign
  - 4. ngif, for, swithch etc

    "Attribute Directive"

    1. change the appearance or the behaviour of the dom, (we can't manipulate the elem as we do in the structural directive)
    2. ngStyle and ngClass

# Angular Pipes

- The Pipes are used to transform the data into a particular format, when we only need that data transformed 'in' a template or html view.
- ex. when we ve a form raw data we can use the pipes to format the data --> 10000 with the pipe we can add currency $10000 or comma 100,00
- Types of pipes
  1. upper case pipe
  2. lower case pipe
  3. decimal / number
  4. currency
  5. date
  6. json
  7. percent
  8. slice
  9. custom

1. Upper case or Lower case - ex - <h2>{{name | uppercase}}
2. decimal / number -> <h2>{{decimalVal | number: '1.2-2'}} --> 1. the int that we ve or we want(if we write 2. then it will add 0 in front of our int and make it 2 dec val) and then the min decimal we want and the -2 for max dec val
3. currency pipe --> <h2>{{dollar | currency: "CAD" : '2.2-2'}} , by default its $ and we can pass the currency types we want as an attr. and sim to number pipe we can also control the decimal of the currency by passing an addition attr
4. Date Pipe - <h2>{{date | date: 'short'}} or shortDate arg for only date. refer the ang docs date pipe for all the args.
5. Json Pipe - to convert the data into str json ex- <h2>{{object | json}}
6. Percent Pipe - to show the percentage.. ex- <h2>{{0.576 | percent : '1.1-2'}} -> this will multiplies the val with 100 and give us the percentage, and we can also control the decimal.
7. Slice Pipe - with this we can slice an array.. ex- <h2>{{nameArray | slice : 0:2}} the start and the end idx of the array
8. Custom Pipe - to create a cust pipe.. create a dir inside app dir called pipes and create a ts file with the name conv .pipe.ts
   - import the pipe and pipeTransform from core, and add the deco @Pipe({name: 'custPipe'})
   - then create a class - export class CustPipe implements pipeTransform{
     transform (value:any, args?:any){
     return "city Name" + value; - our cust logic
     }}
     - and then register the pipe in our module.ts, as same as comp
     - <h2>{{userCity | custPipe}}
   - sim to the comp, we can also gen the cust pipe using the cli `ng g pipe Pipes/pipename ` will gen the pipe inside the pipes folder
   - will generate the boiler plate, we can just add our logic
   - "Cust Pipes with args " for ex if we wana create a summary of a paragraph with only the first 20 words, then we can use the .substring(0, 20) method in our cust pipe's logic
   - or if we wana do this dynamically pass the args val inside transform(val:string, lenth:number)
   - now we can pass this arg inside our pipe <h2>{{paragraph | custPipe : 20}}

# Angular Service

- so far we ve shared the data b/w comp that has related with each other
- if we wana share the data b/w comp that has not related then we ve to use the ang services.
- We use the ng Services to share the data and methods among the comps whether it is related or not.
- for ex. if we ve array we wana share, first we can create a service and declare the array and then import the serv to the comps when we wana use the array
- this service will helps us to avoid the code repeatation and save time. \\

- "Manual creation" , lets create a dir called services inside the app dir and then create a file with the naming conv .service.ts and create a service class with the logic
- then we ve to create a new instance for that class inside our comp constr. \\

- "Dependency Injection" is more cleaner way of impl this, the sim way but instead of creating new inst inside the contr we can pass the service as a constr param (inside the comp)
- ex. constructor(private postService: PostService)
- but if we compile this we will get null injector error, coz inorder to provide the service class as an injectable class
- means we ve to provide the service class as an provider
- in our comp @Component(providers:'PostService') .. now we can use that as dependency injection

## DI Providers and injectable deco

- There are 2 ways we can inject the service class, one by using the provider prop in @comp()
- the 2nd method we ve injector deco for the service class, with this we can make the service class injectable for all comps
- "@Injectable({providedIn:'root'})" in the service file, with this we can inject the service class from the root app.
- with this now we don't ve to create a new instance at every comp we wana use service class
- by usinfg the cli the cmd is `ng g s services/servicename`
- Usually all the data manupulating stuff, such as load, save, delete data inside the service file

- "Ang Interface" - if we made any mistakes in the array obj, or typo then the data won't be fetched properly.
- to solve this prob, we can make the blueprint of the array obj. and set that to the new array that we wanat create.
- to create the blueprint of the obj, we ve to use the "interface" using the cli --> `ng g i models/interfacename` --> inside the interface define our obj
- and then go to our array and make that type as our interface.

# Angular Template driven Form

- "Form Types" -- we can't do the validation by using only the 2 way data binding approach.
- for this ang has 2 approach for the form
  1. Template Driven Form - by using the ang directive (ngForm)
  2. Reactive Form or Handcoding

1. Template Driven Form
   - with this approach we ve the full control on the form
   - "ngForm" directive - for the template driven form module we ve to import the form from ang
   - to make a form template driven <form #templateVar='ngForm' (submit) = "onSubmit(templateVar)"> now the form is temp driven

"Handle Form Data" - In ang to handle form data we ve 2 classes 1. form group and 2. form control - 1. formgroup -- - 2. formControl -- use this inside the form fields <input formControl> - we use this inside the form tag <form formGroup> - inside the formgroup and form control class we ve few methods value(), touched(), untouched(), dirty(), pristine(), valid(), errors() etc. to access these props we ve to create a new Instance of the formgroup.. New formControl(); - we can create the inst by 2 ways 1. handcoding or Reactive Form and 2. by using the ang directive (ngForm) or Template Driven Form - Inside the app we can ve multiple formgroup and inside the form group we can ve multiple formcontrol. - - In the template drive form we do all the things inside the html templ view .

"ngModel and form Control class" - for the form group we add the "ngForm" directive and for the form Control/input fields we ve to add the "ngModel" directive, and the i/p fields must ve name prop to use this directive

"Template driven Form Validation" - <input #email = 'ngModel' (change)="getValue(email)"> - <div \*ngIf = 'email.touched && email.isValid'> email id required </div>--> those are the form objs we can find more props in the console.log

"Form Validation Types" - minlength, maxlen, - email i/p fields to add the email field we ve to use the pattern attrn <inpput pattern= "paste the pattern"> the val we can find in html email pattern and paste the regex as val - text area - we can add required prop

"Disable the submit " - until all the required fields are filled, we can do this by prop binding - ex. <button [disable]= "formTemplVar.invalid">

## Angular Reactive Forms

- or handcoding, from the scratch in the comp file.
- import the ReactiveFormModule from the ang Form in the app comp
- then import the fromGroup and FormControl from the ang forms
- then vo to create the instances for them --> this.form = new FormGroup({name: new FormControl()}) -- with all the req form fields with the formcontrol instance as the val
- now we can conncect this form intance to our form elem, with the prop binding
- <form [formGroup='form']> and then bind the i/p
- <input formControlName='name'>

- Validation (reactive)
  - multiple validation for the form control -- this.form = new FormGroup({name: new FormControl('', [validators.required, validators.minlenght(5), validators.pattern(regex)])}) .. lly we can add multiple validators as an [].
  - "form submit" as same as the template drivern just pass the onSubmit() to the prop binding [onSubmit], unlike templ driven form we don't need to pass any args on the onSubmit(), just the method is enough.
  -

"Nested Form Groups" - inside our main form group we can add the child or nested form gruop ex. if we wana add the contact details. - to create a nested form group inside the main form we ve the prop called "form Group Name" - ex. <div formGroupName='contactDetails'>

"Reactive Form Array" - the formArray from the core - to create a one just create a instance - ex. skills: new FormArray([]), to push the skills inside the [], first we ve to convert the data into form array - ex. addSkills(skills: HtmlInputElemen){
(this.form.get('skills') as FormArray).push(new FormControl(skills.value)) } - and to render this in view - ex. <li \*ngFor="let skill of skills.controls"> will render all the form control vals inside the skills[]

    - "REmove the form array val" - <li ngFor = "let i =index">
                                      <button (click)= 'remove(i)'>

                                      remove(index){
                                        this.skill.removeAt(index)}


    "Form Builder" - from the ang forms
        -   inject into the constr
        -   with this form builder class we can build 3 types of class instances 1. FormGroup, 2.FormControl, 3.FormArray
        -   ex. constructor(fb:  FormBuilder){
                            this.form = fb.group({
                                fullName: ['',[] ] --- this [] represents the controls instance, so we can pass in all the validators
                            })  }


    "Custom validation in Reactive form"
        - lets say we wana type the user name w/o any space in it. and we wana show error if any spaces.
        - for this case we don't ve any built in validators
        - the good pracitise is to create a file for the validators inside the app dir, create a folder validators and then the file --> nospace.validators.ts
        - we need to import 2 modules - 1. abstract control and 2. validation error module

        - ex. export class noSpace{
                static noSpaceValidation(control: AbstractControl) : ValidationError | null
                  {
                    let controlValue = control.value as string;
                    if (controlValue.indexOf(' ')>= 0 ){    ---> the indexOf(can match the val, or any char count)
                        return {noSpaceValidation : true}
                    } else {
                        return null;
                    } } }
        - so when we re dealing with form validator we re not creating the new instances so make that method as static.
        - now inport this cust validator in our comp, and assign that to our username field.

### Angular Routing and Navigation

    - Navigating b/w the pages (comps)
    - Router is the main building core of the ang framework
    - This includes bunch of directives and modules which helps us impl the routing and navigation

"Routers implementation Steps" 1. configure the routes 2. Add router outlet 3. Add nav links to the paths.

- while creating a new ang project, the automatic router conf is a option if we choose y then it will autmatically setup for us.
- but lets see how to do it from the scratch

1.  Configuration
    -inorder to use the router we ve to register the router inside the ang app.module.ts - ex. RouterModule from @angular/router and then register this - while register ve to give the path and comp..Router.forRoot([{path:'posts', component: PostlistComp}]) --> with this kvp we re telling ang to load this comp when the url changes to this path
    - now to show the comp inside the browser with the router, we ve to add the router outlet -- <router-outlet>
    - "Router-Link" by using this we can nav to a specific router when click the btn or link ex. <button routrerLink='/posts'>

- "Base Url" - inide the index.html we ve <base href='/'> tag which tells the root/ base url of our ang app. w/o this ang doesn't know the how to open and nav thru the website
- "Router Link vs href" -- in traditional html pages we use <a href=''> tag(href) to nav thru the pages

  - in ang we can use routerLink istead of the href for almost everything <a routerLink=''>
  - the diff b/w them is if we use the href it will loads the entire page
  - but if we use the routerLink it will loads the comp w/o loading/ refreshing the emtire page
  - when the ang app loads inside the browser for the 1st time it will load all the necessary files (js, css) to the browser, when we nav to the specific comp/pages the ang app will download only the relavant file for that particular comp
  - so using the href is not at all efficient, and if we use this then there is no use of using the ang framework

- "Router Link Active" thisprop/direcitve shows whether the link is active or not.

  - ex <button routerLinkActive="cssclassname">

- "Router Params and vars"

  - give the path the param val --> /post:id
  - then in our html tag pass the index vals b/w the comp <div \*ngFor = 'let post of posts; let i =index' >
  - then make the router link as prop binding and pass the param <button [routerLink] = "['/post', i]">
  - lets capture this router param in the other comp, with the "Activated Route" Module from router, and then inject into the constr
  - and the logic is - ngOnit: void {
    this.route.parmMap.subscribe(value =>{
    let id = value.get('id')
    }) }
    - the paramMap has observable to access that we need to subscribe.

- "Observable" - is from the "rxjs"

  - RxJs is a lib for composing async and event based programs by using the observable sequence
  - the observable is a seq of data that is emitting data asyn or synchronously over period of time.
  - An observable will continuously obser a set of stream data and automatically update or track the seq of data whenever there is a change
  - the name conv is const obsTest$ = new Observable (observer => {observer.next('hello world')}).subscribe(val => {console.log(val)})
  - in order to use the observable we ve tto subscribe, and the .next() is a return fn.
  - and inside the subscribe we can capture all the return val.
  - we can return seq of data / as much as return data we want

- keep the subscribe() open is not a good practice, since its not efficient leads to mem consuming and slows app
- we can close the subscription using "unsubscribe()"

  - "Multiple Router Param" - /post/:id/:title

## Query Params -- ?

    - when we dealing with the router param, if we define any params then we must pass the param when we nav to the route
    - but in query param we don't ve to do that
    - mostly we use this query params for sorting, filtering and searching
    - <button [queryParam="{pageno:1, orderBy: 'newest'}"]>
    - and each query params separated by & sign
    - and to capture the query params its sim steps as the router params
    - ex. this.route.parmMap.subscribe(value =>{console.log(value)})

- "create a separate router module, and register that in our app module" `ng g module app-routing --module app --flat`

## Routing Programatically cli

- now this time while creating the app select yes for the routing, which will automatically generate the routing module
- import the router from router and then inject in the constr, -- this.router.navigate(['['/posts', 1, 'postTitle']']) , we can define the router params inside this navigate
- and to pass the query param -- navigate([['/posts', 1, 'postTitle'], {queryParams: {page:1, orderBy:'newest'}}])

- "Wild Card Routers"
  - for defining the 404 routes..
  - we can define the route path by \*\* , means its a 404 page
  - {path : '\*\*', component: pageNotFoundComp}
  - when our router path doesn't match with any of the defined path that we mentioned then show the page not found comp
  - Note: always define the wild card route at the end our router paths

### route Guard

- are interface provided by the angular
- allow us to control the accessibility of the route based on the codn provided in the class implementation of that interface.
- like the routes based on the specific condns ex - the user login or not.. there are 5. route guards 1. canActivate 2. canActivate Child 3. canLoad 4. canDeactivate 5. Resolve
- `ng g g guardName` it will ask which among the 5 guards we wana impl
- canActivate and canLoad are similar except which the canActivate guard the individual comp and canLoad decides whether to load the entire module or not..
- if we ve some requirements to load the data b4 the page actually loads then we can use resolve Guard.

- **Fork join** is a RxJs operator.. is used when we ve a group of observables. ex - return forkJion([observer1, observer2]);

### Http interceptor

- unique type of ng service we can impl to intercept the incoming and outgoing requests
- and to create one `ng g interceptor name` - will implement the http interceptor interface..
- like for ex - if we wanna add headers (auth bearer tokens) to our request then we can add the interceptor to do for us.. now all our req will be intercepts by the interceptor and adds the token in our header..

- in the nest.js, where the req pass to the mw which does some kinda things like auth, body parser, cors, morgan, and additional other 3rd party functionality we can plug into.. "refer the scrn shot".
- next in the pipeline we can see we ve guards which does canActivate based on some sort of authorization and f/w to route handler.

- and then the interceptor "which are powerful" coz we can ve the direct access to the req b4 it hits to the route handler. and also we can mutate the res after it has gone thru the route handler.
- the way we can do this is in the interceptor we ve next .js and the pipe **RxJs** operators on the res..
- and we can see from the diagram we can really inject the DI on any stages of the pipeline in the diagram..
- **tap** is just a side effect operator.. which will allow us to get access to the res of the route handler ex - return next.handle().pipe().tap((res)=> {})

- **Pipes** just transform the incoming data into any form we wanted to work with..it has lot of flexibility in both transforming and validating the data..

### RxJs

- everything is **Straming** seq of events stream..
- is one of the most useful js lib, its the tool that helps us to deal with the data that flows thru the dimension of time
- the js doesn't give everything that we need to work with async streams of data, we ve promises that only works with the single val, we can ve cb fn but it leads to cb hell.
- rxjs solves this issue. and gives us the powerful functional lib for dealing with streams.
- the Rxjs can be sync or asynchronous.
- **Observable** create a observable (with the observer) and the observer will send using the next() to send the val, which can be of anything a Str, obj..
- to make the observable start **emitting vals** we call .subscribe()

to make an observable from the click() event, ex const clicks = Rx.Observable.fromEvent(document,"click") then we subscribe to it

- **subscribe()** takes an fn (cb) that calls every time an event emmits a val.. and this is what they call **reactive programming**

- we can also covert the **promise** directly into an observable. it is extremely useful when working with js lib that builds on promises

  - ex - `const prom = new Promise((resolve,reject) =>{setTimeout()=> {resolve('resolved')}, 1000);
      const observ = Rx.Observable.fromPromise(prom) ` // then we subscribe to it

- we can also set the timer ex - `const timer = Rx.Observable.timer(1000); //
                                timer.subscribe()
                                ex - interval.subscribe((int) = > print(new Date(getSeconds())))` // lly we can set the interval and on observable

  - will prints the seconds at 1 sec interval

- Rx.Observable.Of('anything' ['anything', 'we want'], true, 32, {a, b}); and subscribe to it. it is important to know, coz obervable we can stream any data we want. keep in mind when building the reactive software.
- **complete** is when the Observable is no longer needed / stop emitting val, / shut down

### Hot vs Cold observables (hot - multi subscription, cold - one)

- cold observable is actually where the data created inside of it. ex create a rand number fn inside the cold observable, and the subscriber will get diff nums every time they call.

- we can make the cold observable hot simply by building the val outside of the observable. by this way all the subscribers will get the same rand num. to make this w/o decoupling the data. we create a new hot observable.
- ex const hot = cold.publish(); hot.subscribe(); and it will emit the data only when we connect ex - hot.connect(); and this time it will share b/w them.

- **Completion observable**

  - when the hot observable reach the complete lifecylce it will send a completed signal. and we can use finally(() => print("all done")).subscribe() ;

- however there are some observables that won't complete automatically. and this will leads to **memory leak** in a data intensive stream. to prevent this we can manually unsubscribe(); // there are also clever ways to do this

- **rsjx map**

  - the map allows us to transform the emitted vals based on some underlying logic.
  - the more practical approach for the dev is when we send an req to api and it resp a json obj, we need to convert the json to js.
  - ex - const json = {"a": "aa", "b": "bb"}; const apiCall = Rx.Observable.Of(json);
    apiCall.map(json => JSON.Parse(json)).subscribe(obj => {print(obj.a));});

  **Do** .do()

  - will allow us to exec the code with the underlying observable,
  - **.filter()** we will give the cond and only vals that meets the condn make it thru

- **.first() and .last()** the first() will only takes the first element from the observable, lly the last operator will takes the last element from the observable.

- **.debounce() and .throttle()** - these two fns will deal with the events that emits way more val than we actually need . ex - mouseMove event in the dom
- so in the throttle we can set the time and it will wait for the next mouse moment (not listen and emit immediately)
- and debounce() does the exact same thing but instead of giving us the first event it gives us the last event.

- **.scan()** scan works exactly like the array reduce fn in the js.

- **switchMap()** this fn is specially useful when we ve one observer we need a val from b4 we get can the 2nd observable.
- ex - `const clicks = Rx.Observable.fromEvent(document, 'click');
      clicks.switchMap(click => {return Rx.Observable.interval(500)}).subscribe(i => print(i))`
  // so the counter/ timer will start and for the new click the timer will reset and start from the 0

  - swithcMap() is commonly used in app dev, when we ve the observable user Id and we need the user id first b4 querying the data.

- **taleUntil()** - allow us to complete an observable based on the val of the another observable. this is actually clever way to unsubscribe the data w/o actually calling it.

- **takeWhile** this one will tells the observable to emmit val until certain condition turns true.

- **.zip()** is actually we can combine observables. zip is actually useful when we ve 2 observables connected in some way, and same length.

  - ex - 2 observables of strings and we can combine them ex - const combn = Rx.Obsservable.zip(a, b) this will combine the a and b from 2 observables according to the idx position.

- **forkJoin()** will wait for both of the observables to complete. and then combine the last 2 vals together.
- this operator is useful when we ve bunch of related api calls nd we want all of em to resolve b4 emitting any data to the ui.

- **catch()** for handling errors.
- **retry()** will rerun the observables as many times we want, when an error is encountered.

- **Subject()** is an observable with few extra bonus features. it has the ability to emmit new data to the subscribers by acting as the proxy to some other data source.. and it has the next()
- we can think of sub as the hot observable but new vals push to it

- ex - we can create a sub and add the subscribers nd calls .next() which is not possible in normal observable.k
- the main benefit of subject is to broadcast new vals to the subscribers w/o relying on the source data.

- **Behavior subject** is similar to the subject except that it has the concept of current val. sim to sharedReplay() the last emitted value will be cached. and every subscriber will get the val. this is powerful when doing things like state management and the frontend app.
-

- **multicast()** is used to send val to multiple subscribers w/o any related side effects.
- this could be highly beneficial if we ve a multiple subscribers for a single data source,

- **Backpressure** means when we ve an observable that emitting way more val than we actually need. we can control these backpressures with some fns ex - debounce(). throttle()
- these fns will filtering lot of data, if we want to keep the data not all at once but for later then we can use **buffer or bufferedCount()** ex - const buffered = event.pipe(bufferedCount(20)); bufferd.subscribe(print); // this event will only emit when the buffer array size reaches the len of 20.

- **memory Leaks**
  - there are 2 ways to prevent the mem leaks in RxJs.
  - 1. unsubscribe() the subscription. 2. takeWhil()e (the recommended way) - with this way it will only emit when the certain condn is true and we don't ve to unsubscribe with this ..
  ----------------------------------------------------------------
### Angular FCC

- features - data binding, templates, pwa, observables, routing 
- pwa - progressive web app. we can install the web page as the standalone mobile or desktop application.
- spa doesn't make req to the server for every url reqs.. ng has its routing functionality to create spa..ng also offers ssr which supports spa.
- if we ve multiple props/ vars in the enum when we compile the code the ts will gen a idx val for each of the val. instead if we mark the enum as **const** then during the compilation the ts will only gen the code for the val/prop we specified in the enum. means it will trim down all the other var's type information.
- in ts **tuple** seems exactly the array except which the tuple has the kvp ex - [a:b, b:c] // can be useful in mapping the ex - employees name and the email etc.

- **fns**
- Rest params - aka spread operator, its useful if we wanna pass n no.of params ex - ...nums - ex - const fn(num1: number, num2: number, **...num3**: number[]): number{ return num1+num2 +num3 reduce((a,b) => a+b, 0) }; // now while calling the fn we can pass the n no. of val in the ...num3 arg .. this is called as Rest params

**- Generic fns**- 
      ex - fuction get<T>(item: T[]) : T[] {return new Array<T>().concat(item)}

- "**classes**" in ng we will be using classes for getting the data from our backend, and we will be using it for the comp, and the class will **wrap our entire business logic** 
- the **#** is also the syntax sugar for the private field ex - instead of declaring private id: string; we can declare #id: string; // which will be consider as the private field.
- static members (vars or fns) with the static we can access the static members w/o even creating a new instance of the class. just call the classname.followed by the static mem.
- we also ve getter and setters methods.
- when to use interface and when to use class - in case if we re retain the type then its better to use classes, specially in backend.
- **Decorators** or annotations with the decorators we can modify the behavior of the class or method during the runtime.

### project 

- to create an workplace `ng new workplaceName --createApplication=false` and we can add the project in this workplace.
- to create an app `ng new app appName`


- polyfils.ts will make sure that our code are backward compatible with some older browsers. it will always add some extra code to our final bundle.
  - and this file imports zone.js which actually patches lot of features which are not supported by lot of browsers.
  
- **Mono repo**
  - in ng we can create and maintain multiple apps in same repo
  - deploy multiple apps/libs in the same repo.
  - to run the app locally `ng serve -o`
  **app.module.ts**
- the comps/pipes need to be registered at the declaration[].
- **Template Syntax**
- **binding syntax** there are 3 ways to bind our info from the ts file to the html. 1. interpolation 2. property binding 3. Event binding
  - 1. interpolation - {{}} - we can bind date, nums and any objs.. 2.  prop binding (with the box syntax) - ex the html elem prop binding ex - <div [innerText]="num" />// this is known as the prop binding we can use with any valid html tag.
      3. event binding - (with the banana syntax/paranthesis) ex - <button (onclick) = "toggle()" />
  
- **Directives**
  -used to change the behavior of the dome element.
  - directives can impl to all the lifecycle hooks.. directives can't ve the template.
  - there are 2 types of directives 1. structural and 2. attribute directives.
- 1. structural directives - will gives performance issues (the structural directives are the ones with * ex - *ngIf)
    - the structural directives can add/ modify our dom. (so any directive that can modify the dom elem is the structural directive) 
  2. Attribute directives - we will be using this most of the time.
      - the attribute directives can modify the dom,  but they **can't  add / remove** elem from the dom.
- **built in directives** *ngIf, *ngFor, *ngSwitch, ngClass, ngStyle.. 
  - ?? - null collision operator and optional chaining - rooms?.availableRooms 

### Pipes

-  | - looks like the union operator.
- as we know pipes are used to transform the data, and "pipes don't change the actual obj."
- some of the built in pipes are .. Date pipes, uppercase, lowercase, currency, decimal, percent, json, slice, async pipes etc.
  - in those pipes the json pipe will display / fetch the whole obj ex {{room |json}} // and the recommended way is to not use this pipe in the prod, only for the debug purpose only.
  - slice pipe accepts 2 parameters start index and end index, ex - <div *ngIf let i of rooms | slice :0 :3 /> // if we ve large data set it is not recommended to use the slice pipe to filter the record.
  
### Lifecycle hooks

- comp instance has lifecycle hooks which can help us to hook into diff events on the comps.
- lifecycle ends when the comp is destroyed.
- some of the lifecycle hooks are ngOnInit, ngOnChang, ngDoCheck, ngAfterContentInit, ngAfterContentCheck, ngAfterViewInit, ngAfterViewChecked, ngOnDestroy.
-  and all of these hooks are impl from their corresponding interfaces


- **ngOnChange** - there is a imp thing called as **change detection**, 
  - "change Detection" - whenever there is some action on the data/view needs to be updated from the parent to the child, the ng uses the Change Detection to achieve this
  - there are few methods on change detection strategy.. ex onPush this can be applied only if we re not modify any data internally in our comp.
  - whenever we re workng with the change detection strategy of onPush the data(which we re trying to assign) shouldn't be immutable.
  - incase if we re trying to modify it, we ve to use the new instance. ex - this.roomList = [..this.roomList, room] // now this way it will just works. (instead of this.roomList.onPush(room)) where we re trying to mutate the state which is not possible with the change Strategy.
  - this onChange hook we can only apply in the comp which has @i/p prop/ (whenever we get the new val).. we can't apply that onchange any where else.
  - this hook is very useful, in case if we wanna control what val needs to be updated when the new data is passed.
  - regardless of where this hook is implemented it will listen for the changes/event in our entire app. 


- **comp communication** - this is related to the ngOnChanges hooks . when two or more communicates, there are diff ways to achieve this.
  - ex - @Input, @Output, using @ViewChild, @viewContent and using services.
  - in our child we can make  @Input() rooms: Rooms[] = [];  now this comp will be part of the html tag. and we can pass the data(and access the Rooms's prop) ex - in the other comp <div [rooms] = "roomslist" /> // this is called the parent and child relationships. in the i/p the data is coming in from the child to the parent.
  - to pass the data back from child to parent, @Output() roomSelected = new EventEmitter<RoomList>(); // the @output are event.. and in the parent whoever subscribe to this event will get access. ex - <div (roomSelected) = "roomSelected($event)" /> // this is how we receive the event from the child

- **ngDoCheck** - this hook will be called when the event is triggered, the recommended is to avoid this hook as much as possible.
  - also we should not use the ngDoCheck and the onchange hook together. since the both are almost do the same thing.

- **@viewChild** - if the comp does not ve the i/p prop and how to access the prop ? 
  - ex - @ViewChild(HeaderComponent, {static: true}) headerComponent: HeaderComponent; // here we re creating a new instance of the header comp..like this we can access.. by default the static prop is false. 
  - but there is a caveat. whenever we ve a async code which has to await for something, and this static prop should be false.
  - this view child will always access the first child in the comp . and if we wanna access more than one child then we ve to use view children hook.
  - **afterViewChecked** hook we should avoid using this hook.
- **view container ref** also with the view container ref we can dynamically load the comp.
- **ngTemplate** tag - this itself won't render anything. but it will help us to render other things.
  - ex - <ng-template #user /> this # is template reference. and we can access that template ref using the viewChild hook. we can dynamically render the html el using the template reference.
    - and with the viewChild hook we can dynamically render the comp. (from the comp ref)

- **ViewChildren** same as view child, but with this we can access more than one children in the component.
  - 
- **ngAfterContentInit** - we ve <ngContent /> element with this we can position our content
  - with this we can achieve the order of the comps,  
  - we can use the deco @ContentChild() to create the instance of the comp, and then access inside the ngAfterContentInit hook.

- **ngDestroy** - the final stage of the comp lifecycle..
  - whenever the comp is removed from the dom this ngDestroy hook will be called. 
  - this hook is useful when we ve mem consuming app and we wanna get removed. and we can make use of this hook.
  - ex - the places like unsubscribe and want to delete the data from the local storage as soon as the comp is removed/ unmount.

### DI 

- dependencies are classes which we can inject into services or comp.
- its a design pattern, ng has a built in DI support. this is one of the unique features in ng, and its powerful.
- this DI pattern says we should not create an instance directly, ex - roomService : RoomService = new RoomService()// we should not create like this. rather
- creating the instance manually like the above, we ve to create using the DI.ex - constructor(private roomService: RoomService){}
- **providers** there are 3 types of DI providers
  - 1. class based 2. value 3. Factory provider.
- @Injectable({providedIn:'root'}) // means we re registering our service in the root module..with this ng will register the service in the app.module providers automatically and also if the service is not used it will remove in the prod build.
  - and once we use this injectable to register our service, then we will get the **singleTon** for the service which we can use thru out our app.

- the flow of DI will be look like this ..Root Module Injector --> Module Injector(in the main.ts.. we ve platformBrowserDynamic()) --> Null Injector

- **Resolution Modifiers** - there are 4 resolution modifiers 
    1. self 2. skip self 3. optional 4. Host
  1. @self() - inside the constructor ex - constructor(@Self() private roomService: RoomService){} // means this particular service should be register/ available here.
  2. @SkipSelf() - lly this modifiers as the name says skip myself from the entire DI tree/ flow.
  3. @Optional() - ex - lets ve a log service which we wanna use em only in the dev and for the prod don't wanna mount this service this can be achieve by using thsi Optional modifier 
     ex - constructor(@Optional private logger: LogService) // now if we remove the provider root obj inside the @Injectable() in the logger service we won't be getting any error, since we marked this service as the optional
  4. @Host() - we use this rarely.

2. Value Providers
   - we can pass the obj as an service, lets say we ve the app config service (which is an endpoint) and we register inside the provider (as the value provider) of the  app. module
   - and to use this service .. constructor(@Inject(APP_SERVICE_CONFIG) private config:AppConfig){}
   - side note : the ts has the types for all the web api that we use in the js the file called lib.dom.ts

### ng Http and observables

- lets see how to interact with the api and handle/make diff http calls
- this http client in ng internally uss RsJx.. the ng http client module from the "ng/common/http"

- **Share Replay RxJs** - will helps to cache the req, so we don't ve to make the req again.
  - since the streams can't  be mutable, if we wanna transform the stream data then we ve to use the **pipe** inside the observable and then use the shareReplay for caching

- whenever we use the streams use the $ as the postfix ex - getRooms$ = this.http.get<RoomList[]>('api/rooms').pipe(shareReplay(1)) // $ is like the naming convention.

- **Async Pipes** - this will manage the subscription for us.. this will work along with the streams that we subscribed. 
- lets say if we ve multiple streams and subscription its hard to track em all and unsubscribe it all. but by using this async pipe it will **unwrap** the data (means, it will unsubscribe)and give the data back to us.
  - ex - {{getRooms$ |async| json}} // so in nutshell it will also take care of the unsubscribe() for us.. so we don't ve to keep track and unsubscribe it all manually.

- **CatchError** to catch the error in the streams by using pipe ex - getRooms$ = this.roomService.getRooms$.pipe(catchError((err) => console.log(err)))

- **Map Rxjs** we can use this inside the pipe (in our stream) to modify/ transform the data.. 

### Http Interceptor and App Initializer

- allow us to intercept the http request, ex - we can change data add the header to our request.
- we can add multiple interceptors (like one for add dateTime stamps, one for the jWT etc), this will sit in b/w the req and the res ..
- to create a interceptor `ng g interceptor name` // will impl the Http interceptor interface.
- this interceptor will ve the **intercept** which extends from the Observables.
- the ng will also adds lot of interceptors internally by default.
- the http header takes Kvp (ex - the token and its val)

- note: we can't modify the req, if we want we can clone the req and then modify it.

- **app initializer** will allow us to inject the fn as app setup.
- this app initializer can load the data b4 our service initialization.

### ng Router

- provides functionality to add routes, provides spa, features to add nested routes.
- here the recommended is to use the **routerlink prop** instead of href in the <a> tag ex <a [routerlink]= "/" > // as we konw the href will reload/ refresh all the content..
- just like the slot we ve to add <route-outlet /> after the routes..

- once we install the material ui, we can also generate their built in route navigation. ex `ng generate @angular/material:navigation app_nav` this will generate a responsive navigation bar for us. w/o even a single line of code.

- **wild card route** as we know path: "**" any route that doesn't match the route we defined is 404 route
- **dynamic routes** path: rooms/:id .. and then in the <button [routerLink]=["/rooms", room.roomNumber]> // and to get the id of this rooms dynamically we can use the activated route service.
- 
- **Activated Route Service** allow to read the outer data, allow to access the snapshot data and allow to access the data from the route config.
  - note the **router data** is also observable ex - constructor(private router: ActivatedRoute){ this.router.params.subscribe() // we can use this approach but again we ve to unsubscribe it.. so instead we can use the other way
  - ex - id : number = 0; id$ = this.router.params.pipe(map(params) => params['roomid'])) // and then {{ id$ | async }} // now by this async pipe way, we no longer need to worry about the unsubscribe()
- 
-**paramMap** if we ve multiple params then we can use the other api approach(which is similar). ex - id$ = this.router.paramMap.pipe(map(params) => params.get['roomid']))


### forms

- **Template Driven Forms** from the formModule, we create the form using html form tags, gives more control using html. using **ngModel** for 2 way data binding.
  - ex - <form class="form-group"> and the i/p fields are the form controls
  - and to access / communicate with our model properties <form class="form-group" [(ngModel)]="room.roomType" > // this way we can bind our roomType prop in the room model.. 2 way data binding.

- **form Validation** - use the disabled prop to avoid submitting the blank form. and use Template var **#** to access the state of the form , or in other words by using this # we will get the instance of the form.
- some of the other form validations are min and max length, email and password, pattern or regex, and these are all some built in html form validators.
- note : the form validation should be happened in both the frontend and backend. not just one place.
  - ex - <form #roomForm="ngForm" (ngSubmit)="addRoom()" > 
  - there are few vals/attr we can pass on this form instance ex -roomForm.pristine, dirty, valid, invalid, value. all are boolean
  - ex if our from control has some validation error then the valid prop will be false and invalid prop will be true. and vice versa.
- **pristine** means fresh state to our form control, no one has ever touched our form. and in the initial state it is true, as soon as someone will interacts the pristine will become **false**
- the form becomes pristine to dirty will never come back. until we reset the form.

- **submit and reset** - use ngSubmit to submit the form and use reset() method or resetForm() method to reset the form.

**custom Directive with forms** 
- we can create a directive with `ng g d hover` .. used to change the behavior of Dom on hover, use host listener to listen for the event. the directives are some helper fns for our Dom elems.
- directives are sim to components, except that they don't have template.
- **ElementRef** will ve the native element, and it will give the ref of the elem in which we re applying this elem ref.
- **DOCUMENT** will give access to the entire document, ex constructor(@Inject(DOCUMENT) private document: Document){} // now we can use all the methods(like tons of em) to access the docs. ex - document.getElementsById();

-**@HostListeners** are the event listeners that will listen to any events that is happening in the parent component. ex - @HostListener('mouseListen') onMouseEnter(){} // lly onMouseLeave().. are built in fns.

**custom validation** lets see how we can use the custom directives for the custom validation.
  - write custom validator for the template driven form
  - add logic to validate the form field. 
  - implement validator interface in the custom directive.

### Router Service

- access the router events, navigate using the router service.
- there are methods like this.route.navigate() or navigateByUrl() // the navigate will takes list of cmds and create entire url for us. and the navigateByUrl() we will just say this is the url just nav us.

- **Feature module and routing** - configure routing at feature module level, forChild used to config the route.
  - we should wrap all the features in a single modules. and it will be easy if we ve lazy loading.
  - there is concept called **scam** means single component and Module.. means for every single comp we should ve the module.
  - ex `ng g m rooms --routing --flat=true` or `ng g m bookings --route=booking --routing --module=app`  to create a room module and routing module in the same dir as our comp.
    - when we ve feature module with its own routing we should always register it b4 the app routing module in the imports []
  - now in this module we can move all the rooms related comps here. from the app.module.
  - note: one comp can be registered as part of only one module.. 

- **nested routes and child routes** - using child routes we can do nested routes 
  - we can do this inside the routes[] add the children prop and move the route inside.

### Lazy Loading

- allows to split the code at module level, load the code when user needs it. Reduce the main bundle size.
- `ng build -c=production` for the production grade build..
- if our bundle size is too big (ex - in mb) then it will take longer to load in the user side, so we can split the code
- the ng routes will helps to split our codes based on different routes.
- so when we re creating the lazy loader, we ve to keep in mind that which comp are going to be reusable, in that case take that comp and put it some where like the reusable.
- and the lazy loaded modules should be isolated

- to do the lazy loading rather than directly importing the module to the router path.. ex - const routes: Routes = [{path: 'room', loadChildren: () => import ('./rooms.module').then ((m) => m.RoomModule)}] // this is how we do lazy loading
- the lazy loading will import when some one clicks on the room module at the run time.
- we know some of the modules are not gon be used frequently, such as the user profile module, in that case/ scenario we can use the lazy loading approach.


### Route Guards

- protect our routes/ authorized routes
- some of the route guards are 1. canActive 2. canActiveChild 3. canDeactivate 4. canLoad 5. Resolve (all these guards are the interface that we can impl)
- ex - `ng g g login` there we can select which of those guards we wanna use. then add the guard in our routes path 
- ex - const routes :Route = [{path: 'employee' canActive: [loginGuard],] 
- lly the canActiveChild for the child/nested routes guard.
- **canLoad** can i load this particular route..and this is only applicable for the **lazy loaded** routes.


- **provider types** there are 2 types 1. root (as we know its been commonly used so far), 2. any
- the diff b/w em is whenever we uses the root we will get the single ton of our service, instead if we use any as the provider type 
  - ex - @Injectable({providedIn: 'any'}); // if we use this provider then it will create a one service for the entire app, and then it creates separate instance for the each lazy loaded module.(in case if its getting used)
  - so we will get new instance (with new val for the service), this is useful in when we wanna pass some other config that we want to override some config inside the lazy loaded module.

- **canDeactivate** - in the scenario, if we ve partially filled form and submit or exit the form page then we can show the msg do you wana to exist the page.
- we can make it like if its pristine the user can't exit and if its dirty then he can exit.

- **Resolve Guard** is actually available for the data Pre Fetching, ex - the user should not allowed to go in the particular view until the data is unavailable.
- impl from the resolve interface ex - export class CommentGuard implements Resolve<Comments[]> {}
- then in our route path we can add (its a kvp) {path: '', resolve: {comment: CommentGuard }}
- then we can use the activated route and use the pipe(pluck('comments')) to pluck the comments on the route.. 

- **JIT vs AOT** ng comes with default JIT enabled, if we disable the AOT then even if there is any error in our code the compiler can't detect it will just build as it is. by default AOT is also enabled.

### Reactive forms

- to creating the forms use form group class to group the form fields(controls),
- use form control class to create the fields. and use **form Builder** to build the complex forms.
  - ex - `bookingForm ! : FormGroup;
          constructor(private fb: FormBuilder){}
          ngOnInit() {
          this.bookingForm = this.fb.group({roomId: new FormControl('')}) }  // this is how we create  the reactive form and to render this in the html we ve to bind this form.
  - ex - <form [formGroup]="bookingForm" /> // this is how we bind the form
- we can also ve the nested form controls..we can create an another form groups inside the existing form .
- .value() prop to get the val of the form field, and to get the value of the disabled form field(controls in disabled state) use **.getRawValue();**

- **Adding controls Dynamically** - we can use addControl to add the controls dynamically and removeControl to remove the control
  - and to render the dynamic controls first check *ngIf and if the control exists then renders it (by get()) method.
  - and to remove the control/form field we ve to keep track of the idx (in the remove()) method and also in the form template.
  - to reset the form ex - this.bookingForm.reset({pass in all the controls/form fields})

- **Validators** use the validators class to access the built in validations(such as min, max etc)... (validation is an interface don't get confused)


- **Patch values and set values** - while binding the data we can use patch val or set values. while using setValue we need to pass all the props. ex- this.bookingForm.setValue({pass in all the form fields})
  - patch value allow us to skip the vals for the form control.

- **listening to form val change** valueChanges events will allow us to listen to all value changes.
- useful to capture the changes happening in the real time. this **valueChanges** is a stream
- ex - this.bookingForm.valueChanges.subscribe((data) => {})
- we can use the updatedOn: Blur prop in our form field, so it won't change the val as we entered instead it will update the val once we re out of the form field. and this is from the Abstract control option.
- this updatedOn has other 2 prop change and submit. 
- we can also use this property to control the template driven forms ex - <form [ngModelOptions] = "{updatedOn: Blur}" />



### Using Rxjs map operators

- mergeMap operator, SwitchMap, exhaustMap..
- **mergeMap** - doesn't care  about the sequence, it will try to post the data or subscribe to the stream as soon as the data is provided.

- **Switch Map** - switch map will cancel any existing requests, if it receives a new data.. so we can use this in the scenario when the subscribers will get only the latest data, then switch map is the way to go.(the prev stream will be canceled)

**Exhaust map** exhaust map cares about the sequence, until or unless the previous req is not completed, it will not subscribe to any changes..


### writing custom validation

- lets see how to add validation for the control, and add form level validation.
- we can create a custom validation class and create a fn (with the parameter of type **AbstractControl** from the ngForm and create our cust validation fn. 
- 

### custom pipe

- ex - `ng g p pipe_name` then we can implement the pipeTransform Interface to the pipe service.

### Global error handling

- the ng has the service called errorHandler. we can extend that service and implement our custom errorHandler.

### Testing 

- `ng test` - the ng by default gives the set up for the unit tests.. it has karma and jasmine to write the test cases.

----------------------------------------------------------------

### trackBy

- can be used alongside with the *ngFor specially with the obj arrays
- it is useful when we ve a big list of users and we wanna update any changes in the list by default the change Detection will refresh the entire list, which may leads to the performance issue.
- but if we use the trackBy then only the changes in the list will be updated instead of the entire list..
- ex - <div *ngFor= "let user of users; trackBy = trackByUser" /> // and the trackBy fn is like trackBy(idx: number, item: any){return item.id }

### Service Workers (web workers)

- as we know the js works on the single threaded(which usually manages all our http).. 
- now js and the browser also offers us an additional thread and we can run the web worker on a number of thread..this thread is different from our main js thread
- this thread is also decoupled in our html pages..means this can also continue running in the bg for ex on our mobile phones ..like push notification etc..
- and another thing the web worker do is it can manage all the different pages and it can listen to outgoing n/w reqs. ex - if we re fetching the static assets like css, imgs or fetching data from api.
- the service worker will listen em and do something with these reqs ex - cache the response in a special cache storage or then also return these res back to our page, so it can also work even if there's **no internet connection**
- so the service worker is like the proxy b/w our frontend app and the http req that we send to the backend..

- `ng add @angular/pwa` will configure our existing package and starts with the preconfigured service worker for us..
- the ng ServiceWorker module from the ng service worker.. (/.ngsw.worker.js) is a auto generated file.. which has lot of functionality we don't wanna write on our own..
- we can also ve the "ngsw.config" file where we configure the service worker which will be generated..
  - to cache the api in ngsw.config file add a new obj "dataGroups":[{}] // this data group is for dynamic data.. and inside the array we can define the url.. and the cacheSize(like how many cache we want for ex only 10 outgoing req we wanna cache, then the max age or the time the cache wanna live.)
     - and also the other properties like timeout:10s, and strategy: "freshness" // this means always reach out the backend first and only use the cache if its offline. 
- another thing to keep in mind is when we transfer the data from the main thread and the web worker the data is being cloned by structural cloned fn.
- which means when it comes to huge obj or the array, it might take some time to create the copy of the clone.
- and we can optimize it by transferable objects.. they can switch their context, they can be transferred from their main thread context to the webworkers w.o cloning that.
- 
-refer mdn doc for the list of supported objects some of em are readable stream, writable stream, arrayBuffer, transform stream, imageBitmap etc..

**limitatinos**
- some of the limitations of the web workers are 
  - no access to the DOM manipulation.
  - no access to the window and the document.(they ve their own global obj called self but not the window obj)
  - no access on the SSR.
- usecase - 
  - once we create the webworker then we can initialize it in the app.comp.ts. and register the path to the webworker, ex - this.worker = new Worker(new URL ("./image-filter.worker", import.meta.url));
  - now the main thread and the webworker will communicate thru the dispatching and receiving msgs.. 
  - in order to dispatch the messages from the main thread to the webworker, ve  to use the postMessage() in the main thread(app.module.ts) ex - this.worker.postMessage(customExpensiveImageData) // this is the crazy expensive image data method we can also move the fn impl of this crazy operation into our webworker.
  - once we dispatch the messages, then the webworker can listen to the event thru addEventListener('message',({data}) => { const processedImage = customExpensiveImageData(data)}; postMessage(processedImage)) // here we re also sending back the processed image back to the main thread. 
  - and in the same way the worker can send some msg to  the main thread. and to receive the message from the webworker in main thread we can impl the onMessage handler fn ex - this.worker.onmessge = ({data}) => {console.log(data)} 

    - 
- 
