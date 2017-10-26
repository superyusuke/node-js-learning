# Robomongo のインストール

Robomongo は MongoDB を GUI で操作するためのツール。これを使わないのであればコマンドラインでおこなう方法もある。

Robo 3T をダウンロードしてインストール。
https://robomongo.org/download

解答→appにドラッグ・アンド・ドロップでインストール

起動

最初に起動すると接続しているデータベースの画面が立ち上がるが、まだどのデータベースとも紐付けていないので、リストには何もない。

create→

name を任意のものに変える MyDBs とか
Address は mongod を実行した際に表示される port とあっていれば、そのままで OK

作ったら、connect (その前にMongoDB を動かすために、`$ mongod` しておくこと)

myDBs→create database→任意の名前でDBを作成する。ここではtestDBとする。

testDB→open shell でクエリを実行できる場所を表示する

次のコマンドを実行。myDBs という db に対して、books というコレクションを作成し、そこに データをインサートする。booksというコレクションがなければ、自動的にこのコマンドによって生成される。

```json
db.books.insert([  
   {  
      "title":"second book title",
      "description":"second book description",
      "price":22
   },
   {  
      "title":"second book title",
      "description":"second book description",
      "price":33
   }
])
```

