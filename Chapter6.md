## 6章ユーザーのデータモデルの作成  
ユーザ情報を保存する場所、保存するためのデータ構造をまずは作成する必要がある  
MVCのM(モデル)にあたる  
モデルのデータを長期保存する手段としてデータベースが用いられる  
RailsライブラリにあるActive Recordを用いることでSQLを直接操作することなしにデータベースを操作できる  
また、マイグレーション(Migration)の機能によりデータの定義をRuby上で行うことができる  
  
### データモデルの生成
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
  
### マイグレーション  
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
  
### ユーザオブジェクトの生成
モデルからオブジェクトを生成するにはモデルの```new```メソッドを使う  
```
>> user = User.new
```
オブジェクトとしての有効性は```valid?```メソッドで確認できる  
生成されたオブジェクトはデータベースにまだ格納されていないので```save```メソッドで登録する  
```new```と```save```を使う以外にも```create```メソッドを使って、モデルの生成と保存をまとめて行うこともできる  
使い方は```new```メソッドと同じ  
そして```destroy```メソッドは```create```メソッドの逆の処理を施す  
```
>> user = User.create(name: "hoge", email: "fuga@piyo.com") # データの生成
>> user.destroy                                             # データの削除
```
ただし、```destroy```を実行してもuserのオブジェクトが削除されるわけではない  
  
### ユーザオブジェクトの検索  
生成されたオブジェクトの検索には```find```メソッドを用いる  
```
>> User.find(1)
```
この場合,引数にはIDが入る
```new```メソッドでデータを生成していても```save```していなければ```find```で見つけることはできないし、  
```destroy```していても```find```で見つけることはできない  
また、```find_by```メソッドにより、ID以外のカラムでの検索もできる  
```
>> User.find_by(email: "fuga@piyo.com")
```
他に```User.first```や```User.all```によるデータの取得もできる  
  
### ユーザオブジェクトの更新  
1つは属性を個別に代入するという方法  
```
>> user.email = "bbb@ccc.com"
>> user.save
```
```save```を呼び出す前に```user.reload```を呼び出すと変更を取り消すことができる  
もう一つの方法は```update_attributes```を使う場合  
```
>> user.update_attributes(name:"aaa", email:"bbb@ccc.com")
```
このメソッドもcreateと同様に保存を一括して行う  
  
　  
### ユーザーの検証  
ユーザ名は空であってはいけない、メールは重複してはいけないのようにそれぞれの属性が条件に合致しているか確認する必要がある  
Active Recordでは検証(Validation)という機能により制約を課すことができる  
検証においてよく使われるケース→存在性の検証、長さの検証、フォーマットの検証、一意性の検証。
  
**有効性の検証**  
具体的なテスト方法としては、まず有効なモデルのオブジェクトを作成し、その属性のうち１つを有効でない属性に変更する  
そしてバリデーションで失敗するか確かめる  
念のため最初に作成時の状態でもテストして有効かどうか確認する。こうするとバリデーションのテストが失敗したとき、バリデーションの実装に問題があったかオブジェクトに問題が合ったか確認できる  
　　
モデルのテストなので```rails generate```で作成した```test/models/user_test.rb```に書く  
```rb
require 'test_helper'

class UserTest < ActiveSupport::TestCase

  def setup
    @user = User.new(name: "Example User", email: "user@example.com")
  end

  test "should be valid" do
    assert @user.valid?
  end
end
```
```setup```という特殊なメソッドを使ってUserオブジェクトを作成する。```setup```はテストの実行前に実行される  
インスタンス変数```@user```を使うことでUserオブジェクトに対しテストすることができる  
assertメソッドは返ってくる値がtrueであれば成功し、falseだと失敗する  
  
モデルのみのテストは  
```
$ rails test:models
```
で実行できる  
```"should be valid"```のテストはUserモデルにバリデーションを定義していないのでテストは通る  
  
  
### 存在性の検証 
渡された属性が存在するかを検証する  
例えばユーザにnameとemailの両方が存在するか  
  
まず、name属性のバリデーションに対するテストを書く  
例えば```user_test.rb```に次のようなテストを追加する  
```rb
test "name should be present" do
  @user.name = "   "
  assert_not @user.valid?
end
```
```assert_not```は```assert```と逆で、```false```を返すときに成功する  
この場合、```"   "```の名前が有効でない(```false```)と返ってきたら成功になる  
次に```user.rb```のモデルにバリデーションを記述する  
```rb
class User < ApplicationRecord
  validates :name, presence: true
end
```
```validates```メソッドの詳細に関しては後ほど？  
  
ユーザオブジェクトを生成すると、ユーザオブジェクトから```valid?```メソッドを呼び出して有効かどうか調べることができる  
また```errors```オブジェクトでエラーメッセージを確認することもできる  
```
>> user.errors.full_messages
=> ["Name cant be blank"]
```
有効でない状態でデータベースに保存しようとしても失敗する→```save```メソッド  
  
emailに関する存在性の検証も同様に実装する  
  
　  
### 長さの検証  
nameとemailに関して最大文字数のテストをする(```user_test.rb```)  
```rb
 "name should not be too long" do
    @user.name = "a" * 51
    assert_not @user.valid?
  end

  test "email should not be too long" do
    @user.email = "a" * 244 + "@example.com" # 256文字
    assert_not @user.valid?
  end
```
文字数を制限する処理を書いていないので```@user.valid?```はtrueを返す(テスト失敗)  
次のように```user.rb```の```validates```メソッドを変更する  
```rb
  validates :name,  presence: true, length: { maximum: 50 }
  validates :email, presence: true, length: { maximum: 255 }
```
```presence:```以降がオプションハッシュになっていて、```length:```の値もハッシュとなっている。
