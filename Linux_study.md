
### grep

オプション
-r：カレントディレクトリの配下のディレクトリを含めて検索する
-E：正規表現を使用する

#### fan、pat、milesを含むファイルを検索する
```
MACBOOK:player Ken$ grep -r -E "(fan|pat|miles)" .
./guitar:pat metheny
./bass:john pattituti
./All_Musicians:fanfan
./All_Musicians:pat metheny
./All_Musicians:john pattituti
./horn:fanfan
./horn:miles davis
```
#### jから始まって、iで終わる行の検索

```
grep -r -E "^j.*$i" .
```
grep
正規表現
^ : 行頭
$ : 行末
. : 任意の1文字
\*: 直前の表現が0回以上

コマンドオプション
-r : 再帰検索
-E : 正規表現
最後の. : 検索起点のパス

```
MACBOOK:player Ken$ grep -r -E "^j.*$i" .
./bass:john pattituti
./All_Musicians:john pattituti
```

#### grep結果をファイルに出力する（リダイレクト）

\> : リダイレクト

grep -r -E "john" . > grep_john

```
MACBOOK:music Ken$ grep -r -E "john" ./player > grep_john.txt
MACBOOK:music Ken$ cat grep_john.txt
./player/bass:john pattituti
./player/horn:john coltrane
MACBOOK:music Ken$
```

#### grep結果をcatなどで表示する（パイプ）

catにパイプ
```
MACBOOK:music Ken$ grep -r -E "john" ./player | cat
./player/bass:john pattituti
./player/horn:john coltrane
```

#### find結果をgrepする

xargsをかまさないと動かない
```
MACBOOK:pocket Ken$ find . -name '*.md' | xargs grep -E 'commit'
./Linux/Linux_テキスト処理.md:grep --color -r commit *
./Linux/Linux_テキスト処理.md:commit とうい文字列を含む行を検索
./Linux/Linux_テキスト処理.md:MACBOOK:pocket Ken$ grep --color -r commit *
./Linux/Linux_テキスト処理.md:git/git ローカルブランチの作成からリモートへのプッシュまで.md:ローカルで更新、add、commit

```
findからxargsを経由せずにgrepする方法
```
MACBOOK:pocket Ken$ find ./music -type f -exec grep -E 'john.*' {} +
./music/grep_john.txt:./player/bass:john pattituti
./music/grep_john.txt:./player/horn:john coltrane
./music/All_Musicians:john pattituti
./music/player/bass:john pattituti
./music/player/horn:john coltrane
MACBOOK:pocket Ken$ 
```
#### xargs
重要コマンド！

前のコマンドの結果を受けっとって、次のコマンドの引数に引き渡す

対処ファイルのgrep
find から grep。grepの前にxargsをはさむ

findコマンドについて
https://qiita.com/ko1nksm/items/7fec71f78a394a80ed2b
```
MACBOOK:music Ken$ find ./player -type f
./player/piano
./player/drums
./player/guitar
./player/bass
./player/horn
```
```
MACBOOK:music Ken$ find ./player -type f | cat
./player/piano
./player/drums
./player/guitar
./player/bass
./player/horn
```

```
MACBOOK:music Ken$ find ./player -type f | xargs cat
bil evans
bud powell
art brakey
elvin jones
wes mongomery
pat metheny
john pattituti
anthony jackson
hino terumasa
fanfan
miles davis
lee morgan
john coltrane
```


#### treeでファイル確認

```
MACBOOK:music Ken$ pwd
/Users/Ken/Library/Mobile Documents/iCloud~md~obsidian/Documents/pocket/music
MACBOOK:music Ken$ tree
.
└── player
    ├── All_Musicians
    ├── bass
    ├── drums
    ├── guitar
    ├── horn
    └── piano

2 directories, 6 files
```

musicフォルダ -> player -> 楽器別のファイル

