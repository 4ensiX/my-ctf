# CTFの練習問題(2019/05/06)
この問題の問題文は特にありません。CTFやる上で覚えておいて欲しいこと、コマンドを詰め込みました。しかしぶっちゃけCTFのForensicsのために必要なことです。
# Writeup
## 問題ファイルの取得
ご存知の通りコマンドライン上でファイルをダウンロードするには</br>
```bash
$ wget https://github.com/StegX/master/raw/master/clpwn/ctf_introductry/practice/data
```
それでは、与えられたファイルを見てみましょう。お分かりの通り拡張子がありませんのでファイルの形式がはっきりしません。ファイルマネージャーによっては、もうどんなファイルか分かっているのかもしれませんがここでは拡張子が分からないとして話を進めていきます。また、この説明はターミナルが使えることを前提としています。もっと早く解く方法もありますがコマンドとファイルの仕組みを知るためには、今回の方法で解くのがいいでしょう。
## ファイルの形式の確認
与えられたファイルがどんな形式かわからないときに使うコマンドは</br>
```bash
$ file [ファイル名]
```
これを使えば今回のファイルの形式が分かります。このような状況では必須のコマンドなので覚えておいてください。分かっているとは思いますが現在いるディレクトリにfileコマンドを使いたいファイルがない場合、例えばディレクトリ名 ctfに入っている dataに対しfileコマンドを使いたい場合</br>
```bash
$ file ./ctf/data
```
というように使います。
### 名前の変更
この操作は行わなくても支障はありませんが簡単のために行います。おそらく先ほどのコマンドで dataがzipファイルであると分かったと思うので拡張子をつけておきましょう。名前の変更を行えばいいので</br>
```bash
$ mv [変更前の名前] [変更後の名前]
```
のように mvコマンドを使ってください。なおこのコマンドはディレクトリの名前を変更したいときにも使えます。今回の場合</br>
```bash
$ mv data data.zip
```
のようにするといいでしょう。
## zipファイルの解凍
zipファイルは圧縮されたファイルです。なので解凍を行いたいと思います。解凍をするためのコマンドは</br>
```bash
$ unzip [ファイル名]
```
です。これを行えば大抵は解凍できます。まずはこれを打ち込んでみましょう。
## パスワードの捜索
おそらくパスワードの入力を求められたと思います。なのでパスワードを探しましょう。与えられたファイル以外にヒントがない場合大抵行うべきことは、
#### 1. ファイルのバイナリを確認する
コンピュータにはファイルが2進数で記録されいます。これがバイナリです。これから、よくバイナリを見るという言葉に出会うと思いますが、これはこの2進数を16進数に変換したものを見ることをいいます。
#### 2. ファイルのExif情報を確認する
exif情報とは、そのファイルが作成された時間、作成された環境などが記録されたものです。最近のctfではここにヒントがあることはあまりありません。
#### 3. 検索する
本当にどうしよもなくなった時は、気になった言葉、引っかかるワードなど検索にかけてみましょう。思わぬところからflagにつながるかもしれません。
#### 4. シックスセンスを信じろ
人間には第六感というものがあり、これは時にものすごい力を発揮するのです。あなたもこの力に救われたことがあるのではないでしょうか。要するに感です。推測力です。</br>

今回利用するのは「1.バイナリを見る」です。そこで使うのは stringsコマンド</br>
```bash
$ strings [ファイル名]
```
というように使います。このコマンドは正確にはバイナリの16進数をASCIIで変換したものを見ています。なので16進数のうちASCIIで変換できないものは表示されていません。よってこの部分にヒントがある場合は意味がないことを頭の片隅にでも置いておいてください。しかし今回はしっかり仕事をしてくれます。これでパスワードが分かった思うので unzipをもう一度使い、パスワードを入力しましょう。
### パスワードが通らない場合
あなたはよほど落ち着きがないか、他の知識のせいで今回は間違いである入力をしている可能性があります。他の知識で入力間違ったあなたはいい推測力をしています。実はその勘違いは問題の後半で役に立ちます。パスワードは素直に入れましょう。pass:onfr8k8<<rot13と書いてあるんですから。
## zipファイルの中身
パスワードが通ったあなたはzipファイルの中身をみることができたでしょう。すると exam.txtというファイルが見つかりましたね。これを開いてみると何だかよく分からない文字列が出てきます。パッと見てこれが何だか分かればそれは素晴らしいです。ですがそうでない人のためにヒントはもう提示されています。少し考えてみて欲しいです。結果的にはヒントはパスワードにありました。パスワードをいくつかの方法で検索にかけてみると出てくるのではないでしょうか。
#### ヒント
パスワードを分解して検索。そして出てきた結果をパスワードに示されているように利用する。とりあえず素直にやればよい。おそらく多くはサイトを利用するのではないでしょうか。
## exam.txtの中身の意味
ここまでくれば exam.txtが何で書かれているのか分かったでしょう。まだよく分からなけらば掛け算を計算して、掛け算をその結果に置き換えてそれを検索してみましょう。そうすれば出で来るはずで す。base64という言葉が。ということで base64を使ってこの文字列をデコードしてください。ここまでこの文章読んだあなたが、しっかりとこの問題を考えながら読んでいることを願います。この変換 はサイトを多くはサイトを利用すると思いますがコマンドライン上でもできます。しかし、今ここでは示しません。
## base64で変換して
出てきたものが何であるか、パット見てわからなければあなたは人生経験が足りません。これについては何も言うことありません。この文字列？を変換したメッセージが今回のflagのようなものです。お疲れ様でした。
# 今回のflagまでの解答
```bash
$ file data
data: Zip archive data, at least v2.0 to extract
$ mv data data.zip
$ unzip data.zip
Archive:  data.zip
[data.zip] exam.txt password: 
   skipping: exam.txt                incorrect password
$ strings data.zip
exam.txtUT
([ux
>aPK
exam.txtUT
([ux
pass:onfr8k8<<rot13
$ unzip data.zip
Archive:  data.zip
[data.zip] exam.txt password: onfr8k8<<rot13
  inflating: exam.txt
$ cat exam.txt
Li0tIC4gLi0uLiAtLi0uIC0tLSAtLSAuIC4uLS0uLSAtIC0tLSAuLi0tLi0gLS4tLiAtIC4uLS4=
$echo "Li0tIC4gLi0uLiAtLi0uIC0tLSAtLSAuIC4uLS0uLSAtIC0tLSAuLi0tLi0gLS4tLiAtIC4uLS4="| base64 -d
.-- . .-.. -.-. --- -- . ..--.- - --- ..--.- -.-. - ..-.
```
このモールス信号を変換して　WELCOME_TO_CTF</br>
小文字でも大文字構わない、_は＃とかになっているかもしれない。
# 覚えておいて欲しいこと

## コマンド
```bash
$ file [filename]   #重要度　超高
$ mv [変更前の名前] [変更後の名前]　#常識
$ unzip [ファイル名]　#重要度　中
$ strings [ファイル名]　#重要度　高
```
## 用語
バイナリ、rot13、base64、モールス信号
