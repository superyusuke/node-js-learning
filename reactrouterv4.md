#A Simple React Router v4 Tutorial

React Router v4 is a pure React rewrite of the popular React package. Previous versions of React Router used configuration disguised as pseudo-components and would be difficult to understand. Now with v4, everything is now “just components”.

React Router V4 は既存のリアクト関係のパッケージが刷新されたものです。以前の React Router は疑似コンポーネントによる実装が必要で理解が難しいものでした。今回の V4 は全てが通常のコンポーネントによって実装できるので、簡潔です。

In this tutorial, we will be building a website for a local sports team. We will go over all of the basics needed to get our site up and routing. This will include:

このチュートリアルでは、地元スポーツチームのウェブサイトを作っていきます。サイトの製作及びルーティングに必要となる基礎知識を全て取り上げていきます。

1. ルーターの選択 Choosing our router
1. それぞれのルートの作成 Creating our routes
1. リンクを使って各ルートを行き来する Navigating between routes using links

## The Code

_Just want to see the website in action? Here it is. No fancy styling, just a simple, functional website. The demo code is also available on CodePen if you want to play around with it there.
_
どのように機能するのか実際のウェブサイトでご覧になりたいことでしょう。次のデモは、綺麗なスタイリングはされていませんが、シンプルな機能だけしかないサイトです。

