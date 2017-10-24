

When starting a new project, you need to determine which type of router to use. For browser based projects, there are &lt;BrowserRouter&gt; and &lt;HashRouter&gt; components. The &lt;BrowserRouter&gt; should be used when you have a server that will handle dynamic requests \(knows how to respond to any possible URI\), while the &lt;HashRouter&gt; should be used for static websites \(can only respond to requests for files that it knows about\).

Usually it is preferable to use a &lt;BrowserRouter&gt;, but if your website will be hosted on a server that only serves static files, then &lt;HashRouter&gt; is a good solution.

For our project, we will assume that the website will be backed by a dynamic server, so our router component of choice is the &lt;BrowserRouter&gt;.

History

Each router creates a history object, which it uses to keep track of the current location\[1\] and re-render the website whenever that changes. The other components provided by React Router rely on having that history object available through the context, so they must be rendered inside of the router. A React Router component that does not have a router as one of its ancestors will fail to work. If you are interested in learning more about the history object \(I think that this is important\), you can check out my article A Little Bit of History.

Rendering a &lt;Router&gt;

Router components only expect to receive a single child element. To work within this limitation, it is useful to create an &lt;App&gt; component that renders the rest of your application \(separating your application from the router is also important for server rendering because you can re-use the &lt;App&gt; on the server while switching to a &lt;MemoryRouter&gt;\).

import { BrowserRouter } from 'react-router-dom'

ReactDOM.render\(\(

  &lt;BrowserRouter&gt;

    &lt;App /&gt;

  &lt;/BrowserRouter&gt;

\), document.getElementById\('root'\)\)

Now that we have chosen our router, we can start to render our actual application.

The &lt;App&gt;

Our application is defined within the&lt;App&gt; component. To simplify things, we will split our application into two parts. The &lt;Header&gt; component will contain links to navigate throughout the website. The &lt;Main&gt; component is where the rest of the content will be rendered.

// this component will be rendered by our &lt;\_\_\_Router&gt;

const App = \(\) =&gt; \(

  &lt;div&gt;

    &lt;Header /&gt;

    &lt;Main /&gt;

  &lt;/div&gt;

\)

Note: You can layout your application any way that you would like, but separating routes and navigation makes it easier to show how React Router works for this tutorial.

We will start in the &lt;Main&gt; component, where we will render our routes.

Routes

The &lt;Route&gt; component is the main building block of React Router. Anywhere that you want to only render something if it matches the location’s pathname, you should create a &lt;Route&gt; element.

Path

A &lt;Route&gt; expects a path prop string that describes the type of pathname that the route matches — for example, &lt;Route path='/roster'/&gt; should match a pathname that begins with /roster\[2\]. When the current location’s pathname is matched by the path, the route will render a React element. When the path does not match, the route will not render anything \[3\].

&lt;Route path='/roster'/&gt;

// when the pathname is '/', the path does not match

// when the pathname is '/roster' or '/roster/2', the path matches

// If you only want to match '/roster', then you need to use

// the "exact" prop. The following will match '/roster', but not

// '/roster/2'.

&lt;Route exact path='/roster'/&gt;

// You might find yourself adding the exact prop to most routes.

// In the future \(i.e. v5\), the exact prop will likely be true by

// default. For more information on that, you can check out this 

// GitHub issue:

// https://github.com/ReactTraining/react-router/issues/4958

Note: When it comes to matching routes, React Router only cares about the pathname of a location. That means that given the URL:

http://www.example.com/my-projects/one?extra=false

the only part that React Router attempts to match is /my-projects/one.

Matching paths

The path-to-regexp package is used to determine if a route element’s path prop matches the current location. It compiles the path string into a regular expression, which will be matched against the location’s pathname. Path strings have more advanced formatting options than will be covered here. You can read about them in the path-to-regexp documentation.

When the route’s path matches, a match object with the following properties will be created:

url —the matched part of the current location’s pathname

path — the route’s path

isExact —path === pathname

params — an object containing values from the pathname that were captured by path-to-regexp

