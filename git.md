本地与远程的差集 :（显示远程有而本地没有的commit信息）

```shell
git log master..origin/master
```

统计文件的改动

```shell
# git diff <local branch> <remote>/<remote branch>
git diff --stat master origin/master
```

## commit 规范

```
feat : 新功能
fix : 修复bug
docs : 文档改变
style : 代码样式改变
ed: 某个已有功能修改
```

 