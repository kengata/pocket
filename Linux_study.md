### ファイルの内容表示
	cat
		標準出力に出力する
	less
		テキスト表示
	more
		テキスト表示
	vi
		エディタ起動

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


ファイルのコピー

ファイルの削除

ファイルの先頭5行を表示

ファイルの末尾5行を表示

ファイルの検索

文字列の検索

ファイル検索結果リストに対してgrepする