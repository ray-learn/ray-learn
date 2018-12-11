---
title: "Win10 环境的SSH管理"
tags:
    - SSH
excerpt: "The management of SSH in Win10"
last_modified_at: 2018-12-11
---

### 背景知识
- `ssh`: OpenSSH SSH client (remote login program) OpenSSH SSH客户端（远程登陆程序）
- `ssh-keygen`: authentication key generation, management and conversion 生成、管理、转换用于认证的密钥
- `ssh-agent`: authentication agent认证代理（复杂情景才可能用到，一般使用不到）  
上面三个程序具体可以在Unix类系统输入`man ssh`, `man ssh-keygen`, `man ssh-agent`，查看详细的说明和下述流程涉及的参数意思  
`${variable}`按照变量字面意思，根据实际情况，写入具体的值
`[optional]`可选

### 管理流程
1. 生成github的公私钥 `ssh-keygen -t rsa -C ${github_email}`->`Enter file name ${github_key}` -> `Password: null or ${custom_github_password}`
2. 生成gitlab的公私钥 `ssh-keygen -t rsa -C "${gitlab_email}"`->`Enter file name ${gitlab_key}` -> `Password: null or ${custom_gitlab_password}`
3. 将步骤1和步骤2生成的`${github_key}[.pub]`和`${gitlab_key}[.pub]`公私钥放在`C:\User\${Username}\.ssh`目录下面
4. 将步骤1和步骤2生成的`${github_key}.pub`和`${gitlab_key}.pub`文件内容用记事本打开，然后拷贝到相应的Github或Gitlab上面，操作路径为：`settings -> SSH and GPG keys -> add new key`
5. 在`.ssh`目录创建`config`文本文件，并将`.txt`扩展后缀去掉。每个账号单独配置一个**Host**，每个**Host**取一个对应的域名或者IP地址，每个**Host**主要配置**HostName**和**IdentityFile**两个属性即可。
    - Host：实际的域名或准确的IP地址，用于`git@Host`中的`Host`字段
    - HostName: 实际的域名或准确的IP地址，与`Host`对应
    - IdentityFile: `${github_key}`或`${gitlab_key}`文件全路径
    - PreferredAuthentications: 配置登陆使用的权限认证，可为`publickey, password publickey`
    - User: 用户名，可选

### 示例配置文件
```
# github
Host github.com
HostName github.com
IdentityFile C:\Users\${Username}\.ssh\github_id_rsa
PreferredAuthentications publickey
User github

# custom gitlab
Host ${gitlab_ip}
HostName ${gitlab_ip}
IdentityFile C:\Users\${Username}\.ssh\gitlab_id_rsa
PreferredAuthentications publickey
User gitlab
```

### 测试方法：
`ssh -Tv git@github.com`  
根据显示的信息，跟踪整个`ssh`的流程

### 注意事项
1. 配置文件使用空格分割字段和值
2. `Host`是域名或准确的IP地址，一定要配置填写正确
3. `ssh-keygen`若有使用密码，后续的`ssh`也需要密码才能访问对应的本地私钥