[CodeSandBox](https://codesandbox.io/s/vVoQVk78?from-embed)

## Installation
React Router has been broken into three packages: react-router, react-router-dom, and react-router-native.

React Router は３つのパッケージに分割されています。react-router, react-router-dom, and react-router-native です。

You should almost never have to install react-router directly. That package provides the core routing components and functions for React Router applications. The other two provide environment specific (browser and react-native) components, but they both also re-export all of react-router's exports.

おそらく react-router をインストールすることは無いと思われます。このパッケージは、React Router アプリケーションの、コアとなるルーティングのコンポーネントとファンクションを提供します。react-router 以外の他の２つは、特定の環境向け(bwoserとreact-native)のコンポーネントを提供していますのが、２つとも react-router が export するものは全て含まれています。

You should pick whichever of the other two is correct for your environment. We are building a website (something that will be run in browsers), so we should install react-router-dom.

まずはこの２つのうちどちらを使うのか、あなたの環境に合った方を選んでください。このチュートリアルではウェブサイトを作っていきますので(基本的にはブラウザ上で実行されるもの)、react-router-dom をインストールしましょう。

```
$ npm install --save react-router-dom
```

### The Router

When starting a new project, you need to determine which type of router to use. For browser based projects, there are `<BrowserRouter>` and `<HashRouter>` components. The `<BrowserRouter>` should be used when you have a server that will handle dynamic requests (knows how to respond to any possible URI), while the `<HashRouter>` should be used for static websites (can only respond to requests for files that it knows about).

新しいプロジェクトを始める前に、どちらのタイプのルーターを使用するのか決める必要があります。ブラウザベースのプロジェクトであれば`<BrowserRouter>`と`<HashRouter>`の選択肢があります。`<BrowserRouter>`を用いるのは、ダイナミック・リクエストを受け付けるサーバー(どんなURIが来ても応答できるようなサーバー)を使用しているさいです。一方で`<HashRouter>`を用いるべきときは、スタティックなウェブサイトを作るさいです。(既に知っているファイルに対するリクエストだけに反応できるウェブサイトです)

Usually it is preferable to use a `<BrowserRouter>`, but if your website will be hosted on a server that only serves static files, then `<HashRouter>` is a good solution.
For our project, we will assume that the website will be backed by a dynamic server, so our router component of choice is the `<BrowserRouter>`.

一般的に`<BrowserRouter>`を選んだほうがいいと思いますが、スタティックファイルにだけしか対応していないサーバーの場合は、`<HashRouter>`を使うことで対応することができます。私達地元チーム向けウェブサイトのプロジェクトでは、ダイナミック・サーバーの上で動いているとして、`<BrowserRouter>`をルーター用コンポーネントに選択することにしましょう。

## History

Each router creates a history object, which it uses to keep track of the current location[1] and re-render the website whenever that changes. The other components provided by React Router rely on having that history object available through the context, so they must be rendered inside of the router. A React Router component that does not have a router as one of its ancestors will fail to work. If you are interested in learning more about the history object (I think that this is important), you can check out my article A Little Bit of History.

それぞれの router は history object を生成します。これは現在の location を記録するために使用され、また location が変更された場合には、ウェブサイトを再レンダリングします。(訳注:正確に意味がわからない)React Router が提供するその他のコンポーネントは、context を通じて history object を使用可能である必要があるため、React Router が提供するその他のコンポーネントは router の内側で render される必要があります。つまり、React Router コンポーネントのうち、親要素に router もたないものは機能しません。history obeject に関するより深い理解を得たい場合には(個人的には非常に重要だと思います)[私が書いたこの記事を参照してください。](https://medium.com/@pshrmn/a-little-bit-of-history-f245306f48dd)

## Rendering a `<Router>`

Router components only expect to receive a single child element. To work within this limitation, it is useful to create an `<App>` component that renders the rest of your application (separating your application from the router is also important for server rendering because you can re-use the `<App>` on the server while switching to a `<MemoryRouter>`).

Router は、一つの子要素だけ受け取ることができます。そのため、`<App>`コンポーネントを作って、残りのアプリケーション用のコンポーネントはその下でレンダリングささせましょう。(訳注: `<App>`の中でさらにコンポーネントを呼び出す、という一般的な React におけるツリー化)

(router と `<App>` を分けるのは、サーバーレンダリングする際にも重要になってきます。何故なら、そうしておけば`<App>` をサーバーサイドにおいた場合には、(訳注:恐らく`<BrowserRouter>`を)`<MemoryRouter>`に切り替えるだけでいいからです。)


```
import { BrowserRouter } from 'react-router-dom'
ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
), document.getElementById('root'))

```

Now that we have chosen our router, we can start to render our actual application.

さて、router の選択については終わりましたので、次は実際のアプリケーションを render させていきましょう。

## The `<App>`

Our application is defined within the`<App>` component. To simplify things, we will split our application into two parts. The `<Header>` component will contain links to navigate throughout the website. The `<Main>` component is where the rest of the content will be rendered.

私達のアプリケーションは、`<App>`コンポーネントの中で定義される必要があります。さて、私達のアプリケーシションを整理するために、２つのパートに分けることにしましょう。The `<Header>` コンポーネントはサイト内を移動するためのリンクを持つ予定でうす。`<Main>` コンポーネントはそれ以外の残りの要素がレンダリングされます。

```
// this component will be rendered by our <___Router>
const App = () => (
  <div>
    <Header />
    <Main />
  </div>
)
```

Note: You can layout your application any way that you would like, but separating routes and navigation makes it easier to show how React Router works for this tutorial.

どんなふうにでもアプリケーションを構造化することができますが、React Router がどのように機能するかを理解するという意味では、route と他のナビゲーションを分けたほうが、よりわかりやすくなると思いますのでそうしました。

We will start in the `<Main>` component, where we will render our routes.

では route を内部でレンダーする `<Main>` コンポーネントに次に取り掛かりましょう。

## Routes
The `<Route>` component is the main building block of React Router. Anywhere that you want to only render something if it matches the location’s pathname, you should create a `<Route>` element.

`<Route>`コンポーネントは、React Router の中心的な役割を担うブロックです。location の pathname に一致した際に、何かをレンダーしたい場所に対して、`<Route>` 要素を配置します。

### Path
A `<Route>` expects a path prop string that describes the type of pathname that the route matches — for example, `<Route path='/roster'/>` should match a pathname that begins with /roster[2]. When the current location’s pathname is matched by the path, the route will render a React element. When the path does not match, the route will not render anything [3].

`<Route>`は path プロップに対して文字列を受けるようになっています。path プロップスに与える文字列は、route がマッチさせたい pathname を指定します。例えば、`<Route path='/roster'/>`と path プロップに '/roster' を与えた場合、pathname が /roster で始まる場合に、マッチすることになります。現在の位置を示す pathname が path プロップと一致した場合、route は React element をレンダーします。一致しない場合は、route は何もレンダリングしません。

```
<Route path='/roster'/>
// when the pathname is '/', the path does not match
// when the pathname is '/roster' or '/roster/2', the path matches

//pathname が '/'の場合はマッチしない
//pathname が '/roster' や '/roster/2' の場合はマッチする

// If you only want to match '/roster', then you need to use
// the "exact" prop. The following will match '/roster', but not
// '/roster/2'.

// '/roster' だけにマッチさせたい場合には、exact プロップを使用すること
// 以下の例は '/roster' にはマッチするが '/roster/2' にはマッチしない
<Route exact path='/roster'/>

// You might find yourself adding the exact prop to most routes.
// In the future (i.e. v5), the exact prop will likely be true by
// default. For more information on that, you can check out this 
// GitHub issue:
// https://github.com/ReactTraining/react-router/issues/4958

// ほとんどの route に対して exact を付けることになると思います。
// 将来的には(例えばv.5とかで)exact プロップがデフォルトで設定されるように
// なる予定です。詳しいことは GitHub リポジトリを参照してください。

```

Note: When it comes to matching routes, React Router only cares about the pathname of a location. That means that given the URL:
http://www.example.com/my-projects/one?extra=false
the only part that React Router attempts to match is /my-projects/one.

route のマッチという観点では、React Router は location の pathname しか考慮に入れません。ですので、http://www.example.com/my-projects/one?extra=false といった URL の場合、React Router は、/my-projects/one に対してマッチするかどうかを判定します。

### Matching paths
The path-to-regexp package is used to determine if a route element’s path prop matches the current location. It compiles the path string into a regular expression, which will be matched against the location’s pathname. Path strings have more advanced formatting options than will be covered here. You can read about them in the path-to-regexp documentation.

route エレメントの path プロップのマッチ判定には、path-to-regexp パッケージが使用されています。そのパッケージが path プロップに与えられた文字列を正規表現に変換して、その変換されたものと location の pathname が一致しているかを判定します。path に与える文字列には多くの option がありますが、このコースで扱う範囲を超えています。興味がある場合は、[path-to-regexp のドキュメント](https://github.com/pillarjs/path-to-regexp)を参照してください。

When the route’s path matches, a match object with the following properties will be created:

route path がマッチした場合、match オブジェクトが生成されます。このオブジェクトは、以下のプロパティを持ちます。

* url — the matched part of the current location’s pathname
* 現在の location pathname の一致した箇所
* path — the route’s path
* route の path
* isExact — path === pathname
* path===pathname (訳注:一致していればtrueが返される)
* params — an object containing values from the pathname that were captured by path-to-regexp
* path-to-regexp で capture した pathname から得られた値を持つ、オブジェクト

You can use [this route tester](https://pshrmn.github.io/route-tester/#/) to play around with matching routes to URLs.

このサイトで URL と route のマッチングについて試してみることができます。

Note: Currently, a route’s path must be absolute[4].

今のところ、route に与えられる path は絶対表記 = absolute でないといけません。 

### Creating our routes
`<Route>`s can be created anywhere inside of the router, but often it makes sense to render them in the same place. You can use the`<Switch>` component to group `<Route>`s. The `<Switch>` will iterate over its children elements (the routes) and only render the first one that matches the current pathname.

`<Route>`は router の内部であればどこでも作成することができますが、全てをある良い箇所に固めて配置したほうが、把握するのが楽になると思います。`<Switch>`コンポーネントを用いて`<Route>`をグループ化しましょう。`<Switch>`は、自分の子要素として存在する route を捜索し、現在の pathname と一致した最初の route をレンダリングします。

For this website, the paths that we want to match are:

今回作成するウェブサイトの場合は、マッチさせたい path は以下のようになります

* / — the homepage
* / — トップページ

* /roster — the team’s roster
* /roster — 登録選手名簿

* /roster/:number — a profile for a player, using the player’s number
* /roster/:number — プレイヤーのプロフィールページ, プレイヤーの number を使用する

* /schedule — the team’s schedule of games
* /schedule — 試合の予定表


In order to match a path in our application, all that we have to do is create a <Route> element with the path prop we want to match.

私達の作るアプリケーションの route エレメントへの path は以下のようになります。

```
<Switch>
  <Route exact path='/' component={Home}/>
  {/* both /roster and /roster/:number begin with /roster */}
  <Route path='/roster' component={Roster}/>
  <Route path='/schedule' component={Schedule}/>
</Switch>
```
// /roster でも /roster/:number でもマッチします。
// /roster で始まる場合に一致するからです

### What does the `<Route>` render?
`<Route>`が何をレンダリングするか

Routes have three props that can be used to define what should be rendered when the route’s path matches. Only one should be provided to a `<Route>` element.

Route の path がマッチした際に何をレンダリングするのか、それを定義するための prop には 3種類あります。どれか一つを`<Route>`に与える必要があります。

component — A React component. When a route with a component prop matches, the route will return a new element whose type is the provided React component (created using React.createElement).

component - React component を与えます。component が prop に与えられている場合にマッチすると、route は 指定された React component を新規要素として返します。 

render — A function that returns a React element [5]. It will be called when the path matches. This is similar to component, but is useful for inline rendering and passing extra props to the element.

render - React element を返す関数を指定します。これは component を prop に渡す場合と似ていますが、しかしインラインでレンダリングしたい場合、また追加の prop を要素に与えたい場合には便利です。

children — A function that returns a React element. Unlike the prior two props, this will always be rendered, regardless of whether the route’s path matches the current location.

children - React element を返す関数を指定します。既にあげた２つの prop 床となり、これはマッチしていないときでも常にレンダリングされます。

```jsx
<Route path='/page' component={Page} />

const extraProps = { color: 'red' }

<Route path='/page' render={(props) => (
  <Page {...props} data={extraProps}/>
)}/>

<Route path='/page' children={(props) => (
  props.match
    ? <Page {...props}/>
    : <EmptyPage {...props}/>
)}/>
```

Typically, either the component or render prop should be used. The children prop can be useful occasionally, but typically it is preferable to render nothing when the path does not match. We do not have any extra props to pass to the components, so each of our `<Route>`s will use the component prop.

一般的には、component か render プロップを用いるべきです。children プロップは時には便利ですが、通常はマッチしていないときには何もレンダリングしないほうがいいでしょう。今回は特に追加で渡したい prop がないので component を prop として採用することにします。

The element rendered by the `<Route>` will be passed a number of props. These will be the match object, the current location object [6], and the history object (the one created by our router) [7].

`<Route>`によってレンダリングされた要素には、prop の number が渡されます。その number は、match オブジェクトと、current location オブジェクトと、history オブジェクトです。(訳注:よくわからない)

### `<Main>`
Now that we have figured our root route structure, we just need to actually render our routes. For this application, we will render our `<Switch>` and `<Route>`s inside of our `<Main>` component, which will place the HTML generated by a matched route inside of a `<main>` DOM node.

root の route の構造については理解したので、次は実際にレンダリングさせていきましょう。今回のアプリケーションでは、`<Switch>` と `<Route>`を`<Main>` component のな内部でレンダリングさせます。そうすると、`<main>` DOM node の中にある route がマッチした際に、その場所へ HTML が生成されます。

```jsx
import { Switch, Route } from 'react-router-dom'
const Main = () => (
  <main>
    <Switch>
      <Route exact path='/' component={Home}/>
      <Route path='/roster' component={Roster}/>
      <Route path='/schedule' component={Schedule}/>
    </Switch>
  </main>
)

```

Note: The route for the homepage includes an exact prop. This is used to state that that route should only match when the pathname matches the route’s path exactly.

homepage 用の route には exact prop を指定しています。これは、この route が '/' だけにマッチするように指定するためです。(訳注:exact がなければ、'/bah/bah' 等全てにマッチしてしまうから。通常トップページ用には exact を用いることになる。)

### Nested Routes
入れ子状になった　route について

The player profile route /roster/:number is not included in the above `<Switch>`. Instead, it will be rendered by the `<Roster>` component, which is rendered whenever the pathname begins with /roster.

プレイヤーのプロフィール用の route である /roster/:number は`<Switch>`の中にはありません。これは、`<Roster>`コンポーネントの中でレンダリングさせることにしました。`<Roster>`は /roster で始まる pathname の場合にレンダリングされるコンポーネントでした。

Within the `<Roster>` component we will render routes for two paths:

`<Roster>`コンポーネントの中で、2種類の path をもった route をレンダーさせます。

* /roster — This should only be matched when the pathname is exactly /roster, so we should also give that route element the exact prop.
* /roster - この route は、pathname が完全に /roster の場合のみレンダーさせたいので、exact prop を与えます。

* /roster/:number — This route uses a path param to capture the part of the pathname that comes after /roster.
* /roster/:number - この route は path param を用いて、pathname の一部、つまり /roster の後に来る部分を、特定し、使用します。


```jsx
const Roster = () => (
  <Switch>
    <Route exact path='/roster' component={FullRoster}/>
    <Route path='/roster/:number' component={Player}/>
  </Switch>
)

```

It can be useful to group routes that share a common prefix in the same component. This allows for simpler parent routes and gives us a place to render content that is common across all routes with the same prefix.
As an example, `<Roster>` could render a title that would be displayed for all routes whose path begins with /roster.

```jsx
const Roster = () => (
  <div>
    <h2>This is a roster page!</h2>
    <Switch>
      <Route exact path='/roster' component={FullRoster}/>
      <Route path='/roster/:number' component={Player}/>
    </Switch>
  </div>
)
```



