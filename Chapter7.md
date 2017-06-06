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
　　
###Usersリソース  
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
