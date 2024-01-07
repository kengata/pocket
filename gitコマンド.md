形式
git xxx

---
# init
初期化

ローカルリポジトリを初期構築する

---
# add
ステージへの追加

更新のあるファイルをステージングするコマンド
git add .
. は未ステージングファイル全部をステージに追加する
ファイルを指定して個別にステージングすることもできる

---
# commit
コミット

ステージされたファイルをコミットするコマンド
git commit

コミットコメントを書くようにviエディタが起動する。コメント記入してviを終了するとコミットされる。

-m  "xxxx" でエディタを開かずにコミットメッセージを登録できる

```
MACBOOK:pocket_2 Ken$ git commit -m "git_study.mdを更新 pocket_2"
[main 514103c] git_study.mdを更新 pocket_2
 3 files changed, 132 insertions(+), 5 deletions(-)
 rewrite bashコマンド.md (100%)

```

---
# status
ステータス

リポジトリの状態を表示する。未ステージグ、未コミットがどれだけあるかわかる。
git status

git status -v
ファイル内の変更まで表示させる。ステージ分

git status -v -v
未ステージ分も詳細表示

---
# push
プッシュ

ローカルリポジトリのコミットをリモートリポジトリに反映させる

---
# pull
プル

リモートリポジトリの変更をローカルリポジトリに取り込む

pull --rebase
リモートの変更を先に入れてローカルの変更をその後につなげる

---
# checkout
ブランチ切り替え

作業対象のブランチを切り替える

checkout ブランチ名
ブランチの切り替え

checkout -b ブランチ名
ブランチを作成した後にそのブランチに切り替える

---
# branch
ブランチ操作

ブランチのいろいろな操作をする

branch -l
ブランチのリストを表示する

branch -d ブランチ名
ブランチを削除する

---
# rm
削除

バージョン管理対象のファイルを管理から叙階した上で削除する。コミット済みのものが対象。

git rm ファイル名

---
# mv
名前変更、所属ディレクトリ変更

git mv a.txt b.txt
git commit

commitが必要

bashのmvコマンドやGUIでファイル名を変更してもgitには反映されない

---
# reset
ステージング、コミットの取り消し

### 直前のコミットを取り消す

HEAD^  は直前のコミットの一つ前の状態

```
git reset --hard HEAD^
```

```
MACBOOK:pocket Ken$ git reset --hard HEAD^
HEAD is now at 575434f gitコマンド.mdを更新
```
### ステージから降ろす
コミットしたくないものをはずすときに使う

git reset ファイル名

---
# revert
コミットを取り消す別のコミットを作成する

事前に戻したいコミットのハッシュを確認
```
MACBOOK:pocket Ken$ git log --oneline
72a1f35 (**HEAD ->** **main**, **origin/main**) リバートする前のコミット
dd7af0f gitコマンド.mdを更新
```

revert 実行
--no-edit オプションはリバートのコミットメッセージを編集しない指定
```
MACBOOK:pocket Ken$ git revert --no-edit 72a1f35
[main 1f07a28] Revert "リバートする前のコミット"
 Date: Mon Jan 8 05:27:51 2024 +0900
 1 file changed, 1 deletion(-)
```

log確認
1f07a28がリバートによる戻しコミット

```
MACBOOK:pocket Ken$ git log --oneline
1f07a28 (**HEAD ->** **main**) Revert "リバートする前のコミット"
72a1f35 (**origin/main**) リバートする前のコミット
dd7af0f gitコマンド.mdを更新

```