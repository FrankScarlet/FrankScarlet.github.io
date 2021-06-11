---
title: Hello Docker Extra 1
categories:
- Docker
feature_image: "https://picsum.photos/2560/600?image=872"
---

Docker应用的番外篇，主角是GitLab。

> 学习用，本文指的是CE(Community Editon)版本。

假如服务器配置充足，或者笔记本配置很顶。那马上就能用Docker启动一个GitLab实例。但我们遭遇的极端条件是一个双核4G内存的服务器，用Docker方式运行容器，启动阶段就报错，无法继续分配更多内存了。因此这个文章介绍了两种方式。


## Docker

执行`docker-compose up`前，需要设置`GITLAB_HOME`环境变量，用于设置共享卷的路径。

```yml
web:
    image: 'gitlab/gitlab-ce:latest'
    restart: always
    hostname: 'gitlab.example.com'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.example.com:8929'
        gitlab_rails['gitlab_shell_ssh_port'] = 2224
    ports:
      - '8929:8929'
      - '2224:22'
    volumes:
      - '$GITLAB_HOME/config:/etc/gitlab'
      - '$GITLAB_HOME/logs:/var/log/gitlab'
      - '$GITLAB_HOME/data:/var/opt/gitlab'
```

## Linux Package

> https://about.gitlab.com/install/
>
> 用CentOS来举例了

```bash
sudo yum install -y curl policycoreutils-python openssh-server perl
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="https://gitlab.example.com" yum install -y gitlab-ce
```

我只选了一些关键的命令，命令前的变量参数影响的是`/etc/gitlab/gitlab.rb`，除了最关键的监听地址设置，里面还有很多参数值得调整。执行完上面的命令后，所有服务应该默认启动了，就能访问主页，然后更改root的密码。

### 调优

为了能节省一点空间，通常推荐增加一个被注释掉的参数，现在想想,`puma`的数量应该也要控制一下。

```rb
unicorn['worker_processes'] = 2
# puma['worker_processes'] = 2
```

### 常用命令

核心是`gitlab-ctl`

```bash
gitlab-ctl status
gitlab-ctl start
gitlab-ctl status
gitlab-ctl tail # 查看日志
```

### 创建备份与恢复

> https://docs.gitlab.com/ee/raketasks/backup_restore.html
>
> 因为之前的机子也用的Package安装方式，所以只截取这个教程里的一部分

```bash
# backup
sudo gitlab-backup create    
# 本来还应该备份一下配置与私有信息，不过我只取了`secrets`
# /etc/gitlab/gitlab-secrets.json
# /etc/gitlab/gitlab.rb

# restore

# 把backup.tar 放入对应的文件夹
sudo cp 11493107454_2018_04_25_10.6.4-ce_gitlab_backup.tar /var/opt/gitlab/backups/
sudo chown git.git /var/opt/gitlab/backups/11493107454_2018_04_25_10.6.4-ce_gitlab_backup.tar
# Stop the processes that are connected to the database. Leave the rest of GitLab running:
sudo gitlab-ctl stop puma
sudo gitlab-ctl stop sidekiq
# Verify
sudo gitlab-ctl status
# This command will overwrite the contents of your GitLab database!
sudo gitlab-backup restore BACKUP=11493107454_2018_04_25_10.6.4-ce
#  记得把gitlab-secrets.json 还原了
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
sudo gitlab-rake gitlab:check SANITIZE=true
```

里面可能遇到的问题：

- 备份命令执行完毕，备份的默认路径是？

从`gitlab.rb`来看，位于`/var/opt/gitlab/backups`

- 备份之后，root密码登录不进去

```bash
# 13.9 之后的新版本
sudo gitlab-rake "gitlab:password:reset[root]"
# 旧版本，只能启动数据库改了
sudo gitlab-rails console
# irb(main):001:0> 
user = User.find(1)
user.password = 'secret_pass'
user.password_confirmation = 'secret_pass'
user.save!
```