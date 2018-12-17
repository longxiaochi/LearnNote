## Mac发现Git log命令展示有中文就乱码
看看是否是MAC下编码问题

## 解决方案

把~/.gitconfig里修改好，source下就行了。
重点关注下面的参数，没有就加上，有的就更新下

```
.........
........
[core]
        quotepath = false
..........
..........
[i18n]
    commitencoding = utf-8
    logoutputencoding = utf-8

```
更新完后直接关掉控制台，重新打开即可。