ファイルの一覧
```
MACBOOK:player Ken$ ls -l
total 48
-rw-r--r--  1 Ken  staff  146  4  5 22:01 All_Musicians
-rw-r--r--  1 Ken  staff   31  4  5 22:01 bass
-rw-r--r--  1 Ken  staff   23  4  5 22:01 drums
-rw-r--r--  1 Ken  staff   26  4  5 22:01 guitar
-rw-r--r--  1 Ken  staff   44  5  6 14:03 horn
-rw-r--r--  1 Ken  staff   21  4  5 22:01 piano
```

各ファイルの内容
All_Musicians
```
MACBOOK:player Ken$ cat All_Musicians
hino terumasa
fanfan
meiles davis
lee morgan
wes mongomery
pat metheny
bil evans
bud powell
john pattituti
anthony jackson
art brakey
elvin jones
```

guitar
```
MACBOOK:player Ken$ cat guitar
wes mongomery
pat metheny
```
horn
```
MACBOOK:player Ken$ cat horn
hino terumasa
fanfan
miles davis
lee morgan
MACBOOK:player Ken$ 
```



---
### 練習シナリオ2 ファイル操作
- ディレクトリ作成
- ファイルをマージして新しいディレクトリに出力
- ファイルをディレクトリに移動
- ファイルの名前変更
- ファイルの階層表示

#### 元ネタファイル作成
```
MACBOOK:pocket Ken$ cat <<eof >trumpet
> hino terumasa
> fanfan
> meiles davis
> lee morgan
> eof
MACBOOK:pocket Ken$ cat <<eof >piano
> bil evans
> bub powell
> eof
MACBOOK:pocket Ken$ cat <<eof >guitar
> wes mongomery
> pat metheny
> eof
MACBOOK:pocket Ken$ cat <<eof >bass
> john patittuti
> anthony jackson
> eof
MACBOOK:pocket Ken$ cat <<eof > drums
> art brakey
> elvin jones
> eof
MACBOOK:pocket Ken$ ls
Linux		SQL		docs		git		piano
Linux_study.md	bass		drums		guitar		trumpet
MACBOOK:pocket Ken$ 
```
	musicianファイルを楽器別に作成
	このあと、music/playerディレクトリを作成する
	マージファイルを作る
	ファイルを移動する
	trumpetファイルをhornに変更


#### ディレクトリ作成
```
MACBOOK:pocket Ken$ mkdir music/player
mkdir: music: No such file or directory
MACBOOK:pocket Ken$ mkdir music
MACBOOK:pocket Ken$ mkdir music/player
MACBOOK:pocket Ken$ tree music
music
└── player

2 directories, 0 files
MACBOOK:pocket Ken$ 
```
	いきなりplayerディレクトリは作れない
	musicディレクトリを作ってから、その下の階層のディレクトリを作る
	tree表示

#### ファイルをマージして新しいディレクトリに出力
```
MACBOOK:pocket Ken$ cat trumpet guitar piano bass drums > music/player/All_Musicians
MACBOOK:pocket Ken$ cat music/player/All_Musicians
hino terumasa
fanfan
meiles davis
lee morgan
wes mongomery
pat metheny
bil evans
bub powell
john patittuti
anthony jackson
art brakey
elvin jones
MACBOOK:pocket Ken$ ls -l music/player/All_Musicians
-rw-r--r--  1 Ken  staff  146  3 26 09:51 music/player/All_Musicians
MACBOOK:pocket Ken$
```
#### ファイルをディレクトリに移動
mvコマンドで移動先のファイル名を間違えると無警告で上書きされるので、
cpコマンドでコピーしてから、元ファイルを削除したほうがよい

```
MACBOOK:pocket Ken$ tree music
music
└── player
    ├── All_Musicians
    └── bass

2 directories, 2 files
MACBOOK:pocket Ken$ ls
Linux		SQL		docs		git		music		trumpet
Linux_study.md	bass		drums		guitar		piano
```
	ファイル確認

```
MACBOOK:pocket Ken$ cp bass music/player/bass
MACBOOK:pocket Ken$ cp drums music/player/drums
MACBOOK:pocket Ken$ cp piano music/player/piano
MACBOOK:pocket Ken$ cp guitar music/player/guitar
MACBOOK:pocket Ken$ cp trumpet music/player/trumpet
MACBOOK:pocket Ken$ cd music
```
	ファイルコピー

