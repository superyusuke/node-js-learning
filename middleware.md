* install redux-logger

```
$ yarn add redux-logger
```

write the following code in app.js

```js
import {applyMiddleware, createStore} from 'redux'
import { createLogger } from 'redux-logger'
```

```js
const middleware = applyMiddleware(logger)
```

* include the middelware constant as second argument in your createStore method as following:

```js
const store = createStore(reducers, middleware)
```



