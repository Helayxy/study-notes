**git push 命令详解**

作用：用于将本地分支的更新推送到远程主机

语法：git push <远程主机名> <本地分支名>：<远程分支名>

例子：git push origin master:master

注意：这里的:前后是必须没有空格的，git pull是<远程分支>:<本地分支>， git push是<本地分支>:<远程分支>

```
git push origin master
上述命令省略了远程分支名，表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建。
```

```
git push origin ：refs/for/master
上述命令省略了本地分支名，表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支，相当于执行git push origin --delete master命令，表示删除origin主机的master分支。
```

```
git push origin
上述命令表示，将当前分支推送到origin主机的对应分支。如果当前分支与远程分支之间存在追踪关系，则本地分支名和远程分支名都可以省略。
```

```
git push
如果当前分支只有一个远程分支，那么主机名都可以省略，可以使用git branch -r查看远程的分支名。
```

```
git push -u origin master
如果当前分支与多个主机存在追踪关系，则可以使用-u参数指定一个默认主机，以后就可以直接使用git push命令了。

```

