# Express から MongoDB に記録する

##/modelを作って、そこにbooks.js という mongoDB のScheme用のファイルを作る。

In the main app directory create a folder called:  
models  
5. Inside models, create a file file: books.js  
6. Edit books.js as below.  
7. Edit app.js adding the code below

## app.js で mongoose 読み込み、Scheme用ファイルの読み込み、apiを書く



```
app.use(express.static(path.join(__dirname, 'public')))

//<<<ここに追加>>>

app.get('*', function (req, res) {
  res.sendFile(path.resolve(__dirname,
    'public', 'index.html'))
})

```


## クライアントから localhost:3000 に post する。

Restlet Client - REST API Testing



ブラウザからlocalhost3000にpost  
nodeが受け取って、mongoを叩くようにする。

