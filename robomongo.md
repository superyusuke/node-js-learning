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




