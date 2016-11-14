##  常用Linux操作

标签（空格分隔）： Linux

---

### 按ID删除文件
```
find . -inum 398445 -exec rm {} -rf \;
```
### 查找
```
sudo find / -name dump.rdb
```
### 查找删除
```
find ./ -name .svn |xargs rm -rf
```
### 生成ssh公钥
```
ssh-keygen -t rsa -C "username@example.com"
```
### Screen 操作

- screen -ls：获取后台运行py的列表
- screen -r 'process_id':进入该id的进程
- screen -S process_name: 以process_name为进程名新建进程
- ctrl+A,D : 退出该进程
- exit： 结束该进程


### virtualbox ubuntu
 安装virtualbox增强 apt-get install virtualbox-guest*

## zsh
```
    git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh  
    cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc  
    chsh -s /usr/bin/zsh
```
## virtualbox共享盘命令
```
#!/bin/zsh -e
sudo mount QdataShare -t vboxsf /home/tt67wq/Share
```

## java环境
```
    vim /etc/profile
    export JAVA_HOME=/home/tt67wq/jdk1.8.0_73 
    export JRE_HOME=/home/tt67wq/jdk1.8.0_73/jre 
    export CLASSPATH=.:CLASSPATH:CLASSPATH:JAVA_HOME/lib:$JRE_HOME/lib 
    export PATH=PATH:PATH:JAVA_HOME/bin:$JRE_HOME/bin（如果你已经有其他的系统PATH，这一句直接把javapath加入系统path即可）
```
然后

```
source /etc/profile
```

## 查询并杀掉进程

```

$ ps auxww | grep 'celery worker' | awk '{print $2}' | xargs kill -9
```

## grep指定目录下的指定后缀文件
```
grep -Rni --include="*.xml"  "delivery_info" openerp/
```
