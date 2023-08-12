## Svelte

- svelte is a compiler that generates minimalized and highly optimized js code
- svelte works slightly different than the traditional framework but it does the samething, it if often called a framework.
- svelte compiles everything down to pure js.

# why svelte ?

- creates dynaic UIs
- produces higher optimized js (with no overhead)
- no virtual dom
- about 30% faster than other frameworks
- great out of the box, animation, transitions
- great ecosystem (sveltKit, svelte native)
- ease of use(easy to learn and do lot more with less code)

- we can get started with the degit scaffolding tool (which will just clone the svelte js template reop)
- `npx degit sveltejs/template project-name` & to use ts `node scripts/setUpTypeScript.js`
- `npm install` and `npm run dev`
- by default it use the roll up package mgr(module bundler) but we can use vite as well
- and we can set up with other module bundlers like vite etc..
- we can define the reactive vals by $: var = "firstName"
- to define an event listeners we ve to use on:click={}; and inside we can use either inline fn or named fn
- we can use the condn statements like {if} {:else} {/if}
- and we can use each loop {each users as user(user.id)} {/each} // just like the react when mapping over use the id as the key, but here we put the id inside the paranthesis
- our data has to stored in the top level comp(which is app.svelte), if we don't use the sotre

# Feedbacke app

lets build a simple feedback application where users can submit feedback and they ve options to mark their satisfaction (from 1-10) (radio buttons)

- and they can write their feedback and delete their feedback.
-

## SvelteKit complete (joy of code)

- sveltekit is basically a meta framework that builds on top of svelte for building rich webapps.
- svelte is powered by vite and baked with every speeds.
- vite for the ssr.
- we can ve spa, mpa, ssr, static site generation in a single app.
- sveltekit is basically a machine that turns a req into a response.
- it blurs the line b/w frontend and backend.
- its type safety it will generate the type bts for us.

## what are the problems that sveltekit solves (over the svelte)

- file based routing
- SSR
- Data Fetching
- zero config (eslint, prettier, ts, playwright, vitest)
- code splitting (loading data on demand)
- Handling env variables
- configurable rendering (SSR, SSG, CSR), static site generation
- deployment

- it is best in both worlds SSR and client side navigation.
  - means when the first time we loads the app(and the client side router kicks in), its gon SSRender it(it basically gives the data from the server) and after that its basically gon be a SPA. so we will get the awesome user experience.
  - when we nav from the link to link or to the page it won't reload the page, it just smoothly flows
- we can set what rendering mode we want on per page or even for the entire site.

  - just by mentional const prerender = true; const csr = false;

- it uses the webplatform, meaning we don't ve to learn some thing new, we can use the native web standard to do things like fetchApi, formData, req, headers etc.
- and when using the forms we can use the form Data instance and pass in our forms into it, ex- new FormData(form)

### apps are more resilient using progressive enhancement

- we can import {enhance } from the forms - and use it inside the forms. so when the js is available it will enhance it, instead of disable it.
  - so just by using this on kw inside our form use: enhance, now we dont ve to use the onSubmit event handler, svelte will take care.
- it is so great for making our own libs and modules, we can use the svelte-package, and we can use it and publish it in the npm

### Sveltekit App

- basically we just need only 4 packages to install sveltekit
- `npm i -D @sveltejs/kit @sveltejs/adapter-auto svelte vite`
- in the vite config file we can impor the svelte kit and assign the config as the prop of plugins: sveltekit() and export it
- we can also include the svelt.config file so we can get the pre processing, and adapter to o/p our project.
  - same like the vite config file we can impor the adapter and the viteprocess and assign it as the props to the config and export it.
  - the preprocessor transfers our .svelte file b4 passing it to the compiler, the files are ts, tailwind, postcss, scss, sass etc.
  - the adapters are to deploy our site in the target platforms.
- the doc recommended way of installing and using the svelekit is using the svelt Cli
- `nppm create svelte` and `npm i `
- the playwright package that comes with svelte is for the end to end testing
- and vites for the unit testing,

## Sveltekit router

- the svelte kit playground --> sveltekit.new .. which opens the stackblitz playground for our project
- initially the page will be rendered twice, once on the server for the initial req and once on the client bcoz of the hydration(fancy word for the js for interactivity for the page after the server returns the html doc).
- the page itself is a svelte component, so just like the regular comp they just mounted and destroyed on the navigation
  - and when we nav thru the pages it uses the CSR and when we refresh the page, then again it will use the SSR.
  - bcoz svelte knows the data thats required for our routes it will pre renders/loads the css and js in advance. just when we hower over the pages.

## layout

- inside the src/routes/+layouts.svelte --> this is the root layout file
- and to include our other comp use the tag <slot/>
- **_nested Routes_**

  - we can nested as much as layouts we want
  - one of the useful aspect of the nested routes is if we select any one route/ layout and if there is an error on that comp, instead of crashing the whole app, it will only throw the error at the particular component/layout.
  - "expected and unexpected errors" in sveltekit if we encountered an unexpected error which contains the sensitive infos, that we don't wanna display, so the error msg and the stack traces are not exposed to the user.
  - by default its unexpected error its internal error. so if we wanna contain the errorr then we ve to create a +error.svelte in every component and handling the error. so whatever the content we put in the error page it will diplay, when there is an error in the component.
  - the Ecpected error are created using the error helpers, which says the status and renders the error comp, and returns the status and the error message.

  ### Dynamic Routing

  - same as the nextjs we can create the dynamic routing using the [variable name]

### Multiple route parameters

- we can use multiple route parameters as long as they are separated by atleast one character.
- **_optional Parameters_**
  - lets say if we re on a international site the route seets link /en/about/ for the lang selection, but we also wana match it for the default route, which is /about/
  - so in this case the normal routing won't work, to make it an optional param, we can wrab the the dynamic route inside another set of brackets --> [[en]]
    - and now this en route will be the optional, so it works with or w/o it in the route params

### REST params

- the [...file] -> this will catch everything, followed by any route after this.

### matching params

- we can create a matching params by create a src/[params]/file that file that exports the matching fn.
- first import the param matcher from the sveltekit, and use a regex to match.
- then in the params we ve to match /route/[slug=slug]/file

### Fetching Data

- lets see how to create API endpoints and loading data for the pages in sveltekit.
- sveltekit makes the creating the API endpoints easier by creating a /api/post/+server.ts file that exports the correspond http verb.
- that fn takes our request and returns the response.
- when we re fetching the data ex- posts from the db, and the res to get the res body, we can simply cast the json(import that) of our post, ex. const posts = await db.post.findMany() return json(posts);

### showing the data using CSR

- just like other frameworks, as we already have our get api endpoint and now we can create a fetch/get method to fetch the data from the endpt.

### SSR

- lets see how to take the advantage of SSR and fetch the data b4 the page loaded.
- in the route dir we can create the +page.ts file and in this we can declare and export the load fn.
  - import the PageLoad from the types and then make async fn pass the fetch as a parameter-- async({fetch} => {}), so the svelte knows the relative path of the server.
  - and then like usual fetch the data from the api and return the post
  - then inside our +page.svelte file import the pageLoad and then declare our post data the svelte will knows post type
- we can use the reactive declaration and destructure the posts ex- $:({data} = posts) .. so if something changes it will update the data.

**_some times we only want to run the code only in the server _**

- the +page.ts is great for fetching data for the page, it works fine on the browser and the server.
- but it won't work if we need to use some secrets or want to use the file system(.env) or the db.
- so to fix this we ve to make the file as +page.server.ts now it works on db fetching or other things etc.
  - and also the load type is now PageServerLoad instead of PageLoad.
- to fetch the page data we can also use the type, "PageData".

### how the layout page can also load the data

- our layout page can also load the data just like the above, +layout.ts or +layout.server.ts as same as the page (above)
- the advantage of doing this is, so that the data return from the layout load fn is not only available to that layout, but also available to all the child routes.
- now the type is LayoutLoad for the +layout.ts, and LayoutServerLoad for the +layout.server.ts
- now the data is available for the entire route and we can app. and the layout.svelte page ve to declare the type as "LayoutData".

### Making our data available everywhere

- in our layout page we can import the page from the $app/store, and render it $page, so we can use this page store for titles and other things.
- ex -{ $page.data.post?.title }

### using the data from the url

