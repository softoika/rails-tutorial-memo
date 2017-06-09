## 7.1 ユーザを表示する  
  
### 7.1.1 デバッグとRails環境  
動的なユーザページを作るということでデバッグ表示を出せるようにする  
application.html.erb
```erb
<!DOCTYPE html>
<html>
  .
  .
  .
  <body>
    <%= render 'layouts/header' %>
    <div class="container">
      <%= yield %>
      <%= render 'layouts/footer' %>
      <%= debug(params) if Rails.env.development? %>
    </div>
  </body>
</html>
```
```erb
<%= debug(params) if Rails.env.development? %>
```
これを追記する  
本番環境やテスト環境ではデバッグ表示したくないので```if Rails.env.development?```の記述をしている(開発環境のみ)  
ビルトインのdebugメソッドとparams変数を用いている  

デバッグ表示の整形のためにcssを設定する  
```scss
@import "bootstrap-sprockets";
@import "bootstrap";

/* mixins, variables, etc. */

$gray-medium-light: #eaeaea;

@mixin box_sizing {
  -moz-box-sizing:    border-box;
  -webkit-box-sizing: border-box;
  box-sizing:         border-box;
}
.
.
.
/* miscellaneous */

.debug_dump {
  clear: both;
  float: left;
  width: 100%;
  margin-top: 45px;
  @include box_sizing;
}
```
Sassのミックスイン機能を用いている  
これによりCSSのグループをパッケージ化して複数の要素に適用することができる  
@mixinで定義したものが@includeしたところで適用されるので関数みたいなもの?　　
　　
### 7.1.2Usersリソース  
Userモデルの中のデータ表示を行う
RESTアーキテクチャの習慣に従う リレーショナルデータベース操作のPOST/GET/UPDATE/DELETEと
HTTPリクエストのPOST/GET/PATCH/DELETE両方に対応している  
REST原則に従う場合、リソース名とユニークIDを用いる。例えば/users/1のように  
RailsのREST機能が有効になっているとGETリクエストはshowアクションとして扱われる  
/users/1へのURLを有効するためにconfig/routes.rbに以下の１行を追加する  
```rb
resources: users
```
/users/1が有効になるだけでなく、RESTfulなUsersリソースで必要となるすべてのアクションが利用できるようになる
名前付きルートも使える  
[resources:usersで使えるURLとそれに対応するアクション、名前付きルート](https://railstutorial.jp/chapters/sign_up?version=5.0#table-RESTful_users)
　　
現状ではルーティングが有効になっているがまだページは表示されない  
表示のためにビューが必要なのでユーザ情報を表示するための仮のビューを作成する  
app/views/users/show.html.erb  
```rb
<%= @user.name %>, <%= @user.email %>
```
この記述は@userが定義されていることが前提であるため、showアクションで@userの定義を行う必要がある 
app/controllers/users_controller.rb  
```rb
class UsersController < ApplicationController

  def show
    @user = User.find(params[:id])
  end

  def new
  end
end
```
idの指定にparamsを使用した。Usersコントローラにリクエストが正常に送信されると、params[:id]の部分はユーザーidの1に置き換わる  
param[:id]は文字列のidを返すが、findメソッドが自動的に数値として解釈する  
  
### 7.1.3 debuggerメソッド
byebug gemによるdebuggerメソッドを用いると簡単にデバッグプリントを挟むことができる  
app/controllers/user_controller.rb
```rb
class UsersController < ApplicationController
def show
    @user = User.find(params[:id])
    debugger
  end
def new
  end
end
```
/users/1にアクセスしてターミナルを見るとdebuggerが呼び出される瞬間の状態を確認できる  
確認し終わったらdebuggerの行を削除する  
  
### Gravatar画像とサイドバー
Gravatarをプロフィールに導入する  
>Gravatarは無料のサービスで、プロフィール写真をアップロードして、指定したメールアドレスと関連付けることができます。その結果、 Gravatarはプロフィール写真をアップロードするときの面倒な作業や写真が欠けるトラブル、また、画像の置き場所の悩みを解決します。というのも、ユーザーのメールアドレスを組み込んだGravatar専用の画像パスを構成するだけで、対応するGravatarの画像が自動的に表示されるからです   
  
gravatar_forヘルパーメソッドを使用して表示を行う  
app/viewers/users/show.html.erb
```rb
<% provide(:title, @user.name) %>
<h1>
  <%= gravatar_for @user %>
  <%= @user.name %>
</h1>
```
デフォルトではヘルパーファイルで定義されているメソッドは自動的にすべてのビューで使用できる  
[Gravatarのホームページ](http://ja.gravatar.com/site/implement/hash/)にも書かれているようにGravatarのURLはユーザーのMD5という仕組みでハッシュ化されている。  
DigestライブラリのhexdigestメソッドでMD5のハッシュ化ができる  
```rb
>> email = "MHARTL@example.COM"
>> Digest::MD5::hexdigest(email.downcase)
=> "1fda4469bcbec3badf5418269ffc5968"
```
MD5ではメールアドレスの小文字大文字は区別されるのでdowncaseメソッドで小文字に統一する必要がある  
gravatar_forヘルパーメソッドを定義する  
app/helpers/users_helper.rb
```rb
module UsersHelper
# 引数で与えられたユーザーのGravatar画像を返す
  def gravatar_for(user)
    gravatar_id = Digest::MD5::hexdigest(user.email.downcase)
    gravatar_url = "https://secure.gravatar.com/avatar/#{gravatar_id}"
    image_tag(gravatar_url, alt: user.name, class: "gravatar")
  end
end
```
```rb
image_tag(gravatar_url, alt: user.name, class: "gravatar")
```
はgravatarクラスとユーザー名のaltテキストを追加したもの。  
この状態でusers/1にアクセスするとGravatarのデフォルト画像が表示される(user@example.comが本当のメールアドレスではないため)
アプリケーションでGravatarを利用できるようにするため、updata_attributesメソッドを使ってユーザ情報(メールアドレスを更新してみる)  
```rb
$ rails console
>> user = User.first
>> user.update_attributes(name: "Example User",
?>                        email: "example@railstutorial.org",
?>                        password: "foobar",
?>                        password_confirmation: "foobar")
=> true
```
  
ユーザーのサイドバーの表示を整える  
asideタグを使用し、rowクラスとcol-md-4クラスも追加する  
app/views/users/show.html.erb
```rb
<% provide(:title, @user.name) %>
<div class="row">
  <aside class="col-md-4">
    <section class="user_info">
      <h1>
        <%= gravatar_for @user %>
        <%= @user.name %>
      </h1>
    </section>
  </aside>
</div>
```
HTML要素とCSSクラスを配置したことによりSCSSで整えることができるようになった  
app/assets/stylesheets/custom.scss
```scss
.
.
.
/* sidebar */
aside {
  section.user_info {
    margin-top: 20px;
  }
  section {
    padding: 10px 0;
    margin-top: 20px;
    &:first-child {
      border: 0;
      padding-top: 0;
    }
    span {
      display: block;
      margin-bottom: 3px;
      line-height: 1;
    }
    h1 {
      font-size: 1.4em;
      text-align: left;
      letter-spacing: -1px;
      margin-bottom: 3px;
      margin-top: 0px;
    }
  }
}
.gravatar {
  float: left;
  margin-right: 10px;
}
.gravatar_edit {
  margin-top: 15px;
}
```
