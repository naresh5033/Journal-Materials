## Angular 

- the angular introduced by Google.
- is a front end framework build using the js by google

SPA - single Page App
    - that doesn't need to reload the page during its use and work with the browser
    - is all about the user exp and the speed 
    - angular is for single page app

Angular Cli - we can update, scaffold, ,maintain and develop an app using the anular cli.
    - ```npm i -g @angula/cli```
    - ```ng new projectName``` 
    - ```ng generate``` gen comp, routes, services, and pipes with a single command
    - ```ng serve``` to run the dev server ```ng serve -port 4330``` to serve with the required port we want

# Angular Component

- in order to work with comp we ve to import the comp modul from @angular/core
- In order to transfer the ts class to an angular comp we ve to decorate the class with @Component({selector, template, style})
- or if we use the separate files not in the inline for the template and styling use the url -- ex: templateUrl prop, and styleUrl and just give the path of the files
- and then we ve to register this comp into the app.module.ts -- inside the @NgModule({})
- if we don't wana create the comp from the scratch by ourself then we can use the ```ng generate``` to create the comp with the boilerplate(including the spec.ts file for the unit testing) --> ```ng g c componentName``` 
- by this way it will also automatically register our comp in the app.module.ts file

# NgOnInit() life cycle hook

  - this onInit interface from the angular core is a life cycle hook
  - ngOnInit() is a callback life cycle method, which will invoke after the comp is fully initialize (or fully loaded in the browser) inside the dom .
  
- "Angular Data binding"
    - to create a js inide the html file in ang we can use the jinja template {{}}
    - this is called the data binding in anular

- "Data sharing b/w the comps"
    - there are 3 ways we can share the data  b/w the comps 
    1. parent to child comp via @input (from ang core), use the deco inside the child comp to capture the var from the parent comp
    2. child to parent via @ViewChild() --> inside that pass the child comp that we wana get the data
    3. child to parent when there is a event via @output and Even Emitter

"After View init" life cycle hook from core ang
    - then impls this hook inside the class then
    - use the ngAterViewInit() deco  this method will call once after wave initialized on the browser 

"@Output and the event emitter"
    - this method is ideal for the events like form data, btn click and other user entries
    - the Output and the EventEmitter from the ang core
    - in plain js we ve onClick prop in btn but in ang we ve to use <button (click)='function name'>

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

"ngFor" 
    - is structural directive
    - used to render an array inside the view
    - with this we can manipulate the dom (add/remove html elems to the dom) 
    - ex. to show the list of our arr <li *ngFor= "our posts "> {{post}} 
    - "Fetching obj array" - <li *ngFor= "our posts "> {{post | json}} --> inorder to fetch the obj in to the browser we ve to convert the obj into json using the pipe op.

    - "Change Direction" 
    - "Array Index" -> to add the index of the array we can directly pass in the elem <li *ngfor= "post array"; let i = index> <button (click)="onDelete(i)">

- "*ngIf directive"
    - for the condn view, and this is also structural directive 
    - ex <div *ngIf='postArray.length > 0' >

- "ngTemplate"
  - as we know in ang we can identify an elem using the # --> template var, and we can use this in our condns
  - but when we wana use the template var we ve to use it inside the ngTemplate tag otherwise it won't work
  - ex. <ng-template #templateVar> (now we can use this inside our condition)
      - <div *ngIf='postArray.length > 0 ; else templateVar ' >
  - we can't use this ng-temp tag as normal html tag, we can only use this for the structural directives

- "ngSwitchCases"
    - rather than using the ngIf several times to check the condns, we can use the ngSwitchCase
    - and there is also *ngSwitchDefault for the default case


- "NgStyle" directive
    - we can do multiple style binding using this 
    - when define the ngStyle condn we ve to define the else part as well.
    - ex. <h1 [ngStyle]="{color = isActive ? 'red' : 'black'}">

- "NgClass" directive 
  - doing multiple inline style binding is not a good practise 
  - so we can put the styles in css file and then make a class binding 
  - ex. <h1 [ngClass]= "{classname : isActive }"> the key is class name (that we wana bind) and the val is condn/ var