- as we know the stateful url means we can use the query parameter from the url for providing the option like sorting and filtering data.
- in the http req we can pass the call params as (url, params, route)
- and to get the params we can use the url.searchParams.get()

### using Parent Layout

- as we see earlier how we can use the parent layout and page comp to be available for the child.
- what if we need a parent layout inside the child load fn, in that case we can await the data using parent, it only works if we return the data from the layout.
- ex - const load: PageServerLoad = async ({ parent}) => { const parentData = await parent()}

### how load fn works

- some times we need to run the load functions, most of the time svelte will do it bts.
- sveltekit tracks the dependencies of the each load functions to avoid having to do the same work during the navigation.
- the load function that awaits the other load functions will rerun if the parent fn reruns.
- if we need to manually rerun the load fn, sk provides us some useful fns like invalidate to where we can pass the url or we can invalidate all, which is gon to rerun all load fn.
- and we can use this fn in bunch of options like ex - function rerunLoad() { invalidate('posts') or invalidate('api/posts') or invalidate(url => url.href.includes('posts)) or invalidateAll() }
- if any ref to the prop of params or url changes, if the load fn calls the await parent and the parent rerun if we declare the dependency with fetch or depends and mark the url with invalidate() to force every load fn to rerun.
- this doesn't cause the comp to be recreated, it just update the data..
- if we want to reset the comp, we can use After Navigate or wrap our component in a keyblock

## Work with Forms

- the form get method req resource from the server and appends the form data at the end of the url.
- in the post method it can return the resource depending on the data sent in the body. and no data is appended to the url instead the data will include in the body.

## Sveltekit Form action

- we can define a method that map to an action, inside the +page.ts or +page.server.ts file
- import Actions from @sveltejs/kit
- and to use this inside our form <form method ="POST" action = "?/reomveTodo" > this is how we define our action method inside the form .. however it seems like query param but followed by / so it knows to invoke the fn.
- even in the button we can use/invoke the fn, <Button formaction = "?/clerToDo" > this will override the action attribute of the form

### Progressive Form Enhancement

- to preserve the awesome SPA experience, and this what progressive form enhancement is for.
- lets pretend if the user doesn't ve js(it fails for whatever reason), our form is still gon work.
- but if the js is available on the page then we can enhance the user experience, so we can do client side validation or whatever else we want.
- The sveltekit has "Enhanced svelte Action" ie unrelated to the form action, this is svelte action, which lets us to hook the life cycle method of elements.
- import {enhance} form $app/forms.
- then use it inside the form --> <form use:enhance >
- so when we submit the form, it gon use the enhance action to update the form, which is gon to update the $app/form store and the form status property
- its also gon to reset the form and rerun the load fn for the page, so we don't ve to invalidate() by ourself.

### Customize the Enhance Action and show the load ui

- we can do it by providing a submit fn that runs b4 the form is submitted, and then we can also return a cb fn that has the access to the result.
- first import the type SubmitFunction from the app/form store and assign it to the addToDo fn
- we can't always rely on the fast response from the server in which case the user might think that our site is broken.
- so we can use the setInterval or await sleep() to assign some time to sleep or slowdown the app.

### Form Validation

- we can do the manual validation or the zod validation.
- as we know the zod is gon to validate the schema that we set
- `npm i zod-form-data` we can just parse the form data or url search params, which we can parse and it can validate that.
- this is made for remix.. the remix and svelte kit uses the web platform so it can work flawlessly.
- ex - const loginSchema = zfd.formData({password: zfd.text()}) and then we can parse it const result = loginSchema.safeParse(formData)

### Advanced Enhanced Action customization

- applyAction - from $app/form.. this action updates the form properties of the current page with the given data and updates the $page status.
- this apply action really depends on the (cb which is result) result.type

### Usiing advanced layout in sveltekit

- Group Layouts -- as we know the group layout allow us to group the related routes inside the directory that wrapped with the parenthesis ex- (Dashboard)

### Breaking out of layouts

- we can make +page@(app).svelte -- if we leave with thee @ then its the root layout, since we specify the (app now app will be the parent
- if we want to reset the layout then we ve to specify with the blank layout ex - +layout.svelte -- and its gon be the root layout

## SvelteKit Hooks

- the hooks are middleware, they are special because the hooks can attach themselves to the other event and trigger behavior based on that.

  - we can hook into the request or we can change the request which will change the response.
  - we can do lota things we can use for authentication, modify the response, can use it for error and performance logging and even creating automatic routes.
  - **_Hooks to Transfer Html_**
    - why would we wanna transfer Html - one best ex is we can think of like internationalization
    - in the html lang tag instead of leaving it in "en" we can target like <htm lang="%lang" /> and
    - in our hook (we ve to import the Handle from the SK)
    - for ex if we wana set our lan to german du. then-- > const handle: Handle =({event,resolve}) => {const locale = 'du'; event.locals.locale = locale; return resolve(event, { transform : ({html}) => html.replace(%lang, locale)}
  - **_measure the page loading time_**
  - **_Hooks for error handling_** Handle error hook..it runs if the unexpected error occurs during the loading or rendering.
  - **_Hooks to modify the API Fetch response _** -- we can use the handleFetch hook, to modify fetch req inside the load or the action fn runs on the svr.
  - **_ Hooks to Parse data from the forms_** -- parseFormData from parseNestedFormData, then we can write the logic like listen to the event, if the method is post (from our form), then we know its a form req and we can parse the form data, and then pass it to the action fn.

- if we wanna use multiple hooks together, then we can use the sequence helper fn from sk, just import from sk, and pass all the hooks inside this fn.

### SK app Deployment

- the sk has the "ADAPTERS"

  - if don't specify any adapters(in config file), by default it will take the adapter auto, means if we deploy the app in platform like versel, netlify, SK will know what the platform is , and will use the right adapter according to those platforms.
  - we can set a cache control headers for certain iterations, which means if 1000 people visits our page, we re not gon make 1000 req, doing the same work on the svr, but the cdn is gon be like oh i ve the same req cached already and just serve the resources.
  - and the cache will be there in the cdn for like hour and then it flushes out, it is simple but very powerful.
  - we can see more about cache control headers in the mdn doc.
    - to set that cache headers in our page.svr.ts file set the async fn and destructure the setHeader, and use the setHeader to assign our cache-control

  # Svelte FCC (li hau)

  - in the compile time if any of the styles or html is unused in the comp, and it won't be added in the output file.
  - if we wana use our styles global then we can use the pseudo class :global and wrap our selector with it ex; :global(h1) {}
  - for animations it will be like @keyframes -global-zoom{}

- if the var name is same as the attribute name then we can write it once <img src = {src}> instead of <img src > is the same.
- there are few things svelte will look and knows that it is updating the state 1. assignment (assgn the var val to another), 2. update statements(operator) 3. updating or assigning the property(of var).. in the output js file can see $$ invalidate(all fn) in our var, means this will kick start the update cycle
- make an event listener (for the event, and the handler fn) and assign it to elem - h1.addEventListener(event, onClick) then <h1 on:Click = {onClick} > once we clicke it will automatically remove the event listener from the dom
- can also add the event modifiers for the event inside the action fn <h1 on:Click | preventDefault|stopPropagation = {()=>{}}>
- just like that we can also pass the options/even modifier to the event but can't add it in the event listener instead <h1 on:Click | capture | nonpassive = () > just like that there are seven Event modifiers we can pass.
- **_REactive Declarations _**

  - $: double --> if we write the vars in dollar and label followed by the var, so everytime the val of var changes it will replicate.
  - also the statements is same -- reactive statements.
  - every time the dependency of the var changes it changes
  - when the changes happens the svelte updates right before it updates the dom (and the dependency var order matters)

- **_tick()_** import from svelte

  - returns a promise and this promise will be resolved when the update is finished.
  - if we really want the latest val of the reactive declaration,
  - so it waits for the dom update to complete

- comp and props-- if we wana pass the prop from the comp(child) we can declare the props like export let double = 2; now we can use this prop inside the parent comp,
- in the same way we can assign a fn to the var and pass it to the props in the parent comp
- to create a event listener in our child comp we can import event dispatcher("createEventDispatcher") and create the event and dispatch the event to the parent.
- forwading component events (means forwarding the events from child to parent to another parent .. all the way down to the root comp)- unlike events the component events doesn't bubble

- "Class Directive" - $: className = any condinal rendering logic, and then we can ues this class in our elems
- or we can also use like <div class:negative={profit < 0} class:positive={profit > 0} > or assign it to the reactive dec - $: negative = profit < 0; so now we can write it like <div class:negative > which is equivalent to <div class:negative={negative} >

- "Bindings " - like we can bind the val(prop) of the i/p <input bind:value={var} > , its a 2 way data binding, changing the prop will update in the var and changing the var will update the property.

  - if the var name is same as the property name then we can use our classic shorthand <input bind:value>
  - and similarly we can bind the prop of the comp as well
  - refer the svele doc there are bunch of things we can bind.

- "Bind:group" - ex group i/p - we can bind the group of i/ps. we can bind only 2 kind of i/p either radio button or checkbox button
  - ex if we ve a group of options like check box then we can use the group prop and assign it to the var <input bind:group>={options} >
- "Bind:this" - is one way binding, we can bind the ref of the var to this elem ex - <input bind:this={var} > (so anytime we want ref to the val of the elem use bind this)
  - and we can also do this for the comp as well
  - when we get the reference ? as we get it once the elem or the comp is mounted.

### components life cycle

- there are 4 components life cycle events 1. onMount 2.onDestroy 3. BeforeUpdate(fires b4 the dom updates) 4. AfterUpdate
- and we can use em only during initialization of the comp
- we can use the life cycle logic in multiple components,
- the onMount wont be fired during the ssr.
- onMount we can perfomrm some task lets say setInterval and when return the fn we can clear the interval this way we don't ve to use onDestroy to clear the settimer.
- lly we can fetch data onMount and return with .abort() so it will destroy.

# Conditional Blocks

- for each block we can use the index ex - {# each colors as color, index}.. this is something we can't do it in the for of loop, but we can do it in the for each loop.

  - we can also use the {:else} block inside the each loop.

- keyed Each block - in the array list, on the mount svelte will go through the list and update every element in order, anything extra(not in use ) will be removed, svelte will ve no idea whether we inserted something or rearranged the order.
- coz, if its str then it can compare, but if its obj it has nothing to compare with.
- so we can use the key, and tell like this is the key of this property, and use this to compare ex- {#each colors as color (color)} .. now the color is the key. or can also use id prop as the key, just like react.

  - this keyed is useful in inserting the elements in the array, so still the elems will ve ref to the idex,
  - and we can use this keyed for the animations, trasistion when we reshuffle the items
  - if we do this inserting ops with the each block we will end up with 2 ops , -- the item will be inserted and the old item will be gon and we ve to add that again in the list (maybe at the end)
  - and similar things will aplicable for removing the item, we remove on and the following item will get displaced and rearranging them, so its always extra ops.

- this is what the keyed each block solve it will keep the ref of the item as the key (can be anything),

  - so if we wanna remove the item, then operational wise its just only one operation.

- " the await block" - can takes the promise and then resolve the result.
  - {#await anyPromise()} {:then result} and if there is any error {:catch error}

### Key block {#key}

- whenever the val of the expression changes the content inside the key block will be recreated.
- this can be useful in the transition, ex when the content of the div changes (inside the key block) the new div will be recreated (with the transition applies everytime the content changes).
- another use case we can use this for the components, so whenevr some props/content in the comp changes it will recreate new one(comp).

## context

- way of passing data for the parent to its children. (unlike props where we need to pass the props to all the child, all the child will recieve the data)
- the way to do is import setContext(takes k&v) in the parent and all the child will receive this by using the getContext(takes key) method
- and these get and set context() has to be called during the comp initialization.
- "communicating thru context" - if we want to reactively update the context val we can use the pub sub mechanism, subscribe for the event and listen for that event.
- if we wana pass the data from the child to the parent, we can use the callback function or event listener, ie how we pass the data all the way up

## svelte store

- if we ve some data or val of the variables that are not part of the app comp hierarchy,
- import {readable , writable } from svelte/store
- writealbe store - for read and write , the readable store for read only
- both of these has methods called subscribe
- in writable store we can use the set() to update the val. and to listen to this we can use the subscribe() method
- "REadable" - takes 2 args ex let store = readable('', (set)=> {set("hi") return ()=>{}}), in the return fn is like clear out and this will only trigger out when there is no more subscribers.

### store contract

- both of those above sotres follow store contract, and there is a special synatax to use the store contract $store = e.target.value.. dollar prefix store val
- this syntax does lot of things, ex it subscribe to the store(during mounting), then updates the val of the store, and also unsubscribe to the store when we unmounted.

## REdux store as svelte store

- there 3 api from the redux store, 1. getState() and 2. subscribe () and dispatch() the actions
- also here we can create a store and use the svelte store contract ($store ) to access the store val.

## VAltio state as svelte store

- the valtio is the proxy based state library
- which wraps the state object and turns the object into a self aware proxy.
- and we can subscribe to the any change that occurs to that obj.
- and when it changes we will get a snapshot of the state object at the point of change.
- similar to the redux store we can also use the svelte store contract syntax to access the state object

## Xstate as svelte store

- svelte stores allow us to bind a state that doesn't belong to the comp hierarchy of our app.
- Xstate is the state machine and state charts for the modern web.

### Dom events as svelte store

- how to convert the dom events into custom svelte store.
- same as the above libraries we can listen to any dom events (like mouse position or scroll position ), any snapshot of the dom events can be used as the store val and we can subscribe to it and listen to them.
- can use the svelte store contract syntax.

### immer for immutable as svelte store

- as we know immer allow us to work with the immutable state in a very convenient way.
- derived() from svelte store can allow us to create a new store which is derived from the another store.
- by default svelte allow us to mutate the objects directly.
- and we can dervie from one store or multiple stores.. and get the derived vals synchronously or asynchronously.
- ex -multiple store.. const newVal = derived([a,b], ([a,b]) => {return a\*b} ). the first arg is the [] and the second cb which takes any val as ref to the array. this will create a new store val.

### tweened() svelte store

- when we are changing the store val, instead of changing the val from 0 to 1, wouldn't be nice to change slowly like 01. 0.2 ...0.9 to 1.
- we gon create a new cust store that will take the val of the state and move slowly to the target state.
- just like the usual store fns create a subscribe fn and set() the initial val and set the timeInterval to update the val (using the set() )
- and we can use the timers logic to end the time. like if current time gt end time, clears out the interval time and current time - starttime / duratation

- the tweened() with this complex logic is implemented in the svelte we can import from the svelte/motion
- this fn takes in initial state and the duration obj as the args
- it also takes delay, easing and interpolation
- refer the doc, we can tweened with colors, date, time and lota things
- "spring()" - also for the same kind of effect

- so tweened() and spring() returns an obj that follows store contract, and when we update the val to the store it doesn't update the val immediately rather it will slowly update to the next state
- when we use spring() then our val will be change slowly from the initial to the target state like a spring.
- and we can define props like stiffness and damping

### Higher order stores

- fn that takes in another fn and return a new fn(with enhanced ability), its like react's HOC

### "RxJs as svelte store

- as we know rxjs we can observable for the event and the observers subscribe to the observable. and we can listen to the event stream.

# REactive class prop using store

- we can make the class itself a store nd subscribe to the class so whenever the prop changes

## undo Redo with store

- like in our store if we make any changes, and presumbly we ve the history of the changes that we re making, and we want the ability to undo or redo the changes that we make.
- the history is the [] of records that every actions we re making, and undo - reverting the actions
- the second approach will be the history [] will be holding the snapshots of the val that we changees
- the 1st approach for the undoredoStore will be undoredoStore.doAction({apply(\_val){return val + val}, reverse(\_val){return val - val}})

- for the undo() we can pop the action from the history[], and for the redo() we can
- or we can use the index for the history[], for undo store the val and reverse the val of the history[] and decrement the idx.
- for the redo() increment the idx and store the val and apply the val to the store.
- in summary the 1st approach is store the history of actions(which is an obj) has apply() and reverse().

- 2nd approach -> store the history of store val snapshots
  - in which we can use the immer js(produce() from immer), so we can make the changes and keep the ref of the objs that we change,
  - so when our store val is an obj, then either we ve to clone it, clone the snapshot to store the history
  - or make changes immutably, don't mutate the obj itself.

### REactive context

- lets see how to make reactive context using store.
- when the val of the parent (set()) changes then all the childs(getContext()) will be get updated.
- in summary the context is being set up during initialization(onMount) therefore the updated val will not be reflected to the children.
- so in order to make it reactive we ve to use the store.

### 3 tips to manage complex stores in svelte

- if we ve a complex state avoid manipulating using the writable store directly, istead

  1. use state management libraries such as state machine, reducer redux
  2. Have a small self contained store.
  3. if we ve a large store, create a derived store out of it, so our comp doesn't ve to subscribe to all the changes made to the store.
     - making the immutable (immer) or proxy based store can allow us to subscribe the part of the store too

## get() store val

- how to get the store val outside of the comp ?
- get() from svelte/store - it will subscribe to the store and get the val of the store and unsubscribe () it immediately.

## Store vs context

- as we know already the diff b/w em, we can make a use em together, we can ve store inside the context to create a reactive store.
- when to use store and when to use context ?
  - lets ve dynamic, local, static, global vals - static means we don't ve to use store, and global means we don't ve to use the context. locals means we gon use context, and for dynamic we can use the store
  - so if we ve local and then dynamic vals then we can use both store and context

## Svelte Actions

- are essentially element level lifecycle fns. they are useful for

  - interfacing with 3rd party libs
  - lazy loaded imgs
  - tooltips
  - adding cust event handlers

- we can define a fn and inside the elems we call the actioon <div use:actionFn >
- since its a lifecycle fn it will be called when the dom is mounted and destroyed,
- or we can optionally return the destroy function inside the actioon function. besides the destroy function we can also call/return update(), this function will takes newParams and this will updates whenever the params changes
- action takes 2 params (elems, params)..elems the elem that calls the action, "params" - this way we can update the params.
- we can apply this action on multiple times on multiple elems.

- "Integrating svelte action with the ui lib"
- "Reusing event listeners with svelte actions"
- "creating events with actions" - we can use actions to create cust events , which will be triggered by the cust triggers
- order of svelte Actions

  - which one to use first on the elem whether the bind prop or the use aciton ?
  - if we re listening the same event listener or changing the elems at the same time, probably the one that register earlier will ve the effect earlier that the other.

- "use:click Outside" - when we click outside of the modal it will close, we can do that with the actions
- "useToolTip" - actions - when we hover over(event) the particular element in the document it will show the tip.

  - we can use our cust tooltip action or the cust comp or combine both and use em
  - use:viewPort Action - will allow us to know whether the element has entered the viewport or exited the view port.

- "use Popper" (popper js lib) this will allow us to create the dynamic tooltip
- "use Lazy IMage" allows us to loads the img lazily only when we re with in the vp, same like the popper we can use couple of cust events like entervpEvent and exitVpEvent, so when we enter the vp we can load the img.
- "Ensemble Actions" - we can ve some actions/single action and apply em to the group of elems

### <slots/>

- is a way for us to compose components, just like the html elems where we can ve elems within other elems ex - p tag within div or anchor
- just like that we can ve comps within the comp.
- the slot is like the place where we can ve our contents into, we can ve multiple of them.
- when we ve a multiple comp and we wana pass them accordingly, then we can use the name slots ex - <slots name="header" >
- and we can pass the slot prop to elems from the parents ex - <div slot="header" >
- by default the slots name will be "default" and its reserved we can't use it.
- <slot:template > - will allows us to wrap over multiple html elems

### passing data accross slots

- we can also assign prop to the slot ex - <slot {prop}> and to use this in the child or sibbling <svelte:fragment let:count > and inside this elem we can use that prop {count}, and in this child we can also bind the prop to new name/val - let:count = {newName}, and use this inside the fragment
- the svelte fragment is the way for us to use the group elems or 2 or more sibbling elems to be passed into the same slot then we ve to use fragments
- "Slot Forwarding" - ex if we ve like 3 slots (naming a, b, c) we can pass the a to b and then b to c now c has both a nd b

- "$$ slots " is a magic variable or obj and this obj contains the key for each of the slots attr pass into the comp.
  - ex {#if $$slots.b}
        - <slot name="b" >  so if don't pass anything into b then we wont see the slot
        - and for the default slots (one w/o name) {#if $$slots.default} - and this statement/condn will only be false if its slot content is empty

### infinite List

- something like in the social medias - the infinite scrolling
- we can create like a comp and then we can pass in like diff data and then we can see how to render and how the each item with in this infinite list looks like.
- the logic would be like <List noMoreData={false} noLoadMore={data.fetchMore}
- and we can write like if there is no more data on the vp then call the fetch fn
  - {#if !noMoredata} <li use:viewPort on:enterViewport={()=>{dispatch('loadMore')}}

### Tabs

- are like the normal tabs, ex tabs in the nav field.

## $$props and $$restProps

- lets see these 2 more magic variables.
- ex - when we ve a comp like button where we define all the cust prop for this button comp and then pass the props, and in feature if somebody wants to add more props to the comp then again we ve add those props as well.
- is there a way we can do it once and pass for all.
- $$ props - all the props that we passed in the comps will be available in this($$ props) as obj, and its reactive when the val of props changes it changes. now we can just spread the props (object)
- <button {...$$props} >
- this'd be fine, but incase if we ve a class that we wana use that for the button and also we define the style props so it will clash we can switch the order of the class and the $$ props but still it won't work.
- that's where we can use the $$ restProps - rest of the propertie that are not defined as props(those may be declared on the parent/sibbling) when using the button comp.

### Lazy component

- just like the lazy img, we will create the comp only when the user sees it (in the later time)
- its useful only when the user wants to see them like load more then only the comp will be visible to him.
- if we wana rename a comp ex- default comp is the elem level comp we can define in the await block and make it like {:then {default: Component}} <Component /> so this is now we rename the comp (from the elem).
- we can make like <div use:viewPort on:enterViewport= {()=>{}} and inside this cb we can make a check if its not loaded then import the comp, and render it. this is the one approach
- and the other way of doing this is <Lazy this={()=>{}} > inside the cb import the comp and if we wana show any loading state fallback fn we can specify inside the <slot>

## svelte:component

- rendering the component conditionally.
- svelte:component is a special element, usually the special elems are starts with "svelte"
- instead of conditionally rendering the comps we can use this elem to do the same
- <svelte:component this={value > 50 ? Foo : Bar } bind:value on:click {...props} />
- so inside the elem the cond to render b/w the 2 comps and then the bindings, directives and the props for those comps

## svelte:self

- when we ve an nested array, and if we wana render the array list over and over again recursively, then we can use the svelte:self - which recursively calls itself over and over again
- and this comp can be only exists in if or each blocks otherwise we can't use this comp.
- coz we know for the recursion we need some base condn to stop the loop (or it may cause the callstack error)

### svelte:window

- is a way to add the event listener and read out some window prop such as window.innerWidth etc, we don't ve to manually set a event listener to listen and update it .
- exx- <svelte:window on:keydown={onkeydown} bind:innerWidth />
- we can bind the props like inner width, inner height, outer width, outer height, scrollX, scrollY, online - an alias for window.navigator.online

### svelte:body

- its similar to the svelt:window, it will allow us to add event listener to the body (instead of the windwo)
- ex- <svelte:body on:mousemove={mouseMove} />

### svelte:head

- it will allow us to add things to the head elems (since head doesn't ve the event listener)
- ex - <svelte:head /> anything that we added inside/under this elem will be inserted to the head such as link, meta, title, style

### svelte:options

- allows us to customize the compile options of our svelte comps.
- the svelt.compile() takes our comp's source code and turns into a js module that exports a class.
- in the sveltte:options the options we can set are immutable, accessors, namespace and tags.
- <svelt:options namespace="svg" > we can use this namespace option to create a cust namespace for the elem.
- lly the accessors are for adding setters and getters. for our props. the default val is false and to use this accessor we ve to set it to false
  - <svg:options accessors={true} /> this can be useful in the tests.
-
- the component can have only one svelte options elem and the options we can group them inside the elem.

### passing css cust properties to the component

- for the elems except the default properties we can pass our cust props
- ex- for the button besides its default props like the bg-color, color, we can pass our cust prop and it has to start with -- followed byt the prop name- <button style="--button-color:red"; background: var(--button-color) /> or we can write the syntatic sugar <button --button-color:"red" --text-color:"white" >
  - and this cust prop is not only bound to this particular elem we can use this in other elems inside our comp as well.
  - and the prop rule of the child element will follow the nearest parent not the root parent (order matters).
  - the caveat is that we can't spread this cust props

### html {@html}

- inside we cab pass anything (of string), event if its like the dynamic string we can still manipulate that, like we can add the specila attr to the button ex - {@html html.replaceAll('<button>', '<button data-special = "naresh >')}

### {@debug}

- its very useful, it will allow us to debug some data of our component.
- for ex we can use this to debug an array -- {#each array as item} {@debug item}

### <script contenxt="module" />

- what are the module context ?
- note: A component can have only one instance level <script/> element. the normal script tags are the instance level elements
- and the <script contenxt="module" /> are the module level script tags
- this will create a module inside our component and anyone can import this
- now this component itself is export default, so we can't ve any var inside that says export default.

### svelte Transition

- the svelte transition lib provides 5 built in transition fns - fade, blur, fly, slide, scale

- "coordinating the transitions" -
- "transition Events" -
- "easing" - adds lot of flavours to the transition, easing fns specify the rate of transition over the time .. easein, easeout, easeInOut
- there are bunch of easing fns there, we can visualize them in the docs using easing visualizer

### Accessible transition

- some people may ve dizziness or motion sickness, when face with the parallex scrolling, zooming effect
- even many os ve the accessibility settings for reduced motions.
- as the svelte transitions depends on the css animations, we can use the @media query to disable or set the duration of the animation to be 0.
- ex - @media (prefers-reduced-animations: reduce) { \* {animation-duration: 0s !important}}
- with only the css we can still experience some delay, so we can use the js soln so we can pass different delay to the transition.
- ex- const mediaQuery = window.matchMedia('(prefers-reduced-motions: reduced)'); let matches = mediaQuery.matches;
- or we can use a cust fn (noTransition) that returns the duration and delay of 0s,

### client-side component API

- anchor prop - means inserting/ appending the comp to the end of the target, its like dom's insertBefore api
- and the other props are target (doc.body), props, intro, context (has new Map()), hydrate
- component.reset(), component.$destroy()

### SSR Component API

## Svelte compiler API

- as we know the svelte compiler compiles the svelte code into js code that the broser can execs.

## svelte preprocessor api

- some times we write like ts or css code and then svelte will preprocess to the svelte (format) as it expects then finally compiles that into js.
- mostly we don't use em we will let the bundlers and the compiler do it for us

### hydrating svelte components

- hydration is process when we ve content in html coming from the server, be it rendered dynamically whenever the new req or the html is generated ahead of the via prerendering.
- and we get this html downloaded to the browser and it creates all the dom elems
- we might ve existing dom elems on the client side by our comp, so instead of recreaating the elems, we look at the existing dom elems we will say like they re the same elems, we can use the existing dom elems instead.
- then we would do the process called hydration, we kind of go thru the elems in our templates that are supposed to create and instead of creating em we will refering or pointing a reference to the elems that's existed on the dom.
- later on when there is a interactions, since we already ve the reference to the dom, we can immediately make changes to it and react to the any user interactions.
-
- lets see how to do this hydration process.
  1. we need to render our comp in the svr side, we wana gen the html and then serve it to the user.
  2. in the index.js file - > SvelteCompiler.compile(svelteCode,{generate: 'ssr', hydratable: true })
  3. we can destructure js from that fn.
  4. then for the client side the same fn but generate prop is dom (instead of ssr)

### svelte register

- to use svelte comps in Node.js w/o bundling, we can use require('svelte/register') after we can use require to include any .svelte file.
- if our routes (we stacked in the mix of static, dynamic, spread, and one with the comp endpt), if the one with the endpt/api method is not defined then the priority will slips and the route will fallthrough(means not used to render the path)
- and then the other comp with the content module in it and inside that load() method we didn't define anythig/nothig, then this comp will fallthrough as well
- this is where the order is important, coz with this order if we decide the comp or the endpt not to handle the route, so with the order we can always fallthru the next one/
- if all of them ve no more handle, then our route will be fallthru to 404 page, coz none of the comp wants to return or handle the res
- the fallthru route is useful when we ve the same matching route, and we can decide at any point of time in case any of our comp don't wanna handle the route anymore.
- in that case we just define return nothing or undefined.

### Sveltekit Layout

- when we nav thru the comps the comps layout will destroy as we nav thru the new one, (and the new one will create)
- so if we want the comps to be persists and stay for the all the pages, thats where the layouts comes in.
- and then we can specify all pages within the routes can use that layout comp as the parent.
- the layout is the special comp which starts with \_\_layout.svelte double underscore. and anything we wrap with this <Layout> tag will be the child, or we can use the <scripts> tags and mention the slots with in the slot elem
- in svelte kit we can nest layouts, and if we wana discard all the layout comp that we ve for our parent, the we can use the "\_\_layout.reset.svelte".(this will reset all the parents layout )
- finally the laoyout are just like the route comp, means that we can ve the scrpt tag and we can make it as the content module and insert the load() fn inside..

### Loading data using page url and params in sk

- lets see how to fetch the data based on the currnet url, and dynamically fetch the data.
- as we know whenever the data changes the props changes, so which means the load() will be called again, to update the props for us, so how does the load() knows that whenever we navigate thru the comps it has to update the props
- it depends on the params (i/p that we passed inside the load() function) and whether it used the params its going to change ex- if we ve dyn route [itemname].svelte (which has the name and the price props) and in the load({fetch, params}); and then inside the script whatever params we call and the load() function changes ex if we ve fn f(param) {console.log(params.price)}, now even if we use this fn inside the scope of load() function (lets say console).
- so now whenver the price props changes the load() function will be called. so it really depends on whatever params we read from the path, and as soon as the val changes the load() function will be called. so our comp is not unmounted rather it updated with the new set of props.

- so besides the params, we can also depend on the **query**, and to use this load({url}; console.log(query)) {const query = url.searhcParams.get('a')} -- so now whenver the search query (a) changes the load() changes.

### Loading Data in layout and Stuff

- we ve seen how we can load data on the page param and the url, lets see how we can do that for the layout as well. and we can see how we can pass the data that loaded from the layout and how we can pass it to the page that uses the layout thru **Stuff**

- as we know whatever we export from the content module it will be like the module level export as opposed to the normal comp export which will be treated as the props.
- but if we ve this in the route comp then the load() exports from this module will be different, means it will ve a different behaviour, (load() will be called to match the route to the path)
- inside the stuff we can pass the props as an kvp obj, ex retun {stuff: data.rating}
- and to read this stuff(from our page comp) pass the stuff as a param inside the load() and then return {ratings:stuff.rating}, so this is how we pass the stuff from the parent.
- in summary the stuff is way to pass the data from the layout parent to the child

### Debugging Sk endpoint and the load fn (using debugger)

- as we know the sk will runs on both side, so as soon as the req comes in (in the server side ) will run the load fn and render the content and returns the res to the client
- and in the client side, it loads all the js and it starts up the sk runtime, and then it will also calls the load() again to set things up and to pass the right comp, (to get the same props on the server side)
- so when we wanna debug the load(), we don't know where it happens either the csr or the ssr.
- we can use the debugger stmt in the client side instead of the console.log.
- if there is any prob in the client side we can use the chrome inspector to debug, but if there is something in the server side . as the node runtime runs our js code.
- so we need to figure out how do we use the chrome inspector to inspect the node runtime. (even if we ve the debugger stmt in the server code, the code will execute w/o run into the debugger), so how do we trigger the debugger ?
- we can actually pass the runtime options to the node command. ex - node --inspect a.js
- and to debug the server in the chrome browser we go to the `chrome/inspect` and in there we can find the remote target, and there (in the remote target ) we can find the (our file a.js and the) option to inspect .. and this will starts the debugger (devtool).
- and the another option is `node --inspect --inspect-brk a.js` -- it will bring our code immediately on the 1st line, it will wait for us to attch the debugger and then continue to step the code. (if we run the code it will wait for us to break the 1st line)
- it is very useful if we ve a code that runs very fast and we actually want to pause and wait for us to start up the inspector.
- note - inside the node_modules -> bin we can find the all the commands that we can use (this is common for all the frarmeworks, but i didn't know that so far)
- in our case we can run `node --inspect node_modules/.bin/svelte-kit dev` - now we will ve our debugger listening and we can see the remote targe (that we can inspect )

### Invalidating the data

- this is an amazing feature that sk provides.

-as we know we can fetch the data using the .fetch() with in any comp, but another way of fetching the data is to invaliding the data with in the load()

- means if the load() function once invalidate the data, the load() function will notice that one of the data is no longer valid. so the load() function will call again to refetch the data.
- to use the invalidate **import invalidate** from the $app/navigation; is a special module. and the wau it works is that we invalidate the fetch fn, we invalidate the endpoint. ex - invalidate('api/promotion'); it will return a promise the promise will done when the whole invalidation is done.
- so it set this (endpoint) data to invalid, and once it invalidate all the load fn that fetches endpoints ve to rerun it, (even if we ve any cache enabled in any of the endpoints the cache will be invalidated as well)

- why do we invalidate rather than manually fetching the promotion from the api, first if we ve many props and our vals got changed then when we re fetching we ve to update everything individually ex - fetch(`api/${params.itemname}`) and similarly we ve to update the endpoint for each props/ params.
- but instead if we use the invalidate on our endpoint, it will update all the props and updates the latest val. so this can invalidate all the endpoint (not just the params).
- in summary if we invalidate it then the whole dependency chain of loading and the stuff, (and props) will be updated.

### Sessions Part -1

- the period of time the client interacting with the server is called session it can be long or short. ex - some websites uses 30 duration, and during the session if we re using the same browser and go to the website then the site can save our session. (and all our interactions with the site is like one whole session).
- so when we use the sk we also wana use the sessions.
- whenever the user make a req, or tryna visit in a page we ve something to retrive, so we drop a tracker and those tracker are in the form of **1.cookies**. every time the same user visits our site he will carry the cookies in the req header
- **2. Local Storage**, the user states in the local storage so later on we can retrieve it from the loacal storage and stuff it to anywhere may be a req body, req header

- the key idea is to store some thing in the user's browser, so later on when he makes the req he will pass that along with the req, so the svr can retrive.
- what kind of info we drop, it could be like the **token JWT**, or **SessionID**

- To retrieve the cookie of the user - (since its a part of the headers), we can console log ex - fn cook({headers}) {console.log(headers.cookie)} and we can see the cookie (str) and to get the val we can parse it (or we can make it as an obj (cast))
- so we can get all the info (extract) user info in the endpoit.. but how do we do it in the page. in the dyn routing's load() even if we pass the header as the arg still we don't get it, bcoz this load() will be called in the svr side as well as the client side.
- so in the client side how do we get the headers (since we haven't even send the req). but instead we can use the "session" as the arg inside the load(). the session is an object.

- to create a session or to create a session for the pages in the hooks file, and we can use this hook to create a get session fn and use it to get the session. now we can pass the header obj as the arg to this fn and from the header we can get the session (parse it) and from there we can get the user - headers.sessoin.user.
- but since the session itself is an object, it consist of all the session information, but we don't want to get them. so make sure we don't wana get the sensitive info from the session.

- still we can't expect to call the getsession() in the client side,
- when we do logout, (or clear the current session), in that case we want our app to forget who we re. wo we want to update our session. how do we do that ?

### session part 2.

- lets see how we can use the **session store** and how we can update our session. (switching diff sessions and letting the svr konw this is no longer active, update the current session)

- as we see the last ex we create a called hooks which can be run only in the svr side.
- if we wanna change/switch the session we wanna update the cookie as well, (and to write to the cookie document.cookie = "user=aaa"; $session = {user:"aaa" };) but in a real life we don't do like this.
- we will make a req to get the info about the user and then we update it.
- when we update the session, we ve to make sure that we update in the place where we sotre the info where we extract the session from. so later when the server makes a req the svr knows that, the current user is also updated.
- in the real life scenario, we actually don't use this approach to store the user information, istead we use the library, third party lib to store the session information

### SvelteKit $App/stores

- the special module provided by SvelteKit, refer the doc - in the module section we can find the list of special modules provided by the Sveltekit.
- the stores are contextual, they are added to the context of our root comp. means the session and the page are unique to each req on the server rather than shared b/w multiple requests handled by the same svr simultaneously. which is what makes it safe to include user specific data in the session.
- earlier we ve seen how with the load() we can access to the page that gives us the current page params, url and query parameters as well and to get these infos in our app we can pass em as the props. or we can do in the other way.
- in the store module we ve page comp we can import `import {page} from '$app/stores'` so now this page contains the url and the params we can extract - const {url, params} = $page;
- so here the page itself is a store and we can subscribe to the page to get the changes that happens. and if we wana fully make use of it, then instead of assigning it to the var we can make that declarative as well ex - $ ({url, params} = $page);
- and we can't use this inside the context modules, most likely we will be using this store(page ) in the script tags.
- and we can use this in the endpoints but it doesn't make any sense to use it there, since when we re serving an endpt, we re not rendering any page.
- and there is one more fn in the store module, getStores() but we ve to call it in the comp initialize, and subscribe to it.

### SK page endpoints

- is a new feature from sk, which may saves us a lot of time typing out lota boilerplates
- as we know how to load() data into the sk, in that load fn we can pass the fetch and fetch the endpoints as (res). and then returns the props as the data. and then use em in our scripts
- so if we ve endpoints for multiple pages,
- if we have the endpoint name same as the comp itself, then we can call the endpoint, instead of getting the page itself. (ex - apple.svelte, apple.ts) in the ts we will ve our endpoints.
- when we re fetching (inside the load()) we can do like const res = await fetch(`api/iphone`, { headers: {'Accept': 'application/json'}});... note - this only works on the client side.

- so when we define the endpoints, there is 2 way of access it from the public point of view, 1. api/apple/\_\_data.json(in this we don't need to pass the header) and the other way is to pass the headers and accept the application json (as response), since its a json res sk know that we re refering to the page endpoint, rather than the page route itself.
- the take away here is if we wana page endpt we ve to name our endpoint as same as the page itself.
  - so if we wana define a endpoint for our page only, then we don't ve to repeat ourselves again to define the load(), that whole cycle was simplified by using the **page Endpoints**

### Named Layout

- to reset the layout, we need to ve a file called "\_\_layout.reset.svelte" , when we ve this file then it will be the root layout of our page, means that it will ignore all the parent's layout comp.
- but if we ve named layout then we can choose which layout we wanna use (for some of the routes we wana use this parent and for others we wanaa use that parent), so we can ve the list of diff layouts in the root layout, and each page choose diff layouts accordingly, (which is more flexible than resetting the layouts)
- ex - we can create a named layout like --> \_\_layout-a.svelte (now this layout name is a) and to use this (to point our page/comp to use this layout), apple@a.svelte - (now this apple comp is using the "a"s layout)
- the named layout we can imagine like the folder structure, we find the closest one. we use the name to find the closest one as long as it found it then we can use that one.
- we can also specify two layouts ex - "__layout-a@root.svelte" now our comp is pointing 2 layouts (a, and the a points the root layout)
- if we want our comp to ve the default layout (the default layout is one w/o any name ex - **layout.svelte), then we can specify our comp like "**layout@default.svelte", Note: this **default** name is reserved kw and we can't use to specify the layout ex - \_\_layout-default.svelte, we can't specify this layout like the named layout.
- when we ve layout named root and we points that to the root ex - "layout-root@root.svelte" in this case it wan't to extends from the layout called root then it notice that is itself, so we can't ve a layout that refers to itself and that will be ended up in a cycle/loop and ve the **recursive** error("recursive layout detcected").

### Passing data from the page comp to layout component with $page.stuff

- so why are we using stuff ? why not stores ?
- as we know about the module singleton, if we tryna import a module with in a certain context, it will always returns the same module instance, meaning if we ve multiple files importing the modules(exports the same store) we will be always getting the same store.
- but in svr side we only run this code once which means we only ve one instance of the store. serving to multiple users. which is bad and dangerous..
- each client uses their own browser and within their browser context they ve their own store instance.

- we can use the stuff in the load() and returns it ex - load({stuff}) {return {stuff}, props: {...stuff}}, and then we can export the stuff in the script tag or the other way is we can import the page stuff
  - ex - import {page} from '$app/stores'; then(we can export) stuff: {SJON.strigify({$page.stuff})}, since its in the page store we can access it everywhere we want to.
- so we can keep passing things into the stuff and eventually we render the layout we can access them, all of them are in the stuff, we can use them to render our layout component .
- this is how we use stuff to pass things back from the route comp to all the way back to the root layout comp.
- when we re using the stuff from the root layout to the components (thru the load()), then while redering $page.stuff (which is from the store), and the store itself is a singleton, and we start use the store which is not the same store for all the other req.
- we can't subscribe the page stores outside of the comp initialization, bcoz the page store depends on the context. when we try to create a new context.. ex - const cont = setContext(foo(), '1,2,3'); this context is a new context every time we create a new comp instance.
- this context is only available for the life time of the comp, so once we finish and create a comp for the new user, then its a new context,
- ie y we ve to call the getContext() only during the comp initializtion, when we try to subscribe from the page it actually reads from the context to get the val from the context. (b4 we create the store)

- now y is the {page} from $app/store works is that bcoz the impl of this page store itself is using the context to get the val of the stuff. that's why its always the diff store val for the diff users.

- for diff users(diff req to the svr) its always a diff store instances. coz the store instance is the context and its being passed down thru context.

### Using Html elems w/o sk

- we don't need js for the forms to work, with js the form elems is just hunts, we can now submit the data w/o having to refresh the page. therefore its called progressive enhancement.
- as we know when using the form elem we can specify some native form props like action(could be an endpoint), method(get, post),
- and the button (inside the form) elem also has the type prop(submit) and inside we can specify like formmethod, formaction etc which will override the form elem's action or method ex <button type="submit" formaction='' formmethod='' /> can be useful when we ve different buttons inside the same form and all are submiiting different endpoints or different methods.

  - or if we want a normal button then specify the type "buttom", then we ve to add a js code to listen for the event.

- **Implicit Submission**
- is a feature that all the browser suppoted, basically its for accessability that the browser can implicitly submit our form.
  - ex - when hit enter it will submit the form.
  - its helpful for user ie inacessible, easier for em to hit enter rather than find where is the button and click.
  - the condns for the implicit submission are if we ve one button that submits the form,
  - or if we ve multiple form then the one with the first submissible button will click.
  - then other case if our form has only one i/p then with or w/o the button, when we hit enter it will submit.

### part 2

- as we know with the forms we re limited to the get and Post methods, what if we wanna to ve the put or delete methods, we can ve it but there is a trick to do that.
- to do that we can add the method as a query params ex - <form action="./form/submit?_method=PUT" method="POST" /> after this query params(?) we define the underscore method to mention the put or delete method.

  - in this ex this method is still a post method, but the sk sees the query params method's put, then it will use the put req handler to handle the request.
  - but by default it will be post as we metioned in the method, to override this post to put(in the query params) then in the "svelte.config" file under the kit prop we can specify to add the method override and the type of method we wanna override - ex - kit : {methodoverride: { allowed :['PUT','DELETE']}}
  - note : this \_method override is only allowed when our form method is 'POST' not for the get method, if we ve the GET method then we can't use the \_method Query params.

- why we re overriding the methods, as we know in the forms we can only do the get or the post method but if wana do the delete operation or the patch operation then this is the way we can override the form's default method behaviour.
- the one more benefits of this put and the delete endpoint is later on we can still make a fetch req,

- after the query param it doesn't ve to be exactly the \_method= it can be our cust name/var as well ex - <form actions="./form/submit?_hey=PUT" method="POST" /> and in the config file we can add this var as the param ex - kit : { parameter : '\_hey', allowed: ['PUT', 'DELETE']}
- the post req can return some body, which we can used as the props for the page that we re seeing, they re like the page endpoints ex - fn get ({url} : Params<RequestHandler>[0]) {return {body: name : url.searchParams.get('name')}} -- now whatever the name that we type in the form it will be the page endpt.

### Progressive enhancement

- the easiest way to progressively enhance our form is to add the **use:enhance** action, it will emulate the browser native behaviour just w/o the full page reloads.
- the main take away here is refer the docs for the $app/forms - and the other stuff like result action types such as Success, invalide, redirect,
- and in the submit fn where we can destruct the form, data, cancel and action ex - const submitcreateNote: submitFunction = ({form, data, cancel, action}) => {const {title, content} = Object.fromEntries(data)} .. and with this we can return the result and the update obj.

  - the update() is like the invalidate.

- **applyAction** also from the $app/form.. the behavior of the applyAction(result) is always gon depend on the result.type

### Protect sk with route hooks

- as the client send the req to the svr, it first hits the handle hook which does 3 things, 1. as the req hits svr b4 the res is generated, 2.render routes and gen res await resolve event 3. after res generated
  - import {Handle} from sk
  - ex const handle: Handle = async ({event, resolve}) => {const res = await resolve(event);}
  - and we can pass in like - event.locals.user = authenticateUser(event); if (event.url.pathname.startsWith("/protected")); and this will protected all the routes under the path (ex - ref from docs)

- and there is also a sequence (helper fn) fn in the @sk/hooks - is a helper fn for sequencing multiple handle calls in a mw like manner


### Are routes actually protected ?

- as from the doc under parallel loading all the load fn from the svr side are running parallel, so sometimes the page.sever does not even know that we re authenticated, and it gon run anyway, and it can make a call to(fetch) to the db w/o even us logged in.
- so every time we make a call to the sensitive info, we ve to run a req to our svr to check to see if the user is actually valid.
- so instead we can use the this approach in the load() in every page.server - ex - const load = async({parent}) => {await parent(); } -- make sure to call this b4 we fetch any data  from the db.
  

### Build a reactive search filter with sk

- lets make use of the store, first we ve to make the decision what all the things we wanna search by, put all the prop we wanna search in a [] 
- ex- const searchProduct = data.products.map((product) => ({...product, searchTerm: `${product.name} ${product.description}`}))
- then we can create a writable store ex - const createStore = (data) => {const {subscribe, set, update} = writable({data: data, filtered: data, search: ','}) return data, filtered search}

- and then we can create a search handler - ex - export const searchHandler = <T extends Record<PropertyKey, any>>(
	store: SearchStoreModel<T>,
) => {
	const searchTerm = store.search.toLowerCase() || ""
	store.filtered = store.data.filter((item) => {
		return item.searchTerms.toLowerCase().includes(searchTerm)
	})
}
- the code for the [repo](https://github.com/naresh5033/svelte-search-filter) 


### No Flikr SSR theme switcher 

- the **Daisy Ui** the ui lib where we can find some cool theme libs, 
- in the layout we can use the form item to tell which theme we want to use. and inside the form action set the endpoint to the diff theme. ex <button formaction="/?/setTheme&theme=dark"> Dark </button>
- then in the server page (endpoint), set the actions refer the code [here](https://github.com/huntabyte/sveltekit-theme-switcher)
- this is wat enable us to ssr the updated color themes based on the user's cookies
- then we can set up the hook (Handle from sk), and this hook will iintercept our req and get the theme from the form ex - const handle: Handle = (async ({event, resolve}) => {let theme: string | null =''; const newTheme = event.url.searchParams.get("theme") }) , and then lly for the cookie theme and then some condn refer the code.
- then use the "transformPageChunk" - that applies custom transforms to the HTML - ex - const res = await resolve(event, {transformPageChunk: {html}})
- then in our layout page we can use some progressive enhancement, in the submit fn. (from $app/form)
- and then in our form **use:enhance={submitUpdateTheme}**
- what if we tryna do in the diff page (isn't gon redirect the user), well that can be simple, we can add another query params in our form action button - &redirectTo={$page.url.pathname}


### improve the performance of the load Fn

- "promise unwrapping" - top level promises will be awaited, which makes it easy to return multiple promises w/o creating a waterfall
- the concept here is if we ve many impl, or some fns such as the setTimeOut then wrap every thing in the aync promise in that way everything will be asyn, and improves the performance of the load function.
  

### Simple Crud app with prisma 

- source code [here](https://github.com/huntabyte/sk-prisma)


### speed up the app with cache control

- lets ve a ex - where we wanna build a movie app, where the user can search for the movie and view a list of movies matching the search term.
- lets say if we ve cache control set in our res max age of 600sec, but the prob is when we use something like vercel which is stateless, that can't keep track of the cache control header, and by default  we re not passing the header along to the browser.
- so the 1st thing we can do to impl the caching is to simply paass the cache control header to our browser. 
- to read the cache as we know, const cacheControl = res.headers.get('Cache-Control'); if (cacheControl){setHeaders(cacheControl: cacheControl); }
- so if we set that for the first time, lets say the page load takes 400ms and the following time it will take like 2ms, so the page is cached in the disk cache.
- lets say if we have bunch of users req for the same movie in a 10min time period, we will res em for every single one which costs us in the 3rd party api,
- in this scenario it will make more sense to ve something like redis cache store
- so if our client make a req the sk svr will check the redis for the cache, if the cache was expired then the svr will make a call to the 3rd party api to fetch the data. and then stores the data in the redis then res back to the client.
  - and this is called the cache miss and cache hit. (coz for the 2nd user it can now fetch from the redis cache)
  - and lastly we ve to set the ttl(time to live) in the redis and pass it along to the client alon with the data
  - refer the code [here](https://github.com/huntabyte/sveltekit-redis-caching)

### Sk authentication with lucia and prisma

- source code i [here](https://github.com/huntabyte/sveltekit-lucia-prisma)

### Error Handling 

- we gon start with the 2 types errors that sk recognizes
- **expected Errors** are the ones that we creates with the sk error helpers, and are expected coz we cereated it (as expected)
- **unexpected errors** usually occurs from the 3rd party lib, 

- **Error pages and Boundary** which enables to handle the error msgs gracefully, by default when the error occurs sk displays the error page with the status code and error msg
- when the error occurs sk will render the nearest error boundary, we can think of the boundary as the blast radius or the error, or how much of our app the error is gon impact.
- for ex if we ve a nav bar and the container which hold all the paged content and if the error occur on the page, our layout will still remain intact, the container is the error boundary here (in our case)k

- there are few scenarios as the error page won't be rendered as we might be expected, 
  1. if the error occurs during the load the root layout.server, (with in the root layout's load()) then sk won't help us, and in that case sk renders **Fallback Error Page** 
     - we can still customize the fallback page, refer the sk docs for the error handling code.
  2. **Handle Error Hook**  - refer the doc for the handle error hook, as he used an third party app called **sentry** to manage the error 
     - in that handleServerError() he is gen a rand error uuid, so we can be return this id to the client, so the user can contact the support team, and the support team can search with the error id, and identifies what type of error that the user experience with.


### SK snapshots

- the snapshot is a way to preserve the ephemeral(lasting for a very short period of time) DoM state
-lets say we wanna impl the snapshot for our form data, the snapshot has 2 methods restore() and capture(), 
- ex - const snapshot: Snapshot = {capture: () => formData, restore: (value) => (formData = value) }.. as the formData is the vals of the i/p or the form fields and we re restoring the vals
- as our snapshot/ form data is preserved if we nav thru the page, with our filled in form data it will be still preserved, and even if we refresh our page the form data will still be there, 
- in conclusion we can use this snapshot for any ephemeral state, it doesn't need to be a form, 


### Streaming with promises (refered as **Defer** in other frameworks)

- when using server load the nested promise will be streamed to the browser as they resolve, this is useful if we ve slow non essential data, since we can start rendering the page b4 all the data is available.
- one of the caveat is that if the platform doesn't support streaming then this will not work, ex the aws Lambda, and the other is that js must be enabled to work, otherwise the promise will just work as the normal
- and finally we should avoide this in the universal load() , if the page is server rendered and this are not streamed 

- sk recentky introduce specific runtime for each of our fn, we can now ve all of our app, except for this one fn run on node and then for that particular fn run on edge (in vercel)

  - for that import config from sk adapter-vercel and then set the config to runtime: edge (refer doc)
  - for ex - in the tutor he download this adapter and then set the rutime as node in the config file and then in the api he assign the runtime as edge

### Chat gpt bot 

- the one thing to know from the chatgpt doc is Tokens will be sent as a data, only the svr sent events will become available with the stream terminated by the **data: [DONE]** messages, this is how we know that we re done receiving tokens from the stream, whenever we get this msg here.
- the source code is [here](https://github.com/huntabyte/chatty/tree/main)


### Sk super forms ( with this lib forms will never be the same)

- a new lib for handling forms 
- as we know we ve been using forms with some kind of schema validation like the zod to validate the form fields and then inside the action we again validate the form data against the schema, and then handle the error or return the success.
- and then in our client side we can then use the form props, and on top of that adding some progressive enhancement may ve increase the complexity of our simple form handling,
  - thats where the **super Form** comes in lets see how incredible this lib is.

- this lib also uses the zod for validation. and it also coerces with the str data from the form into correct type
- `npm i -D sveltekit-superform zod`
- The source code is [here](https://github.com/huntabyte/superforms-demo)
- this lib also has the <superDebugger /> comp which is super handy for debugging, as we can see the real time data changes as we type in the form and the page status
- there are so many cool features this lib has and one of them is the **tainted form check**
  - when  tryna modify the form fields then close the tab or open the another page in the same tab, a confirmation dialog will pop up and prevent us from losing the changes.

- Client Side validation
  - ex - const {form, constraints } = superform(data.form)
  - and then for each of the form i/p we can use <i/p bind:value= {...$constraints.firstname} />
  - and similarly for all the other i/p fields as well 
  - so it will give a nice warning about each fields if the user misses or messed up

- "Validators"
  - ex - validators : {firstname: (firstname) => (firstname.length < 1 ? "the name is too short" : null)}
  - or additionally we can also pass in the zod schema ex - validators : newContactSchema


### Better Protected routes REdirects 

- when the use tryna access to the particular route and its protected, requires the use to login, once the user is done login then he has to redirect to the route that he was tryna access not the root or home page
- we can accomplish this easily by url 
- ex - const load = async (event) => {const formUrl = event.url.pathname + event.url.search ; throw redirect(302, `/login?redirectTo=${formUrl}`)} or we can make this as a fn like the handler() in the utils and then reuse whenever we wana redirect our route
- and then in our action page(server page) - where we redirects him to the home page earlier.
  - ex - const actions = { default: async (event) => { loginUser(event); const redirectTo = event.url.searchParams.get('redirectTo')}}

- there is one more thing we can do to make it little bit better is to add some msg, so the user can know why there are redirected.
- we can pass in the msg var in the handle fn refer the code .


### Pagination in SK
- when we re sending the fetch req(GET) to the db we can use like "GET/items?limits=10" which can be like our firs req and the page 1, then for the second req for the next 10 items we can send like skiep the 1st 10 items ex "GET/items?limits=10&skip=10" is the second page
- the code is [here](https://github.com/huntabyte/sk-pg)


### The best UI lib for svelte

- **skeleton UI** 
  - which is tightky integrated with the tailwind, to install skeleton
  - `npm i @skeletonlabs/skeleton --save-dev`
  - the skeleton docs has the **svelte section** where we can find all the comps, and the actions of the svelte that the skeleton has to offer.
  - and in the utility section we can find some of the useful utilities like local stores(which is a extended version of svelte store that allows us to easy interaction with the browser local storage), modals, pop ups, 

- in the ex - he used the app shell from the svelte section.


### Pass messages b/w pages in sk (flash msgs)

- the sk flash msgs is a sk lib that usually passes temprary data to the next req, usually from form action and endpoints,
  - `npm i sveltekit-flash-message`


 - lets ve a scenerio where the user registers for an acc, we wanna redirects him to the login page and send him a flash msg, telling him to check the email.
 - first wrap our load fn with this flash message fn, its bcoz the lib has to access with our event and has to set the cookie with our msg, and then grab that cookies when it comes back in.
   - ex - const load: LayoutServerLoad = loadFlashMessage(async (event)=> {return {someData: "anything we want"}})
   - then in the server page's action, set the pass in the event (which is gon to set the cookie on the event and then it grabs the when it gets to the layout server load)\
   - now inside of the layout page we can access the msg using the page store from $app/stores
   - and then finally to reset the flash store, whenever we redirect or nav to other page we can restore the flashstore, by using the "beforeNavigate()" lifecycle hook (and inside we can set it like if we re coming from and the going to is not eq then the flash is undefined)
   - refer for the source code [here](https://github.com/huntabyte/flash-messages)


### The Anti component Library (shadcn)

- the community has created a shadcn-ui for the svelte version, with some limited ui comps as the shadcn is for the react based.
- to add the comps `pnpx shadcn-svelte add ` will show the list of currently available comps that we can add.


