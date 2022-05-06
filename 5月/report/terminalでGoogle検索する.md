---
marp: true
backgroundColor: #333
color: #f2f2f2
style: |
  * {
    color: #f2f2f2 !important;
    background: #333 !important;
    width: 100%;
  }
  header {
    padding-top: .4rem;
    font-size: .8rem;
  }
paginate: true
---

# ターミナルで Google 検索をする

## WD3A 岡崎 流依

---

## 目次

1. 作成に至った背景
2. 完成した物
3. 実際のコード
4. 解説

---

# 作成に至った背景

---

<!-- _header: 作成に至った背景 -->

## 普段から開発をしていると、<br/>英語やエラーを Google で検索したいことがありますよね

---

<!-- _header: 作成に至った背景 -->

## そこで今回はターミナルで Google 検索をできたらいいなぁということでシェルスクリプトを作成しました！

---

# 完成したものがこちら

---

![image](./terminal-google.gif)

---

# 実際のコード

---

```
g() {
  url='https://google.co.jp/search?q='
  for arg;
  do
    query=`echo $arg | nkf -WwMQ | tr = % | tr -d '\n' | sed -e 's/%%/%/g'`
    url="${url}${query}+"
  done
  open -a "Google Chrome" $url
}
```

---

# 解説

---

<!-- header: 解説 -->

## ターミナルからブラウザを起動する方法

---

### open コマンドに -a オプションを付けて、アプリ名を指定すると開ける

```
$ open -a "Google Chrome"
$ open -a "Firefox"
$ open -a "Safari"
```

### 指定した URL を開く方法

```
$ open https://google.co.jp -a "Google Chrome"
$ open -a "Google Chrome" https://google.co.jp
```

### URL で Google 検索をする方法

```
$ open -a "Google Chrome" "https://google.co.jp/search?q=東京+天気"
```

---

## ターミナルで実行できる関数を作る

---

### [ g + 検索したいワード ]で検索するようにしたいので、関数名は g にする

引数は arg で受け取っているので url の後に引数をくっつけていく

```
g() {
  url='https://google.co.jp/search?q='
  for arg;
  do
    url="${url}${arg}+"
  done
  open -a "Google Chrome" $url
}
```

---

### 今作成した関数を試しに実行してみる

普段を zsh を使用しているので、~/.zshrc に書き込む

```
$ vi ~/.zshrc
```

更新を忘れずに

```
$ source ~/.zshrc
```

---

### 引数が英語だと検索できる！

でも、引数を日本語にすると場合エラーが出る。。。

```
$ g ほげ
The file /Users/s2200090/Desktop/https:/google.co.jp/search?q=ほげ+ does not exist.
```

---

## なぜエラーになるのか？

日本語が使用出来ないため、エラーが出る

---

## google 検索の URL を再度見てみる

一見普通に見える

```
https://www.google.com/search?q=天気
```

でも、コピーすると日本語じゃなくなってしまう

```
https://www.google.com/search?q=%E5%A4%A9%E6%B0%97
```

---

天気 → %E5%A4%A9%E6%B0%97

## これは日本語がパーセントエンコードされて使用されている！

---

## じゃあターミナルでエンコードしよう！

調べていると nkf コマンドを使用するのが良さそう！
使用してみるとコマンドがないらしい

```
$ command not found: nkf
```

---

## Homebrew でインストールするよ〜

```
$ brew install nkf
```

---

### nkf コマンドの使い方

詳しくは man コマンドで見ておくれ〜

```
$ nkf <オプション> <パス/ファイル名>
```

---

## 試してみる

```
$ echo 天気 | nkf -WwMQ
=E5=A4=A9=E6=B0=97
```

= を % に置き換える

```
$ echo 天気 | nkf -WwMQ | tr = %
%E5%A4%A9%E6%B0%97
```

長い文章を打ってみる

```
$ echo eccコンピュータ専門学校 | nkf -WwMQ | tr = %
ecc%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E5%B0%82%E9%96%
%80%E5%AD%A6%E6%A0%A1
```

---

改行を削除する

```
$ echo eccコンピュータ専門学校 | nkf -WwMQ | tr = % | tr -d '\n'
ecc%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E5%B0%82%E9%96%%80%E5%AD%A6%E6%A0%A1%
```

改行の位置に % が 2 つ続いてしまうのを削除して 1 つにする

```
$ echo eccコンピュータ専門学校 | nkf -WwMQ | tr = % | tr -d '\n' | sed -e 's/%%/%/g'
ecc%E3%82%B3%E3%83%B3%E3%83%94%E3%83%A5%E3%83%BC%E3%82%BF%E5%B0%82%E9%96%80%E5%AD%A6%E6%A0%A1%
```

---

## 実際に関数に適用させる

echo の結果を変数に格納したいのでバッククォートで囲う

```
query=`echo $arg | nkf -WwMQ | tr = % | tr -d '\n' | sed -e 's/%%/%/g'`
```

全体のコード

```
g() {
  url='https://google.co.jp/search?q='
  for arg;
  do
  query=`echo $arg | nkf -WwMQ | tr = % | tr -d '\n' | sed -e 's/%%/%/g'`
    url="${url}${query}+"
  done
  open -a "Google Chrome" $url
}
```

---

# vim で開いて再読み込みしたら完成〜！！🥳🥳🥳

---

# 参考

Mac でターミナルからブラウザを立ち上げる
https://kazuph.hateblo.jp/entry/2012/09/20/091710

<br/>

url エンコード（コマンドライン／シェルスクリプト）
https://qiita.com/kkdd/items/d5a32b5981ed4b41d384
