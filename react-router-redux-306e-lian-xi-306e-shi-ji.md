BrowserRouter as Router にしておいて、`<Router/>`で囲むのもよくあるパターン。読み込むものが native 用でもなんでも行けるので。

```jsx
// REACT-ROUTER
import {Router, Route, IndexRoute, browserHistory} from 'react-router';
import {BrowserRouter, Route, Switch} from 'react-router-dom';
import Menu from './components/menu';
import Footer from './components/footer';
 
const Routes = (
  <Provider store={store}>
      <BrowserRouter>
        <div>
        <Menu />
        <Switch>
            <Route exact path="/" component={BooksList}/>
            <Route path="/admin" component={BooksForm}/>
            <Route path="/cart" component={Cart}/>
        </Switch>
        <Footer />
        </div>
      </BrowserRouter>
  </Provider>
)
 
render(
  Routes, document.getElementById('app')
);
```