```
MACBOOK:music Ken$ ls
player
MACBOOK:music Ken$ tree
.
└── player
    ├── All_Musicians
    ├── bass
    ├── drums
    ├── guitar
    ├── piano
    └── trumpet

2 directories, 6 files
MACBOOK:music Ken$ cd player
MACBOOK:player Ken$ cat All_Musicians
hino terumasa
fanfan
meiles davis
lee morgan
wes mongomery
pat metheny
bil evans
bud powell
john pattituti
anthony jackson
art brakey
elvin jones
MACBOOK:player Ken$ cat bass
john pattituti
anthony jackson
MACBOOK:player Ken$ cat drums
art brakey
elvin jones
MACBOOK:player Ken$ cat guitar
wes mongomery
pat metheny
MACBOOK:player Ken$ cat piano
bil evans
bud powell
MACBOOK:player Ken$ cat trumpet
hino terumasa
fanfan
meiles davis
lee morgan
MACBOOK:player Ken$ 

```
	ファイル確認

```
MACBOOK:pocket Ken$ rm bass
MACBOOK:pocket Ken$ rm drums
MACBOOK:pocket Ken$ rm guitar
MACBOOK:pocket Ken$ rm piano
MACBOOK:pocket Ken$ rm trumpet
MACBOOK:pocket Ken$ ls
Linux		Linux_study.md	SQL		docs		git		music
```
	ファイル削除

#### ファイルの名前変更
```
All_Musicians bass drums guitar piano trumpet
MACBOOK:player Ken$ mv trumpet horn
MACBOOK:player Ken$ ls
All_Musicians bass drums guitar horn piano
MACBOOK:player Ken$
```
	trumpet を horn に変更

#### ファイルの階層表示
```
MACBOOK:pocket Ken$ tree music
music
└── player
    ├── All_Musicians
    ├── bass
    ├── drums
    ├── guitar
    ├── horn
    └── piano

2 directories, 6 files
MACBOOK:pocket Ken$ 
```








---
### 練習シナリオ１ ファイル操作
- ファイル１を作成
- ファイル１に書き込み
- ファイル１をコピーしてファイル２を作成
- ファイル２に追加書き込み
- ファイル１とファイル２の差分を表示
#### ファイル１作成
```
MACBOOK:pocket Ken$ touch file1
MACBOOK:pocket Ken$ mv file1 music1
MACBOOK:pocket Ken$ mv music1 jazz
MACBOOK:pocket Ken$ ls -Fl
total 16
drwxr-xr-x  10 Ken  staff   320  3 24 06:34 Linux/
-rw-r--r--   1 Ken  staff  6393  3 24 06:30 Linux_study.md
drwxr-xr-x   3 Ken  staff    96  2 24 21:49 SQL/
drwxr-xr-x   6 Ken  staff   192  2 24 22:47 docs/
drwxr-xr-x   9 Ken  staff   288  2 24 22:44 git/
-rw-r--r--   1 Ken  staff     0  3 24 06:32 jazz
```
	
	touchでファイル作成、名前をmvで変更
	jazzファイルを作成

#### ファイル１に書き込み
- vi
	viエディタの画面は省略
	```
	MACBOOK:pocket Ken$ vi jazz
	MACBOOK:pocket Ken$ cat jazz
	miles davis
	john coltrane
	bil evans
	sonny lollins
	jaco pastrius
	wayne shorter
	wes montgomery
	MACBOOK:pocket Ken$ 
	```

- echo
	```
	MACBOOK:pocket Ken$ echo 'ozone makoto' >> jazz
	MACBOOK:pocket Ken$ cat jazz
	miles davis
	john coltrane
	bil evans
	sonny lollins
	jaco pastrius
	wayne shorter
	wes montgomery
	ozone makoto
	MACBOOK:pocket Ken$ 
	```

- cat
```
MACBOOK:pocket Ken$ cat <<eof >>jazz
> chick corea
> eof
MACBOOK:pocket Ken$ cat jazz
miles davis
john coltrane
bil evans
sonny lollins
jaco pastrius
wayne shorter
wes montgomery
ozone makoto
chick corea
MACBOOK:pocket Ken$ 
```

