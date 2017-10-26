# MongoDB のインストール

https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/

幾つかのインストール方法があるが、今回は homebrew でmongoDB Comunity server をインストールする。

以下、ルートディレクトリにて実行。

```
$ brew update
$ brew install mongodb
```
どの bin が実行されているのか確認したい場合以下のコマンド

```
$ which mongod
```

データベース用のフォルダを作る。ルートディレクトリ以下に /data/db ができる。標準で MongoDB はここをデータベースのデータを記録するディレクトリとして使用する。

```
$ mkdir -p /data/db
```

データベースを動かす

```
$ mongod
```