- "Structural Vs Attr Directive"

    "Structural"
  - 1. the structural directive are responsible for the html layout
  - 2. shape or reshape the dom elem by add/ remove the elems
  - 3. can identify with the leading * sign
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
2. decimal / number -> <h2>{{decimalVal | number: '1.2-2'}}  --> 1. the int that we ve or we want(if we write 2. then it will add 0 in front of our int and make it 2 dec val) and then the min decimal we want and the -2 for max dec val 
3. currency pipe --> <h2>{{dollar | currency: "CAD" : '2.2-2'}} , by default its $ and we can pass the currency types we want as an attr. and sim to number pipe we can also control the decimal of the currency by passing an addition attr
4. Date Pipe - <h2>{{date | date: 'short'}} or shortDate arg for only date. refer the ang docs date pipe for all the args.
5. Json Pipe - to convert the data into str json ex- <h2>{{object | json}}
6. Percent Pipe - to show the percentage.. ex- <h2>{{0.576 | percent : '1.1-2'}} -> this will multiplies the val with 100 and give us the percentage, and we can also control the decimal.
7. Slice Pipe - with this we can slice an array.. ex- <h2>{{nameArray | slice : 0:2}} the start and the end idx of the array
8. Custom Pipe - to create a cust pipe.. create a dir inside app dir called pipes and create a ts file with the name conv .pipe.ts
      - import the pipe and pipeTransform from core, and add the deco @Pipe({name: 'custPipe'})
      - then create a class - export class CustPipe implements pipeTransform{
                                      transform (value:any, args?:any){
                                        return "city Name" + value;     - our cust logic
                                      }}
        - and then register the pipe in our module.ts, as same as comp
        - <h2>{{userCity | custPipe}}
    - sim to the comp, we can also gen the cust pipe using the cli ```ng g pipe Pipes/pipename ``` will gen the pipe inside the pipes folder
    - will generate the boiler plate, we can just add our logic 
    - "Cust Pipes with args " for ex if we wana create a summary of a paragraph with only the first 20 words, then we can use the .substring(0, 20) method in our cust pipe's logic
    - or if we wana do this dynamically pass the args val inside transform(val:string, lenth:number)
    - now we can pass this arg inside our pipe <h2>{{paragraph | custPipe : 20}}


# Angular Service
  
   -   so far we ve shared the data b/w comp that has related with each other 
   -   if we wana share the data b/w comp that has not related then we ve to use the ang services.
   -   We use the ng Services to share the data and methods among the comps whether it is related or not.
   -   for ex. if we ve array we wana share, first we can create a service and declare the array and then import the serv to the comps when we wana use the array
   -   this service will helps us to avoid the code repeatation and save time. \\
  
  
   -  "Manual creation" , lets create a dir called services inside the app dir  and then create a file with the naming conv .service.ts and create a service class with the logic
   -  then we ve to create a new instance for that class inside our comp constr. \\

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
  
- by usinfg the cli the cmd is ```ng g s services/servicename``` 
- Usually all the data manupulating stuff, such as load, save, delete data inside the service file

- "Ang Interface" - if we made any mistakes in the array obj, or typo then the data won't be fetched properly.
- to solve this prob, we can make the blueprint of the array obj. and set that to the new array that we wanat create.
- to create the blueprint of the obj, we ve to use the "interface" using the cli --> ```ng g i models/interfacename``` --> inside the interface define our obj
- and then go to our array and make that type as our interface.

# Angular Template driven Form

- "Form Types" -- we can't do the validation by using only the 2 way data binding approach.
- for this ang has 2 approach for the form 
  1. Template Driven Form  - by using the ang directive (ngForm)
  2. Reactive Form or Handcoding

1. Template Driven Form 
   - with this approach we ve the full control on the form 
   - "ngForm" directive - for the template driven form module we ve to import the form from ang
   - to make a form template driven <form #templateVar='ngForm' (submit) = "onSubmit(templateVar)"> now the form is temp driven

"Handle Form Data"
    -    In ang to handle form data we ve 2 classes 1. form group and 2. form control 
    -    1. formgroup -- 
    -    2. formControl -- use this inside the form fields <input formControl>
    -    we use this inside the form tag <form formGroup>
    -    inside the formgroup  and form control class we ve few methods value(), touched(), untouched(), dirty(), pristine(), valid(), errors() etc. to access these props we ve to create a new Instance of the formgroup.. New formControl(); 
    -    we can create the inst by 2 ways 1. handcoding or Reactive Form  and 2. by using the ang directive (ngForm) or Template Driven Form
    -    Inside the app we can ve multiple formgroup and inside the form group we can ve multiple formcontrol.
    -     - In the template drive form we do all the things inside the html templ view  .

"ngModel and form Control class" 
    - for the form group we add the "ngForm" directive and for the form Control/input fields we ve to add the "ngModel" directive, and the i/p fields must ve name prop to use this directive

