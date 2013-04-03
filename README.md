Ryudai.rb #6

## ソースとか
このREADMEは[github](http:google.com)にあります(まだ書いてない)
@tompng さんの [lisp2.rb](https://gist.github.com/tompng/5222780)
私の [lisp.rb](https://gist.github.com/atton-/5300354)

## あなた何したの
基本的にペンさんのをベースにしてます

* Parser に BasicObject を使うように
* defun で引数が少なかったら arguments error 出すように

してみました

## んで何なの
ruby で lisp っぽい何かです

## 何してるの
Ruby だと (hoge fuga,(piyo 1,2)) とかできるのでlispっぽくしてみた物体らしいです

* method_missing を利用して、呼びたいメソッドの名前と引数のリストにする
* メソッドの名前をkeyにしたHashを持つ
* Hash から引くことでメソッドとかを呼びます

## メソッド
`@methods` にメソッド類があります。 Hashです。

## メソッド呼び出し
Hash に、メソッド名を key にして lambda を入れてます
呼び出しの時はまんま メソッド名を key にして Hash から lambda を取ってきて call します

## メソッド定義
`:defun ` を自前で定義しています
メソッド名、 引数(任意の数)、 処理を取ります
処理を lambda にしてメソッド名をkeyにしてHashに格納してます

## 変数
ChainHash で持ってます。

## 変数の定義
変数名をkeyにしてvalueを持ちます
なんでChainするかというと、要はスコープです

## 変数の参照
メソッド呼び出しの時とかはHashが追加されるので、グローバルなxとローカルなxがあればローカルなxが参照されます
逆に、グローバルなyがあってメソッドの引数はxしか無いのにメソッドでyを呼べば、グローバルなyが呼ばれます

## パーサ
method_missing です。

## なして method_missing で parse してるの
(hoge fuga) は本来
メソッド hoge に 引数 fuga を渡す
ものです
引数から解釈されるので
method_missing が呼ばれて
:fuga, [] が渡されます
その後に hoge が呼ばれて
:hoge, :fuga
な感じになります。

引数から評価されるので作用的にできますね(あってる?)
ってな感じで parse していきます。

## そんな感じで lisp い
実際 parse してるのは do end な block でくくった部分を instance_eval ってたりとか
それからの method_missing
んでHashに入れたりHashにあるか確認したりして実行

## おまけ
なんかメソッド呼び出しの時に arguments error とか実装できそうな気もしてきた
