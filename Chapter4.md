RubyもしくはRailsに標準で用意されている関数を組み込みヘルパー、それに対して新しく作った関数をカスタムヘルパーと呼ぶ  
  
Pythonと違って関数やif、elseの行末に「:」はいらない。どの構文もendで終える  
関数の呼び出し時には「(」「)」を省略できる
メソッド名に記号を使うことができる 「?」など  
Pythonと同じく引数に初期値の設定ができる
文字列メソッドempty?は空文字でもtrueになるみたい。provideが実行されてなくてもtrue  

Rails Console(Rails用のインタラクティブ環境)には"development"と"test"、"production"の3つの環境がある  
デフォルトではdevelopment環境が選択される  
これに関しては7章で詳しく説明される  
  
RubyのAPIリファレンスとしては[Rubyリファレンスマニュアル (通称: るりま))](https://docs.ruby-lang.org/ja/)や[るりまサーチ]は(https://docs.ruby-lang.org/ja/search/)が便利  
  
### 文字列  
文字列の結合は「+」  
Rubyには文字列内で式展開ができる  
```rb
first_name = "Michael"
"#{first_name} Hart" # -> "Michael Hart"
```
シングルクオートによる文字列は式展開をしない  
  
  
### 標準出力
rubyにおける標準出力はputsを使う
```rb
puts "hoge" # -> nil
```
putsはnil(他の言語でいうnull)を返す  
  
rubyにもprint関数はあるが、putsが行末に改行が入るのに対し、printは末尾に改行が入らない

