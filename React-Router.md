# React Router 6 (Bob Ziroll) ⚛️

## Updates

With an update made to v 6.4.5 of React Router, Mirage JS is causing some errors when using the `redirect` function from React Router. **tl;dr:** a library that Mirage JS is using under the hood (`pretender`) uses a polyfill for `fetch`, and that polyfill does not adhere to the `fetch` specifications, in that it does not return a response with a `body` property. In React Router 6.4.5, they included a new check to make sure that a response has a `body` property, which makes any `redirect` call in React Router fail.

You can work around this issue by modifying the response that comes back when calling `redirect`, like so:

Instead of

```js
return redirect("new-url");
```

You can capture the response in a variable and add a body property manually:

```js
const response = redirect("new-url)
response.body = true  // It's silly, but it works
return response
```

## Info

React Router is the most popular routing library for React applications and one of the most downloaded React support libraries ever.

**Topics**

- What are SPAs?
- Basic router setup
- Route
- Link
- Route parameters
- Nested routes and Outlet
- Layout and Index routes
- Relative paths
- NavLink
- Outlet context
- Search parameters
- Link state
- 404 page / Splat routes
- Loaders
- Actions
- Form & form data
- defer()
- Await
- Suspense
- Error handling & errorElement
- useRouteError
- useNavigate
- useNavigation
- useLocation
- useLoaderData
- useActionData
- Protected Routes
- Deploying with Netlify

### Intro -- React router DOM

- As we know the react router is for enabling the client side routing
- in the index.html we can't specify anything other than the .js extn but if we wana use .jsx then we can use the vite or some fws, the vite can compile any jsx (react content)

1. " BrowserRouter" - is a context provider, we need to cover/wrap our app with the browser router, so that we will ve the whole suite that comes with react router dom to control our app
2. Routes - route is a part of url that specifies where we re, where as the routes can be nested
3. "Route" - comp has 2 props, path and element {} -- which react elem we wana render
4. "Link" - will nav through our app in much easier way, and also it will avoid loading the paging so that state changes will not effected

"Mirage js svr" is a way to create a mock svr that will intercept any api req that we made and returns the data from the mock svr

# Nested routes

- "Route params" - in our case we don't wana create separate route for each of the vans instead we can use the route params --> api/van/:id -> this colon will change the id into the var(its saying there is some thing gon be there followed by the van/) so we can modify our routes easily.
- "Outlet" comp from react-router-dom, refer docs for this comp, but this pulls the content of our page, or if we wana show any other part of our page
- we use the outlet comp anytime we ve parent routes that's wrapping the child routes, and it renders the child route elem props

- Layout routes -- is the parent route and they shared ui routes ex nav or header. it will use an outlet comp
- "useParams()" - hook will allow us to grab any of the params that we ve in our url
- Nesting Route - the Route comp is either self closing or the non self closing comp, this route comp will allow us to nest our routes.. ex path="/host/income/" or /host/reviews.

# "Relative Paths" (for the parent, child route path)

- if we dont use the / in the child routes then the path will be relative to the parent route

" Index Routes" -- lets ve scenario when the layout comp and the child comp ve route path.

- so by using the index routes we can display the child comp just by using the prop called index(instead of the path) --> <Route index element={<dashboard/>}/>
- means the path is whatever the parent path and then render the layout and inside stick in the elem.
- it give us the chance to render an elem inside the parent's <Outlet/> at the same path as the parent route

- " To nest or not to nest" -- when there is no shared scenario then there is no need to do nested set up
- "NavLink" -- is same like the link except its fn and the style prop and the class name prop both takes the fn as its val. ex . <NavLink to="/home" classname={({isActive})=> isActive ? "my-link" : null}> (the obj may be either isAcctive or isLoading)or inplace of classname it can also takes inline style prop.
- the navlink comp has another prop "end"(tells the react to end the matching here) this will avoid the nested active routes

  # Relative links

  - its same like the relative routes, where the child routes are relative to the parent link.
  - the Link comp ve a prop called retative, <Link to="/" relative="path"> --> means one level back to the routing structure in our path structure not in our routing hierarchy

"usetOutletContex" - this hook is like the context provider --> <Outlet context={van}> -- whatever val that we passed inside the context is receivable in the other comp . and in the receiving comp use the hook to destructure the obj/val --> const {van} = useOutletContext()

- now we can ve the access to the van obj/val

# Search params (query params)

- the query params represent a change in ui
- the common task with em are sorting, filtering and pagination
- used as a single source of truth for certain app state.
- they re kvp in the url and start with ? --> /vans?type=rugged
- we can ve multiple query parms in the url, just by separating em with the &

"useSearchParams" --> is like the useState(), but the diff is state live inside the mem of comp, where as the searchparams just reads the url