"Template driven Form Validation"
    -  <input #email = 'ngModel' (change)="getValue(email)">
    -  <div *ngIf = 'email.touched && email.isValid'> email id required </div>--> those are the form objs we can find more props in the console.log 

"Form Validation Types"
    - minlength, maxlen, 
    - email i/p fields to add the email field we ve to use the pattern attrn <inpput pattern= "paste the pattern"> the val we can find in html email pattern and paste the regex as val
    - text area - we can add required prop 

"Disable the submit " 
    - until all the required fields are filled, we can do this by prop binding
    - ex. <button [disable]= "formTemplVar.invalid"> 

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

"Nested Form Groups" 
    - inside our main form group we can add the child or nested form gruop ex. if we wana add the contact details.
    - to create a nested form group inside the main form we ve the prop called "form Group Name"
    - ex. <div formGroupName='contactDetails'>

"Reactive Form Array"
    - the formArray from the core 
    - to create a one just create a instance 
    - ex. skills: new FormArray([]), to push the skills inside the [], first we ve to convert the data into form array
    - ex. addSkills(skills: HtmlInputElemen){
              (this.form.get('skills') as FormArray).push(new FormControl(skills.value)) }
    - and to render this in view
    - ex. <li *ngFor="let skill of skills.controls"> will render all the form control vals inside the skills[]

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

"Routers implementation Steps"
    1. configure the routes
    2. Add router outlet 
    3. Add nav links to the paths.
   
   - while creating a new ang project, the automatic router conf is a option if we choose y then it will autmatically setup for us.
   - but lets see how to do it from the scratch
 
 1. Configuration
     -inorder to use the router we ve to register the router inside the ang app.module.ts
            - ex. RouterModule from @angular/router and then register this
              - while register ve to give the path and comp..Router.forRoot([{path:'posts', component: PostlistComp}]) --> with this kvp we re telling ang to load this comp when the url changes to this path
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
  - then in our html tag pass the index vals b/w the comp <div *ngFor = 'let post of posts; let i =index' >
  - then make the router link as prop binding and pass the param <button [routerLink] = "['/post', i]">
  - lets capture this router param in the other comp, with the "Activated Route" Module from router, and then inject into the constr
  - and the logic is - ngOnit: void {
                          this.route.parmMap.subscribe(value =>{
                            let id = value.get('id')
                          }) } 
    - the paramMap has observable to access that we need to subscribe.

- "Observable" - is from the "rxjs"
    - RxJs is a lib for composing async and event based programs by using the observable sequence
    -  the observable is a seq of data that is emitting data asyn or synchronously over period of time.
    -  An observable will continuously obser a set of stream data and automatically update or track the seq of data whenever there is a change 
    -  the name conv is const obsTest$ = new Observable (observer => {observer.next('hello world')}).subscribe(val => {console.log(val)})
    -  in order to use the observable we ve tto subscribe, and the .next() is a return fn.
    -  and inside the subscribe we can capture all the return val.
    -  we can return seq of data / as much as return data we want

- keep the subscribe() open is not a good practice, since its not efficient leads to mem consuming and slows app
- we can close the subscription using "unsubscribe()"
  
  -  "Multiple Router Param" - /post/:id/:title


## Query Params    -- ? 
    - when we dealing with the router param, if we define any params then we must pass the param when we nav to the route
    - but in query param we don't ve to do that 
    - mostly we use this query params for sorting, filtering and searching
    - <button [queryParam="{pageno:1, orderBy: 'newest'}"]>  
    - and each query params separated by & sign
    - and to capture the query params its sim steps as the router params 
    - ex. this.route.parmMap.subscribe(value =>{console.log(value)})

- "create a separate router module, and register that in our app module" ```ng g module app-routing --module app --flat```


## Routing Programatically cli               
  - now this time while creating the app select yes for the routing, which will automatically generate the routing module 
  - import the router from router and then inject in the constr, -- this.router.navigate(['['/posts', 1, 'postTitle']']) , we can define the router params inside this navigate
  - and to pass the query param -- navigate([['/posts', 1, 'postTitle'], {queryParams: {page:1, orderBy:'newest'}}])

- "Wild Card Routers"
    - for defining the 404 routes.. 
    - we can define the route path by ** , means its a 404 page
    - {path : '**', component: pageNotFoundComp} 
    - when our router path doesn't match with any of the defined path that we mentioned then show the page not found comp
    - Note: always define the wild card route at the end our router paths