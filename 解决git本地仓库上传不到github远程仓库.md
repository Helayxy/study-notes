#### 解决git本地仓库上传不到github远程仓库

##### 1.查看本地仓库和远程仓库的连接方式：

```shell
git remote -v
```

```shell
origin  https://github.com/Helayxy/study-notes.git (fetch)
origin  https://github.com/Helayxy/study-notes.git (push)
```

结果若是https开头则是https连接方式，这种方式一般不会出现问题，若是git开头，说明ssh方式

##### 2.删除连接方式：

```shell
git remote rm origin
```

##### 3.切换为https连接方式：

- 查看github上的仓库地址，选择https方式，如下图所示；

```shell
https://github.com/Helayxy/learning-materials.git
```

- 使用https方式添加远程仓库：

```shell
git remote add origin https://github.com/Helayxy/learning-materials.git
```

##### 4.使用https连接方式一般情况下上传、拉取代码都没有问题