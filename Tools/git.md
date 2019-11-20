本地与远程的差集 :（显示远程有而本地没有的commit信息）

```shell
git log master..origin/master
```

## 统计文件的改动

```shell
# git diff <local branch> <remote>/<remote branch>
git diff --stat master origin/master
git diff --cached  # 查看要提交的内容； 查看 git add 的内容
git diff # 会显示已做出但未添加到索引的任何更改；查看未 git add 的内容
git status # 获得情况的简要摘要
```

## commit 规范

```json
feat : 新功能
fix : 修复bug
docs : 文档改变,修改文档
style : 代码样式改变,格式修改，不影响代码逻辑，注意不是 css 修改
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
perf : 性能优化
test：增加修改测试用例
build : 改变了build工具 如 grunt换成了 npm
revert : 撤销上一次的 commit
chore：构建过程或辅助工具的变动，其他修改, 比如构建流程, 依赖管理
ed: 某个已有功能修改
deps: 升级依赖

scope: commit 影响的范围, 比如: route, component, utils, build...
subject: commit 的概述
body: commit 具体修改内容, 可以分为多行
footer: 一些备注, 通常是 BREAKING CHANGE 或修复的 bug 的链接.

// 推荐以动词开头，如： 设置、修改、增加、删减、撤销等
```

## git 步骤

```shell
# 创建并切换到新分支
git checkout -b feature/reviewStyle
# 删除分支
git branch -D  feature/reviewStyle
# 合并
git pull
git merge origin/develop
# git stash save    git stash pop
 git add .
 git commit -m 'feat:创建折线图组件和mock数据文件'
 git push origin feature/charts 
# http://git.dataexa.com:83/insight/Insight-View/data-center/merge_requests/new?merge_request%5Bsource_branch%5D=feature%2Fcharts  发起合并请求 
```

## git clone

```shell
git clone -b dev_jk http://10.1.1.11/service/tmall-service.git # 选择分支下载
```