You can use this route tester to play around with matching routes to URLs.

Note: Currently, a route’s path must be absolute\[4\].

Creating our routes

&lt;Route&gt;s can be created anywhere inside of the router, but often it makes sense to render them in the same place. You can use the&lt;Switch&gt; component to group &lt;Route&gt;s. The &lt;Switch&gt; will iterate over its children elements \(the routes\) and only render the first one that matches the current pathname.

For this website, the paths that we want to match are:

/ — the homepage

/roster — the team’s roster

/roster/:number — a profile for a player, using the player’s number

/schedule — the team’s schedule of games

In order to match a path in our application, all that we have to do is create a &lt;Route&gt; element with the path prop we want to match.

&lt;Switch&gt;

  &lt;Route exact path='/' component={Home}/&gt;

  {/\* both /roster and /roster/:number begin with /roster \*/}

  &lt;Route path='/roster' component={Roster}/&gt;

  &lt;Route path='/schedule' component={Schedule}/&gt;

&lt;/Switch&gt;

What does the &lt;Route&gt; render?

Routes have three props that can be used to define what should be rendered when the route’s path matches. Only one should be provided to a &lt;Route&gt; element.

component — A React component. When a route with a component prop matches, the route will return a new element whose type is the provided React component \(created using React.createElement\).

render — A function that returns a React element \[5\]. It will be called when the path matches. This is similar to component, but is useful for inline rendering and passing extra props to the element.

children — A function that returns a React element. Unlike the prior two props, this will always be rendered, regardless of whether the route’s path matches the current location.

&lt;Route path='/page' component={Page} /&gt;

const extraProps = { color: 'red' }

&lt;Route path='/page' render={\(props\) =&gt; \(

  &lt;Page {...props} data={extraProps}/&gt;

\)}/&gt;

&lt;Route path='/page' children={\(props\) =&gt; \(

  props.match

    ? &lt;Page {...props}/&gt;

    : &lt;EmptyPage {...props}/&gt;

\)}/&gt;

Typically, either the component or render prop should be used. The children prop can be useful occasionally, but typically it is preferable to render nothing when the path does not match. We do not have any extra props to pass to the components, so each of our &lt;Route&gt;s will use the component prop.

The element rendered by the &lt;Route&gt; will be passed a number of props. These will be the match object, the current location object \[6\], and the history object \(the one created by our router\) \[7\].

&lt;Main&gt;

Now that we have figured our root route structure, we just need to actually render our routes. For this application, we will render our &lt;Switch&gt; and &lt;Route&gt;s inside of our &lt;Main&gt; component, which will place the HTML generated by a matched route inside of a &lt;main&gt; DOM node.

import { Switch, Route } from 'react-router-dom'

const Main = \(\) =&gt; \(

  &lt;main&gt;

    &lt;Switch&gt;

      &lt;Route exact path='/' component={Home}/&gt;

      &lt;Route path='/roster' component={Roster}/&gt;

      &lt;Route path='/schedule' component={Schedule}/&gt;

    &lt;/Switch&gt;

  &lt;/main&gt;

\)

Note: The route for the homepage includes an exact prop. This is used to state that that route should only match when the pathname matches the route’s path exactly.

Nested Routes

The player profile route /roster/:number is not included in the above &lt;Switch&gt;. Instead, it will be rendered by the &lt;Roster&gt; component, which is rendered whenever the pathname begins with /roster.

Within the &lt;Roster&gt; component we will render routes for two paths:

/roster — This should only be matched when the pathname is exactly /roster, so we should also give that route element the exact prop.

/roster/:number — This route uses a path param to capture the part of the pathname that comes after /roster.

const Roster = \(\) =&gt; \(

  &lt;Switch&gt;

    &lt;Route exact path='/roster' component={FullRoster}/&gt;

    &lt;Route path='/roster/:number' component={Player}/&gt;

  &lt;/Switch&gt;

\)

It can be useful to group routes that share a common prefix in the same component. This allows for simpler parent routes and gives us a place to render content that is common across all routes with the same prefix.

