
#### 目標
以下をコマンド操作で実施する
1. マークダウンファイルの作成
2. ローカルリポジトリへのコミット
3. リモートリポジトリへのプッシュ
4. githubでのmdファイルの表示

***
#### 作業ログ

ここをgitのローカルリポジトリにする
/Users/Ken/Documents/git/Pocket

**git init**
git管理したいフォルダを作る

ブランチがない気がするが、、、
コミットしていなからか？

とりあえず.gitignoreと、git_study.mdをつくる

.gitignoreはobsidianの管理フォルダ.obsidianが管理されると
面倒そうだったので

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
git add .

