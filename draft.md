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