- const [searchParmas, setSearchParmas] = useSearchParams()
- this searchPars obj ve diff method such as .get("type") --> this key will returns the val

"using links to add the search params" -- we can also use the links to just specfy the query ex- <Link to="?type=jedi">Link</Link> if we wana remove/the search param in our url then simply use the empth str in the link or . which will return to the current path -- <Link to=".">Link</Link>

- but this method of clearing has the caveat.
- so instead we can use the setSearchParams() --> <button onclick={()=> setSearchParams("type=jedi")}> (when using this fn no need to add the ?)or just pass an obj or empty obj for the clear.
- but there is a caveat in this setter fn, if we ve more query params and each one is in diff comp then method wont work its only good for the single query param that we care about not in complex of multiple nested query params cause it will replace the existing parms.
- in order to merge the query parms or to ve a multiple parms in the url
- "Merge search params" just like the useStat the setSearchparms() it can take a replacement val for the state or the callback fn, and the callback fn will receive the prev state and we can use that to make changes to it
- ex - function genparam(key, val){
  setSearchParams(prevParams => {
  if (val ===null) {
  prevParams.delete(key)
  }else {
  prevParams.set(key,val)
  return prevParams
  }
  })}

- and inside our link we can use this fn <Link to={genParam("type", "jedi")}">Link</Link>

"Link state" - lets say if we re in the filtering where we ve like bunch of filter but we wana clear anyone and not all em at a time, it won't give good user experience,

- as we know the browser has ability to save some kinda state b/w one link and the one url and the next
- in our scenario we can use the state inside the link by using the prop <Link to={van.id} state={{searchParam: searchParam.toString()}}> just by passing the state along with the link. (now we can maintain the state of our link even when we refresh the page)
- now we can receeive this state that comes along with the link by using useLocation() hook
- "useLocation()" this hook has diff pros such as pathname, search, hash, state, key

# 404 page

- there is something called splat route or catch all route that has everything to do with that we need to put in our path.
- inside our parent route comp just put the <Route to="\*" element={<h1>page not found <h1/>}>
- so any routes that has not defined in our comp no will directly go to this catch all/splat route

# Happy path or the sad path

- "Data Layer Apis" is introduced in the react route 6.4.
- the sad path for the error handling, form validation, loading state etc.
- The react router has introduced us the way that we can load our data b4 it ever transition us to the route
- "Loaders" --> when the comp loads, during the delay the fetch req will fetch the data.
- technically export the loader() from the page that fetches the data
- and then passes the loader prop to the route that renders the page
- then use the useLoaderData() hook in the comp to get the data

Note : the <BrowserRouter> and the few other comp wont accept the data api, so we ve to use the some other fn such as "createBrowserRouter()". "createRoutesFromElements()"

- const router = createBrowserRouter(createRoutesFromElements(<Routes to="" element={van.id}/>)) --> this will convert our route elem into an obj and pass that obj in to the createBrowserRoute
- now we can wrap our app with the <RouterProvider router={router}> (has router prop, and it takes our router var as the var ) comp instead of the <BrowserRouter>

- we Router comp has the loader prop wheere we can pass the cust loader(),
- to make this loader work we can use the useLoaderData() hook in the comp to get the data (whatever we re returning in the cust loader() we can pull the date by using this hook).
- its diff from useEffect() coz of the fact that we re not fetching the data from inside of our comp, react router can delay the rendering of the comp until the loader has finish its task.
- the loader() runs b4 the comp loads and the route changes happens
- the Router comp has the prop errorElem, if we put this prop in the parent router then it can be able to catch the errror anywhere in the child hierarchy

- "useRouteError" is the hook to catch the error that we thrown and then displays it.

# Protected Route

- its a paradigm/pattern that we used to protect the routes.
- - the purpose is to allow only the logged user to access the data .

- in react router when we ve nested routes that are all displaying the pages same time, and sometimes its possible that we may end up with the "Request waterfall". --> everything waits for the prev req to finish inorder for the next one to start.
- "<Navigate>" comp is just like link but instead to clicking this comp will take us to the specified url as soon as the comp renders, ex login form as soon as we login the form it will navigate to the corresponding comp.

- "Protected Routes with Loaders" --> when we re using loaders in our comps all of the loaders will run parallel and when the final loader finishes the entire completed page will be displayed
- To create a protected routes when we re using the loaders we ve to use the "redirect()" and this fn will happen inside of the loader()
- but the current downside is it needs to happen in every protected routes as of react router 6.4
- but the newer updates has the middleware inside the router that will allow us to run the fn b4 any of those loaders happen --> ex: if (!isLoggedIn) { redirect("/login?message=u must login first") }
  - we can pull this query str and display this msg to the user,
  - "Consume msg from the search param" on the login page- we can use the useSearchParam() and .get() in the searchParams to get the msg or
  - we can include the loader(or the native web interfaces) -->
  - ex: export function loginLoder({request}){
    console.log(new URL(request.url).searchParams.get("message"))} // or we can simply return this
  - as we know the req obj and the URL contr are the native web as opposed to the react's lib.
  - now to consume this just use the useLoaderData() hook in our comp

