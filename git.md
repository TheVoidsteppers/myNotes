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

git 步骤

```shell
# 创建并切换到新分支
git checkout  -b feature/reviewStyle
# 合并
git pull
git merge origin/develop
# git stash save    git stash pop
 git add .
 git commit -m 'feat:创建折线图组件和mock数据文件'
 git push origin feature/charts 
# http://git.dataexa.com:83/insight/Insight-View/data-center/merge_requests/new?merge_request%5Bsource_branch%5D=feature%2Fcharts  发起合并请求 
```

