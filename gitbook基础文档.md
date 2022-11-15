# git基础使用文档

## 1. 基础配置
> 配置及其所在文件查看
```shell
git config --list
git config --list --show-origin
```
> 用户配置信息设置
```shell
1. 全局git配置设置
git config --global user.name "lei"
git config --global user.email "lei@.com"

2. 项目git配置设置
git config user.name "lei"
git config user.email "lei@.com"
```

## 2. ssh配置
ssh相比较http貌似更安全和方便
> ssh密钥生成(默认在 ~/.ssh/文件目录下创建)
```bash
ssh-keygen -t rsa -C "你的邮箱@xxx.com" -f ~/.ssh/id_rsa_gitlab
```
id_rsa_gitlab: 新生成的ssh密钥，该操作还会生成一个id_rsa_gitlab.pub的公钥文件
> git的config文件设置
```bash
# gitlab的文件配置
Host gitlab.com
    HostName gitlab.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_rsa_gitlab
# 配置文件参数
# Host Host可以看作是一个你要识别的模式，对识别的模式，进行配置对应的的主机名和ssh文件
# HostName 要登录主机的主机名
# User 登录名
# PreferredAuthentications publickey,password,keyboard-interactive
# IdentityFile 指明上面User对应的identityFile路径
```
> 添加公钥文件到gitlab
```shell
cat ~/.ssh/id_rsa_gitlab.pub
```
在github,gitLab,Tgit相应的位置,找到SSH-key的配置位置,用记事本打开.ssh文件夹下的id_rsa.pub文件(公钥,没有pub的是私钥,私钥很重要,不能随意透漏出去)，这个文件中存放的就是刚才生成的ssh-key；把文件中所有的字符串复制，粘贴到相应位置中保存即可.
添加公钥：
复制生成后的 ssh key，通过仓库主页 「管理」->「部署公钥管理」->「添加部署公钥」 ，将生成的 public key 添加到仓库中。
> 配置全局的git用户和邮箱，操作同上

## 3. 基础使用
> 工作区
```shell
git add < file >
将file添加到了暂存区
```
> 查看git修改
```markdown
git log
选项
    -p 显示每次改动的差异
    --name-only 显示已经修改的文件清单
    --graph 使用图形显示分支合并历史
```
> 查看标签
```markdown
git tag -l
支持使用正则:
    例如: git tag -l '1.8.*'
给分支打标签
    git tag -a v1.2 9fceb02
    v1.2:标签名称
    备注: git push不会将标签推送到远端
    需要使用命令: git push orgin [tagname]
删除本地标签:
    git tag -d <tagname>
    然后使用: git push <remote> :refs/tags/<tagname> 来更新远程仓库标签
```

> 分支提交回滚
```markdown
1. 本地分支回退
git reset
    git reset 将分支记录回退几个提交,来实现撤销改动
    git reset HEAD~ 一般用于本地分支,对远程分支无效

2. 远程分支回退
git revert
    撤销更改远程分支，需要使用 git revert
    会引入一个新的提交记录,用于更改原有的提交记录
```
> git cherry-pick

有些小的改动，使用git merge貌似没有必要，直接使用git cherry-pick将改动push到目标分支上
```markdown
git cherry-pick <版本号>
```