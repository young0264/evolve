# File Installation(Source)

```bash
$ brew install wget
$ brew install gcc
$ wget <http://download.redis.io/releases/redis-7.0.8.tar.gz>
$ tar -zxvf redis-7.0.8.tar.gz
$ mv redis-7.0.8 redis
$ cd redis
$ make

$ make PREFIX=/home/centos/redis install
#해당 디렉터리 하위에 bin 폴더 생성
```

*   다시 빌드

    ```bash
    make distclean
    make PREFIX=/Users/young/redis install
    ```

```bash
/Users/young/redis/bin  pwd
/Users/young/redis/bin

 /Users/young/redis/bin  ls
redis-benchmark redis-check-aof redis-check-rdb redis-cli       redis-sentinel  redis-server
```

```bash
# redis 실행
/Users/young/redis/bin/redis-server /Users/young/redis/redis/redis.conf
```
