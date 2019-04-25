#190425 리눅스 교육
------
* 톰캣 쉘 설정하기

	 * 쉘 프로그래밍
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
 --->java_home을 원래는 적을 필요없지만 그건 로그인해서 쉘이 있을때나 가능하고
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

	* `kill-l`은 시그널의 종류를 확인할 수 있음
	* `kill -9 pid -> kill -9 1710` 이런식으로 죽일수 있음
	* 프로세스를 직접 죽이고 싶을때

* 쉘로 작성한 tomcat을 직접 서비스로 등록하기

	* `chkconfig --add tomcat` 을 해서 서비스에 등록한다.


* 권한 바꾸기

  * `chown webmaster:root hello.txt` 권한 : webmaster , 그룹 : root , 대상파일 : hello.txt

* 파일 .tar로 만들기

  * `tar cvf root.tar /root` 루트디렉토리를 .tar로 만듬

* .tar를 압축하기

	* `gzip root.tar` -> root.tar.gz로 생성됨

* 압축풀기

	* `gzip -d root.tar.gz` -> root.tar.gz 가 root.tar로 됨

* tar 풀기

	* `tar xvf root.tar` ->root.tar 가 root 디렉토리로 나옴

* 한번에 집이랑 tar 풀기

	* `tar xvfz root.tar.gz` -> 바로 root 디렉토리가 나*

---
# NETWORK 교육

* OSI7계층 TCP/IP 4계층

 	* OSI 7계층 전송계층

* 전송 계층
	* TCP UDP
		* TCP는 전화 통화와 비교하면 됨 연결을 맺고 전송하는 방식
			* 하나라도 데이터가 빠지면 큰일나는것들은 tcp로
		* UDP는 편지와 비교하면 됨 연결을 안맺고 전송하는 방식
			* 음악이나 도영상 Streming 서비스 같이 좀 유실되도 상관없는것은 udp로
* HTTP(HYPER TEXT TRANSFER PROTOCOL)
	* `hyper` 연결되어있는
	* `text` 텍스트
	* `protocol` 프로토콜
* 클라이언트는 웹 서버에 request를 하고 서버는 클라이언트에 response를 한다.
	* request가 있으면 항상 response가 되어야함
	* 서버가 request를 받으면 고정되어있는 정보면 디스크에 저장된 것을 보내주거나
	  프로그램으로 http를 만들어 response 한다.
* 902동까지가 ip 801호가 port ip + port 가 socket address 이다.
* TTL = 패킷이 총 거쳐가는 횟수
* 핑 차단하는법
	* ` vi /etc/sysctl.conf` 에서 `net.ipv4.icmp_echo_ignore_all = 1` 설정을 해주고나서`sysctl-p` 를 쳐주면 핑차단이됨
* ip 도메인 설정해주는 법
	* `C:\Windows\System32\drivers\etc\hosts` 파일에서 밑에 자신의 아이피를 적어주고 도메인 적어준뒤에 뒤에 포트번호 해주면 톰캣관리자로 접속가능

* cafe24 https://github.com/jungmintaeng/Bit-academy-Homework

* /root에서

--------

# github

* 삭제 후 저장소도 삭제하기
	* rm -f hello.txt 했다면 깃저장소도 맞춰줘야하기떄문에 `git rm hello.txt` 후에 `git commit -m "삭제"` 하면됨
