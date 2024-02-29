
# shutdown
システムを終了する

今すぐ再起動
sudo shutdown -r now
# halt
システム停止／再起動

# hostname
ホスト名を表示する
```
MACBOOK:pocket Ken$ hostname
MACBOOK.local
```
# sudo
別のユーザーとしてコマンドを実行する
# su
別のユーザーになる
# arch
マシンのアーキテクチャを表示する

```
MACBOOK:pocket Ken$ arch
i386
```
# chroot
rootディレクトリを変更する
# export
環境変数と定義を有効にする

export -p

-p すできにエクスポートされている変数を一覧表示

# env
環境変数を表示／指定してコマンドを実行する
# fc
コマンド履歴を表示／編集して実行
fixcommand
# history
コマンド履歴を表示する

# alias / unalias
コマンドやオプションに別名をつけて管理する

```
alias mysql='mysql -u root -p'
```
# date
日付表示
```
MACBOOK:pocket Ken$ date
2024年 2月25日 日曜日 20時18分05秒 JST
```
# top
プロセス状況をリアルタイムで表示する
# df
ファイルシステムの使用状況を容量で表示する
# exit
シェルを終了する