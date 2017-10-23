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

touch は UNIX コマンドで、指定したファイルがなければ、空の新規ファイルを作成する

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
app.get('/', function (req, res) {
  res.sendFile(path.resolve(__dirname, 'public', 'index.html'))
})
app.listen(3000, function () {
  console.log('App web-server listening onport 3000')
})
```



