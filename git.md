### 保存临时修改
```
git stash
done something
git stash pop
```
### 恢复文件到最近的版本
git checkout  components/raa/RaaUserInfo.php

### 恢复文件到某个版本
git checkout $commit-id-previous(前8位即可) file/path

### 提交代码
```
git add .
git commit -m "研报查询api"
git status
git pull --rebase
git push origin HEAD:refs/for/raa_0.8
git pull --rebase
```

### 设置github的ssh公钥
1. 查看是否已有密码
```
cd ~/.ssh
```
2. 生成密钥对
```
ssh-keygen -t rsa -C 305xxxxxx@qq.com //github邮箱账号
```
3. 查看公钥内容
```
cd ~/.ssh
ls //id_rsa是私钥 id_rsa.pub是公钥
cat id_rsa.pub
```
4. 复制内容到github,github->头像->setting -> ssh key ->新增一个就行（title随便写）
5. 测试是否成功
```
ssh -T git@github.com
```

### 克隆仓库
克隆版本库的时候，所使用的远程主机自动被Git命名为origin。如果想用其他的主机名，需要用-o选项指定，这里我指定为github
git clone -o github https://github.com/XXXXXXXX(仓库的地址)
可以使用ssh连接git服务器，不过需要在服务器端放置公钥.ssh/authorized_keys, 在客户端放置私钥id_rsa
git clone  ssh://ip/path/to/git

### 解决冲突
```
git pull --rebase #产生了冲突
vim xxx #查看冲突的文件，修改完成
git add . #提交解决
git rebase --continue
```

### 设置用户及保存密码
```
git config --global user.name cloverliu
git config --global user.email 305471598@qq.com
git config credential.helper store #设置保存账号密码
echo "[credential]" >> .git/config
echo "    helper = store" >> .git/config
git config credential.helper 'cache --timeout=3600' #也可设置缓存一段时间
```
或者在请求远程地址是带上用户名密码：http://username:password@git.server.url/path/repo


### change-Id丢失
报错时会有提示，按照提示操作即可，一般如下
```
gitdir=$(git rev-parse --git-dir); scp -p -P 29418 harryhe@10.10.96.212:hooks/commit-msg ${gitdir}/hooks/
git commit --amend
```

### 恢复删除文件
git reset HEAD somefile
git checkout -- somefile


### 恢复到到之前的某个节点
git log #查看节点列表
git reset commit-id #恢复到某个接口
(该操作只会清除本地没有提交成功的commit内容，不会影响已经修改的文件)

### push到错的分支
git reset --hard HEAD   恢复到push前，然后重新push
git commit --amend

### 从当前节点创建分支
git checkout -b temp


### 有些删除文件的状态一直清除不了
git commit -am "comment for commit"


### git push到远程分支时报错"remote:error:refusing to update checked out branch:refs/heads/master"
修改.git/config添加
```
[receive]
denyCurrentBranch = ignore
```
并执行git config --bool core.bare true


### 推送仓库到新的仓库
``` 
git push --mirror new_repository_url
```

### gitignore无效问题
```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'
git push -u origin master
```

### 删除无用分支
```
git remote prune origin
```

### 常用命令
```
git config --list 查看配置
git log
git status
git remote 查看远程库
git remote -v 查看
git tag -l "v3.3.*" //查看tag并过滤
git tag v1.0 //在当前分支的当前commit创建一个tag
```