#### ファイル１をコピーしてファイル２を作成
```
MACBOOK:pocket Ken$ ls
Linux		Linux_study.md	SQL		docs		git		jazz
MACBOOK:pocket Ken$ cp jazz jazz2
MACBOOK:pocket Ken$ ls
Linux		SQL		git		jazz2
Linux_study.md	docs		jazz
MACBOOK:pocket Ken$ 
```
	cp

#### ファイル２に追加書き込み

```
MACBOOK:pocket Ken$ echo 'lee morgan' >> jazz2
MACBOOK:pocket Ken$ cat jazz2
miles davis
john coltrane
bil evans
sonny lollins
jaco pastrius
wayne shorter
wes montgomery
ozone makoto
chick corea
lee morgan
MACBOOK:pocket Ken$ 
```

#### ファイル１とファイル２の差分を表示
diff
-c : copied context形式
-u : unified context形式
-y : side-by-side形式

jazzの内容
```
MACBOOK:pocket Ken$ cat jazz
miles davis
john coltrane tenor sax
bil evans
sonny lollins
jaco pastrius
wayne shorter
wes montgomery
bud powell
ozone makoto
chick corea
```
jazz2の内容
```
MACBOOK:pocket Ken$ cat jazz2
miles davis trumpet
john coltrane
bil evans piano
sonny lollins
jaco pastrius
wayne shorter
wes montgomery
ozone makoto
chick corea
lee morgan
MACBOOK:pocket Ken$ 
```

diff オプション 変更前ファイル 変更後ファイル

```
MACBOOK:pocket Ken$ diff jazz jazz2
1,3c1,3
< miles davis
< john coltrane tenor sax
< bil evans
---
> miles davis trumpet
> john coltrane
> bil evans piano
8d7
< bud powell
10a10
> lee morgan
```
	diff オプションなしだとこんな
	<がjazz、>がjazz2を表す。数字は多分行数


```
MACBOOK:pocket Ken$ diff -c jazz jazz2
*** jazz	2024-03-24 10:42:09.000000000 +0900
--- jazz2	2024-03-24 10:18:15.000000000 +0900
***************
*** 1,10 ****
! miles davis
! john coltrane tenor sax
! bil evans
  sonny lollins
  jaco pastrius
  wayne shorter
  wes montgomery
- bud powell
  ozone makoto
  chick corea
--- 1,10 ----
! miles davis trumpet
! john coltrane
! bil evans piano
  sonny lollins
  jaco pastrius
  wayne shorter
  wes montgomery
  ozone makoto
  chick corea
+ lee morgan

```
	-c オプション copied context
	変更前、変更後がそれぞれ表示される
	変更後に削除された行が-
	変更後に追加された行が+
	修正行が!


```
MACBOOK:pocket Ken$ diff -u jazz jazz2
--- jazz	2024-03-24 10:42:09.000000000 +0900
+++ jazz2	2024-03-24 10:18:15.000000000 +0900
@@ -1,10 +1,10 @@
-miles davis
-john coltrane tenor sax
-bil evans
+miles davis trumpet
+john coltrane
+bil evans piano
 sonny lollins
 jaco pastrius
 wayne shorter
 wes montgomery
-bud powell
 ozone makoto
 chick corea
+lee morgan
MACBOOK:pocket Ken$ 
```
	 -uオプション unified context
	 ファイルがマージされて表示される
	 同一行はマージ
	 修正前は修正、削除とも-
	 修正後は修正、追加とも+
	 無印と+だけが今残っているとみればよい
	 慣れれば-cよりも見やすい


```
MACBOOK:pocket Ken$ diff -y jazz jazz2
miles davis						      |	miles davis trumpet
john coltrane tenor sax					      |	john coltrane
bil evans						      |	bil evans piano
sonny lollins							sonny lollins
jaco pastrius							jaco pastrius
wayne shorter							wayne shorter
wes montgomery							wes montgomery
bud powell						      <
ozone makoto							ozone makoto
chick corea							chick corea
							      >	lee morgan
MACBOOK:pocket Ken$ 
```
	-yオプション side by side
	追加、削除はわりと見やすい
	変更行は見にくい


