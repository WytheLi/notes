- #### 项目部署

  - 基于ubuntu 16.04系统

  - 服务器：阿里云服务器

  - Gunicron(Web服务器) + Nginx(代理服务器)

- #### 环境搭建

  - mysql安装

    ~~~sh
    apt-get install mysql-server
    apt-get install libmysqlclient-dev
    ~~~

  - redis安装
    ~~~sh
    sudo apt-get install redis-server
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

- 项目拓展包

  ~~~sh
  pip freeze > requirements.txt

  pip install -r requirements.txt
  ~~~

  - ps：在安装 Flask-MySQLdb 的时候可能会报错，可能是依赖包没有安装，执行以下命令安装依赖包：
    ~~~sh
    sudo apt-get build-dep python-mysqldb
    ~~~

- Nginx (代理服务器)

  - 安装

    ~~~sh
    sudo apt-get install nginx
    ~~~

  - 运行和停止

    ~~~sh
    /etc/init.d/nginx start #启动
    /etc/init.d/nginx stop  #停止

    # 查看进程
    ps aux | grep nginx
    ~~~

  - 配置文件

    ~~~sh
    # 编辑文件:/etc/nginx/sites-available/default

    # 如果是多台服务器的话，则在此配置，并修改 location 节点下面的 proxy_pass 
    upstream flask {
            server 127.0.0.1:5000;
            server 127.0.0.1:5001;
    }
    server {
            # 监听80端口
            listen 80 default_server;
            listen [::]:80 default_server;

            root /var/www/html;

            index index.html index.htm index.nginx-debian.html;

            server_name _;

            location / {
                    # 请求转发到gunicorn服务器
                    proxy_pass http://127.0.0.1:5000;
                    # 请求转发到多个gunicorn服务器
                    # proxy_pass http://flask;
                    # 设置请求头，并将头信息传递给服务器端 
                    proxy_set_header Host $host;
                    # 设置请求头，传递原始请求ip给 gunicorn 服务器
                    proxy_set_header X-Real-IP $remote_addr;
            }
    }
    ~~~

- Gunicorn (绿色独角兽 是一个python WSGI HTTP服务器)

  - 安装

    ~~~sh
    pip install gunicorn
    ~~~

  - 查看选项

    ~~~sh
    gunicorn -h
    ~~~

  - 运行

    ~~~sh
    # -w: 表示进程（worker） -b：表示绑定ip地址和端口号（bind）
    gunicorn -w 2 -b 127.0.0.1:5000 运行文件名称:Flask程序实例名
    ~~~

- 将代码拷贝到服务器

  - scp

    ~~~sh
    scp -r 本地文件路径 root@39.106.21.198:远程保存路径
    ~~~

  - git clone

    ~~~sh
    # 安装git
    sudo apt-get install git
    # clone
    git clone [资源地址]
    ~~~

    ​

