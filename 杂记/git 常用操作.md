### git 常用操作

#### 初始化本地库

```
git  init
```

#### 查看本地库状态

```
git status
```

![image-20210704151438859](C:\Users\kun\AppData\Roaming\Typora\typora-user-images\image-20210704151438859.png)

#### 将文件添加暂存区

```
git add ......    (......文件名)
```

#### 删除暂存区文件

```
git rm --cached ......  (......文件名)
```

####  将暂存区文件提交本地库，形成历史版本

```
git commit -m "日志信息" 文件名
```

#### 查看版本信息

```
git reflog
```

![image-20210704152721814](C:\Users\kun\AppData\Roaming\Typora\typora-user-images\image-20210704152721814.png)

```
git log  //查看详细日志命令
```

![image-20210704152813751](C:\Users\kun\AppData\Roaming\Typora\typora-user-images\image-20210704152813751.png)

#### 回到之前版本号

```
git reset --hard "版本号"  
```

![image-20210704153928748](C:\Users\kun\AppData\Roaming\Typora\typora-user-images\image-20210704153928748.png)

```
git reset --hard 1a9083c
```

#### 查看分支

```
git branch -v
```

#### 创建分支

```
git branch hot-fix   //创建 hot-fix 分支
```

#### 切换分支

```
git checkout hot-fix
```

#### 合并分支

```
git merge hot-fix
```

![image-20210704160758801](C:\Users\kun\AppData\Roaming\Typora\typora-user-images\image-20210704160758801.png)

#### 当合并冲突后

```
可以手动解决冲突，然后git commit -m "merge test"  不能带文件名
```

![image-20210704163051467](C:\Users\kun\AppData\Roaming\Typora\typora-user-images\image-20210704163051467.png)



