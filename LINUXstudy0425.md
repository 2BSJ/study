#190425 리눅스 교육
------
* 톰캣 쉘 설정하기

** 쉘 프로그래밍
```
cd /etc/init.d 에서 vi tomcat으로 만들어서 쉘프로그래밍을한다

#!/bin/sh
#
# Startup script for Tomcat for HMO
#
# chkconfig: 35 85 35
# description: Start Tomcat
#
# processname: tomcat
#
# Source function library.

. /etc/rc.d/init.d/functions

export JAVA_HOME=/usr/local/cafe24/jdk 
** --->java_home을 원래는 적을 필요없지만 그건 로그인해서 쉘이 있을때나 가능하고
    부팅중에는 쉘이 없이 실행되기때문에 부팅시에 필요하다**
export CLASSPATH=.:$JAVA_HOME/lib/tools.jar
export CATALINA_HOME=/usr/local/cafe24/tomcat
export PATH=$PATH:$JAVA_HOME/bin

case "$1" in
** $1은 start**

	start)

		echo -n "Starting tomcat: "
		daemon $CATALINA_HOME/bin/startup.sh
		touch /var/lock/subsys/tomcat
		echo
		;;
	stop)
		echo -n "Shutting down tomcat: "
		daemon $CATALINA_HOME/bin/shutdown.sh
		rm -f /var/lock/subsys/tomcat
		echo
		;;
	restart)
		$0 stop
		sleep 2
		$0 start
		;;
	*)
		echo "Usage: $0 {start|stop|restart}"
		exit 1
esac
exit 0
```

** 쉘을 설정후모드를 변경해준다

```
chmod 755 tomcat ->tomcat의 모드를 755로 바꿔준다
```

** tomcat 실행하기
```
/etc/init.d/tomcat start
```

* 시그널 보내기

** `kill-l`은 시그널의 종류를 확인할 수 있음
** `kill -9 pid -> kill -9 1710` 이런식으로 죽일수 있음
** 프로세스를 직접 죽이고 싶을때

* 쉘로 작성한 tomcat을 직접 서비스로 등록하기

** `chkconfig --add tomcat` 을 해서 서비스에 등록한다.
