# Redux-thunk axios express の最小限の環境構築

## 環境の全体像
* フロントは React Redux Webpack
* サーバーサイドは Express MongoDB
* Express のウェブサーバーにクライアントがアクセスすると、React アプリケーションが動いている index.html を返す
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
1. React-redux 


Webstorm の場合、create new project → Express App で作成する。

## Webpack の設定

## src 以下の React-Redux アプリケーションの作成

## Express アプリケーションの変更