title: GitHug，关于git的一个小游戏
date: 2016-05-22 10:06:31
categories:
- Git
tags:
- githug
---

git这工具功能太多，要配合实际操作才能了解一些晦涩的命令。
嘛，常用的也不多，毕竟git现在趋于主流，不学不行啊。
作为一个程序员连github都不知道，git都不会用，那真是...

<!--more-->

## 游戏说明
**以下是我的通关攻略,如果看见奇怪的字符串，那一定是commitid-hash。**
**要了解git某些命令，请使用git help xxx或者成为一个**[猴子](http://backlogtool.com/git-guide/cn/)

游戏的安装方式：[githug官网](https://github.com/Gazler/githug)的README中有详细介绍
如果你确实和我一样不会英语。可以看看这个：https://codingstyle.cn/topics/51

## 我的攻略
1.git init

2.git config `--global` user.name "ldc4"
   git config `--global` user.email "544420314@qq.com"

       --global
           For writing options: write to global /.gitconfig file rather than the repository .git/config, write to $XDG_CONFIG_HOME/git/config file if this file exists and the/.gitconfig
           file doesn't.

           For reading options: read only from global ~/.gitconfig and from $XDG_CONFIG_HOME/git/config rather than from all available files.

           See also the section called "FILES".
3.git add .

4.git commit -m "readme"

5.git clone https://github.com/Gazler/cloneme

6.git clone https://github.com/Gazler/cloneme  my_clone_repo

7.vim .gitignore
添加*.swp

8.vim .gitignore
添加!lib.a

9.git status

10.git status

11.git rm deleteme.rb

12.git rm `--cached` deleteme.rb

13.git stash
You've made some changes and want to work on them later. You should save them, but don't commit them.

14.git mv oldfile.txt newfile.txt
git add .
git rm oldfile.txt
- 感觉做复杂了
git mv oldfile.txt newfile.txt

15.mkdir src
git mv *.html ./src

16.git log

17.git tag new_tag

18.git push `--tags`

19.git add .
git commit `--amend`

20.git commit -m "init" `--date` "2016-5-20 13:14:52"

21.git reset to_commit_second.rb

22.git reset `--soft` HEAD^

23.git checkout `--` config.rb

24.git remote

25.git remote -v

26.git pull origin master

27.git remote add origin https://github.com/githug/githug

28.git pull `--rebase` (或者 git rebase)
rebase与merge的区别得注意一下

29.git diff
关于diff怎么读：http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html

30.git blame config.rb

31.git branch test_code

32.git checkout -b my_branch

33.git checkout v1.2

34.git checkout tags/v1.2
当branch和tag有相同的名字的时候，需要加前缀才能切换到指定tag，否则默认切换到指定的branch

35.git branch test_branch HEAD^
HEAD^ == HEAD~1

36.git branch -d delete_me
git `--help` branch可以查看帮助、git help branch也可以，不是很懂为什么要这样

37.git checkout test_branch
git push origin test_branch

38.git merge feature

39.git fetch
git fetch之后还要checkout一个分支才能看到分支

40.git rebase master feature

41.git repack -d
在前面，我们已经看到在 .git/objects/??/ 目录中保存着我们创建的每一个 git 对象。这样的方式对于自动和安全地创建对象很有效，但是对于网络传输则不方便。 git 对象一旦创建了，就不能被改变，但有一个方法可以优化对象的存储，就是将他们“打包到一起”。

感觉并没有什么鸟用啊

42.git checkout new-feature
git blame README.md
git log
git checkout master
git cherry-pick ca32a6da

43.git grep TODO

44.git rebase -i ea9a
根据命令提示 reword来修改提交信息

45.git rebase -i 0775
使用squash来合并提交

46.git  merge `--rebase` long-feature-branch
git commit -m "xxx"

47.git rebase -i ce7a8d
从上往下执行的

48.git bisect start master f608824
git bisect run make test
这题瞬间懵逼
http://www.oschina.net/translate/letting-git-bisect-help-you

49.git add -p feature.rb
提交部分改动

50.git reflog
git checkout solve_world_hunger

51.git revert 8da5
与 reset 不同的是，revert 只会撤销当前的 commit，而之后的 commit 操作的修改还会保留，但是reset 还会将之后的所有 commit 操作的修改全部退回 staging area 或丢弃。

52.git reflog
git checkout 335d717

53.git merge mybranch
然后进入冲突文件修改冲突，
git add poem.txt
git commit -m "xxx"

54.git submodule add https://github.com/jackmaney/githug-include-me ./githug-include-me
不太懂这个和clone有什么区别。。

55.This is the final level, the goal is to contribute to this repository by making a pull request on GitHub.  Please note that this level is designed to encourage you to add a valid contribution to Githug, not testing your ability to create a pull request.  Contributions that are likely to be accepted are levels, bug fixes and improved documentation.

## 别人的攻略
http://www.jianshu.com/p/482b32716bbe

## 写在最后
顺便吐槽一下markdown 关于 `--`：如果不加反引号，就会显示一个横杠