# Express から MongoDB に記録する

##/modelsを作って、そこにbooks.js という mongoDB のScheme用のファイルを作る。

Edit books.js as below.  

```js
'use strict'
var mongoose = require('mongoose')
var bookSchema = mongoose.Schema({
  title: String,
  description: String,
  image: String,
  price: Number
})

var Books = mongoose.model('Books', bookSchema)
module.exports = Books
```


## app.js で mongoose 読み込み、Scheme用ファイルの読み込み、MongoDB を叩く API を書く

7. Edit app.js adding the code below

```js
app.use(express.static(path.join(__dirname, 'public')))

//<<<ここに追加>>>

app.get('*', function (req, res) {
  res.sendFile(path.resolve(__dirname,
    'public', 'index.html'))
})

```

追加するもの

```js
// APIs
var mongoose = require('mongoose')
mongoose.connect('mongodb://localhost:27017/bookshop')
Books = require('./models/books.js')

//-----POST BOOKS----------
app.post('/books', function (req, res) {
  var book = req.body
  Books.create(book, function (err, books) {
    if (err) {
      throw err
    }
    res.json(books)
  })
})
// END APIs
```

## クライアントから localhost:3000 に post する

Chrome extension から以下のものインストール
Restlet Client - REST API Testing


1. クライアントから localhost3000/books に Json を post   
1. express サーバーが受け取って、MongoDBを叩く
1. MongoDB に記録される


### Http Request をする
post メソッドで json を送る。body に以下を貼り付ける。
![](/assets/req.png)


```json
[
   {
      "title":"test title 1",
      "description":"test description 1",
      "image":"",
      "price":11
   },
   {
      "title":"test title 2",
      "description":"test description 2",
      "image":"",
      "price":22
   }
]
```

### Response を受け取る
ここでは JSON を返すようにしたので、成功すると返ってくる。
![](/assets/res.png)
