React Router 6.4 Changes: Data Fetching and Submission.

_KeyWords:_
import {
. RouterProvider, createBrowserRouter, createRoutesFromElements, Route,
. Form, useNavigation, useNavigate, redirect, useActionData,
. useLoaderData, useRouteError,
. Suspense, Await, defer,
. useFetcher }
from "react-router-dom";

_Check app JSX routes and wraps_

Main index page ReactDOM
. . import ReactDOM from 'react-dom/client'
. . ReactDOM.createRoot(document.getElementById('root')).render(
. . <React.StrictMode>
. . . . <App />
. . </React.StrictMode>
)

_import { RouterProvider, createBrowserRouter, createRoutesFromElements, Route } from "react-router-dom";_

Router createBrowserRouter
. . createRoutesFromElements(
. . . . Route path="/" element={<RootLayout/>} errorElement={<ErrorPage />}>
. . . . . . Route index element={<WelcomePage/>} _index elements do not have paths_
. . . . . . Route path="/blog" element={<BlogLayout/>}
. . . . . . . .Route index element={<BlogPostsPage /> loader={blogPostsLoader}} _Loader for api.getPosts_ Nested Root
. . . . . . . .Route path=":id" element={<PostDetailPage />} loader={blogPostLoader} _Loader for ap.getPost_ Nested Root
. . . . . . Route path="/blog/new element={<NewPostPage /> action={newPostAction}} _action for submission, request object_
. . . . Route
. );
);

function App() { return <RouterProvider router={router} />; }

Wrap routes in _CreateBrowserRouter_, and _createRoutesFromElements_

_Index_ - index routes do not have paths, take parent element
. Path - nested paths continue path from parent element.

_useLoader();_
. Can get rid of useState functions, load data from the api page functions
. App.jsx loader={blogPostsLoader} - import BlogPostsPage, { loader as blogPostsLoader } from "./pages/BlogPosts";

_useRouteError();_
. load error messages and handling from the api page functions
. create separate ErrorPage.jsx and link on root element within Router.
. <Route path="/" element={<RootLayout />} errorElement={<ErrorPage />}>
. -import { useRouterError } from 'react-router-dom

action - substitute onSubmit logic

_<Form> component_
Wrap the form element in Form instead of form, import from react-router-dom
Can get rid of onSubmit
-import { Form } from 'react-router-dom'

_useNavigate_, navigate('/welcome') and redirect('/welcome') can both be used in same component. Use redirect within an action function.

_useNavigation_ - hook that gives us navigation information.
. navigation.state - idle, loading, or submitting. What state is the action in?
. can disable form buttons whilst navigation.state === 'submitting'

_useActionData_ - stay on form page, and get access to returned error data which we can use in the UI.
. {data && data.status (error status, has error) && <p>{data.message}</p>}
. return data, and use data in an action.
. -import NewPostPage, { action as newPostAction } from "./pages/NewPost";

request object from react-router-dom takes information from a form but has not been sent away to server yet.

---

- - - Advanced - - -

import { _Suspense_ } from 'react'
import { _defer, Await_ } from 'react-router-dom'
. can be used in the export async function loader() action.
. DeferredBlogPost.jsx return defer({ posts: getSlowPosts() }); _returns promise stored in posts (result of getSlowPosts)_
. use in conjunction with <Await resolve={loaderData.posts} errorElement={<p>Error loading blog posts.</p>}></Await> within component JSX.
. loaderData refers to information gleaned from loader function action object { posts: getSlowPosts }
. errorElement can be added to Await.
. Await can be wrapped in <Suspense></Suspense>. Add a fallback property to show an internim fallback element to show whilst awaiting/loading data.
. <Suspense fallback={<p>Loading...</p>}>
. Within Await tags you must show the data which will be loaded after the promise is fulfilled
. <Await>
. {(loadedPosts) => <Posts blogPosts={loadedPosts}}
. </Await>
. . . Why use this setup? Instead of waiting for 2seconds~ to resolve a fetch request promise before loading the page, we can go
. . . to the page with general layout and merely wait for the specific element rather than the entire page, and whilst the request
. . . is being resolved, we can create a fallback element e.g. <p>Loading...</p>.
.
. . . We could have a admin page with a loader function requesting multiple pieces of information for multiple elements e.g.
. . . export async function loader() {
. . . . . return defer ({ posts: getSlowPosts(), list: getSlowList(), records: getSlowRecords() })
. . . instead of waiting for every single fetch request to complete before loading the page, we may want to load the page first,
. . . and then showing each element requiring fetched data as the requests complete in real-time after loading the page.
.
. . . Add _await_ to tell react to complete the promise before loading the page for certain elements.
. . . return defer ({ posts: getSlowPosts(), list: _await_ getSlowList(), records: getSlowRecords() }) _react will wait for list request to complete before
. . . loading the page_.

_useFetcher_
. use fetcher.Form instead of just <Form> to stop redirect logic when form is submitted.
. use fetcher.submit({email: enteredEmail}, {method: 'post', action: '/newsletter'}) to submit information manually.
