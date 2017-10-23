* install redux-logger

```
yarn redux-logger
```

* import {applyMiddleware, createStore} from'redux'
* import logger from 'redux-logger';
* write the following code
const middleware =applyMiddleware(logger())
* include the middelware constant as secondargument in your createStore method as following:
const store = createStore(reducers,middleware);

```

```