---
### ファイルのマージ
cat ファイル１ ファイル２ > ファイル３
cat out file > file2

---
### ファイルの内容表示
	cat
		標準出力に出力する
	less
		テキスト表示
	more
		テキスト表示
	vi
		エディタ起動

---
### リダイレクト
コマンドの出力先を変更する
通常のコマンドの出力先は標準出力、エラー標準出力

1>
標準出力のの出力先変更

2>
エラー標準出力の出力先変更

\>
1>の省略形

\>>
追加書き込み

echoで標準出力（画面）に出す
```
MACBOOK:pocket Ken$ echo "山形 景"
山形 景
MACBOOK:pocket Ken$ 
```

echoの出力を画面からファイルに変更
```
MACBOOK:pocket Ken$ echo "山形 景" > out
MACBOOK:pocket Ken$ cat out
山形 景
MACBOOK:pocket Ken$ 
```
cat の出力先をoutファイルに変更して追加書きする

cat out >> out
なぜかループする
outに書き込んだものをさらに標準入力して続けちゃうみたい
EOFを伝える必要あり

control +  d : EOF入力
control + c : キャンセル

ヒアドキュメント
cat << EOF
EOFの文字列が入力されるまで標準入力を受け付ける
control + d で止めるのと同じようなもの

ヒアドキュメントの標準入力をcatに読み込ませてoutファイルに出力する
文字列EOFが入力終了の目印になる
```
MACBOOK:pocket Ken$ cat <<EOF > out
> ---<Start>---
> Trek
> Giant
> Specialized
> Cannondale
> Bianchi
> ---<End>---
> EOF
```

outファイルの内容を確認
```
MACBOOK:pocket Ken$ cat out
---<Start>---
Trek
Giant
Specialized
Cannondale
Bianchi
---<End>---
```

同じようにoutファイルに追記する
```
MACBOOK:pocket Ken$ cat <<EOF >> out
> ---<Start:2>---
> TIME
> Anchor
> Ralleigh
> ---<End:2>---
> EOF
```

追記されたoutファイルを標準出力
```
MACBOOK:pocket Ken$ cat out
---<Start>---
Trek
Giant
Specialized
Cannondale
Bianchi
---<End>---
---<Start:2>---
TIME
Anchor
Ralleigh
---<End:2>---
MACBOOK:pocket Ken$ 
```

エラーをファイルに書くにはどうすれば

findコマンドの結果をoutファイルに書く
```
MACBOOK:pocket Ken$ find docs > out
MACBOOK:pocket Ken$ cat out
docs
docs/写真の整理.md
docs/iCloud引越し.md
docs/readme.md
docs/books.md
MACBOOK:pocket Ken$ 
```

findコマンドをエラーにして、同様にoutに出力されるか確認
dogsという存在しないファイルを検索してエラーになる
outファイルは0行のためcatで出力されない
エラーは書かれていない。リダイレクトを2>にすればできる

```
MACBOOK:pocket Ken$ find dogs > out
find: dogs: No such file or directory
MACBOOK:pocket Ken$ cat out
MACBOOK:pocket Ken$ ls -l
total 8
drwxr-xr-x  10 Ken  staff   320  3 20 20:43 Linux
-rw-r--r--   1 Ken  staff  2514  3 20 23:22 Linux_study.md
drwxr-xr-x   3 Ken  staff    96  2 24 21:49 SQL
drwxr-xr-x   6 Ken  staff   192  2 24 22:47 docs
drwxr-xr-x   9 Ken  staff   288  2 24 22:44 git
-rw-r--r--   1 Ken  staff     0  3 20 23:22 out
MACBOOK:pocket Ken$ 
```

