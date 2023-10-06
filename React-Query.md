# react-query (Tanstack)

- this react-query or tan stack query is a powerful async state management library for ts/js, solid, vue, svelte etc.
- it handles the caching really well, it handles the prefetching, parallel query, SSR, loadmore/infinite scroll, updates etc. (refer the documentation)
- there are 2 main things we can do with the react query, 1. query and then the 2. mutation.
- and the corresponding hooks are useQuery and useMutations from this library.
- the useQuery takes 2 args queryKey and the query fn
- the query key -> everything in this query key is cached and stored in the memomry
- and anytime we wanna mutate the query we do wana invalidate any query related to the mutations(since we re changing the data of the query)
- by doing the invalidation we forced that to be marked as stale, (since we already set the stale timer), as soon as it need to be rerendered on the page, it actually showed up and rendered the data for us.

## pagination

- just as the earlier fns, but this time around, the useQuery() takes the queryKey(need to put the page inside the query key), query fn and the keep previous data : true , this will show the previous data while we re loading the new data

In order to run this project you need to start both the client and the API.

### inifinite scrolling

- we ve to use a different hook, useInfiniteQuery(), instead of the useQuery(),
- this hook is same as the useQery(), it takes query key and the query fn but the difference here is it also take the getNextPageParams()
- and now the query fn also takes the pageParams, and this pageparam is whatever gets returned by the getNextpageParam
- similarly we can use the getPreviousPageParam() and destructure the obj that are corresponding to the getPreviousPageParam, such as isFetchingPreviousPage, hasPrevPage, fetchPreviousPage etc just like the getNextpageParam.

## useQueries()

- if we wana run the query for each of our post, then we can think of like mapping the post with the useQuery hook, ex - postQuery.data.map(post => {useQuery()})

- but the problem here is that we can use the hook for the fn, or for loop or the conditional statements. so to get over this

- we can use the useueries(), which will allow us to pass the array of query it will run

### prefetching the data

- lets say whenever we hover over the link we wana actually query the data for that.
- refer the App.jsx
- its like create a mouse event and a handler fn

### initial data and the placeholder data

- are the other optioal props we can set inside the useQuery()
- the diff b/w em is, the placeholders data will be replaced by the whatever our query is, and its essentially always marked as stale
- while the initial data is gon saved and stored inside our cache, and its gon be marked as fresh if we ve som stale timer setup.
-

## dependencies

- `npm i @tanstack/react-query-devtools`
- `npm i @tanstack/react-query`- is a react query library.

## Start Client

- the client is a simple vite app.
- then wrap our app with the react query client provider(which is the instance of the query client).

1. `cd client`
2. `npm run dev`

## Start API

1. `cd api`
2. `npm start`

## Suspense

- this suspense in the next app router
- this update allows us to stream in data, ie fetched on the server and pass it to the client and show it in the real time as the data comes in, in real time to the user.
- in the past to show the loading state <suspense fallback=...> <child data= asyc()>
- in this suspense fallback we use the loading state to show the loading state while the user rendering the child data, when the child's await() is done fetching the data, the fall back is replaced with the child data,
- but now in the child comp

- import the useQuery(from tanstack) and the suspense from the react.
- then make a fn useWaitQuery(props: {wait: number}) { inside we can use the useQuery}
- then the useQuery takes the query key and the Query fn as usual + suspense : true;
- then returns the query data as string.

- then the another fn, fn Mycomponent (props, {wait: number}) {const [data] = useWaitQuery(props); return result: {data}};
- export default fn Mypage(){
  return
  <>
  <suspense fallback= {waiting 5000 ms}}
  <MyComponent wait={5000} />
  <MyComponent wait={1000} />
  <MyComponent wait={4000} /> </>
- in this ex we can see the see the comps resolved and the data is streamed into the dom as the mentioned time ms.
- so we can nicely stream the data with this pattern which was not possible b4.

- The ReactQueryStreamHydration from the tan stack react query next experimental
- along with the query client provider we can wrap our children with this component.
- what this ReactQueryStreamHydration does is responsible for hydrating the query client on the server and then dehydrating the query client on the server
- which essentially means serializable data.