As an example, &lt;Roster&gt; could render a title that would be displayed for all routes whose path begins with /roster.

const Roster = \(\) =&gt; \(

  &lt;div&gt;

    &lt;h2&gt;This is a roster page!&lt;/h2&gt;

    &lt;Switch&gt;

      &lt;Route exact path='/roster' component={FullRoster}/&gt;

      &lt;Route path='/roster/:number' component={Player}/&gt;

    &lt;/Switch&gt;

  &lt;/div&gt;

\)

Path Params

Sometimes there are variables within a pathname that we want to capture. For example, with our player profile route, we want to capture the player’s number. We can do this by adding path params to our route’s path string.

The :number part of the path /roster/:number means that the part of the pathname that comes after /roster/ will be captured and stored as match.params.number. For example, the pathname /roster/6 will generate a params object :

{ number: '6' } // note that the captured value is a string

The &lt;Player&gt; component can use the props.match.params object to determine which player’s data should be rendered.

// an API that returns a player object

import PlayerAPI from './PlayerAPI'

const Player = \(props\) =&gt; {

  const player = PlayerAPI.get\(

    parseInt\(props.match.params.number, 10\)

  \)

  if \(!player\) {

    return &lt;div&gt;Sorry, but the player was not found&lt;/div&gt;

  }

  return \(

    &lt;div&gt;

      &lt;h1&gt;{player.name} \(\#{player.number}\)&lt;/h1&gt;

      &lt;h2&gt;{player.position}&lt;/h2&gt;

    &lt;/div&gt;

\)

You can learn more about path params in the path-to-regexp documentation.

Alongside the &lt;Player&gt; component, our website also includes &lt;FullRoster&gt;, &lt;Schedule&gt;, and &lt;Home&gt; components.

const FullRoster = \(\) =&gt; \(

  &lt;div&gt;

    &lt;ul&gt;

      {

        PlayerAPI.all\(\).map\(p =&gt; \(

          &lt;li key={p.number}&gt;

            &lt;Link to={\`/roster/${p.number}\`}&gt;{p.name}&lt;/Link&gt;

          &lt;/li&gt;

        \)\)

      }

    &lt;/ul&gt;

  &lt;/div&gt;

\)

const Schedule = \(\) =&gt; \(

  &lt;div&gt;

    &lt;ul&gt;

      &lt;li&gt;6/5 @ Evergreens&lt;/li&gt;

      &lt;li&gt;6/8 vs Kickers&lt;/li&gt;

      &lt;li&gt;6/14 @ United&lt;/li&gt;

    &lt;/ul&gt;

  &lt;/div&gt;

\)

const Home = \(\) =&gt; \(

  &lt;div&gt;

    &lt;h1&gt;Welcome to the Tornadoes Website!&lt;/h1&gt;

  &lt;/div&gt;

\)

Links

Finally, our application needs a way to navigate between pages. If we were to create links using anchor elements, clicking on them would cause the whole page to reload. React Router provides a &lt;Link&gt; component to prevent that from happening. When clicking a &lt;Link&gt;, the URL will be updated and the rendered content will change without reloading the page.

import { Link } from 'react-router-dom'

const Header = \(\) =&gt; \(

  &lt;header&gt;

    &lt;nav&gt;

      &lt;ul&gt;

        &lt;li&gt;&lt;Link to='/'&gt;Home&lt;/Link&gt;&lt;/li&gt;

        &lt;li&gt;&lt;Link to='/roster'&gt;Roster&lt;/Link&gt;&lt;/li&gt;

        &lt;li&gt;&lt;Link to='/schedule'&gt;Schedule&lt;/Link&gt;&lt;/li&gt;

      &lt;/ul&gt;

    &lt;/nav&gt;

  &lt;/header&gt;

\)

&lt;Link&gt;s use the to prop to describe the location that they should navigate to. This can either be a string or a location object \(containing a combination of pathname, search, hash, and state properties\). When it is a string, it will be converted to a location object.

&lt;Link to={{ pathname: '/roster/7' }}&gt;Player \#7&lt;/Link&gt;

Note: Currently, a link's pathname must be absolute \[4\].

A Working Example

In case you don’t feel like scrolling back to the top of the page, here are the links to the demos:

CodeSandbox

CodePen.

Get Routing!

Hopefully at this point you are ready to dive into building your own website.

We have covered the most essential components that you will need to build a website \(&lt;BrowserRouter&gt;, &lt;Route&gt;, and &lt;Link&gt;\). Still, there are a few more components that this did not cover \(and props of components that were covered\). Fortunately, React Router has excellent documentation website that you can use to find more in-depth information about its components. The website also provides a number of working examples with source code.

Edit 7/15: Added CodeSandbox demo

Another Option

\(This is just a bit of self-promotion. If you like the React Router no-configuration approach, you can skip this section. \)

Not a fan of the lack of configuration? While I think that React Router is very useful, I wrote an alternative router that may better suit your desires. It is called Curi and it uses a centralized configuration object to control routing. Curi is not exclusive to React, but it works very well with it. This means that you can use Curi to add navigation to non-React projects \(although React is the best supported renderer for Curi at the moment\).

You can learn more about it at https://curi.js.org.

Notes

\[1\] locations are objects with properties to describe the different parts of a URL:

// a basic location object

{ pathname: '/', search: '', hash: '', key: 'abc123' state: {} }

\[2\] You can render a pathless &lt;Route&gt;, which will match every location. This can be useful for accessing methods and variables that are stored in the context.

\[3\] — If you use the children prop, the route will render even when its path does not match the current location.

\[4\] — There is work being done to add support for relative &lt;Route&gt;s and &lt;Link&gt;s. Relative &lt;Link&gt;s are more complicated than they might initially seem to be because they should be resolved using their parent match object, not the current URL.

\[5\] — This is essentially a stateless functional component. Internally, the big difference between the components passed to component and render is that component will use React.createElement to create the element, while render will call the component as a function. If you were to define an inline function and pass it to the component prop, it would be much slower than using the render prop.

&lt;Route path='/one' component={One}/&gt;

// React.createElement\(props.component\)

&lt;Route path='/two' render={\(\) =&gt; &lt;Two /&gt;}/&gt;

// props.render\(\)

\[6\] — The &lt;Route&gt; and &lt;Switch&gt; components can both take a location prop. This allows them to be matched using a location that is different than the actual location \(the current URL\).

\[7\] — They are also passed a staticContext prop, but that is only useful when doing server side rendering.

JavaScriptReactReact Router

A single golf clap? Or a long standing ovation?

By clapping more or less, you can signal to us which stories really stand out.





5K

41

Follow

Go to the profile of Paul Sherman

Paul Sherman

More from Paul Sherman

A shallow dive into React Router v4 Animated Transitions

A slightly edited conversation from Reactiflux

Go to the profile of Paul Sherman

Paul Sherman



209



Related reads

45% Faster React Functional Components, Now

Go to the profile of Philippe Lehoux

Philippe Lehoux



1.8K



Related reads

React, Relay and GraphQL: Under the Hood of the Times Website Redesign

Go to the profile of Scott Taylor

Scott Taylor



907



Responses

Write a response…

Conversation with Paul Sherman.

Go to the profile of Bala abhinav kirthy

Bala abhinav kirthy

Aug 9

Hi, I have a question. How can i programmatically change routes? i.e., Without having to use a &lt;Link&gt; tag. Say, I want to render a route when the user clicks on a &lt;button&gt; how can I go about doing that?. Great work with the tutorial btw, I think you covered quite some ground here :-\)



10

1 response

Go to the profile of Paul Sherman

Paul Sherman

Aug 10

Check out my answer on StackOverflow https://stackoverflow.com/questions/31079081/programmatically-navigate-using-react-router/42121109\#42121109 I would note that button navigation can be bad for accessibility \(if you’re just treating it as a link\), so I tend to limit programmatic navigation to form submission.



65



Show all responses

Go to the profile of Paul Sherman

Never miss a story from Paul Sherman, when you sign up for Medium. Learn more