findのエラー標準出力(2>)をoutにリダイレクト
ls-lでoutが0バイトじゃないことを確認してcat
エラーメッセージを確認
```
MACBOOK:pocket Ken$ find dogs 2> out
MACBOOK:pocket Ken$ ls -l
total 16
drwxr-xr-x  10 Ken  staff   320  3 20 20:43 Linux
-rw-r--r--   1 Ken  staff  3192  3 20 23:24 Linux_study.md
drwxr-xr-x   3 Ken  staff    96  2 24 21:49 SQL
drwxr-xr-x   6 Ken  staff   192  2 24 22:47 docs
drwxr-xr-x   9 Ken  staff   288  2 24 22:44 git
-rw-r--r--   1 Ken  staff    38  3 20 23:24 out
MACBOOK:pocket Ken$ cat out
find: dogs: No such file or directory
MACBOOK:pocket Ken$ 
```


エラーもノーマルもリダイレクトするには

/tmpの中にroot権限がないと参照できないファイルある
```
MACBOOK:pocket Ken$ ls -l /tmp/
total 8
drwx------  4 root    wheel  128  7 23  2023 PKInstallSandbox.8YSNMu
drwx------  4 root    wheel  128  3  6  2021 PKInstallSandbox.WtZf11
drwx------  3 Ken     wheel   96  3 20 17:09 com.apple.launchd.MyQyUVmSfl
drwx------  3 Ken     wheel   96  3 20 17:09 com.apple.launchd.dyU9sXfMoF
drwxr-xr-x@ 4 Ken     wheel  128  3 20 17:10 com.google.Keystone
srwxrwxrwx  1 _mysql  wheel    0  3 20 17:09 mysql.sock
-rw-------  1 _mysql  wheel    4  3 20 17:09 mysql.sock.lock
drwxr-xr-x  2 root    wheel   64  3 20 17:09 powerlog
```

findの標準出力
エラー行（＝エラー標準出力）は出力されない

```
MACBOOK:pocket Ken$ find /tmp/ > out
find: /tmp//PKInstallSandbox.8YSNMu: Permission denied
find: /tmp//PKInstallSandbox.WtZf11: Permission denied
MACBOOK:pocket Ken$ cat out
/tmp/
/tmp//PKInstallSandbox.8YSNMu
/tmp//com.google.Keystone
/tmp//com.google.Keystone/.keystone_system_install_lock
/tmp//com.google.Keystone/.keystone_install_lock
/tmp//powerlog
/tmp//mysql.sock
/tmp//com.apple.launchd.dyU9sXfMoF
/tmp//com.apple.launchd.dyU9sXfMoF/Render
/tmp//mysql.sock.lock
/tmp//com.apple.launchd.MyQyUVmSfl
/tmp//com.apple.launchd.MyQyUVmSfl/Listeners
/tmp//PKInstallSandbox.WtZf11
MACBOOK:pocket Ken$ 
```

標準エラー出力のリダイレクトを加える
標準出力をoutファイルに切り替える、
さらにエラー標準出力をoutに切り替えの標準出力にコピーする

find /tmp/ > out 2>&1

```
MACBOOK:pocket Ken$ find /tmp/ > out 2>&1
MACBOOK:pocket Ken$ cat out
/tmp/
/tmp//PKInstallSandbox.8YSNMu
find: /tmp//PKInstallSandbox.8YSNMu: Permission denied
/tmp//com.google.Keystone
/tmp//com.google.Keystone/.keystone_system_install_lock
/tmp//com.google.Keystone/.keystone_install_lock
/tmp//powerlog
/tmp//mysql.sock
/tmp//com.apple.launchd.dyU9sXfMoF
/tmp//com.apple.launchd.dyU9sXfMoF/Render
/tmp//mysql.sock.lock
/tmp//com.apple.launchd.MyQyUVmSfl
/tmp//com.apple.launchd.MyQyUVmSfl/Listeners
/tmp//PKInstallSandbox.WtZf11
find: /tmp//PKInstallSandbox.WtZf11: Permission denied
MACBOOK:pocket Ken$ 
```



### テーマ
これらをリダイレクト、パイプを組み合わせて使いこなすこと
- ファイルのコピー cp
- ファイルの削除 rm
- ファイルの先頭5行を表示 head
- ファイルの末尾5行を表示 tail
- ファイルの検索 find
- 文字列の検索 grep
- ファイル検索結果リストに対してgrepする