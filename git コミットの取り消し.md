
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


