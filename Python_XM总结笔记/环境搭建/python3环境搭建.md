- 安装git
~~~sh
 sudo apt-get install git
~~~

- ubuntu安装python3

  - 安装python3

    ~~~sh
    apt-get install python3
    ~~~

  - 安装pip

    ~~~sh
    apt-get install python3-pip
    # 升级pip版本
    python -m pip install -U pip
    ~~~

  - 安装python虚拟环境

    ~~~sh
    pip3 install virtualenv
    pip3 install virtualenvwrapper

    使得安装的virtualenvwrapper生效，编辑~/.bashrc文件，内容如下:
    VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3	# 让其自动选择Python3解释器
    export WORKON_HOME=$HOME/.virtualenvs
    export PROJECT_HOME=$HOME/workspace
    source /usr/local/bin/virtualenvwrapper.sh

    # 使编辑后的文件生效
    source ~/.bashrc
    ~~~

- 安装配置MySQL

  ~~~sh
  apt-get install mysql-server	# 过程中会提示你设置密码
  apt-get install libmysqlclient-dev
  ~~~

  ​

