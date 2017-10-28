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


## app.js で mongoose 読み込み、Scheme用ファイルの読み込み、apiを書く

7. Edit app.js adding the code below

```
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

## クライアントから localhost:3000 に post する。

Chrome extension から以下のものインストール
Restlet Client - REST API Testing


クライアントから localhost3000/books に post   
nodeが受け取って、mongoを叩くようにする。

