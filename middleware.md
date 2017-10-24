# Middleware

_**store → provider → actions → middlewares → reducers**_

A Redux Middleware sits between the dispatch of an action and the reducer, basically, it allows us to execute some tasks after an action has been dispatched and the reducer is executed.

We can use third parties Middlewares or we can even build acustom one for our own needs. For instance, we could create a middleare that fires a new action when a specific action has been dispatched and so creating chains of actions.

Let’s use a third party middleware called: redux-logger.Redux-logger is a middleware that capture all actions and logs nicely the previous state and the next one, providing a great visibility on how the store is behaving.

Redux の Middleware は、action の dispatch と、reducer の間に基本的には位置します。そしてなんらかのタスクを、アクションが dispatch された後に実行し、そしてそれを受けてその後に reducer は変更されます。

サードパーティ製のミドルウェアを使うこともできますし、自分で必要なものを書くこともできます。例えば、あるアクションがディスパッチされた後に、新しいアクションを発火させるミドルウェアを作ることもできます。この場合、アクションの連鎖を作り出すことができます。

では今回はサードパーティー製のミドルウェアである redux-logger を使っていきます。redux-logger は全てのアクションの発行を捕捉して、ログを綺麗なフォーマットで記録してくれます。またその際に一つ前のstateと次のstateを表示します。これによってストアがどのように変化していくのかを、うまく可視化できます。

install redux-logger

```
$ yarn add redux-logger
```

write the following code in app.js

```js
import {applyMiddleware, createStore} from 'redux'
import { logger } from 'redux-logger'
```

```js
const middleware = applyMiddleware(logger
```

include the middelware constant as second argument in your createStore method as following:

```js
const store = createStore(reducers, middleware)
```



