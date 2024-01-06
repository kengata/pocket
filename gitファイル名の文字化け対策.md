日本語のファイル名が文字化けする場合
[文字化け対策](https://dev.classmethod.jp/articles/git-avoid-illegal-charactor-tips/)

とりあえずこのリポジトリだけに設定する
git config --local core.quotepath false

git全体の設定する場合は
git config --global core.quotepath false

治った

`MACBOOK:pocket Ken$ git status`
`On branch master`
`Untracked files:`
  `(use "git add <file>..." to include in what will be committed)`
`git_study.md`
`viエディタ使い方.md`

2024/1/7
globalのほうを実施済み