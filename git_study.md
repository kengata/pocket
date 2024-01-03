#### 目標
以下をコマンド操作でやる
1. マークダウンファイルの作成
2. ローカルリポジトリへのコミット
3. リモートリポジトリへのプッシュ
4. リモートリポジトリからのプル
5. ブランチの作成
6. 競合の解消

***
#### 作業ログ

ここにgitのローカルリポジトリをつくる
/Users/Ken/Documents/git/Pocket

**git init**
git管理したいフォルダを作る

masterブランチが自動でつくられる

とりあえず.gitignoreと、git_study.mdをつくる

.gitignoreはobsidianの管理フォルダ.obsidianを指定する。
管理されると面倒そうだったので

**git status**
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

**git add**
ファイルをステージングする

**git commit**
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

#### リモートリポジトリへのプッシュ

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

#### git push
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