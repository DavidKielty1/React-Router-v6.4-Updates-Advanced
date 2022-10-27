React Router 6.4 Changes: Data Fetching and Submission.

Main index page ReactDOM
. . import ReactDOM from 'react-dom/client'
. . ReactDOM.createRoot(document.getElementById('root')).render(
. . <React.StrictMode>
. . . . <App />
. . </React.StrictMode>
)
import { RouterProvider, createBrowserRouter, createRoutesFromElements, Route } from "react-router-dom";

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

Wrap routes in CreateBrowserRouter, and createRoutesFromElements
. . createBrowserRouter,
. . createRoutesFromElements,
. . -import { createBrowserRouter } from 'react-router-dom'

Index - index routes do not have paths, take parent element
. . Path - nested paths continue path from parent element.

useLoader();
. . Can get rid of useState functions, load data from the api page functions
. . App.jsx loader={blogPostsLoader} - import BlogPostsPage, { loader as blogPostsLoader } from "./pages/BlogPosts";

useRouteError();
. . load error messages and handling from the api page functions
. . create separate ErrorPage.jsx and link on root element within Router.
. . <Route path="/" element={<RootLayout />} errorElement={<ErrorPage />}>
. . -import { useRouterError } from 'react-router-dom

action - substitute onSubmit logic

<Form> component
    Wrap the form element in Form instead of form, import from react-router-dom
    Can get rid of onSubmit 
    -import { Form } from 'react-router-dom'

useNavigate, navigate('/welcome') and redirect('/welcome') can both be used in same component. Use redirect within an action function.

useNavigation - hook that gives us navigation information.
. . nevigation.state - idle, loading, or submitting. What state is the action in?
. . can disable form buttons whilst navigation.state === 'submitting'

useActionData - stay on form page, and get access to returned error data which we can use in the UI.
. . {data && data.status (error status, has error) && <p>{data.message}</p>}
. . return data, and use data in an action.
. . -import NewPostPage, { action as newPostAction } from "./pages/NewPost";

request object from react-router-dom takes information from a form but has not been sent away to server yet.
