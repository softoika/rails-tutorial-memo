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
第３引数はオプション引数となっていて、ハッシュを引数に取る。  