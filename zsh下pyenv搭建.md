## 安装

```

git clone https://github.com/yyuu/pyenv.git ~/.pyenv

```



## 环境变量

```

echo 'export PYENVROOT="$HOME/.pyenv"' >> ~/.zshrc

echo 'export PATH="$PYENVROOT/bin:$PATH"' >> ~/.zshrc

echo 'eval "$(pyenv init -)"' >> ~/.zshrc

exec $SHELL

source ~/.zshrc

```



## 安装虚拟环境

```

 git clone https://github.com/yyuu/pyenv-virtualenv.git ~/.pyenv/plugins/pyenv-virtualenv
 echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
 source ~/.zshrc
```

## 利用国内源加速安装python版本

```

v=3.5.2|wget http://mirrors.sohu.com/python/$v/Python-$v.tar.xz -P ~/.pyenv/cache/;pyenv install $v

```
或者下载tar包放置在.pyenv/cache目录下， 然后执行pyenv install即可安装



## 创建虚拟环境

```

pyenv virtualenv 2.7.10 env2710

```

## 激活虚拟环境

```

pyenv activate env2710

```
