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










