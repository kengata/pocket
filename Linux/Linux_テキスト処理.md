# tail
ファイルの末尾から数行を表示する

tail /var/log/system.log
systemlogを後ろ10行表示する

tail -n 20 /var/log/system.log
後ろ20行

tail -f /var/log/system.log
自動更新
# head
ファイルの先頭から数行を表示する

head books.md

head -n 

---
# diff
ファイル比較

diff -c books.md books_cpy.md
copied context形式で出力

```   
 ---
 カラマーゾフの兄弟  
MACBOOK:pocket Ken$ diff -c books.md books_cpy.md
*** books.md	2024-02-17 22:51:49.000000000 +0900
--- books_cpy.md	2024-02-23 12:06:48.000000000 +0900
***************
*** 6,12 ****
  藤沢周平の小説
  - 橋ものがたり
  - たそがれ清兵衛
! - 蝉時雨
    
  ---
  カラマーゾフの兄弟  
--- 6,12 ----
  藤沢周平の小説
  - 橋ものがたり
  - たそがれ清兵衛
! - 蟹時雨
    
  ---

```

diff -C 10 books.md books_cpy.md
相違箇所10行の指定。Cは大文字

-c : copied context形式
-u : unified context形式
-y : side-by-side形式

diff -y books.md books_cpy.md
side-by-side形式

diff -u books.md books_cpy.md
unified context形式。これが見やすい

```
MACBOOK:pocket Ken$ diff -u books.md books_cpy.md
--- books.md	2024-02-17 22:51:49.000000000 +0900
+++ books_cpy.md	2024-02-23 12:06:48.000000000 +0900
@@ -6,7 +6,7 @@
 藤沢周平の小説
 - 橋ものがたり
 - たそがれ清兵衛
-- 蝉時雨
+- 蟹時雨
```

---
# awk
テキストのパターンの検知、処理

/etc/passwdから:を区切り文字として5番目の項目がSystemの行を
表示する
awk -F":" '$5 = "Unprivileged" { print NR, $0 }' /etc/passwd

books.mdファイルのls -lコマンド結果を受け取り、5番目の
項目を表示する。空白がデフォルトの区切り文字っぽい
ls -l books.md | awk '{print $5}'

```
MACBOOK:pocket Ken$ ls -l books.md | awk '{print $5}'
832
MACBOOK:pocket Ken$ ls -l books.md
-rw-r--r--  1 Ken  staff  832  2 17 22:51 books.md
```


---
# grep
文字列検索

-E
正規表現

-R
再帰的読み込み

*
全ファイル検索

grep --color -r commit *
キーワードに色がつく

commit とうい文字列を含む行を検索
```
MACBOOK:pocket Ken$ grep --color -r commit *
git/git ローカルブランチの作成からリモートへのプッシュまで.md:ローカルで更新、add、commit
git/gitファイル名の文字化け対策.md:  `(use "git add <file>..." to include in what will be committed)`
git/git_study.md:`No commits yet`
git/git_study.md:  `(use "git add <file>..." to include in what will be committed)`
git/git_study.md:`nothing added to commit but untracked files present (use "git add" to track)`
git/git_study.md:### git commit
git/git_study.md:`MACBOOK:Pocket Ken$ git commit`
git/git_study.md:`[master (root-commit) c011a83] igonoreファイルを追加`
git/git_study.md:ステージしてコミット：git add / git commit
git/git_study.md:	`Your branch is ahead of 'origin/main' by 1 commit.`
git/git_study.md:	  `(use "git push" to publish your local commits)`
```

カレントディレクトリ配下のmdファイルでtopicかdevelopを含む行を検索する
キーワードに色をつける
grep --color -Er "topic|develop" --include='*.md' ./

```
MACBOOK:pocket Ken$ grep --color -Er "topic|develop" --include='*.md' ./
.//git/new_feature.md:developブランチにコミット
.//git/git_study.md:topicブランチを作成してチェックアウト
.//git/git_study.md:MACBOOK:pocket Ken$ git checkout -b topic
.//git/git_study.md:Switched to a new branch 'topic'
.//git/git_study.md:* topic
```

origin〜HEADを含むファイルを、配下のフォルダのmdファイルから検索する
```
grep --color -Er "origin.*HEAD" --include=*.md
```

```
./git/git_study.md:    git push origin HEAD:<name-of-remote-branch>
./Linux_テキスト処理.md:origin〜HEADを含むファイルを、配下のフォルダのmdファイルから検索する
./Linux_テキスト処理.md:grep --color -Er "origin.\*HEAD" --include=*.md
MACBOOK:pocket Ken$ 
```


---
# cat 
ファイルの内容を標準出力に出力する

先頭5行を出力する
cat ファイル | head -n 5
```
MACBOOK:pocket Ken$ cat books.md | head -n 5
将棋入門  
詰将棋がのっている

---
竹光始末 
```

行番号をcatでつけてから先頭5行を表示
cat -n ファイル | head -n 5
```
MACBOOK:pocket Ken$ cat -n books.md | head -n 5
     1	将棋入門  
     2	詰将棋がのっている
     3	
     4	---
     5	竹光始末 
```
