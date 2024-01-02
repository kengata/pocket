#### 目標
以下をコマンド操作で実施する
1. マークダウンファイルの作成
2. ローカルリポジトリへのコミット
3. リモートリポジトリへのプッシュ
4. githubでのmdファイルの表示

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
