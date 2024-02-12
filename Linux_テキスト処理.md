


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
