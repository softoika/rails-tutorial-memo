##7.1 ユーザを表示する  
###7.1.1 デバッグとRails環境  
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
　　
  
