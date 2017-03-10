RubyもしくはRailsに標準で用意されている関数を組み込みヘルパー、それに対して新しく作った関数をカスタムヘルパーと呼ぶ  
  
Pythonと違って関数やif、elseの行末に「:」はいらない。どの構文もendで終える  
メソッド名に記号を使うことができる 「?」など→Rubyにはbool値を返すメソッド名の末尾に?を付ける習慣がある isEmptyではくempty?    
文字列メソッドempty?は空文字でもtrueになるみたい。provideが実行されてなくてもtrue  
rubyではあらゆるものがオブジェクト。nilもオブジェクトなのでメソッドを持つ(nil.to_sで空文字列を返す)

Rails Console(Rails用のインタラクティブ環境)には"development"と"test"、"production"の3つの環境がある  
デフォルトではdevelopment環境が選択される  
これに関しては7章で詳しく説明される  
  
RubyのAPIリファレンスとしては[Rubyリファレンスマニュアル (通称: るりま))](https://docs.ruby-lang.org/ja/)や[るりまサーチ]は(https://docs.ruby-lang.org/ja/search/)が便利  
  
モジュールはメソッドをまとめておくもの```application_helper.rb```のように  
```rb
module ApplicationHelper
end
```
のようにして囲む。  
include文を使用するとモジュールを取り込むことができる(ミックスイン[mixed in])  
Rubyでは明示的にincludeしないと使用できないが、Railsでは自動的にヘルパーモジュールをインクルードしているので  
includeを書かなくてもいい  
  
### 文字列  
文字列の結合は「+」  
Rubyには文字列内で式展開ができる  
```rb
first_name = "Michael"
"#{first_name} Hart" # -> "Michael Hart"
```
シングルクオートによる文字列は式展開をしない  
文字列メソッドにはempty?以外にnil?やinclude?がある。include?は文字列を引数に取る(contain?ではない)  
文字列メソッドsplitの初期引数は' '
  
  
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
  
### 関数  
Pythonと同じく引数に初期値の設定ができる  
関数の呼び出し時には「(」「)」を省略できる  
  
Rubyの関数には暗黙の戻り値がある。  
暗黙の戻り値は最後に評価された式の値が返される  
```rb
def string_message(str='')
  if str.empty?
    "It's an empty string!"
  else
    "The string is nonempty."
  end
end
```
この場合returnが書かれていないにも関わらず最後に評価された"It's an empty string!"か"The string is nonempty."のどちらかが返される。  
  
上の関数は次のような書き方もできる
```rb
def string_message(str='')
  return "It's an empty string!" if str.empty?
  return "The string is nonempty."
end
```
最後のreturnは省略できる。  
  
### 配列  
添字はPythonと同じように-1で末尾を指定できる。例：```a[-1]```
配列メソッドでも特定の要素を参照できる。```a.first```は```a[0]```に等しい。```a.last```は```a[-1]```に等しい  
```a.second```もある。  
配列の長さはlengthメソッドで取得できる  
また、sort!メソッドで配列をソートできる。Rubyでは末尾に!を付けたメソッドを破壊的メソッドと呼び、  
sort!のように配列の内容を変更するメソッドが破壊的メソッドと定義される。  
pushメソッドで引数に指定した要素を配列の末尾に加えることができる  
また、「<<」演算子でもpushメソッドと同じ.例:```a << 6```は```a.push 6```と同じ。```a << 6 << 9```のような使い方もできる。
  
範囲(range)と呼ばれる書き方もある。```0..9```と書いて0から9を表す。  
配列に変換するときはto_aメソッドを使う.例:```(0..9).to_a```。カッコは省略できない。```('a'..'e').to_a```のように連続する文字にも対応している。  
範囲は添字として使うことができる。例：```a[0..2]```でa[0],a[1],[2]の範囲の配列を取得できる。  
