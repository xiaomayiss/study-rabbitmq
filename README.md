# study-rabbitmq
学习rabbmitmq的demo示例
#安装依赖文件
yum -y install gcc glibc-devel make ncurses-devel openssl-devel xmlto perl wget
#安装/配置erlang 语言环境
    #安装
    wget http://www.erlang.org/download/otp_src_18.3.tar.gz  //下载erlang包
    tar -xzvf otp_src_18.3.tar.gz  //解压
    cd otp_src_18.3/ //切换到安装路径
    ./configure --prefix=/usr/local/erlang  //生产安装配置
    make && make install  //编译安装

    #配置
    vi /etc/profile  //在底部添加以下内容
        #set erlang environment
        ERL_HOME=/usr/local/erlang
        PATH=$ERL_HOME/bin:$PATH
        export ERL_HOME PATH

    source /etc/profile  //生效

    #check 
    erl  //如果进入erlang的shell则证明安装成功，退出即可。

#下载安装RabbitMQ
    #安装
    cd /usr/local  //切换到计划安装RabbitMQ的目录，我这里放在/usr/local
    wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.1/rabbitmq-server-generic-unix-3.6.1.tar.xz  //下载RabbitMQ安装包
    xz -d rabbitmq-server-generic-unix-3.6.1.tar.xz
    tar -xvf rabbitmq-server-generic-unix-3.6.1.tar


    mv rabbitmq_server-3.6.1/ rabbitmq

    #set rabbitmq environment
    vi /etc/profile
        #set rabbitmq environment
        export PATH=$PATH:/usr/local/rabbitmq/sbin
    source /etc/profile

#常用命令
    启动服务：rabbitmq-server -detached【 /usr/local/rabbitmq/sbin/rabbitmq-server  -detached 】
    查看状态：rabbitmqctl status【 /usr/local/rabbitmq/sbin/rabbitmqctl status  】
    关闭服务：rabbitmqctl stop【 /usr/local/rabbitmq/sbin/rabbitmqctl stop  】
    列出角色：rabbitmqctl list_users
    
    
#配置网页插件以及用户/vhost
    mkdir /etc/rabbitmq
    rabbitmq-plugins enable rabbitmq_management

    #配置linux 端口 15672 网页管理 5672 AMQP端口
    firewall-cmd --permanent --add-port=15672/tcp
    firewall-cmd --permanent --add-port=5672/tcp
    systemctl restart firewalld.service

    #add user
    rabbitmqctl add_user superrd superrd  //添加用户，后面两个参数分别是用户名和密码，我这都用superrd了。
    rabbitmqctl set_permissions -p / superrd ".*" ".*" ".*"  //添加权限
    rabbitmqctl set_user_tags superrd administrator  //修改用户角色

    #get
    http://192.168.231.6:15672/
