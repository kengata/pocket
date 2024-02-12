コミット済、push済みの更新を戻したいときはどうすればよいか
# リバート
revert
コミットを打ち消すコミット
マージコミットの場合はどちらのマージ元に戻すのか選択する
ローカルでリバートしたらリモートにpushして反映すル必要あり
リモートのコミットログには何を取り消したかが記録として残る

git revert コミットID

### リバートの前にコミット状況の確認（ローカル）

#### git log
コミットのログ表示
```
MACBOOK:pocket Ken$ git log
commit c1db43222ccd5d02f4228a304c840133975bef17 (HEAD -> topic)
Author: kenichiro yamagata <kenichiro.yamagata@gmail.com>
Date:   Sat Feb 10 20:33:27 2024 +0900

    topicブランチで更新

commit b94d641fd9e1e2d01ac430f5c75b08241b12c9f6 (main)
Author: kenichiro yamagata <kenichiro.yamagata@gmail.com>
Date:   Sat Feb 10 15:56:59 2024 +0900

    ok

commit a0fbea183aacdaf6d98be2e5d6b2df7346ca69b8
Author: kenichiro yamagata <kenichiro.yamagata@gmail.com>
Date:   Sat Feb 10 15:47:00 2024 +0900

    Revert "取り消したいコミット"
    
    This reverts commit f1120b0d9fb06523943a8f411d8ab95fd2b4f08d.

commit f1120b0d9fb06523943a8f411d8ab95fd2b4f08d
Author: kenichiro yamagata <kenichiro.yamagata@gmail.com>
Date:   Sat Feb 10 15:45:35 2024 +0900

    取り消したいコミット
```

#### git log --oneline
コミットのログを１行で表示
```
c1db432 (HEAD -> topic) topicブランチで更新
b94d641 (main) ok
a0fbea1 Revert "取り消したいコミット"
f1120b0 取り消したいコミット
6174bc8 ok
a993b91 ok
bf3499e (origin/main) ok
ef396d4 ok
1779244 Merge branch 'topic' into main
2f46168 ok

```

#### git log --oneline --graph
コミットのログを１行でグラフ表示
```
* b94d641 (**HEAD ->** **topic**, **main**) ok
* a0fbea1 Revert "取り消したいコミット"
* f1120b0 取り消したいコミット
* 6174bc8 ok
* a993b91 ok
* bf3499e (**origin/main**) ok
* ef396d4 ok
*   1779244 Merge branch 'topic' into main
|\  
| * 2f46168 ok
| * 2878742 ok
* |   5da8d25 Merge pull request #4 from kengata/topic
|\ \

```
mainブランチからtopicブランチを作成
この時点でmainブランチの内容はtopicと同期しているので
グラフは分岐しない。topicを複数回更新しても変わらず

HEADがこのリポジトリの先頭
mainブランチがtopicに置いてかれているのがわかる
```
MACBOOK:pocket Ken$ git log --oneline --graph
* 525501b (HEAD -> topic) commit at topic branch,update コミットの取り消し
* c1db432 topicブランチで更新
* b94d641 (main) ok
* a0fbea1 Revert "取り消したいコミット"
* f1120b0 取り消したいコミット
```

ここでmainブランチのほうを更新してみる

写真の整理.mdというファイルをmainブランチのほうで、
作成してadd、commitした

git log --oneline --graph

mainブランチ側ではtopicブランチで何が起きてるか知らないらしい
b94d641の次にtopicブランチで入っているcommitはmainブランチのlogには出てこない
topicのc1db432と525501bのこと
```
MACBOOK:pocket Ken$ git log --oneline --graph
* 05018c1 (HEAD -> main) commit at main branch,add new file 写真の整理
* b94d641 ok
* a0fbea1 Revert "取り消したいコミット"
* f1120b0 取り消したいコミット
* 6174bc8 ok
* a993b91 ok
* bf3499e (origin/main) ok
```

ここでtopicブランチにmainをマージしてみる
どうなることやら、枝分かれして、マージコミットが一本入るはず
```
MACBOOK:pocket Ken$ git status
On branch topic
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   git コミットの取り消し.md

no changes added to commit (use "git add" and/or "git commit -a")
MACBOOK:pocket Ken$ git branch
  main
* topic
MACBOOK:pocket Ken$ git merge main
Merge made by the 'recursive' strategy.
 写真の整理.md | 34 ++++++++++++++++++++++++++++++++++
 1 file changed, 34 insertions(+)
 create mode 100644 写真の整理.md
MACBOOK:pocket Ken$ git log --oneline --graph
*   d07d993 (HEAD -> topic) Merge branch 'main' into topic
|\  
| * 05018c1 (main) commit at main branch,add new file 写真の整理
* | 2571479 commit at topic branch,update gitコミットの取り消し.md
* | 525501b commit at topic branch,update コミットの取り消し
* | c1db432 topicブランチで更新
|/  
* b94d641 ok
* a0fbea1 Revert "取り消したいコミット"
* f1120b0 取り消したいコミット
* 6174bc8 ok
```
topic側はコミットされていない作業ツリーのファイルがあってもマージできた
git log のグラフはmainブランチが本線だから左側にいてほしい気がするが、
topicブランチにチェックアウトしているからか軸がtopicのように
表示されれている気がする

b94d641がmainからの分岐点
c1db432、525501b、2571479がtopicブランチでの更新
05018c1がmainブランチでの更新
d07d993がtopicにmainをマージした際のcommit

main側にtopicをマージしたらどうなるんだろ。
たいして変わらんだろうから気にすることじゃないか

ここでtopicブランチをpushする
それでもって、マージコミットを履歴ごとなかったことにする
resetコマンドかな

マージコミットが消せなかった、、、

