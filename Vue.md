# Nuxt 3 Minimal Starter

Look at the [nuxt 3 documentation](https://v3.nuxtjs.org) to learn more.

## Setup

Make sure to install the dependencies:

```bash
# yarn
yarn install

# npm
npm install

# pnpm
pnpm install --shamefully-hoist
```

## Development Server

Start the development server on http://localhost:3000

```bash
npm run dev
```

## Production

Build the application for production:

```bash
npm run build
```

Locally preview production build:

```bash
npm run preview
```

## Twitter Clone App

- Its a full stack nuxt js project with Vue and tailwind for the frontend, prisma(for ORM) and Mongodb for the DB.
- Auth(jwt) for the authorization
- Image upload (Cloudinary)
- the backend file structure --> server/api/ and every file that goes inside the api will map to an endpoint
  `npm nuxi init nuxt-app-name`

# Dependencies

- tailwindcss `npm i -D @nuxtjs/tailwindcss` `npx init tailwindcss`
- Icons - from hero icons `npm i @heroicons/vue`
- prisma - for the ORM `npm i -D prisma`and `npx init prisma` will generate the schema.prisma file in the prisma directory.
- `npx prisma db push` to push the schema to the database. it will generate the prisma client.
- bcrypt `npm i -D bcrypt` for hashing the password.
- the server will return the user(register) obj in response, which contains the encrpted password, which is not a good practice either we can exclude the field. we can make a directory called transformers, which is like a DTO and there we can exclude the fields that we don't want. and use the Dto for the transfer.
- for the auth, we will use JWT `npm i jsonwebtoken`
- Tailwind forms `npm i @tailwind/forms`
- url- patter `npm i url-pattern` -- easier than regex string matching patterns for urls and other strings turn strings into data or data into strings. ex- var pattern = new UrlPattern('/api/users(/:id)'); and we can match the pattern by pattern.match('/api/users/10');
- jwt decode `npm i jwt-decode` to decode the access tkn (once the user login we ve to gen the access tkn every 10m and verify/decode it)
- Formidable `npm i formidable` to handle the multipart(req body/post that includes img) file.
- cloudinary `npm i cloudinary` for uploading the media files. and this will become the provider for our media files.
- human-time `npm i human-time` to read the time in readable format.
- headless ui `npm i @headlessui/vue` for the dialog model.
- mitt `npm i mitt --save` is the event bus (when we re passing the props to the child which inherits from the hierarchy of parents which can some time leads to the props drilling)
- There are basically two ways of making unrelated components communicate with each other: 1.Pinia (Vuex in the previous version) 2.Event Bus
- Event bus is related to the classic Events/Listeners architecture. You fire an event that will be treated by a listener function if there is one registered for that event.

# Nuxt 3

- The nuxt provide us the file routing system that works on our folders and it will automatically create our routes
- "Routing" one core feature of the nuxt is the system route, every .vue file inside the pages/dir will creates a corresponding url
- "NuxtPage" <NuxtPage> is the exactly like the router in the vue
- if we wana catch the id from the event then we can make [id].vue or [id]dir so it will catch the route with the id param.. routes.params.id.. when we wana make a dynamic route we can use this
- const rooute = useRoute() --> this will gives the route as the proxy obj and we can access/get that by route.params
- like this useRoute hook there are other few inbuild methods and hooks are available in vue w/o imports
- "UseNextApp()" will give the access to the current context..aka is a build in composable that provides a way to access the runtime context of the nuxt application.
- we can access vue app instance, runtime hooks, runtime config vars, internal states such as ssr stateContext and payload
- Navigatioin <NuxtLink> navigatioin b/w the routes
- "components" every comps(in the comps dir) in the nuxt are auto imported and can be use anywhere in the application
- and the comp dir/file path works just like the pages based on the name of the file and the level of the dir/filepath
- "Layouts" default.vue is the default layout (for all the page)of our app and to display the content on the layout we ve to use <slot>
- whatever layout we define in the layout dir that will automatically be registered and we can use em in our pages.
- "definePageMeta({layout: "custom",})" by using this method we can define what layout we wana use for our page
- "Assets" 2 dirs are maintains our assets 1. publc dir and 2. assets dir
- the public directory is used as the public server for the static assets publicly available at the defined url in our application
- unlike the assets dir, whatever files we put in the public dir we can simply access them by /followed by the url or the filename(of img)
- we can use the pckg called "Vite svg loader" `npm i vite-svg-loader` we can import the svg from the asset/pub dir and to render them directly like the component ex <MyIcon> . before we ve to define in our vue config file
- "Icons.svg.org" can find some icons (svg ) in this site, and in this site we can download the icons as the vue components
- "Composable" (dir is auto imports)are the equivalent of hooks or the reusables(logic). the naming conv is to use the use before the file name (same as hooks) ex .useUtils.js. and it will automatically available w/o imports.
- Go check out the "Vue Use" site. the library very useful fo the vue community/devs. this lib has ton of methods/hooks that we can use in our application.
- `npm i @vueuse/core` has ton of pre defined fns we can use in our application
- "plugins" nuxt will automatically reads the files in the plugins directory and loads em in the creation of the vue application. and inside the file we can use it like export default defineNuxtPlugin(()=>{}). this is the way to add the fn in our plugins and to get the fn from our app context we can extract like- const {$fnName} = useNextApp()
- like i said often times the plugins are used to inject the logic of certain lib into the nuxt context.

# Middleware

- "The middleware directory" nuxt provides the customizable route mw fn that we can use in our app .. the idea is to "extract the code and that we wanna run before nav to the particular route".
- there are 3 types of middleware 1. anonymous 2. named route middleware 3. global route middleware(to trigger everywhere and the suffix is /middleware/auth.global.ts).
- and to use the mw fns export default defineNuxtRouteMiddleware((to,from)=>{})
- and to use the mw in our pages definePageMeta({middleware: 'auth'})
- we can also use the addRouteMiddleware() inside the definePageMeta().. to add the mww dynamically

# Modules

- are the libs..are async and can be published on npm to save the time of other devs
- to use the modules we ve to register in the nuxt.config file

# state management

- vuex, and pinia in vue2 and useState in vue 3
- Nuxt provides the useState() composable to create a reactive and ssr friendly shared state across the comps
- useState() works only during the setup or life cycle hooks
- useState is the ssr ref replacement
- so which is better the pinia or the usestate
- well the pinia is good for the more complex apps where we keep adding the features and the usestate is good for the simple/small app
- `npm i @pinia/nuxt` and we can define the stores by export default const store = defineStore('counter' {state:()=>{}, getter:()=>{}, action:()=>{}) -- this is the vue 2 version
- and the vue 3 version is as same but inside the defineStore we use the ref, computed and the fn (or the action)

# Server

- server dir.. nuxt automatically scans the files inside the ~/server/api, ~/server/middleware, ~/server/routes dirs to register api and server handles with hmr support
- and each file should export the default fn defined with the defineEventHandler((event) =>{}) method
- we can now universally call this api using await $fetch('/api/filename') in our frontend to fetch the data from the backend.
- this handler will return the json data, a promise or use event.node.res.send() to send the response.
- the event obj(h3 event obj) has the content, node(res and req obj)
- and its a good practice to follow the name conv (its not necessary) ex for a get method endpt /api/hello.get.ts.. the api file name followed by the method we wana use for the endpoint.

# Nitro

- the Nitro is the server engine for the nuxt app.
- its a cross platform support, serverless out of the box, hybrid mode for the statc or serverless sites.
- automatic code splitting and async loaded chunks
- "Caching Api" it provides the powerful caching s/m builds on top of the storage layer.
- ve the storage layer, mounting points etc refer the doc.

  # Rendering

  - ssr - each time the user requests the page and the server is able to serve the page on each request.
  - static sites are sim to the ssr. with the main difference is that the static site are render at the build time therefore no server is needed.
  - with the client side rendering we ve very bad seo performance. indexing and updating the content delivered via client side rendering takes more time.
  - Making a static page interactive in the browser is called "Hyderation"
  - whats neww in nuxt 3.
  - well it allows "Hybrid rendering" - it allows diff caching rules per routes using the route rules and decides how the svr should response to a new request to given url
  - with this we can define the route rules inside the config file

# Data Fetching

- nuxt provides useFetch, useLazyFetch, useAsyncData, useAsyncLazyData to handle the data fetching.
- "useFetch" to universally fetch data from any url
- this composable provide the convenient wrapper around the useAsycData and the $fetch
- the useFetch freezes our app until the moment we recieves the data and renders the data
- Where as the useLazyFetch we can use the loader while the app is loading
- "useAsyncData" to get access to the data that resolves asynchronously.
- it is equivalent to the useFetch(url) -- useAsyncData(url, () => $fetch(url))
- useRequestHeader() -- to access the cookies to the api from the server side.

# SEO and Meta

- useHead()- provide the app head prop in our config file which allows to customize the head of our application.
- put the title and the description as the metadata, which will be useful for the seo.
- we could ve some dynamic data "Reactivity" supports all props computed, getter refs and reactive

# Hooks

- "Lifecycle Hooks" nuxt provides powerful hooking s/m to expand almost every aspect using hooks
- go to the docs and see all the available App hooks (runtime)
- we can use the hooks with the modules or the plugins
- nuxt/kit is the lib to create the modules, we can also loop thru the nuxt context and add our own hook inside the module that we wana create

# Nuxt config.ts

- we can add css, or modules or any config related things in this file.
- it will gives the universal configuration for all the modules that we used.

# Nuxt Content

- @nuxt/content
- one of the usefule module provide by the nuxt community is the content.
- the nuxt content reads the static content in the content directory such as .md, .yml, .csc, .json files to create a powerful data layer in our application.
- it will read those files and create the html page
- `npx init content-app -t content`
- we can also creare our documentation with the content, to make this static site we can use the lib docus @nuxt/themes/docus
- `npx init nuxi@latest init -t themes/docus`

# Vue (udemy)

- is the declarative approach (we tell the goal and it will take care of others) and its reactivity
- vue is js framework to develop the reactive frontends
- different ways of utilizing the vue , spa - server only sends one html thereafter vue takes the full control the ui and mpa some pages are still rendered on and backed up by the server
- for the i/p elem we can use the props **v-model** directive
- **v-on:click** for the button click
- **v-for** to loop over the list ex - v-for="goal in goals" {{goal}}
- to create a vue app if we use the cdn in the <script/> tag .. const app = Vue.createApp({data(){return {courseGoal: 'some val!..'}}}); app.mount('#user-goal') ..section id

- **interpolation and data binding** - {{ is the interpolation}} with here we can use the props key and the val will be appear on the view ex {{courseGoal}} .. when we can't be able to use the interpolation ex inside the quotes "" that time we can use the data binding **v-bind**

- some times we dont ve to use the interpolation so we can use the **data binding**- to bind the val of the html el<a v-bind:href="" /> : colon followed by the attr name
- **methods** takes the objs which are fns ex - methods: { outputGoal(){}, } and we can use it in the interpolation {{outputGoal()}}
- and any obj that we use in the return stmt will be available to access in the createApp() and we can acess inside the methods using this kw ex this.courseGoal
- **v-html** to bind the raw html we can use this directive ex <p v-htm='outputGoal()' /> but this could lead to csrf so care full when using this directive

- **event binding** - adding event listeners to the html element **v-on** ex - <button v-on:click="" />
- **v-on:input** to listen the i/p event. ex setName(event, lastName){ return this.name = event.target.value + ' ' + secondName; } and to use this fn in the..<input v-on:input="setName($event, 'kumar')"/> ..if we wanna pass the second param (where the 1st param of the ev listener method will always be the event) we can also skip the event param (explicitly mentioning it since its default param) then when render the method we ve to explicitly dec the $event arg followed by the 2nd arg.
- **event modifiers** there are some built in event modifiers in vue and we can use it with the period followed by the event binding ex <button v-on:submit.prevent="" /> this the js prevent default modifier and there is also "stop" modifier to stop propagation and lly for the "left", "right" and "middle" modifiers to tell only listen on left or right or middle click by default its left
- **key modifiers** for the keyboard event listeners like one for the enter ex **v-on:keyup** for any key to press and release event listener lly we can use the keydown ev listener .. to listen for the specific key v-on:keyup.enter=''
- **v-once** for locking once.. where in some scenarios we wanna preserve the initial state of the data and save it, and don't wanna reflect on the consecutive changes then we can use this v-once ex -

- **2 way data binding** - Data binding + event binding = 2 way data binding..

  - vue has the special binding we can use for this .. ie **v-model** prop is a shortcut for v-bind:value and v-on:input ..
  - and this is the 2 way binding we re listening to the i/p event and writing the val back to the i/p el. event

- methods are not not good for o/p some dynamic vals(or 2 way data binding) {{someFns()}} since it will exec whenever the data changes in the pages regardless of the that method called or not and its perf issue..which is not so optimized and the better soln is "computed".

**Computed** - props are like the methods.. the computed is the 3rd big config option we can use in the createApp() 1. data, 2. methods 3. computed..

- computed is just like the methods but the fns we called inside the computed will exec diff than of the methods ex - computed: {fullName(){}}.. and we can use it inside the interpolation {{fullName}} just like the data prop..
- now this fn will called only once that's the diff the perf ..the computed will cache any of the dep of the computed changed (and only re evaluate and recache if the dep of the computed changes)
- so for performance its better to use computed than method for outputing the vals.. only use method if we know that we wana recalculate for anything the val is change
- we still bindt the events to **methods** and we **don't bind the event with computed**

- **Watchers** is a fn we can use when any of the dep changes .. inside the watcher we can def any data() props ex - watch: {name(value){this.fullname = value + 'kumar' }}.. so whenever the name prop inside the data() changes the watcher will watch for the changes and updates ..
- this the wathcer accepts value as the default parameter which is the last changed/updated value..
- this also accepts the second parameter oldvalue or the **prev val**
- in most cases the watchers are tedious we can achieve the same thing with the computed
- but there is one scenario where the watcher shines - lets say we wanna reset the counter when the val exceeds to 50..
- ex - watch: {counter(value){if(value > 50){this.counter = 0}} and
- another ex - would be **http req** , or timers (setTimers)

- **methods** - use with the event binding or data binding {{}}
- **computed** use with the data binding, and use for data that depends on other data.
- **watcher** not used directly in the template.. for any non- data update we want to make (are least used upon these 3).

- **v-bind and v-on shortcut** instead of using em we can use the shortcut for them ex - **@** ex - <button @click="" /> for the v-on
- and : colon for the v-bind ex - :value .. and there is no short hand for the v-model

- **dynamic style with the inline styling** - we can add click listener to any html el (not only for the buttons)
- we can bind the style with **v-bind:style** or shorthand :style ex - <div :style="{bg: black}" />
- lly we can bind the css class dynamically ex v-bind:class or :class for the shorthand. and we can use <div :class="{active : boxSelect}" /> and we can use them with normal class along with the dynamic class binding
- ex - <div class="demo" :class="{active: boxSelected}" />

- **classes and computed props** - we can also move the html logic inside the computed and then bind the class to the computed method/prop.. it can be helpful for more complex(conditional) dynamic prop

### Rendering contents with conditions

- **v-if** for rendering the contents conditionally. ex we can use an arr to add the obj and if the arr len is 0 then don't render the el (p tag per se)
- ex - const app = Vue.CreateApp({data(){return {goals: [], enteredVal: '''}}}, methods: {addGoals(){this.goals.push(this.enteredVal)})
- and in our html we can use the v-model in the i/p to bind the enteredVal. and in the btn add the click listener and for the add goal and in the p tag <p v-if="goals.length === 0"> No goals added />
- and <ul v-else> <li/> this v-else followed by the v-if will only render the list if the v-if is unsatisfied.

**v-show** - hides and shows the items with the css...we can use this only if we ve an el whose visibility changes alot like (toggle btn)

- **v-for** to render the list of data ex to loop thru the arr <li v-for="goal in goals"> {{Goal}}</li>
- to add the index - v-for="(goal,idx) in goals"> {{goal}} - {{idx}} </>
- lly to the arr we can also loop thru the obj and grab the vals ex - v-for="value in { name: 'naresh', age: 24}" > {{value}} </ >
- sim to idx we can also pull the key ex - v-for="(value, key) in {..} // and use it in the interpolation // and we can also use the idx as well as the 3rd arg. (val, key, idx)
- lly to remove the el in the arr in the method (instead of push use splice) ex methods: { remove(idx) { this goals.splice(idx, 1) } // splice 1 el at a time..
- and now <li v-for="(goal, idx) in goals" @click="remove(idx)" >{{goal}}{{idx}} </>

**list and keys** - vue reuses the dom el, to optimize the performance.. and that can leads to the bugs if the el contains the same state so identify we can use the key attr

- we can use the **key** attribute to the el which is a non default html attr instead its use by vue to understand and identify the el and we when use it along with the v-for we can bind the idx val for this ex -**v-bind:key="idx"** //idx or item (goal) or some id we can use..
- and its a good practice to use the :key attribute alon with the v-for loop

### monster slayer game(project)

- we ve monster health, our health and attacking strategies such as special attack, heal, surrender..
- refer the code for the health bar reduction we use the style bind to the data prop ex - <div class="monster_class" :style="{width: monsterHealth}" /> // lly for the player
- or we can put those js logic in the style and put em in the computed property
- and we can impl the special attack strategy by only using the btn once in a 3 times and hide em when its not in use and simple logic is add a data prop (current round) initial val 0 and keeps track of em (for every attack inc the current round).
- and only show the btn if its %3 =0 ex - <button :disabled='currentRound %3 !==0 @click='specialAttack >
- "Heal functionality" - only for the player.. and the healing should not be exceed 100% of the health
- adding the game over screen - we can add the condition to check the health of the player and the monster to see whose health reach 0 first
- we can add a watcher that watches for the health of the player and the monster.

### Vue Bts

- we can see how the vue works bts and understanding the virtual Dom
  - vue has the built in reactivity that helps to update the data automatically bts.. it turns our data obj into a reactive data obj, by wrapping the obj into js feature called **proxies**
  - it uses the proxy extensively on the objects
- **Proxy** - as we know js is not reactive.. so it uses proxy ex const proxy = new Proxy(data, handler); //takes handler fn.. const handler = { set(target, key, value) { } }
- the target is the data, and key the prop (of data) we set val to, val - the val we set to the prop.. and in the handler() the setter fn is triggered whenever the new prop is declared

- in a nutshell this is what vue does bts, whenever the prop changes it will update..

- **one app vs multiple app** - we can use multiple apps and mount to diff section(id) ex app2.mount('#section-2');
- but we can't use/share the data prop, and each apps ve no connection..

- **Templates** - by mounting the vue app in certain part of our html, we re making the html part as the template
  - we can also add the template inside our app ex - const app = Vue.createApp( template: `<p> {{ goal}}</p>`) // we can use the html template inside the app.
- **refs** getting the val from the i/p el diff way.. we ve been using v-model or @input binding to get the vals..in this approach we log the vals the user enters (each one).
- vue has the feature that retrieve the dome el only when we need em instead of all the time ie **refs** just like the key attr we add the non html attr refs
- basically vue stores the val and memoize em ex - <input type=""" ref="userText" /> and we can access this by the vue's build in prop (all the built in props prefix $) ex - setText(){ this.message = this.**$refs.userText.value\*\* // the refs is a obj with kvp.
- and to see the i/p obj -- console.dir(this.$refs.userText); we can see all the props (including the value prop)

- **how virtual dom works** - uses the copy of the original js based dom in the mem virtual dom. exists only in the mem..
- and when the data changes it compares the old virtual dom to the new virtual dom and updates are made to the virtual dom first and differences are then rendered to the real dom.

- **app life Cycle** - or instance lifecycle..
- starts with createApp({}).. this will followed by the bunch of lifecycle hooks.
  - 1. beforeCreate() 2. created() (still nothing will render) and in this phase it will compile the templates 3. beforeMount() (right b4 we see something on the screen) 5. mounted() is where see the rendered template on the screen ..and this is where the mounted the vue instance .. and any data changed will cause the
    2. beforeUpdate() and then 8. updated()
  - and we ve unmount scenario as well where the app will be unmounted and all its content will be removed and there are 2 hooks for this lifecyle
    1. beforeUnmount() 2.unmounted()

### Components

- here is where we used the unmount life cycle hook.. when we don't need the comp just use and destroy em for the performance.
- to reuse the code and split the code into small chucks .. to create a comp in vue - app.component('friend-contact', {data(), methods(), computed()}) // inside our cust html tags ..which is an identifier of our comp
- its like the app . and it takes all the data, templates, methods and computed and everything that app takes.

---

### better dev environments

- just make it like the normal project with the vue cli.. to create a new project `vue create vue-first-app`
- **App.vue** is the naming convention for our main vue file..
  - now inside our main.js we only ve - createApp(App).mount('#app');

### components communication

- **parent child communication** props.. are the custom html attributes .. for using the same comp with diff data.
- inside the props attribute we can define all the keys we want ex - export default { props: ['name', 'phone'], } and we can use this props inside our methods with the this kw ex this.name.
- the props vals are immutable.. vue use the term called **unidirectional** data flow.. means the data from app to the cust component can be only change in the app not in the comp..
- but there are 2 ways of changing it..1.make the prop as the data prop and access it. and another approach we will see later

-**validating props** we can ve the props obj and define all our props with the types and the default val(initial val) may be a fn for the complex logic ex isFavourite: { type: String, required: true, default: function(){}, validator : function(value){ return value === '1' || value === '0'; } the validator returns true or false based on the value.

- we can **bind the props** just like any other html attr ex :is-favourite="true"
- and lly we can use the v-for for our cust comp props and while using we must use the key attr(which is mandatory) ex - <friend-contact v-for="friend in friends :key="friend.id" :name="friend.name" /> lly dynamically bind all the other props of that comp.
- **emitting cust event child => parent communication** - if the child comp wants to talk to its parent it has to emit an event (to tell about any changes that happens)then the parent can listen for the event
- sim to the $refs the **$emit()\*\* is an built-in prop for the vue and we can use this to emit() to emit the cust event and then listen from the parent comp. ex - this.$emit('toggle-event') .. the second arg we can add any ex this.$emit('toggle-event', this.id) .. now this will carry the id as the data// and in the app bind this event to any js fn ex - @toggle-event="toggleStatus"
-
- **defining and validating** cust event.. ex sim to the props ..we can also use the emit: [], in our comp/app config and put all our events in the array. or an ogj ex emit: { 'toggle-favourite' : function(id) { if (id) return true; } }..
- the key take away is the **props** are there to send the data into the component and the **events** are there to send the data out of the component.
- we can also pass this emit direct to the html click binding ex <button @click="$emit('delete', id)" /> and we can define the event handler in the app.
- ex - deleteContact(friendId) { this.friends = this.friends.filter(friend => friend.id !== friendId) // here we re checking for not equality coz if its equal we wanna keep it, if its non equal we return false, and it they are eq we want to remove the friend.. since the **items are dropped if we mention false**
- so we wanna return false if we find the matching id .. to convert the str to num the short hand age = +age; //this will cast the age props (which is str) into num

**provide + Inject** to rescue when we ve events or data pass thru multiple components or levels in the hierarchy instead of emitting events we can use this approach.

- this pattern means provide the data in one place and **inject** the data in other place .. and skip all the intermediate comps..
- ex - in the app comp use provide: { topics: [] } and in the child comp inject this ex - inject: ['topic'], // we can inject the data only from the parent not from the sibling..
- if we wan't to use the dynamically instead of passing the [] in the provider we can make the provider as the fn type ex - provider() { return { topics: this.topics} }; // now any changes in the topics data prop will be updated in the provider and the child that injects.

- we can also impl this provide + inject for the cust events
- this provide and the inject should n't always consider as the first choice..// our props and inject are the default way of communication mechanism

### dive deep into components

- lets see the component registration and styling.. and **slots**, dynamic comps , names and folder structure
- **global vs local comp** - when we register our components in the main.js then its **globally available** any where in our apps ..
- this has one downside if the comp is bigger then in the init state vue needs to download em all (not optimized ) ex - some comp we need only once ex - header comp so we don't ve to registered globally instead import em only in the app component. and use it inside the app config ex - export default { components: { 'the-header': TheHeader } } // takes kvp key is the cust html and the val is comp.
- or use the cust header tag as TheHeader ex components: {TheHeader: TheHeader } // now when we can use this with the self closing tag in the html ex <TheHeader/>
- **scoped styles** we can make our styles scoped to only the templates we use in a comp ex <style scoped>

- **Slots** will gives the option of structuring our code and splitting our code into multiple different comps.. where we want to use our comp as a wrapper around dynamic content.
- the slots allow us to receive the html content from outside of the component, basically like the props but the props are for the data .. and slots are meant to be for the html code /template code the comp needs.

- **named slots** - since we can ve only one unnamed slot we can use named slots for the other sections in our html ex - <slot name='header' >
- **v-slot** will tell where the content will go to ex - <template v-slot:header> and the short hand **#** ex - <template #header> .. (like the ng)// now this template will goto the header slot.. and this is impl from the parent and we can use it in the child

- **slot styles and compilation** - when sending the content from the parent to the child the parent's scoped child won't be applied or passed.
- **$slots** is another prop we can use to see the content of that slots received in the component ex - mounted(){ console.log(this.$slots.header) } // $slots props followed by the name of the slots ..
- and we can also check to see if the data received for the slot ex - <header v-if="$slots.header" >

- **scoped slots** - the concept is to pass the data inside of the defining slot to the comp where we pass the markup to the slots. ex - <slot :goal="goals"> .. and we can access the val in the passing slot ex - <template #default="slotProps"> {{slotprops.goal}}</template >
- this scoped slots are advanced feature

- **Dynamic Components** - ex - when we click the button then one component will shown nd the other shouldn't
- we can add a event listener to the buttons and then use v-if to render the comp based on the mouse clicked.. instead vue has a special syntax for render the dynamic components.
  -ie **<component :is="selectedComponent"">** tag takes one prop "is"
- **keep dynamic comp alive** when we re switching between the dynamic components the data will be lost (comp life cycle ends) to keep the data or keep the comp alive we can use the **<keep-alive>** tag to wrap our <component > tag .. now the state of this comp will be cached..
- **teleporting the el** - teleport is the built in vue feature <teleport to="cssSeletor" > .. takes one prop "to" to the css selector(ex - body, #app, etc) selecting an html el on our entire page, where this page should actually be added to the html mark up.

### Project

- if we add any event listeners to the child comp ex - <base-button @click="setSeletedTab('stored-resource') > then by default this event listener will be **fall thru** and then apply to the root html el in the base button template which in this case is native html button.
- when we wanna delete things from the array using the provide and inject pattern then the normal filter() method will not work as we expected (it will delete the el from the array), but the updated array will not pass to the child so when its injected regardless of the changes occured in the arr it will still render the old arr instead we can do..
- we can find the indx and use the splice() to remove the el this method will works and remove the el as we expected ex - removeResource(resId) { const resIdx = this.storedResource.findIndex(res => res.id === resId); this.storedResource.splice(resIdx, 1); } // now all the comps injected will get noticed as the el remove from the arr.

### Forms

- there are some i/p built in modifiers we can use in the forms ex - <input v-model.number='age' > // will convert the age prop to num.
- and some of the other modifiers are **lazy modifiers ** we can add to sync after the changes events ex "v-model.lazy".. lly we can use the **trim** to remove the white space at the beginning and the end of the text we entered
- when we re using the checkboxes in the i/p we wanna make sure to set that eq to arr ex interests = [] otherwise all the vals will be grouped and if select one all the val will get selected..
- and also when using the checkbox we ve to use the **value attr** default html attr which is a unique identifier.. so we can know which one we selected and deselected
- lly for the radio button..
- **form validation** - for the i/p field form validation we can set the **blur event** this event occurs when the el looses its focus, for ex - when the user exist the i/p field..its useful we can listen for the event in the form validation if the user left /fills something in the field.

- note : the default behavior of the button tag is submit prop, if we don't want then make the type as button so it won't be having the default behavior of submitting ex <button type="button" >

### Http req (adding backend)

- for the backend, he will be using firebase..
- the arrow fns () => {} // are just like the regular fns but the key diff is that this kw inside of em does refer to the same context as it does outside of them.. means with the normal fn if we use the this it will be scoped..

- **loading when the comp mounts** - we can use the mounted() life cycle hook to load the data when the comp initialize

### Routing MPA and SPA

- the router package `npm i vue-router` will provide the routing functionality and we can import "createRouter" from the vue router
- ex - const router = createRouter({ routes: [], history: createHistory(),) // uses the browser's built in history to keep track of the user history..
- the routes array will ve the objs --ex routes: [{ path: '/teams', component: TeamList}] // lly we can add multiple paths and map to its corresponding comp.. and lly we can use the redirect props ex { path: '/', redirect: '/team'} or we can set the alias: props..
- **<router-link>** is vue's built in link tag just like the reacts Link tag.. doesn't entirely load or refresh the page better alternative for the <a /> tag
- **programmatic navigation** lets say we ve some task (fns in the methods) once the fn is finished we want to nav to some other page (ex login and then to dashboard) and to do that in the methods ex - confirmInput() { this.$router.push('/teams') } // sim to the redirects in the react-next
- **$route** just like the $router props this is also available and we can access/parse the pathname with this prop ex this.$route.path
- or we can access the params from this route prop ex **this.$route.params**.teamId

- **wild card routes** for catching all the routes(not found) ex - { path: '/:notfound(.\*)', component: NotFound} ..

- **using nested routes** - inside the path we can define the children prop and add the child routes we want.. when we re using the <router-view> component it is responsible only for the parent route if there is any child/nested route then we ve to use another <router-view> component in the comp where the child was defined as the child(in the parent comp)
- **named routes** we can assign names to the routes as well, and we can use this name to redirect the route... this will be useful in the bigger app..then we don't ve to update all the comp whenever we ve decided to change the path.

- **using Query params** - or the search params that comes after the ? ex '/teams/id?sort=asc&orderby=id' .. we can add the query params inside the computed prop ex - team() { return { params: { teamId: this.id}, query: { sort: 'asc' } } and we can access the query param by **this.$route.query** ve all the info about our query.

**rendering multiple <router-views> with named router views** we can load multiple components per route and then send them to diff router views.. same like slots we can ve one unnamed router view (default)

- **controlling and the scroll behaviour** ex - if we ve multiple routes nav like then the user clicks the diff route then he ve to scroll all the up.. we can do this in the "main.js" just below the routes: add "scrollBehavior(){}" // this method will be called by the view-router whenever the page changes.
- ex - scrollBehavior(to, from, savedPosition) { return { left: 0 , top: 0 } } // from - the route we re coming from, to - the page we re going to, savedPosition- is only set if re using the back button. // or the last position where the user left.

- **Navigation Guards** - its like the ng route guard to avoid the users to accidently access certain routes.
- this guards are the fns or the methods which called by the vue automatically when the page changes
- we can use **beforeEnter(to, from, next){ }** to the route we can register this in the route level or the comp and in the comp - beforeRouteEnter(to, from, next) { next () }
- lly other guards beforeRouteUpdate(to,from, next) {}
- another global guard **afterEachGuard()** can be used in router.afterEachGuard(function(to, form){ }) can be useful for sending analytic data ex - logs
- **leave guard** guard for b4 leaving the page. ex - accidentally leaving the page while filling the form.. **beforeRouteLeave(to, from, next){}**

- **utilizing route metadata** - on the route fields we can add the extra **meta prop** ex meta: { needsAuth: true }

- **organizing the routes** - we can make the routes related comp and grouped as pages and the comps related grouped as comps. or create a router.js and move all the routing related logic and the files in that..
-

### Animation and Transition.

- will make our ui better experience.. for the basic animation like move the el in the x we can use the .animate{ transform: translateX(-50px) } // its a simple jump ..
- to make a real animation we can use the transition css prop.. ex -transition: transform 0.3s ease-out;

- **@keyframes** ex -@keyframes slide-fade { 0% { transform: translateX(0) scale(1); } 70% {transform: translateX(-120px) scale(1.1); } } .. and to use this in our css .animate { animation: slide-fade 0.3s ease-out forward } // the forward.. the final state will be freeze.
- so far we ve been seeing how to animate when the el is appearing on the screen now how to animate when the el is disappeared on the scr, means the el will be deleted as soon as we select(delete) no way to animate the delete el.
- but we can delay the disappearance of certain el until the animation is done.

- sometimes this css props is simply not enough for the animation.. so we can use the vue's built in animation template
- **<trasition>** we can wrap the animation el with this tag (from vue).. this tag add the el bunch of css props ... it has **-enter-from class**, **-enter-to** class, and **-enter-active** class..
- these are all from the el mounted phase.. enter to right when the animation finishes.. enter-from (first) .. enter-active (at the same time )
- lly for the el unmounted phase .. ex **-leave-from class**, **-leave-active class**, **-leave-to class** ..

- and to make the css .. .v-enter-from { opacity: 0, transform: translateY(-30px) } // lly for the other 2 class and lly for the leave .v-leave-form { } // and the other 2 leaving class.
  we can give the cust name to those vue css classes ex - para-enter-from {} // but now vue won't know these classes so ve to reg em ex - <transition name="para"> .. and in some cases if we wanna use completely diff name ex - if we use the 3rd party lib for the animation then we can use <transition enter-to-class="some-css-name"> we can map the original phase's class name with the cust name.
- the <trasition> el wants one child el, if the child el again has 2 (root) el, then the root els is the direct child of the transition then the animation won't work .. then the work around is remove the transition el and wrap any of the one root (child) el. where we want the animation to apply for

- **Transition b/w multiple els** there are exceptions where we can ve multiple child to the transition el .. ie like any of the 2 child el will be added to the dom at the same time ex - 2 btns(toggle) with the if stmt at a time anyone can be added to dom not the both.
- **using transition event** - there are diff types of events we can listen in the transition ex <transition @before-enter="beforeEnter" @enter='enter' @before-leave="beforeLeave" @after-enter="afterEnter" @after-leave="afterLeave">
  - some other events are @enterCancelled and @leaveCancelled..
- **animation with the js** there are some 3rd party js animations lib **ex greensock** ..like that we can use the js hooks to make the animations..
- if in that case where we use the js for the animation then we can tell vue to not to look for the css prop ex - :css="false"

- **<transition-group>** as opposed to <transition> it will work alon with the group of els. it good for lists and the transition won't render el in the dom but the transition group will render el in the dom.. and the transition prop will apply to all the el in the group

### Vuex (state management)

- this module is for state management which replaces provide and inject to pass the data across our app. and the data in the state are global..(hence the global state management)
- **creating the vuex store** - `npm i --save vuex@next` .. and import the createStore from the vuex and use em like .. const store = createStore({ state({ return { counter: 0}})} );
- and to use the store in the comp we can use **$store** props {{$store.state.counter }}
- **Mutation** we should not mutate the state in this way ex this.$store.state.counter+1 // instead vuex has the approach we can use app/wide central data store/ state store.. mutations - has the logic to update the state
- by triggering all the comps will want to edit the state do it in the same way.. in the main.js when we create our store add the mutations prop (which takes method) ex - mutations: { increment(state) {state.counter++; }} // this is the recent state..
- and then in our comp we can use this in the handler fn ex - methods: { addOne() { this.$store.commit('increment') } } // the commit() is built in available in the store

- we can also add payload to the mutations ex - mutation: { increase(state, payload) { state.counter= state.counter + payload.value; } } // and while using or comiting we can use it along with the mutation fn and the payload ex - methods: { addOne() { this.$store.commit('increment', {value: 10})} // with the payload
- **Getters** a better way of getting data.. we can add the getters prop to the store ex - getters: { finalCounter(state) { return state.counter \* 2 }} // and we can get the val with this getters in any comp. ex - computed: counter() { return this.$store.getters.finalCounter; }} // lly we can ve multiple getters and they rely on each other (state)

- **Async code with Actions** - the key requirement of the mutations is that they ve to be synchronous.. beside Mutations and Getters it also has **Actions** ..
- the comp should triggers Actions which in turn call mutations.. the actions could use the async code .. so the good practice is to put actions in b/w the comp and the mutations..
- just like the getters and mutations we can add the actions prop ex - actions: { increment(context, payload){ context.commit('incMutations', payload) } // this context obj has the commit fn we can commit a mutation..
- and to use the actions in the comp we can **dispatch** the actions (just like the redux) ex - this.$store.dispatch({type:'increase', value:10} ) the val is the payload..
- **context** obj has commit, dispatch (multiple actions inside the action), root getters and root state, etc..

- **mapper helpers** - is a utility function from vuex .. ex computed: { ...mapGetters(['finalCounter']) }};
- **mapActions()** lly we can also use the mapActions() just like the mapGetters() // takes the arr of actions..
- **modules** we can create modules with all the actions, getters, mutations and states.. and inside our store we can merge em. but only the state is local (scoped ) to the modules.. mutations, actions, getters are global..
- **namespace Modules** to avoid the name clash of the modules from the diff stores .. in that scenarios we can use the **namespaced: true** inside our module obj and to access this in our computed - return this.$store.getters['numbers/counter/]; // the module name followed by the action and in the mapGetters and mapActions - ...mapGetters('numbers', ['counter'])
- to organize them all we can create a store dir and make separate namespaces modules dir and put actions, getters, mutations in a separate file..

### Main course project

- coach finder app.. it uses all the things so far we ve learnt such as router, vuex, animations..etc
- this app will ve 2 main features 1. find a coach and 2. send req msg to the coach.
- and in the find coach there are several sub features such as list all available coaches, view coach details, register as a coach, contact a coach..
- and in the req msgs features has couple of sub features.. contact a coach, view incoming reqs.

- and then the data modeling or vuex store layout then derive the design and comps layout..
- if we don't wanna fetch the new data every time then we can simply use the **lastFetch** prop in our vuex module .. this lastFetch will holds the time stamp whenever we fetch the new data for ex - we can make a fn (shouldIUpdate) and make the time stamp for 1min, so if its less than 1min then the lastfetch will be cached..
- take a look at the weird syntax for the router animations <transition> tag.. in the code.

### Vue Authentication

- give the user to access the login and logout.
- just like the nest js auth (refer the notes), but here for the spa the server will send the token to the client and the client will send back the token and only the server can validate the token.
- as we know to store the item in the local browser the built in js fn ex - localStorage.setItem('token', responseData.idToken) ... we can also call the getItems()// lly for the logout .. logout(context) { localStorage.remove('token'), localStorage.remove('userId'); }
- and to logout the user when the token expires -- set the time out fn and inside the cb call the logout and and clears the timeout when the user logout

- **async components** vue has a fn we can use to load the components asynchronously.. import it from vue and then call const BaseDialog = defineAsyncComponent(() => import('./components/baseDialog)) // now vue will exec this fn only when its needed
- just we can put all the components that are not loaded or only needed occasionally, this will comes in handy

### the composition API

- this is one of the key features of the vue 2 to vue 3 .. the composition api is the diff way of building the comps(only the js code changes) slightly the way we used to be.
- so far we ve been using the **Options Api** lets see how we can move from the options api to the composition api.

- so far we ve used the configured obj in our createApp.. and from the config obj we ve configured the data, methods, computed, etc.. this approach is just fine if we want we can stick with this
- but there are 2 main limitations / issue we may face if we build bigger apps.. 1. code that belongs together logically is split up across multiple options (data, methods, computed).. this becomes annoying in huge app, we ve to make the changes in data, methods, computed, watchers etc..
- 2. re-using the logic across components can be tricky or cumbersome.

- **composition api** - in the composition api we use the setUp() method we added to the comp config obj, with this setup() we will manage our data, methods, and computed .. then we can expose with the templates for the interaction
- in a simple term we re gon merge the data, methods, and computed, watchers into this setUp() method..
- **replacing data with refs** - here this ref is diff its a fn we can import from the vue (earlier the $ref is used to refer the dom) but now its bit more.. it refer to **value** which we can then used in the template.. the ref creates a reactive val..then the vue will watch for the changes of the val and updates to the dom
- ex - to create a ref export default setUp() { const uName = ref('max'); return { userName: uname } // where the userName is the what we used in the interpolation {{userName}} // to get the val from the uName .. we can use the val prop ex userName: uName.value..or if its (ref) like an obj (ex user) then we can get the vals like userName: user.value.name;
- there is also another syntax for setUp just we don't ve to define the setUp and return anything instead we can use the **<script setup>** tag and our code looks like <script setup> import {ref} from 'vue'; const uName= ref('max'); </script>

- **reactive** fn.. if we re working with the objs instead of using the ref we can use the reactive fn .. the reactive is bit like ref but its explicitly made for the objs. the ref() can works with any kind of types obj, num, strs.. but the reactive works only with the objs.
- ex - const user = reactive({ name: 'max', age: 32 }); and to access the val we now no need to use the value prop we can directly access the fields ex - user.name='xam', user.age = 20; // since there is no wrapping obj with the val prop (the proxy)
- if we use both ref and the reactive and then console it we can see the ref is value obj and the reactive is the proxy obj (that wraps around our obj) ..
- there are some helpers we can use **isReactive** and **isRef** to find whether something is reactive or ref..
- **toRefs** we give the obj and this fn will turn that obj's prop val into refs

- **Replacing methods with the regular fns** - in the setUp() we can define a fn and expose that fn to the template

- **Replacing computed props with the computed fns** - just like the option api how we use the computed props we can import the computed fn and then set up the fn inside thd computed fn .. here this computed() takes a fn ex- const uName = computed(function(){return firstName.value + lastName.value}) .. these fields are the ref() obj so we ve to use the value to access// the computed props are "readonly" refs .. setting the val is not allowed ex - nName.value ='new'; // is not allowed.
- **2 way binding with the composition Api** - with the "v-model" which can also accepts refs and the reactive vals, ex v-model= firstName; // vue will automatically detects it is refs and it updates the val props whenever we type into the fields so we don't need to bind the val ex firstName.value..

- **working with the watcher** is useful with the http or some data changes.. just like the ref, computed now the watch is also fn and is part of the composition api..
- the watch() takes 2 args 1. deps..telling when to exec the watch 2. cb the actual fn to exec - ex - watch(uAge, function(){}) // we can define more deps by defining the [] as the first arg and def all our deps inside the array.

- **Template Refs** - the el in our template..earlier if we ve a ref attr to the i/p el then we can read / assign a val to the ref using this kw ex fn setLastName() { lastName.value = this.$ref.lastNameInput.value; } but now "this" kw won't work inside the setUp().. but the workaround is to def the refs inside the setUp()..
- ex - setUp(){ const lastNameInput = ref(null); } // then now we can use the fn setLastName(){ lastName.value = lastNameInput.value.value; }//..the 1st value is the ref() and the 2nd val is the pointer to the i/p el

- **mix and use** we can also use option api in one component and composition api in the other comp .. just mix and use..its completely fine..
- the setUp() method accepts 2 params 1. props (whatever props we pass in the comp) .. the props obj are reactive by default ex - setUp(props) { const uName = computed(function{ return props.firstName + props.lastName; }); ..
  - the second param is the context .. and this context has 3 props (objs we can console log and see) 1. attrs,(attrs are any fall thru props, that we didn't define in the prop) ex class if we didn't define in the props it will fall thru into the attrs 2. **emit** is a fn we can call to emit a cust event.. but here to emit instead of this.$emit we ve to use.. context.emit('cust-event')
  - 3. **slots** will give the access to any slots data we might ve in the comp.

-**Provide and inject** - in the composition api just like the other methods the provide is also a fn we can import from the vue ex - provide('userAge', uAge) ..// 1st arg key (of any str) 2nd actual val
and to inject .. again we can use the inject() from the vue.. ex - const age = inject('userAge') //
note : if we wanna change the injected val then we ve to change only in the place where we provide em .. not in the injected..

- in the summary the diff between the option api and the composition api is that the data() is changed with the **refs() and reactive()**

  - and the methods is changed with the function() and the computed: {} is changed with the computed fn. and the watch:{} is changed with the watch(deps, (oldval, newval) => {})
  - and provide and inject is replaced with the provide and inject functions

- **Lifecycle hooks** in optional api we ve beforeCreated() and created().. but now these hooks are **replaced** with the setUp() method instead.
- and the other hooks such as beforeMount(), mounted() are replaced with the onBeforeMount(), onMount() etc ... so totally in the composition api we ve **6 life cycle hooks**
- and to use em in the setUp() ex - onBeforeMount(function(){console.log('onBeforeMount'))..

- to watch only one field in the props ex -user we don't ve option we only can be able to watch the entire props of the obj ex - watch (props, fn(){}).. for watching any particular prop/ field then we can destruct the field/ obj ex - const {user} = toRefs(props); and now we can use the user as the deps inside the watch()

- **Routing, params and composition Api** with the optional api we used to ge the route params by this.$route .. but now with the composition api we now ve no access to the this kw so.. there are 2 ways we can get the params 1. we can set the params:true in the router path:[] and then inside the setUp(props) receive the params as the props
- **useRoute()** to get the route objs .. we can console.log and see its a proxy obj aka reactive obj.. now we can use like const route = useRoute(); route.params..

- **using vuex with the composition api** - sim to the route with the composition api, we can use the **useStore** for the composition api.
- **useStore** composable or hook which is meant to be exec inside of the setUp() and to use this .. setUp() { const store = useStore(); const counter = computed(function(){ return store.getters.counter;})

### Mixins and custom composition fns

- is the fn or the idea of reusing the code in the options and composition api .. lets see there are 2 options available for us 1. Mixins (optional Api) and 2. custom composition fns (composition api)..
  -lets see what are the things we can often reuse.. we can re-use the **html styling**, logic and events .. and combining these 2 we had the concept of component.. what if when we ve few comps that ve/shares sim logic ex userlist and the project list comps both ve search(), data, watcher ..therefore in options api we ve a concept called **mixins**
- **Mixins** will allow us to share the computed(), methods, data, watchers, lifecycle hooks and which ever configs we need across the comps..
- we can create a dir named mixins(name is optional) and then create a file ex - alert.js and put all the shared logic inside this file .. and to use this mixins in our comp just import and export default { mixins: [alertMixin], }

- when we ve the mixins like the above in the options api, this mixin will merge with the data props in our config of the comp..

  - **Global mixins** - this will reflect in all the components not just the one we explicitly added .. and we can import em in the main.js file ex import loggerMixins from './mixins/logger.js'; app.mixin(loggerMixins);

- the mixins ve some drawbacks, for ex - when working in a bigger project, we don't know where some of the val coming from ..
- that's why we ve cust composition fns when working with the composition api..

- **cust hooks and composables** instead of mixins we can make our cust hooks for the re usability of the code .. the naming of this dirs can be **composables** or **hooks**
- and hooks and composables are better and versatile than the mixins..and with this we can also avoid the major drawbacks that the mixins have ..
- even if we ve bunch of hooks and composables we can easily find where the data coming from .. which is a major advantage of using the hooks over mixins in bigger apps..
- as we know the props are reactive (in the setUp() method) but the properties of the props are not reactive.. and if we want to ve the properties of the props to be reactive then setUp(props) { const projects = computed(function() { return props.user ? props.users.projects : []; }) // now the compute() will watch out for the properties of the props

### vue 2 to vue 3 (optional)

- **teleport and fragment** - as we know we can use the <teleport to="body"> element to map to the css prop.. when we want to teleport some el in the particular place in the dom..
- **fragment** - with the vue 3 we can ve multiple el inside of our comp's template (which was not possible in the vue 2) .. this option is just like the react fragments </>

# Nuxt js

- nuxt build on top of the vue js and make the dev of vue very easier.. it allows the creation of universal vue apps.. (SSR)
- some of the features are config via file and folder structure..
- nuxt creates and optionally renders the vue app.. this page(index.html) can be render on the server.. so its(nuxt) is not a server side framework.. it just runs on the svr and prerender our app. nuxt offers optimization..
- to create a nuxt app `npm i -g create-nuxt-app` and then now create the nuxt app `create-nuxt-app my-first-app`
- **pages** dir the files inside the pages dir ve to be vue comps and then be interpreted as routes/urls we can visit.. if we run our app iside the page dir (ex - index.js) we can see the source code in the console .. means rendering from the server side
- what we can build with nuxt ? -- 1. universal App 2. SPA 3. Static App..
- 1. universal App - first view is rendered dynamically on the svr .. after the first load the app will turns into a spa. and its great for **SEO**
- 2. spa - app starts after first load .. app stays spa.. like a normal vue app but simplified development..
- 3. static app - pre-render views are loaded .. after the 1st load, app turns into spa.. great for SEO.

- **dynamic routes** with the id.. there are couple of ways we can create the dynamic routes 1. /user/\_id.vue now this page will be dynamic
- and to access the dynamic params .. $route.params.id //
- and the 2nd way of creating dynamic route is to create a \_id dir ex - /users/\_id/index.vue

- **adding links and navigation around** - just like the <router-link> el.. in nuxt we can add <nuxt-link to="/"> el
- **validating route** parameters .. we can ad the validate method to the config in the route page .. ex - export default { validate(data) { console.log (data) } } .. this data has objs of params, query, and stores ..
- this validate() is a special method that nuxt exec b4 rendering the route.. now we can make the.. validate(data) { return data.params.id == 1 } // now this page will load only if the id is 1..
- **nested routes** ex /users/users.vue and inside we can use the <nuxt-child/> will marks the place of where the diff sec of the nested route will be loaded..

### layouts, comps and pages

- the layout is the main wrapping of the pages.. and the pages can ve one or more child pages(nested route) and one or more comps..
- to add the layout to the pages just add the **layout prop** (provided by the nuxt) in the config.. ex - export default { layout: 'users } now this page will look for the user layout and loads it.
- there are some reserved layouts in the nuxt **default.vue** and **error.vue** these layouts are reserved..

### project

- - **Handling data & vuex** - for load and control data with ease.. to render the data on the svr side we can use the **asyncData()** in the config obj..
- - this asyncData() will be called only on the page and this method will be only exec on the svr.. if there is any fn we defined it will wait for the task to finish b4 it reach the client.. hence we will get the complete pre render data..
- - and the **this** kw will not work inside the async data method since this method will runs b4 the component created() lifecycle ..
- - there are 2 ways to use this asyncData() one with the promisse ex - asyncData() { return new Promise(setTimeout() {},1000;);
- - or the another way with the callback ex asyncData(context, callback) { setTimeout(callback() => {callback(null, { Posts: [{}]}) }, 1000); .. (personal note : it should be loadData() equivalent in the svelte)
- inside the cb the 1st arg we can use error ex cb(new Error(), {})
- **using promise in asyncData()** - asyncData(context) { return new Promise(resolve, reject) => { setTimeOut(() => {resolve( {post:[{}]}} , 1000) }).then(data => { return data }) .. // most of the time we don't use the promise based approach since we use the 3rd party lib like axios which does the promise for us..

### Adding Vue js store

- the vuex is already built in nuxt .. and there aer 2 modes we can use 1. classic mode(one store and one module) and define the actions, mutations in single file 2. module mode( create multiple js file in the store) every file becomes the namespaced module.
- **vuex fetch, and nuxt server init** instead of the asyncData() we can use the **fetch()** it can either runs on the server side or the client and behaves exactly same as the asyncData() .. but this fetch() is not the ideal method.. instead we can use the **nuxtInit()** in the store
- we ve to add that in the action and this method will be dispatch by the nuxt ex - actions: { nuxtServerInit(vuexContext, context) { return new Promise(resolve, reject) => { setTimeOut(() => {resolve( {post:[{}]}}}, // the payload is the context... the original code is slightly diff, we use the vuex.context.comit('setPost') .. to commit the mutations..

### connecting the app to the Backend

- just like the vue he is using firebase for the db and use axios to fetch the data..
- and then inside the methods he is just making the post, put, fetch methods from the db.
- **sync the backend with the vuex store** the idea here is to in sync with the data (that we re fetching from the db to the vuex store)

### Nuxt config, plugins & modules

- thru out this section it was only about the nuxt config file..
- **plugins** - is the feature that will allow us to load certain functionality or code b4 our app is fully rendered/ mounted..so we don't ve to wait for the main.js.
- **modules** we can add the nuxt modules created by others in our nuxt app ... refer the nuxt module github - it has **pwa** module which will automatically turns our nuxt app into pwa by gen **service workers** which caches our automatically generated o/p files..
  - and some of the other modules we can see in that repo is axios, auth, router, proxy, oauth, dotenv etc.. if we add this any of these modules then it will be injected in our app and we can access by context.app.$axios //

### middleware and authentication

- the mw is a fn that executes b4 the page is loaded.. we can attach the mw to all our route or per layout basis.
- just create a mw fn ex - export default { function(context) {} } and to use this in our page in the config props just add the mw prop ex - middleware: 'log' // the log is the file name of the mw. . and if we add this to (ex - layouts) the mw will execs multiple times if we added on diff levels
- **persisting login token across pages** - inside the stores just save the token in the localStorage and then in the pages (actions) just get the token from the localStorage.. but storing the token in the local storage won't work..
- since the mw will runs in both the server and the client and the server doesn't ve the localStorage..so we can make a condn if the process is client then store the token otherwise don't .. but still this is not the ideal .. we should be able to run the mw on the server side too..
- **impl cookies** - since we don't ve the option to store the token in the server but still we can access the token in the server with the help of cookies' ex - `npm i --save js-coodie`
- now we can send the cookie along with the http to the server.. this package will ve the jwt and we cna send it in the header of our http req.

- **adding svr mw** the server mw (in the nuxt.config file) is the collection of node and express compatible mws.. that will be exec prior to the nuxt rendering process.
- for the diff kind of app we can config the mode in the nuxt.config file.. for ex- if we want a spa we can set the mode: 'spa' and run npm build in the dist dir we will ve our app and can deploy it.. lly for the universal, and static app
- if we re building spa we don't ve to use nuxtServerInit() instead use the lifecyle hook created() and mounted()..
- for the static app just set the mode to the 'universal' and run `npm run generate` will pre gen all the static files/ pages and if we go to the dist dir and `http-server -p 8082` we can see all the pages pre loaded static pages
-
