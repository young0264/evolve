# Installing Tomcat

* [https://tomcat.apache.org/download-90.cgi](https://tomcat.apache.org/download-90.cgi)
* core에 9.xx대 tomcat tar.gz 설치(10점대 부터는 Java EE라는 패키지가 상당부분 Jakarta EE 로 변경됨)
* 해당 파일 경로 bin폴더에 보면 [start.sh](http://start.sh) 라는 스크립트파일이 있음. 실행시키면 tomcat이 실행됨.

***

*   conf 폴더의 ‘server.xml’ 파일 실행

    * 현재 실행중인 tomcat server 포트번호 변경 가능



> ```
> allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1"
> //로컬 주소만을 허용한다는 뜻.!
> ```

*   webapps/manager/META-INF/context.xml 파일

    ```xml
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1" />
    ```

    이 부분을 주석처리.
*   webapps/host-manager/META-INF/manager.xml 파일

    ```xml
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
             allow="127\\.\\d+\\.\\d+\\.\\d+|::1|0:0:0:0:0:0:0:1" />
    ```

    동일하게 주석처리.



*   상단 tomcat-users.xml 파일, 해당부분 주석 해제 후 아래와 같이 수정

    ```xml
    <!-- <role rolename="tomcat"/>
    <role rolename="role1"/>
    <user username="tomcat" password="<must-be-changed>" roles="tomcat"/>
    <user username="both" password="<must-be-changed>" roles="tomcat,role1"/>
    <user username="role1" password="<must-be-changed>" roles="role1"/> -->

    <!-- admin에 admin-gui 추가 -->
    <role rolename="manager-gui"/>
    <role rolename="manager-script"/>
    <role rolename="manager-jmx"/>
    <role rolename="manager-status"/>
    <user username="admin" password="admin" roles="manager-gui, manager-script, manager-jmx, manager-status"/> 
    <user username="deployer" password="deployer" roles="manager-script"/>
    <user username="tomcat" password="tomcat" roles="manager-gui"/>
    ```
