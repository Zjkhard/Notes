#### ssh

[官方文档](https://docs.github.com/cn/authentication/connecting-to-github-with-ssh)



#### 本地仓库上传githun

1.  将暂存区添加到工作区

   ```
   git add .
   ```

2. 将工作区上传本地仓库

   ```
   git commit . -m 'infos'
   ```

3. 将本地仓库推送到远端

   ```c++
   $ git remote add github sshAddres  // sshAddres github 仓库中的ssh地址  github 远端仓库别名 添加一次就ok
   $ git push github --all
   ```

   