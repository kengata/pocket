#### 目標
以下をコマンド操作でやる
- [x] マークダウンファイルの作成
- [x] ローカルリポジトリへのコミット
- [x] リモートリポジトリへのプッシュ
- [x] リモートリポジトリからのプル
- [ ] 競合の解消
- [ ] ブランチの作成

***
#### ローカルリポジトリの作成、add/commit

##### git init
git管理したいフォルダを作る
masterブランチが自動でつくられる
とりあえず.gitignoreと、git_study.mdをつくる

.gitignoreはobsidianの管理フォルダ.obsidianを指定する。
管理されると面倒そうだったので

gitのローカルリポジトリをここに作成
/Users/Ken/Documents/git/Pocket

##### git status
状況をみる。コミットされていないファイルがわかる

`MACBOOK:Pocket Ken$ git status`
`On branch master`
`No commits yet`
`Untracked files:`
  `(use "git add <file>..." to include in what will be committed)`
`.gitignore`
`git_study.md`
`nothing added to commit but untracked files present (use "git add" to track)`

masterブランチが作られている？git initで？
よくわからないがコミットしてみる

##### git add
ファイルをステージングする

##### git commit
ステージングされたファイルをコミットする

viエディタが起動する、、、脇道にそれる

[[viエディタ使い方]]

とりあえずgitのコメント行を消して、コメントを追加して保存終了する

コミットできた
コミットコメントの#のコメントは消さなくてOK

「igonoreファイルを追加」がコメント

`MACBOOK:Pocket Ken$ git commit`
`[master (root-commit) c011a83] igonoreファイルを追加`
 `1 file changed, 3 insertions(+)`
 `create mode 100644 .gitignore`

.gitignoreファイルの配置はgitフォルダの直下でよいみたい。.gitのほうじゃなくて

日本語のファイル名が文字化けする、、、脇道にそれる
[[gitファイル名の文字化け対策]]

ステージングするときに以下で未ステージのファイルを全部登録できる
ステージングとインデックス追加は同じ意味
**git add .**

***
覚えたこと
ローカルリポジトリの作成 ： git init
ステージしてコミット：git add / git commit

目標の２まではクリア
***
#### リモートリポジトリへのpush

git hubでリポジトリを作成しておいて、ローカルからpushする流れらしい
ポケットリファレンスの本にはgithubのことは書かれていないのでこれを参照
[github使い方](https://atmarkit.itmedia.co.jp/ait/articles/1701/24/news141.html)

これを作成した
https://github.com/kengata/pocket

作成直後のページにあるこれ。既にあるリポジトリからpushするを使う
##### …or push an existing repository from the command line
git remote add origin git@github.com:kengata/pocket.git
git branch -M main
git push -u origin main

##### 実際に投入したコマンド
git remote -v
git remote add origin git@github.com:kengata/pocket.git
git remote -v
git push origin main

ここまではOK
	`MACBOOK:~ Ken$ cd Documents/git/pocket`
	`MACBOOK:pocket Ken$ git remote -v`
	`MACBOOK:pocket Ken$ git remote add origin git@github.com:kengata/pocket.git`
	`MACBOOK:pocket Ken$ git remote -v`
	`origin git@github.com:kengata/pocket.git (fetch)`
	`origin git@github.com:kengata/pocket.git (push)`

これエラーになるじゃん、、、
	`MACBOOK:pocket Ken$ git push origin main`
	`error: src refspec main does not match any.`
	`error: failed to push some refs to 'git@github.com:kengata/pocket.git'`

ローカルブランチの名前がmasterだからだめっぽい

よく分からないままgithubのページの２、３行目のコマンドを投入
	`MACBOOK:pocket Ken$ git branch -M main`
	`MACBOOK:pocket Ken$ git push -u origin main`
	`Counting objects: 18, done.`
	`Delta compression using up to 4 threads.`
	`Compressing objects: 100% (15/15), done.`
	`Writing objects: 100% (18/18), 4.23 KiB | 1.41 MiB/s, done.`
	`Total 18 (delta 5), reused 0 (delta 0)`
	`remote: Resolving deltas: 100% (5/5), done.`
	`To github.com:kengata/pocket.git`
	 * `[new branch]      main -> main`
	`Branch 'main' set up to track remote branch 'main' from 'origin'.`

成功！
最初のエラーになったコマンド、ブランチ名をmasterにしたらOKだったのかも

##### git push
	引数（-u origin main）なしでpushできた

push前にstatus確認。リモートのorigin/mainよりローカルが先行しているって。
	`MACBOOK:pocket Ken$ git status`
	`On branch main`
	`Your branch is ahead of 'origin/main' by 1 commit.`
	  `(use "git push" to publish your local commits)`
	`nothing to commit, working tree clean`

pushする
	`MACBOOK:pocket Ken$ git push`
	`Counting objects: 4, done.`
	`Delta compression using up to 4 threads.`
	`Compressing objects: 100% (4/4), done.`
	`Writing objects: 100% (4/4), 1.34 KiB | 1.34 MiB/s, done.`
	`Total 4 (delta 2), reused 0 (delta 0)`
	`remote: Resolving deltas: 100% (2/2), completed with 2 local objects.`
	`To github.com:kengata/pocket.git`
	   `475a132..4552b15  main -> main`

---
覚えたこと
githubでのリモートブランチ作成
ローカルでのリモート追跡？ブランチ作成
ローカルのリボジトリ名変更
リモートブランチへのpush

3のリモートリポジトリへのpushまでクリア

---
#### リモートリポジトリからのpull

##### git pull
これだけ
githubで直接更新したファイルがローカルリポジトリに取り込まれた
	`MACBOOK:pocket Ken$ git pull`
	`Updating 4552b15..1b89238`
	`Fast-forward`
	 `README.md | 1 +`
	 `1 file changed, 1 insertion(+)`
	 `create mode 100644 README.md`

---
#### 競合の解消

1. リモートのreadmeの３行目をgithubで更新
2. 並行してローカルのreadmeの３行目を更新
3. pull
4. push
5. 競合発生
6. 競合解消 rebase
7. add/commit
8. push

pullの実行後
```
MACBOOK:pocket Ken$ git pull
Updating ca51c5c..1138cdd
error: Your local changes to the following files would be overwritten by merge:
README.md
Please commit your changes or stash them before you merge.
Aborting
```

pushの実行後
```
MACBOOK:pocket Ken$ git push
To github.com:kengata/pocket.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'git@github.com:kengata/pocket.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

```

git pull -rebase
リモートの変更を先に取り込む
```
MACBOOK:pocket Ken$ git pull --rebase
First, rewinding head to replay your work on top of it...
Applying: readmeの更新 local
Using index info to reconstruct a base tree...
M README.md
Falling back to patching base and 3-way merge...
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
error: Failed to merge in the changes.
Patch failed at 0001 readmeの更新 local
Use 'git am --show-current-patch' to see the failed patch
Resolve all conflicts manually, mark them as resolved with
"git add/rm <conflicted_files>", then run "git rebase --continue".
You can instead skip this commit: run "git rebase --skip".
To abort and get back to the state before "git rebase", run "git rebase --abort".
```
