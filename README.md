# 東京都の国基準tコロナデータの csv 化

## URL

https://github.com/sarkov28/c19_tokyo_kunishihyou

## Features

東京都が https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/kunishihyou.html で公表している国基準データ pdf のうち、2021-07 末以降に waybackmachine（web.archive.org）で入手可能なものの csv 化を試みました。

今後は、自動ダウンロードしたデータ pdf からも csv 化してく予定です。  
2021-08-02 時点で、2021-07-06〜2021-08-01 のデータが入手可能でした。

ファイル名が t1 で始まっているファイルは、
- 全日付のデータを1つのファイルに入れてあります。
- 日付のカラムを追加してあります。

ファイル名が t2 で始まっているファイルは、
- 日付毎にファイル化されています。
- pdf を比較的そのまま csv 化したファイルです。

t1 の形式、t2 の形式、どちらにおいても、
- (1a) 各日付において、データ行（＝表頭ではない行）は7行あります。
- (1b) 7行のデータの順序は安定していると思われます。
- (1c) データ行の右2つのカラム（＝実質的なデータカラム）は安定していると思われます。
- (1d) その左2つのカラムは、不安定です。

(1c) に示したように、実質的なデータのカラムは安定していると思われます。

t1 形式のファイルは、プログラム等で処理しやすいことを意図して作成したのですが、フォーマットの揺れがあるので微妙です。

## Note

このデータの作成にあたっては、python を使い、tabula.read_pdf() で DataFrame に変換してから処理しました。

この際、以下の問題がありました。  
これらが tabula.read_pdf() を含む処理系の問題なのか、ソースデータの pdf の問題なのかは調べていませんが、(2b) に書いたフォーマットの変化を考えると、ソースデータの問題なのかも知れません。

- (2a) 2021年7月の短い期間の間にも、変換した DataFrame のフォーマットが何度も変化しています。
- (2b) pdf の表示ではデータの入っているカラムが、DataFrame では無くなっている（空欄になっているのではない）ことがあります。（私は読み込んだデータを全て出力しています。ある日に存在したカラムが、別の日に無くなっていることがあります。）

このため、得られた csv は、残念ながらよく注意しながら取り扱う必要があります。

## Detail

pdf ファイルから、DataFrame にするには、上述したように python + tabla.read_pdf() を使っています。  
得られた DataFrame はそのままでは表になっていなかったので、適宜加工しています。

pdf ファイルの入手方法は、2021-08-01 以前と以後で異なります。

2021-08-01 以前は、以下の方法です。
- pdf ファイルの url は、7月31日の場合、url=https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/kunishihyou.files/kuni0731.pdf である。
- url がわかると、そのファイルのアーカイブが waybackmachine に保存されているのか、また保存されているならその url は何かを、https://archive.org/help/wayback_api.php の先頭に記載されている API で知ることができる。
- この url をダウンロードすれば、pdf ファイルが入手できる。

2021-08-02 以後は、以下の方法です。
- https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/kunishihyou.html の記載から、pdf ファイルの url を知ることができる。
- この url をダウンロードすれば、pdf ファイルが入手できる。

## Plan

東京都は、このリポジトリの元となるデータ
https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/kunishihyou.html
を、休日も含め、毎日更新しているようです。

このリポジトリは、当面の間、週に1度程度は更新しようと思っています。

2021-08-01 までのデータは、waybackmachine（web.archive.org）のデータに依存しています。この方法だと、waybackmachine にデータがないと処理出来ません。

2021-08-02 以降は、東京都のサイト https://www.fukushihoken.metro.tokyo.lg.jp/iryo/kansen/corona_portal/info/kunishihyou.html から該当のデータ pdf を手元に自動でダウンロードするようにしたので、（waybackmachine にデータがなくても）そこから処理できるはずです。

ただし、手元のPCにトラブルがあった場合には、この自動ダウンロードが成功しない可能性があります。  
この場合、該当する csv が作成できない日が生じる可能性があります。

また、元となるデータ pdf の発表方法やフォーマットなどに大きな変化があった場合は、更新を諦めるかも知れません。

## FAQ

いかにも疑問に思われそうな箇所についての予めの回答です。

(1)  
問  
t1 の形式のファイルには、なぜ表頭の行（＝"日付" で始まる行）が複数あるのか。  
答  
表頭に該当する行も日によって一定しておらず、不安定です。  
例えば、07-16 の表頭の行は、"区分" の次のカラムがありません。  
他を参照するなどしてこれを私が埋めるのは不適切だと考えました。  
この状態をそのまま示すため、表頭の行を各日付で出力しています。

## Author

https://twitter.com/sarkov28

twitter やブログに、コロナに関することを色々と書いています。

- 新型コロナ関連の主な事柄のリスト  
  https://sarkov28.hatenablog.com/entry/2021/01/25/193020
- 新型コロナ関連の主な事柄のリスト（特に重要な項目）  
  https://sarkov28.hatenablog.com/entry/2021/01/25/193753

## License

このリポジトリは、東京都公表のデータを加工したものです。  
自由に使っていただいて構いませんが、内容は保証出来ません。  
フォーマットの揺れがあるため、思わぬ問題が発生している可能性があります。
