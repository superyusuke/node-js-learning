# 環境作成

## SET-UP DEPENDENCIES AND WEBPACK

* Open terminal and run the following commands:

```
$ mkdir reduxApp
$ cd reduxApp
$ npm yarn -y
```

* Webpack を以前にインストールしたことがなければ以下のコマンドを実行する。Webpack をグローバルにインストールする。

```
npm install webpack -g
```

* Install the following dependences:

```
$ yarn add babel-core@6.21.0 babel-loader@6.2.10 babel-preset-react@6.23.0 babel-preset-es2015@6.18.0 babel-preset-stage-1@6.16.0 redux express webpack
```

* Create a Webpack config

touch は UNIX コマンドで、指定したファイルがなければ、空の新規ファイルを作成する。ウェブパックのコンフィグは後で書く。

```
$ touch webpack.config.js
```

## node のフレームワーク express で開発用のウェブサーバーを作る

1. server.js file をメインディレクトリの中に作る。\(メインディレクトリ/server.js\)
2. server.js を以下のようにする

```js
'use strict'
const express = require('express')
const app = express()
const path = require('path')
// DEFINES A FOLDER FOR THE STATIC FILES
app.use(express.static('public'))
// DEFINES THE MAIN ENTRY POINT
app.get('/', (req, res) => {
  res.sendFile(path.resolve(__dirname, 'public', 'index.html'))
})
app.listen(3000, () => {
  console.log('App web-server listening onport 3000')
})
```

app.get\('/',\(req,res\)=&gt;{}\) は、url "/" に対してクライアントが get した場合にコールバックを実行するということが記述されている。index.html を response に送って、resolve = 終了させている。

### express のサーバーで返すように指定した public/index.html を作る

```
<!DOCTYPE>
<html>
<head>
  <title>Hello redux</title>
  <meta name="viewport" content="width=device-width,initial-scale=1.0 maximum-scale=1.0"/>
</head>
<body>
<div id="app"></div>
<script src="bundle.js"></script>
<h1>Hello first Redux App</h1>
</body>
</html>
```

### express によるWebサーバが動くか確認

`$ node server.js`

ブラウザでアクセス→ [http://localhost:3000](http://localhost:3000)

## Webpackの設定

webpack.config

```
var path = require('path')
const webpack = require('webpack')
module.exports = {
  entry: './src/app.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'public'),
  },
  watch: true,
  module: {
    loaders: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        loader: 'babel-loader',
        query: {
          presets: [
            'react',
            'es2015',
            'stage-1'
          ],
        },
      },
    ],
  },
}
```

### entry:

Webpack にどこがプリケーションのエントリーポイントか教えている

### output:

Webpack にバンドルして出力する bundle.js を配置するか指定している

### watch:

これが true の場合、Webpack に対してエントリーポイントのファイルに変更が合った場合に bundle.js を更新するように設定している。これは webpack コマンドで起動する。\(そのためにグローバルにウェブパックをインストールした\)

### loaders:

Webpack にどんな compiler を使うか指定

### loader:

Webpack に Babel を compiler として使うように指定

### query:

Webpack に Babel compiler を使って、ES6/es2015 もしくは ES6/stage-1 javascript version で書かれたソースを、ブラウザが読める JavaScript に変換するように指定している。 React は JSX もコンパイルするようにする指定。

## Redux アプリを作って、webpack でコンパイルし、アクセスする

* src という名前のフォルダを開発ディレクトリ直下に作成する
* app.js というファイルを src 内に作成する
* app.js を以下のようにする

```js
'use strict'
import { createStore } from 'redux'

//STEP 3 define reducers
const reducer = function (state = {}, action) {
  switch (action.type) {
    case 'POST_BOOK':
      return state = action.payload
      break
    default:
      return state
  }
}

// STEP 1 create the store
const store = createStore(reducer)
store.subscribe(function () {
  console.log('current state is: ', store.getState())
  console.log('current price: ', store.getState().price)
})

// STEP 2 create and dispatch actions
store.dispatch({
  type: 'POST_BOOK',
  payload: {
    id: 1,
    title: 'this is the book title',
    description: 'this is the book',
    price: 33.33,
  },
})
```

### webpack でsrc/app.js をコンパイルし public/bundle.js に吐き出す。public/index.html が public/bundle.js を読み込む。立ち上げた express のウェブサーバーは、このpublic/index.html を リクエストに返してクライアントが受け取る。結果、Redux 周りが表示する log が開発者ツールに表示される。

次のコマンドと、node server.js は別のターミナルで実行する。ウェブパックの設定ファイルで指定した entry: './src/app.js' をコンパイルする。しかもウォッチで走る。public/bundle.js に吐き出される。

```
$ webpack
```

先程作った express によるウェブサーバーにアクセスする。次のコマンドを実行した後に http://localhost:3000 へアクセス。

```
$ node server.js
```

Chrome 等の開発者ツールで確認するとコンソールに表示があれば成功。



