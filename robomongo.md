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

## 入力したデータを見る

testDB→collection→books まで降りていき、books をエンターすると、開く。
右側のテキストマークをクリックすると、json形式そのもので中身が見える。

## update

対象のフィールドがあれば、ここでは title、value が更新される。


```json
db.books.update({  
   "_id":ObjectId("<<ダブルクオートの中に対象のidを入れる>>")
},
{  
   $set:{  
      "title":"title was updated"
   }
})

```

対象のフィールドがない場合、ここでは discount、新たにフィールドが作成された上で追加される。

```json
db.books.update({  
   "_id":ObjectId("<<ダブルクオートの中に対象のidを入れる>>")
},
{  
   $set:{
      "discount": 20}
   }
})

```

### 対象がない場合だけ、新規ドキュメントを作成する

{upsert: true} というオプションを追加することで、filter query (ここでは`{"_id" : ObjectId("59f143a477342cfbea9a9c10")}`という部分。検索対象。)が「ない場合」のみ、新しいドキュメントを作成する。

```json
db.books.update(
    {"_id" : ObjectId("59f143a477342cfbea9a9c10")},
    {$set: {"title": "this book didn't exist"}},
    {upsert: true}
)

```

### より大きい $gt: と 複数対象 {multi: true}

```json
db.books.update(
   {"price" : {$gt:20}},
   {$set: {"discount": 55}},
   {multi: true}
)
```

### 