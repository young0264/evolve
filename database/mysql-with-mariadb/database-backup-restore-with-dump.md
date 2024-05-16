---
description: 현재 db를 기준으로 ddl 파일 만들기(backup) or ddl 파일을 기준으로 db에 반영(restore)
---

# database backup , restore (with dump)

### \[ 데이터베이스 백업 파일 만들기 ]&#x20;

**접속 정보 지정**: `-h`, `-u`, `-p` 옵션을 사용하여 호스트, 사용자 이름 및 암호를 지정

* `-h`: MySQL 호스트를 지정
* `-u`: MySQL 사용자 이름을 지정
* `-p`: MySQL 암호를 입력할 수 있도록 요청 (**-p**이후 비밀번호는 붙여서 적어야함)

```
백펍 파일 만들기 예시 : 
mysqldump -h 172.10.40.141 -u young -ppifz9 web_app_db --no-data > tps-scheme.ddl
```

```sh
mysqldump --opt -u [username] -p[pwd] [dbname] > [backupfile.sql]
```

* \[username] 데이터베이스 사용자 이름
* \[pwd] 데이터베이스에 대한 암호(-p와 암호 사이를 띄어쓰지 말 것)
* \[dbname] 데이터베이스의 이름
* \[backupfile.sql] 데이터베이스 백업의 파일 이름(ddl 파일 이름)
* \[-opt] mysqldump 옵션



### \[ 백업 파일로 데이터베이스 복원 ]

```
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```

```
-- 단일 서버 예시 --
mysql -h mydemoserver.mysql.database.azure.com 
      -u myadmin@mydemoserver 
      -ptestdb < testdb_backup.sql
      

-- 유연한(?) 서버 예시 --
mysql -h mydemoserver.mysql.database.azure.com 
      -u myadmin 
      -ptestdb < testdb_backup.sql
```

