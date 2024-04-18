# Git使用教程

## 0.快速版本

如果你现在已经新建了仓库（最好设成私有的），本地有个文件夹，如何快速上传文件夹至仓库。

首先进入这个文件夹，右击点击Git Bash Here。

```
git init   // 初始化版本库
git add .   // 添加文件到版本库（只是添加到缓存区），.代表添加文件夹下所有文件 
git commit -m "first commit" // 把添加的文件提交到版本库，并填写提交备注
git remote add origin 你的远程库地址  // 把本地库与远程库关联
git push -u origin main    // 第一次推送时
git push origin main  // 第一次推送后，直接使用该命令即可推送修改
```

## 1.Git设置(config)

```
git config --list

git config --global user.name "用户名"
git config --global user.email 邮箱地址
```

## 2.查看仓库状况(status)

```
git status
```

## 3.初始化仓库(init)

```
git init
```

## 4.往仓库里添加文件(add)

```
git add test.txt
```

![img](https://pic1.zhimg.com/80/v2-b388715d84dc5363b65752d5a8f09920_1440w.webp)

## 5.将改动提交到本地版本库(commit -m)

![img](https://pic3.zhimg.com/80/v2-5ecf63843ec0e35ef0236ca90c94cb4e_1440w.webp)

```
git commit -m "提交信息"
```

```
git commit -m “message”，-m 参数表示可以直接输入后面的 “message” ，如果不加 -m 参数，那么是不能直接输入 message的，而是会调用一个编辑器一般是 vim 来让你输入这个 message。
git commit -a -m “massage”，加的 -a 参数可以将所有已跟踪文件中的执行修改或删除操作的文件都提交到本地仓库，即使它们没有经过 git add 添加到缓存区，注意，新加的文件（即没有被 git 系统管理的文件）是不能被提交到本地仓库的。建议一般不要使用 -a 参数，正常的提交还是使用 git add 先将要改动的文件添加到暂存区，再用 git commit 提交到本地版本库。
git commit --amend，追加提交，它可以在不增加一个新的commit-id的情况下将新修改的代码追加到前一次的commit-id中
```

我们再看看仓库状态，结果显示 nothing to commit, working tree clean，这表示已经没有内容可以提交了，即全部内容已经提交完毕。

![img](https://pic4.zhimg.com/80/v2-0a8da4bc9ada50f4987fe8da85fe978f_1440w.webp)

## 6.日志(log)

```
git log
```

输入 **git log** 命令，打印 Git 仓库提交日志，会显示作者、时间和你写的提交信息：

用 `git log `查看版本号（commit 后面那一长串就是版本号）：

## 7.分支功能(branch checkout merge)

branch 是分支的意思，多人合作时，你一个模块，别人一个模块，你们各建一个分支就能保证改动互不干扰，等最后做完，把你们的分支合并起来就好。

git init 会初始化一个主分支 master，也是我们现在的默认分支。

**查看分支**

```
git branch
```

可以看到我们就在 master 这里：

在此基础上**新建分支**用 

```
git branch a
```

其中 a 是我起的分支名字。然后再用 **git branch** 查看，可以看到 master 有了一个分支，这时候分支 a 跟分支 master 是一模一样的内容。

![img](https://pic3.zhimg.com/80/v2-bd39adb5ac0a43368e26e03377627522_1440w.webp)

```
git checkout “分支名”
```

**切换分支**。

```
git merge
```

把你的**代码合并到主分支 master**上来。

1. 切换到 master 分支，如果你已经在了就不用切换了;
2. 执行 git merge a ，意思就是把 a 分支的代码合并过来，不出意外，
   这个时候 a 分支的代码就顺利合并到 master 分支来了

```
git checkout master
git merge a
```

**删除分支**：有添加就有删除，假如分支新建错了，或者 a 分支的代码已经**顺利合并**到 master 分支来了，那么 a 分支没用了，用 

```
git branch -d “分支名”
```

 删除。删除分支 a 的语句为 git branch -d a，这里不再演示。
**强制删除分支**：上面说了 **git branch -d** 是删除分支的意思，有些时候可能会删除失败，比如如果 a 分支的代码还**没有合并**到master，你执行 git branch -d a 是删除不了的，它会智能的提示你a分支还有未合并的代码，但是如果你非要删除，那就执行 

```
git branch -D a “分支名”
```

## 8.版本打标签(tag checkout)

**新建 v1 tag**

```
git tag v1
```

**查看标签**

```
git tag
```

**切换至标签**

```
git checkout v1
```

## 9.SSH Key

**查看本地Git SSH密钥**

要想本地与远程仓库交互，必须有一个安全机制防止数据泄漏 ，这个安全机制就是 SSH，因此远程交互之前需要进行 SSH 配置。

```
ssh
```

如果返回如下界面说明 SSH 已安装

```
ssh-keygen -t rsa
```

（指定 rsa 算法生成密钥），接着连续三个回车键（不需要输入密码），然后就会生成两个文件 id_rsa （密钥）和 id_rsa.pub （公钥）。

**本地密钥与Github上公钥配对**

![img](https://pic1.zhimg.com/80/v2-107f7d659e5e423a499e7a2d6d7b9360_1440w.webp)

![img](https://pic4.zhimg.com/80/v2-165bcd64e9a3aaa5fb22f3de0c2b8d03_1440w.webp)

**验证绑定**

如果返回下面的提示，说明绑定成功

```
ssh -T git@github.com
```

![img](https://pic3.zhimg.com/80/v2-153f6e613952e22951b3ed3ee24bf45e_1440w.webp)

## 10.从github上下载仓库到git(clone)

**git clone with HTTPS**

```
git clone https://github.com/project/repo.git
```

**git clone with SSH**

```
git clone git@github.com:project/repo.git
```

## 11.通过Git提交代码到GitHub(push origin master)(git remote add origin "SSH")

- git push：翻译为推，当你的代码更新，需要把本地的推到远程仓库
- git pull：翻译为拉，当远程仓库有更新，你需要把远程的拉到本地进行合并

```
git remote add origin 你的远程库地址  // 把本地库与远程库关联
```

```
git pull “URL或origin” “要下载Github的分支名”
git pull git@github.com:Wpp0420/-wpp.git main
git pull origin master 
```

```
git push origin master # 把本地代码推到远程 master 分支
git push origin “要上传的Git本地的分支名”
```

**一般我们在 push 之前都会先 pull ，这样不容易冲突。**

## 12.设置新的命令别名(config --global alias.)

```
git config --global alias.别名 git命令
```

```
git config --global alias.ci commit
git ci
git config --global alias.pullom 'pull origin master'
git pulom
```

## 13.查看写前后区别(diff (commit版本)(分支)(暂存区和版本库))

```text
git diff <$id1> <$id2> # 比较两次提交之间的差异
git diff <branch1>..<branch2> # 在两个分支之间比较
git diff --staged # 比较暂存区和版本库差异
```

## 14.版本回退（reset）

```
git reset --hard 版本号 (抛弃当前工作区的修改)
git reset --soft 版本号 (回退到之前的版本，但保留当前工作区的修改，可以重新提交)
```

如果你的文件只在本地，执行上面的步骤就可以啦。

如果你的文件提交到 GitHub 上，还需要执行如下命令同步到远端：

```text
git push origin 分支名
```

本地的版本落后于远端的版本，因此我们还需要在上述命令中加上`--force`参数：

```text
git push origin 分支名 --force
```

## 15.修改master为main

```
git config --global init.defaultBranch main
以上方法只是让以后创建的项目默认分支为main, 但对于已经创建的项目则无能为力, 所以我们还需要对已存在的项目逐个进行修改.
```

修改已创建项目的主分支为main
切换到主分支master

```
git branch -M main
```

命令, 把当前master分支改名为main, 其中-M的意思是移动或者重命名当前分支
