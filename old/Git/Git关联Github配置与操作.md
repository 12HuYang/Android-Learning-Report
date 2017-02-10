## Git关联Github配置与操作 ##
[**Git与GitHub关联,并用Git管理GitHub**(这个帖子说的很详细)](http://blog.csdn.net/vipzjyno1/article/details/22098621)
### 1.安装Git for windows客户端:  ###
	一路下一步就ok.常规的.exe程序安装.
### 2.关联GitHub: ###
1. 修改`Git Bash`的配置 : 将`Git Bash`设置为快速编辑模式(在快捷方式上右键即可出现配置界面)
2. 使用命令 : 创建本地ssh: `ssh-keygen -t rsa -C "xxxxxx@xxx.com" ` (GitHub邮箱 : 该命令后面的邮箱就是GitHub的注册邮箱)
3. 进入生成的ssh目录 :` C:\Documents and Settings\Administrator\.ssh` 中, 使用记事本打开 id_rsa.pub 文件, 将该文件中的内容复制
4. 登录你的`GitHub`-->`Account Setting`-->`SSH-KEY`-->`Add SSH key`将上面复制好的`ssh-key`复制进去,标题你随便取,最后点击`Add Key`完成配置.
5. 验证是否配置完成,在命令行中输入:`ssh -T git@github.com`,如果出现`Hi xxxxxx! You've successfully authenticated, but GitHub does not provide shell access.`就说明配置成功.

-------

如果出现:**`Host Key Verification Failed`**的错误,请按照下面方法解决;

--------
**按照以下步骤:**

1. **`mkdir ~/.ssh`**
2. **`vim known_hosts`** - if you already have known_hosts, skip this.
3. **`ssh-keyscan -t rsa github.com`** > **`~/.ssh/known_hosts`**
4. **`ssh-keygen -t rsa -C "user.email"`**
5. Add the id_rsa.pub key to SSH keys list on your GitHub profile.	

### Git的操作 ###
	1. 从GitHub上拷贝到SSH,然后添加到Git中.
	2. 像一般的版本控制一样操作(例如:svn)
	3. 注意顺序:重新扫描-->缓存改动-->提交-->上传
[贴个图文并茂的帖子吧:[Git Gui for Windows的建库、克隆（clone）、上传（push）、下载（pull）、合并](http://blog.csdn.net/fym0512/article/details/7713006)]

