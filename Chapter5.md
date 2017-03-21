Webアプリケーションを作成するときに、ユーザーインターフェイスの概要をできるだけ早いうちに把握しておくことがしばしば有用  
→モックアップを作成する  
[Mockingbird](https://gomockingbird.com/home)というモックアップ作成サービスガある
  
HTMLファイルのレイアウトとして  
```html
<!DOCTYPE html>
```
はHTMLのバージョンとしてHTML5を使用するということを宣言している。ただし、HTML5は比較的新しく、特にIE 9以前の旧式ブラウザでは
サポートが不完全である場合がある。  
そのため、次のJavascriptコードをヘッダー部分に書いてこの問題を回避する  
```html
<!--[if lt IE 9]>
  <script src="//cdnjs.cloudflare.com/ajax/libs/html5shiv/r29/html5.min.js">
  </script>
<![endif]-->
```
この文には```<!--[if lt IE 9>```と書いてあるがこの部分がIEのバージョンが9より小さいならという意味を表している。
また、この書き方は条件付きコメントと呼ばれるものでInternet Explorerで特別にサポートされている書き方。  
  
すべてのHTML要素にはクラスとidの両方を指定することができる。  
クラスとidの違いはクラスはページの中で何度も使用できるのに対し、idは一度しか使用することができない点です。  
クラスの書き方例  
```html
<div class="container"></div>
```
  
navタグには「その内側がナビゲーションリンクである」という意図を明示的に伝える役割がある  
  
erbでリンクを生成するにはRailsヘルパーの```link_to```を使用する  
```link_to```の第一引数はリンクテキスト、第二引数はURL  
第３引数はオプション引数となっていて、ハッシュを引数に取る。ハッシュを引数に取ることで柔軟にHTMLタグのオプションに対応することができる。  
例えば次のような埋め込みRubyによる記述は  
```rb
<%= link_to "Sign up now!", "#", class: "btn btn-lg btn-primary" %>
```
このようなリンクタグを生成する
```html
<a href="#" class="btn btn-lg btn-primary">Sign up now!</a>
```  
  
画像を使いたい場合はlink_toとimage_tagを使う
次のように書く  
```rb
<%= link_to image_tag("rails.png", alt: "Rails logo"), "http://rubyonrails.org" %>
```
link_toの第一引数にimage_tagが与えられている  
この埋め込みRubyは次のHTMLタグの記述に等しい
```html
<a href="http://rubyonrails.org"><img src="rails.png" alt="Rails logo"></a>
```
このようにlink_toの第一引数にimage_tagなどのヘルパーを取ることで入れ子のタグ構造を再現することができる  
image_tagの```alt: "Rails logo"```もオプションハッシュとなっている  
  
また、表示に使う画像はapp/assets/imagesに置く。ここに置いたものがimage_tagで使われている  
  
  
### BootstrapとカスタムCSS  
BootstrapとはTwitter社が公開しているオープンソースのWebデザインフレームワーク  
Bootstrapを使用すると、洗練されたWebデザインとユーザーインターフェイス要素を簡単にHTML5アプリケーションに追加することができる  
Bootstrapを使うことでアプリケーションをレスポンシブウェブデザイン(スマホやPCなど様々なデバイスに最適化できるデザイン)にできる  
railsにBootstrapを適用させるにはgemファイルに以下を追加する(必要に応じてバージョンを記述する)  
```
gem 'bootstrap-sass', '3.3.6'
```
  
カスタムCSSはapp/assets/stylesheetsディレクトリに.scssのファイルを置くことで使うことができる  
scssはSass(Sassy CSS)と呼ばれるCSSを拡張した言語　Sassを処理できる  
次のようにカスタムCSSの@importを使うことでBootstrapを使うことができる  
```
@import "bootstrap-sprockets";
@import "bootstrap";
```
  
HTMLファイルにclassオプションを加えることでBootstrapのデザインを指定できる  
例えば  
```html
<header class="navbar navbar-fixed-top navbar-inverse">
```
や  
```html
<div class="container"
```
のように。
  
ちなみにcssは適用対象にクラス、ID、HTMLタグまたはその組み合わせを指定できる  
クラスを対象とする場合、クラス名の先頭にドット「.」を付ける  
例:
```css
.center {text-align:center;}
```
IDの場合は「.」の代わりに「#」を先頭に付ける  
同じページに一度しか使わない場合にIDを用いる  
また、IDを使う利点としては、一度しか使われないというメッセージ性を付与することができる以外に、ページ内ジャンプに使うこともできる  
たとえば  
```html
<a href="#footer">ページ末尾へ</a>
```
で  
```html
<div id="footer">
.
.
.
</div>
```
の部分に飛ぶことができる　　
  
  
### パーシャル  
埋め込みRubyの記述を別に押しやって記述を簡略化するにはパーシャルが用いられる  
renderというヘルパーを用いて実現する  
```rb
<%= render 'layouts/hoge' %>
```
この例では```app/views/layouts/_hoge.html.erb```というファイルから省略されている記述が挿入される  
このようにパーシャルとして使われるファイルの名前には先頭に「_」を付ける命名規則がある  