# Actions (form)

- the action attr in the react form which will tell us which endpts to send the form data to, and then the endpt on the svr will take the data and process it or do something with it.
- "useNavigate()" we ve seen the <Navigate> comp, under the hood this comp will call this hook to do the navigation,
- so we can use this hook to nav once the user login or any other purpose.
- the diff b/w the redirect() and this useNavigate() hook is, one thing to keep in mind any hooks that we ve in react they must run in the top level of our comp code.
- however when we re working with the fns like loaders or action fns or anything we can't able to use the hooks coz those fns are not a comp and not rendering any ui.
- so the useNav() hook works inside the comp and the redirect() will works when working with the fns.
- and finally when we use this useNav() hook often times we ve to use it inside the useEffect()

## form (<Form>)

- the react router form comp, natively forms know how to handle the state and how to submit(by the action attr) to the backend.
- the action prop in the <Router>
- "action fn params" --> the param that we pass in the action fn (may be search param or the url)
- "action fn request" --> the obj that we pass in the action rn ex: const action({request}){const data = request.formData()}; one of the pieces of the req obj is the form data that coming from the form, and we can use the .get() on this formData() to gather the infos.
- with this cust action() we can replace our handleChange() in the form.

- History Stack (First in last out)
  - we can think of it as a the browser maintains the array of the locations that we visited.
  - and when we transition from one route to the other it will automatically pushes the history stack for us.
  - "Form replace" the Form comp has the replace attr on the submission of the form(nav event) we can use this attr to replace the current route in the history stack with the route or upcoming entry in the history stack. ex: <Form replace>
- "UseActionData" this hook will helps us to grab the data from our action fn. just like the loader fn
  

"Actioin Error Handling"
    - we can remove all the error handling and the error handling state inside our comp instead we can use the try catch block inside the action().
  
"UseNavigation()" 
    - is not to be confused confused with the useNavigate()
    - this hook will give us the obj(are state, location, formMethod, formAction etc) which will provide the info about the current status of navigation in our app
    - ex: if react-router is in the process of loading, or submitting the form etc.
    - now that we ve access the state we can use this in our button to make it disable while logging ex: <button disabled={navigation.state==="submitting"}> {navigation.state==="submitting" ? "Logging in" : "log in"}

"Get Previous Route path name"
    - we can pull in the req obj and create a obj and access its prop. (in the native web)
    - ex : const async function action({request}){
                    const url = new URL(request.url)
                    console.log(url.pathname)}
    - so with this the user can be redirects to the url.pathname
    - now we can use this pathname and redirects() to the protected path
    - question ? how can we pass infos using the url --> we can use query/search params, so we can also grab the search param from the url obj
    - ex: const pathname = new URL(request.url).searchParams.get("redirectTo") 
              - return redirect(pathname)


# Deffering Data  (defer from react-router)

- the user experience while clicking the link, which will transition us to the route immediately and give us the nice api that will allow us to render some kinda loading state. all of whihc we will be able to do w/o any of the react state.
- "Promises and Deffered Data" --> as we know the async() always returns the promise.
- the "defer()" is allow to wait or defer the loading of our data to the later pt, therefore that will not block the ui from loading.
- the defer({}) accepts obj, the key of the obj is the data that will later gets resolved whenever the promise gets resolved.
- so when we re using this fn inside the async() we don't even need to use the await kw
- ex: const async function loader(){
                return defer({weather: weatherPromise})} 

# Await component  (refer the code )

    - <Await> the await from react-router allow us to do few things 
    - there re 2 attr this comp has 
    - 1. resolve - this resolve prop takes promise 
    - once the await comp successfully resolves this promise, it will call the render prop child fn. or it expects a child fn
    - ex: <Await resolve={datapromis.van}> {(vans)=>{}}

# React Suspense (not from the router)

    - <Suspense> comp
    - react introduces the ability for mostly the third party libs to suspend the rendering of our react app until something has finished.
    - the benefit of this is we can treat our comps as if they were synchronous
    - suspense allow us wrap our suspended comp (which is <Await> comp)
    - this comp has fallback prop, the idea is, it gives react an elem to render while its waiting for the other comp i-e suspending to finish
    - ex: <Suspense fallback={anyElem}>
                <Await resolve={any}>
            </Suspense>
# cloud Fire Store

- for pull our data from the firestore DB