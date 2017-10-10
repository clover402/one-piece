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

### 常用命令
git log
git status
git remote 查看远程库
git remote -v 查看

