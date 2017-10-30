# Redux-thunk axios Express MongoDB の最小限の環境構築

## 環境の全体像
* フロントは React Redux Webpack
* サーバーサイドは Express MongoDB
* (Express のウェブサーバーにクライアントがアクセスすると、React アプリケーションが動いている index.html を返す)
* 両者は axios redux-thunk を使って通信する

### ディレクトリ構造
* /src/以下にフロントの React app 開発用のソースを用意する
* /直下の app.js にバックエンドの Express app を配置する
* /public には、Webpack でコンパイルした JS、エントリーポイントとなる index.html を配置。Express app は、/pulic/index.html を返す。

## 構築の手順概要

1. Node のフレームワークである Express によるアプリケーションを、 Express Generator で作成する。これがサーバーサイドを担当する。
1. フロントエンドに必要な package を yarn add で追加する。
1. yarn でインストール。
1. クライアントが Express サーバーにアクセスした場合に、index.html を返すように /app.js を修正。(同時に必要のない ejs 等の view を削除する)
1. Webpack で /src/app.js → /public/bundle.js に出力するよう設定 
1. /src/app 周りに React-Redux アプリケーションを構築

### 1. Express App の作成

一度もしたことがなければ

```
$ yarn global add express-generator nodemon
```

次に、対象のディレクトリ内で

```
$ express
```

Webstorm の場合、create new project → Express App で作成可能。

### 2. フロントエンドに必要なパッケージを yarn add する

#### Global に必要なもの
一度も入れたことがなければ

```
$ yarn global add webpack
```

以下は、全プロジェクトで実行

```
$ yarn add webpack babel-core babel-loader babel-preset-react babel-preset-es2015 babel-preset-stage-1 redux react-redux redux-thunk axios redux-logger mongoose 
```

### 3. yarn で package.json に書かれたものをすべてインストールする

```
$ yarn
```

## 4.Express サーバーが /public/index.html を返すように /app.js を修正


### /app.js の修正

jadeを使わないので其の部分を削除して、かわりにget\(\)で読み込ませるものを与える。

/app.js の view に jade を使うことを指定している以下の箇所を、削除。

```js
// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');
```

25行目あたりにある以下を削除して、

```js
app.use('/', index);
app.use('/users', users);
```

同じ位置に次のものを配置する

```js
app.get('*', function(req, res){
res.sendFile(path.resolve(__dirname,
'public', 'index.html'))
})
```

### /public/index.js を作成して読み込ませる
app.get で読み込むファイル /public/index.js を作成する

### 動いているか確認
```
$ nodemon
```
 localhost:3000 にアクセスして、先程作った  index.html が表示されれば OK


## 6.Webpack で /src/app.js → /public/bundle.js に出力するよう設定 

### webpack.config.js を作成する

```
$ touch webpack.config.js
```

webpack.config.js を設定

```js
var path = require('path')
const webpack = require('webpack')
module.exports = {
  entry: './src/client.js',
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

### webpack の entrypoint である src/client.js を作成する
/src/client.js を作って適当に js を適当に書く。 

```js
const doLog = (text) => {
  console.log(text)
}

doLog('Welcome Webpack')
```

### /public/index.html で bundle.js を </body> 直前で読み込む

<script src="bundle.js"></script>


### webpack で実行する

```
$ webpack
```

localhost:3000 にアクセスして、console に表示されれば OK

## 6. /src/app 周りに React-Redux アプリケーションを構築


