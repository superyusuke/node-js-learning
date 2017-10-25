# MongoDB
## NOSQL DATABASES
Thanks to their performances and flexibility to scale up,NoSQL have gained a lot of traction in the last few years.But what a NoSQL database is?

パフォーマンスの高さとスケールアップに必要な柔軟性を持つことから、NoSQL のここ数年、人気を高めている。しかし、NoSQL とはどのようなものであろうか?

In a relational database, data is stored in tables that are related to each other to avoid repeating data and saving space.In recent years cloud space became significantly cheaper and space is no longer a concern.

リレーショナル・データベースの場合は、データはテーブルの中に保持され、繰り返しを避け容量を無駄にしないために、それぞれが関連付けられている。しかし、昨今、容量に対するコストはとてつもなく安くなっており、最早気にするようなことではなくなった。

In a NoSql database, instead of tables, data is stored in“name-value” pair format called Json which you can easily read and write from your code because is a subset of javascript. In fact, if you take a closer look of a json file you would see that it contains javascript objects and arrays.

NoSQL データベースにおいては、テーブルは使われず、代わりに name と value の組み合わせ、つまり Json と呼ばれる形式で保存されており、それゆえ可読性が高く、コードを書いて記録させるのも容易である。Json は JavaScript の一部だからだ。実際、Json ファイルをよく観察するとわかるのだが、これは JavaScript オブジェクトと、配列によって構成されている。

With a NoSQL database you don’t have the constraints related　of being interfaced with a specific relational-database. You just create and consume data with great flexibility and performances.

NoSQL データベースは、特定のリレーショナル・データベースのインターフェイスと関連付けられることによって生じる欠点がない。単にデータを作成し、使用するだけなので、柔軟性が非常に高く、パフォーマンスも良い。

In fact, NoSQL databases don’t need to be tight to a fixed schema.

実際的な意味での利点の一つとして、NoSQL データベースは、固定されたスキーマに合わせる必要はない。(訳注: リレーショナル・データベースの場合、各データ全てに同じフィールドを持つことになる。しかし NoSQL の場合、全てのデータが同じ name をもつ必要はない。)

### 用語
[]で囲まれたデータベース全体を、collection と呼ぶ。各データを document、Json における name に相当する部分を field と呼ぶ。

```json
[  
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
]
```
