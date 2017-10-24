## Routes
The `<Route>` component is the main building block of React Router. Anywhere that you want to only render something if it matches the location’s pathname, you should create a `<Route>` element.

`<Route>`コンポーネントは、React Router の中心的な役割を担うブロックです。location の pathname に一致した際に、何かをレンダーしたい場所に対して、`<Route>` 要素を配置します。

### Path
A `<Route>` expects a path prop string that describes the type of pathname that the route matches — for example, `<Route path='/roster'/>` should match a pathname that begins with /roster[2]. When the current location’s pathname is matched by the path, the route will render a React element. When the path does not match, the route will not render anything [3].

`<Route>`は

```
<Route path='/roster'/>
// when the pathname is '/', the path does not match
// when the pathname is '/roster' or '/roster/2', the path matches

// If you only want to match '/roster', then you need to use
// the "exact" prop. The following will match '/roster', but not
// '/roster/2'.
<Route exact path='/roster'/>

// You might find yourself adding the exact prop to most routes.
// In the future (i.e. v5), the exact prop will likely be true by
// default. For more information on that, you can check out this 
// GitHub issue:
// https://github.com/ReactTraining/react-router/issues/4958

```

Note: When it comes to matching routes, React Router only cares about the pathname of a location. That means that given the URL:
http://www.example.com/my-projects/one?extra=false
the only part that React Router attempts to match is /my-projects/one.