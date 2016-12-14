
.. _install:

========
安装部署
========

EMQPLUS企业版R2服务器可跨平台运行在Linux、FreeBSD、Mac OS X或Windows服务器上。

.. NOTE:: 产品部署建议Linux、FreeBSD服务器，不推荐Windows服务器。

---------------------
EMQPLUS企业版R2程序包
---------------------

EMQPLUS企业版服务器每个版本会发布Ubuntu、CentOS、FreeBSD、Mac OS X、Windows平台程序包与Docker镜像。

联系EMQ公司获取程序包: http://emqtt.com/about#contacts

安装包命名由平台、版本组成，例如: emqplus-enterprise-centos7-r2.zip

.. _install_on_linux:

---------------
Linux服务器安装
---------------

CentOS平台为例，获取安装包: emqplus-enterprise-centos7-r2.zip

.. code-block:: bash

    unzip emqplus-enterprise-centos7-r2.zip

控制台调试模式启动，检查 *EMQ* 是否可正常启动:

.. code-block:: bash

    cd emqttd && ./bin/emqttd console

*EMQ* 消息服务器如启动正常，控制台输出:

.. code-block:: bash

    starting emqttd on node 'emq@127.0.0.1'
    emqttd ctl is starting...[ok]
    emqttd hook is starting...[ok]
    emqttd router is starting...[ok]
    emqttd pubsub is starting...[ok]
    emqttd stats is starting...[ok]
    emqttd metrics is starting...[ok]
    emqttd pooler is starting...[ok]
    emqttd trace is starting...[ok]
    emqttd client manager is starting...[ok]
    emqttd session manager is starting...[ok]
    emqttd session supervisor is starting...[ok]
    emqttd wsclient supervisor is starting...[ok]
    emqttd broker is starting...[ok]
    emqttd alarm is starting...[ok]
    emqttd mod supervisor is starting...[ok]
    emqttd bridge supervisor is starting...[ok]
    emqttd access control is starting...[ok]
    emqttd system monitor is starting...[ok]
    dashboard:http listen on 0.0.0.0:18083 with 2 acceptors.
    mqtt:tcp listen on 0.0.0.0:1883 with 8 acceptors.
    mqtt:ssl listen on 0.0.0.0:8883 with 4 acceptors.
    mqtt:ws listen on 0.0.0.0:8083 with 4 acceptors.
    Enterprise EMQ Broker 2.13 is running now

CTRL+c关闭控制台。守护进程模式启动:

.. code-block:: bash

    ./bin/emqttd start

启动错误日志将输出在log/目录。

*EMQ* 消息服务器进程状态查询:

.. code-block:: bash

    ./bin/emqctl status

正常运行状态，查询命令返回:

.. code-block:: bash

    $ ./bin/emqctl status
    Node 'emq@127.0.0.1' is started
    emqttd 2.1.3 is running

*EMQ* 消息服务器提供了状态监控URL::

    http://localhost:8083/status

停止服务器::

    ./bin/emqttd stop

.. _install_on_freebsd:

-----------------
FreeBSD服务器安装
-----------------

FreeBSD程序包下载: http://emqtt.com/downloads/latest/freebsd

FreeBSD平台安装过程与Linux相同。

.. _install_on_mac:

----------------
Mac OS X系统安装
----------------

Mac下开发调试MQTT应用，配置文件'etc/emq.conf' log段落打开debug日志，控制台可以查看收发MQTT报文详细:

.. code-block::

    ## Console log. Enum: off, file, console, both
    log.console = both

    ## Console log level. Enum: debug, info, notice, warning, error, critical, alert, emergency
    log.console.level = debug

    ## Console log file
    log.console.file = log/console.log

*EMQ* 在Mac平台下安装启动过程与Linux相同。

.. _install_on_windows:

-----------------
Windows服务器安装
-----------------

Windows平台程序包获取解压后，打开Windows命令行窗口，cd到程序目录。

控制台模式启动::

    bin\emqttd console

如启动成功，会弹出控制台窗口。

关闭控制台窗口，停止emqttd进程，准备注册Windows服务。

.. WARNING:: EMQ-2.0暂不支持服务注册

*EMQ* 注册为Windows服务::

    bin\emqttd install

*EMQ* 服务启动::

    bin\emqttd start

*EMQ* 服务停止::

    bin\emqttd stop

*EMQ* 服务卸载::

    bin\emqttd uninstall

.. _install_docker:

--------------
Docker镜像安装
--------------

