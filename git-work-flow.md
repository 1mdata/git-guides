#### Git 在工作中的使用流程
已 `git-guides` 这个项目为例，我们需要在 `git` 仓库中建立不同的分支用于我们的日常工作需要，对于的工作分支如下：

分支|说明  
--------|:--------
master|主分支，平时的所有工作基本都围着这个分支展开
beta|测试分支，主要用于测试已完成的功能
prod|发布分支，`beta` 测试完成后通过后，相应功能切换到 `prod` 准备进行发布
*release*|稳定分支，`prod` 中稳定的功能定时更新到此分支(脚本自动维护)

在我们的工作中，我们只需要用到 master 和 beta 分支，prod 的分支供发布人员和发布脚本使用。

下面开始说一下我们在本地开发中如何使用 master 和 beta 分支
项目需求来的时候，某个特定的功能被指定到自己，我们需要怎样完成呢？

1，首先我们需要更新一下 master 分支的代码，命令如下：

```
git checkout master 		// 切换到`master`分支
git pull origin master  	// 更新`master`分支内容
```
__注意__
一定要确保 `master` 分支的内容是最新的

2，从 master 分支创建一个自己的本地开发分支 dev

```
git checkout -B dev 		// 创建一个全新的 dev 分支，即使 dev 存在也会重新创建
```
__注意__
这里使用了 -B 参数，这是一个具有破坏性的参数，你需要确保你新创建的分支中没有未被同步到 master 分支中的内容，如果有的话这个命令执行完后，哪些未被同步/合并的内容将不复存在。

3，在 dev 分支中愉快的开始编写你的功能

```
愉快的 coding 中 ...
中间 会有很多针对 dev 分支的提交
```
4，功能完成后在 `dev` 中提交你的代码并将代码切回 `master` 分支，在这里我习惯使用 `rebase` 的方式，当然你也可以使用 `merge` 的方式，相关命令如下：

```
git commit -a -m "用户登录功能完成"     // 提交 dev 中开发的内容
git checkout master   				 // 切回 master 分支
.
git rebase dev 						 // 使用 rebase 将 dev 中的更新 同步到 master 中
... or ...
git merge dev 						 // 使用 merge 将 dev 中的更新同步到 master 中
.
```
5，更新本次已完成的功能到仓库 `master` 中

```
git pull origin master			// 拉取线上的更新内容，并解决掉可能出现的冲突
git push origin master			// 推送本次已完成的功能到线上仓库
```
