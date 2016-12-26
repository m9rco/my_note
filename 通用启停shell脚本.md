shell脚本
```
#! /bin/bash

SERVICE_NAME="picture service"
ROOTPATH="/root/projects/odoo_tool/src"
PROCESS_NUM=17

count_process(){
    num=$(ps aux | grep gunicorn | grep 10096 | wc -l)
    return $num
}

start(){
    count_process
    num=$?
    if [ "$num" -ge "1" ]
    then
        echo "$SERVICE_NAME is running"
        return
    fi
    pyenv activate env352
    echo "$SERVICE_NAME starting"
    export PYTHONPATH=$ROOTPATH
    export LOG_CFG=$ROOTPATH/util/logging.yml
    cd $ROOTPATH & setsid gunicorn -w 16 -k gevent -b 0.0.0.0:10096 app.app:app > /dev/null 2>&1
    sleep 1s
    count_process
    num=$?
    if [ "$num" -ge "1" ]
    then
        echo "$SERVICE_NAME started"
        return
    fi
}

stop(){
    count_process
    num=$?
    if [ "$num" -eq "0" ]
    then
        echo "$SERVICE_NAME is not running"
        return
    fi
    echo "$SERVICE_NAME stopping"
    ps aux | grep gunicorn | grep 10096 | awk '{print $2}' | xargs kill -9
    sleep 1s
    count_process
    num=$?
    if [ "$num" -eq "0" ]
    then
        echo "$SERVICE_NAME stopped"
        return
    fi
}

arg="$*"
echo ${arg}ing;
case $arg in
    'start')
        start;;
    'stop')
        stop;;
    'restart')
        stop
        start;;
    '*')
        echo 'Usage: start | stop | restart';;
esac
```