EMQPLUS企业版R2 Docker镜像获取:

解压emqplus-enterprise-docker镜像包::

    unzip emqplus-enterprise-docker-r2.zip

加载镜像::

    docker load < emqplus-enterprise-docker-r2

启动容器::

    docker run -itd --net='host' --name emqplus20 emqplus-enterprise-docker-r2

停止容器::

    docker stop emqplus20

开启容器::

    docker start emqplus20

进入Docker控制台::

    docker exec -it emqplus20 /bin/bash

.. _tcp_ports:

---------------
TCP服务端口占用
---------------

EMQPLUS企业版 R2消息服务器默认占用的TCP端口包括:

+-----------+-----------------------------------+
| 1883      | MQTT协议端口                      |
+-----------+-----------------------------------+
| 8883      | MQTT(SSL)端口                     |
+-----------+-----------------------------------+
| 8083      | MQTT(WebSocket), HTTP API端口     |
+-----------+-----------------------------------+
| 18083     | Dashboard管理控制台端口           |
+-----------+-----------------------------------+

上述占用端口可通过etc/emq.conf配置文件的'Listeners'段落设置:

.. code-block:: properties

    ## TCP Listener: 1883, 127.0.0.1:1883, ::1:1883
    mqtt.listener.tcp = 1883

    ## SSL Listener: 8883, 127.0.0.1:8883, ::1:8883
    mqtt.listener.ssl = 8883
    
    ## HTTP and WebSocket Listener
    mqtt.listener.http = 8083

通过注释或删除相关段落，可禁用相关TCP服务启动。

18083端口是Web管理控制占用，该端口由`emq_dashboard`_插件启用。

控制台URL: http:://localhost:18083/ ，默认登录用户名: admin, 密码: public。

.. _quick_setup:

--------
快速设置
--------

*EMQ* 消息服务器主要配置文件:

+----------------------+-----------------------------------+
| etc/emq.conf         | EMQ消息服务器参数设置             |
+----------------------+-----------------------------------+
| etc/plugins/\*.conf  | EMQ 插件配置文件                  |
+----------------------+-----------------------------------+

etc/emq.conf 中两个重要的虚拟机启动参数:

+-----------------------+------------------------------------------------------------------+
| node.process_limit    | Erlang虚拟机允许的最大进程数，emqttd一个连接会消耗2个Erlang进程  |
+-----------------------+------------------------------------------------------------------+
| node.max_ports        | Erlang虚拟机允许的最大Port数量，emqttd一个连接消耗1个Port        |
+-----------------------+------------------------------------------------------------------+

.. NOTE:: Erlang的Port非TCP端口，可以理解为文件句柄。

node.process_limit = 参数值 > 最大允许连接数 * 2

node.max_ports = 参数值 > 最大允许连接数

.. WARNING:: 实际连接数量超过Erlang虚拟机参数设置，会引起EMQ消息服务器宕机!

etc/emq.conf配置文件的'Listeners`段落设置最大允许连接数:

.. code-block:: properties

    mqtt.listener.tcp = 1883

    mqtt.listener.tcp.acceptors = 8

    mqtt.listener.tcp.max_clients = 1024

*EMQ* 2.0消息服务器详细设置，请参见文档: :ref:`config`

.. _init_d_emqttd:

-------------------
/etc/init.d/emqttd
-------------------

.. code:: shell

    #!/bin/sh
    #
    # emqttd       Startup script for emqttd.
    #
    # chkconfig: 2345 90 10
    # description: emqttd is mqtt broker.

    # source function library
    . /etc/rc.d/init.d/functions

    # export HOME=/root

    start() {
        echo "starting emqttd..."
        cd /opt/emqttd && ./bin/emqttd start
    }

    stop() {
        echo "stopping emqttd..."
        cd /opt/emqttd && ./bin/emqttd stop
    }

    restart() {
        stop
        start
    }

    case "$1" in
        start)
            start
            ;;
        stop)
            stop
            ;;
        restart)
            restart
            ;;
        *)
            echo $"Usage: $0 {start|stop}"
            RETVAL=2
    esac


chkconfig::

    chmod +x /etc/init.d/emqttd
    chkconfig --add emqttd
    chkconfig --list

boot test::

    service emqttd start

.. NOTE::

    ## erlexec: HOME must be set
    uncomment '# export HOME=/root' if "HOME must be set" error.

.. _emq_dashboard:       https://github.com/emqtt/emqttd_dashboard

