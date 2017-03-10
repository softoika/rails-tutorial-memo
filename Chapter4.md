RubyもしくはRailsに標準で用意されている関数を組み込みヘルパー、それに対して新しく作った関数をカスタムヘルパーと呼ぶ  
  
Pythonと違って関数やif、elseの行末に「:」はいらない。どの構文もendで終える  
関数の呼び出し時には「(」「)」を省略できる
メソッド名に記号を使うことができる 「?」など→Rubyにはbool値を返すメソッド名の末尾に?を付ける習慣がある isEmptyではくempty?    
Pythonと同じく引数に初期値の設定ができる
文字列メソッドempty?は空文字でもtrueになるみたい。provideが実行されてなくてもtrue  
rubyではあらゆるものがオブジェクト。nilもオブジェクトなのでメソッドを持つ(nil.to_sで空文字列を返す)

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
文字列メソッドにはempty?以外にnil?やinclude?がある。include?は文字列を引数に取る(contain?ではない)  
  
  
### 標準出力
rubyにおける標準出力はputsを使う
```rb
puts "hoge" # -> nil
```
putsはnil(他の言語でいうnull)を返す  
  
rubyにもprint関数はあるが、putsが行末に改行が入るのに対し、printは末尾に改行が入らない

### 制御構文  
条件分岐はif、else、そしてelsif  
else ifでもelifでもない
論理演算子には&&,||,!のほかに(Pythonのように)and, or, notが使える
  
rubyには「後続if」という特殊な書き方がある  
```rb
if !x.empty?
  puts "x is not empty"
end
```
という書き方を  
```rb
puts "x is not empty" if !x.empty?
```
と書き換えることができる  
　　
オブジェクト + 論理演算子という書き方は論理値を必ず返すので、二重否定をすれば(```!!obj```と書く)必ずそのオブジェクトの論理値を返す。
