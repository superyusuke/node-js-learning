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
app.get('/',(req,res)=>{}) は、url "/" に対してクライアントが get した場合にコールバックを実行するということが記述されている。index.html を response に送って、resolve = 終了させている。

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
これが true の場合、Webpack に対してエントリーポイントのファイルに変更が合った場合に bundle.js を更新するように設定している。これは webpack コマンドで起動する。(そのためにグローバルにウェブパックをインストールした)

### loaders:
Webpack にどんな compiler を使うか指定

### loader:
Webpack に Babel を compiler として使うように指定

### query:
Webpack に Babel compiler を使って、ES6/es2015 もしくは ES6/stage-1 javascript version で書かれたソースを、ブラウザが読める JavaScript に変換するように指定している。 React は JSX もコンパイルするようにする指定。

## Redux アプリを作って、webpack でコンパイルし、アクセするする

1. src という名前のフォルダを開発ディレクトリ直下に
• make a file app.js inside src folder
• copy the following code and save app.js



