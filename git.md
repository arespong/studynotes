### 1、git的安装

```shell
-- 安装命令
apt install git
-- 查看git是否安装成功
 git --version
```

### 2、git的功能常用命令

2.1 git相关配置

```shell
-- 查看git的相关命令
git --help
-- 在Git中配置自己的名称和电子邮件地址
git config --global user.name "arespong"
git config --global user.email "arespong@163.com"
-- 通过查看.gitconfig来验证配置更改
git config --list
--配置公钥与私钥
ssh-keygen -t rsa -C "arespong@163.com" 
-- 查看公钥然后复制到github/码云公钥与私钥上
cat ~/.ssh/id_rsa.pub

```

2.2 远程clone项目

```shell
git clone "https://github.com/arespong/studynotes.git";
```

2.3 git的添加、提交、提交以及版本冲突解决