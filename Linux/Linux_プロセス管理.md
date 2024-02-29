
# ps
プロセス状況を表示する

PID プロセスID
TTY 端末名
TIME 累積CPU利用時間
CMD 実行ファイル名

ps -e | head

-e すべてのプロセスを表示する

```
MACBOOK:pocket Ken$ ps -e | head
  PID TTY           TIME CMD
    1 ??         1:38.31 /sbin/launchd
   55 ??         0:03.73 /usr/sbin/syslogd
   56 ??         0:06.32 /usr/libexec/UserEventAgent (System)
   59 ??         0:01.10 /System/Library/PrivateFrameworks/Uninstall.framework/Resources/uninstalld
   60 ??         0:06.06 /usr/libexec/kextd
   61 ??         0:12.66 /System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/FSEvents.framework/Versions/A/Support/fseventsd
   63 ??         0:00.59 /System/Library/PrivateFrameworks/MediaRemote.framework/Support/mediaremoted
   65 ??         0:01.58 /System/Library/CoreServices/appleeventsd --server
   66 ??         0:03.13 /usr/sbin/systemstats --daemon

```