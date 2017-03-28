## 6章ユーザーのデータモデルの作成  
ユーザ情報を保存する場所、保存するためのデータ構造をまずは作成する必要がある  
MVCのM(モデル)にあたる  
モデルのデータを長期保存する手段としてデータベースが用いられる  
RailsライブラリにあるActive Recordを用いることでSQLを直接操作することなしにデータベースを操作できる  
また、マイグレーション(Migration)の機能によりデータの定義をRuby上で行うことができる  
  
モデルの作成にあたり、まずはデータモデルの定義を設計しておくとよい。  
次に```rails generate```コマンドでモデルを生成する  
```
$ rails generate model User name:string email:string
```
```rails generate```の最初の引数は```model```であり、これはモデルを生成することを示している  
コントローラを生成したい場合には```model```の代わりに```controller```が入る  
その次の引数```User```はモデル名を表す  
それ以降の引数はモデルの持つ各属性を表し、```【属性名】:【データ型】```という書式を取る  
主キーとなるIDは省略できる(書かなくても生成される)
  
```rails genarate```により、マイグレーションと呼ばれるファイルも生成される(```db/migrate/[timestamp]_create_users.rb```)  
マイグレーションによりデータ構造の変更が容易になっている  
マイグレーション自体はデータベースへ与える変更を定義した```change```メソッドの集まり  
```change```メソッドでは```create_table```というRailsのメソッドを用いてデータベースのテーブルを作成している  
マイグレーションの実行（マイグレーションの適用という)は次のコマンドで行う  
```
$ rails db:migrate
```
これでデータベースが生成される  
マイグレーションはロールバック(rollback)により元に戻すことが可能である  
```
$ rails db:rollback
```
