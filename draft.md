### The Router

When starting a new project, you need to determine which type of router to use. For browser based projects, there are `<BrowserRouter>` and `<HashRouter>` components. The `<BrowserRouter>` should be used when you have a server that will handle dynamic requests (knows how to respond to any possible URI), while the `<HashRouter>` should be used for static websites (can only respond to requests for files that it knows about).

新しいプロジェクトを始める前に、どちらのタイプのルーターを使用するのか決める必要があります。ブラウザベースのプロジェクトであれば`<BrowserRouter>`と`<HashRouter>`の選択肢があります。`<BrowserRouter>`を用いるのは、ダイナミック・リクエストを受け付けるサーバー(どんなURIが来ても応答できるようなサーバー)を使用しているさいです。一方で`<HashRouter>`を用いるべきときは、スタティックなウェブサイトを作るさいです。(既に知っているファイルに対するリクエストだけに反応できるウェブサイトです)


Usually it is preferable to use a <BrowserRouter>, but if your website will be hosted on a server that only serves static files, then <HashRouter> is a good solution.
For our project, we will assume that the website will be backed by a dynamic server, so our router component of choice is the <BrowserRouter>.
History
Each router creates a history object, which it uses to keep track of the current location[1] and re-render the website whenever that changes. The other components provided by React Router rely on having that history object available through the context, so they must be rendered inside of the router. A React Router component that does not have a router as one of its ancestors will fail to work. If you are interested in learning more about the history object (I think that this is important), you can check out my article A Little Bit of History.
Rendering a <Router>
Router components only expect to receive a single child element. To work within this limitation, it is useful to create an <App> component that renders the rest of your application (separating your application from the router is also important for server rendering because you can re-use the <App> on the server while switching to a <MemoryRouter>).
import { BrowserRouter } from 'react-router-dom'
ReactDOM.render((
  <BrowserRouter>
    <App />
  </BrowserRouter>
), document.getElementById('root'))
Now that we have chosen our router, we can start to render our actual application.