# Linux command list

| 명령                     | 용법                                                                                                                                                                                                            |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| --help                 | <p></p><p>* 함께 사용하는 모든 플래그 반환</p>                                                                                                                                                                             |
| `ls`                   | 디렉토리의 내용을 나열합니다.                                                                                                                                                                                              |
| `alias`                | 별칭 정의 또는 표시                                                                                                                                                                                                   |
| `unalias`              | 이미 정의된 별칭에서 alias 제거 명령어                                                                                                                                                                                      |
| `pwd`                  | 현재 디렉터리 절대 경로 출려                                                                                                                                                                                              |
| `cd`                   | <p>‘change directory’ 액세스 하려는 디렉터리로 전환</p><ul><li>앞에 ‘/’의 유무에 따라 절대경로와 상대경로 지정 가능.</li><li>cd : 홈 폴더로 이동</li><li>cd .. : 한레벨 위로 올림</li><li>cd - : 이전 디렉토리로 복귀</li></ul>                                       |
| `cp`                   | <p></p><p>파일과 폴더 복사 명령어.</p><ul><li>cp [카피할 대상 파일/폴더 이름] [새로운 파일/폴더 이름]</li><li>디렉터리 전체 복사 : cp -r dir_to_copy/ new_copy_dir/ &#x3C;Linux에서는 폴더가 슬래시(/)로 끝남></li></ul>                                        |
| `rm`                   | 파일 및 디렉터리 제거                                                                                                                                                                                                  |
| `mv`                   | <p>파일 및 디렉터리를 이동(이름 바꾸기)합니다.<br>mv [source_file] [destination_folder]/</p>                                                                                                                                    |
| `mkdir`                | <p>디렉터리를 생성합니다.<br>하위 디렉터리 생성 : mkdir -p movies/2004/</p>                                                                                                                                                     |
| `man`                  | <p>다른 명령의 매뉴얼 페이지를 표시합니다.<br>ex) man mkdir</p>                                                                                                                                                                |
| `touch`                | 파일 액세스 및 수정 시간을 업데이트                                                                                                                                                                                          |
| `chmod`                | 파일 권한 변경 r(읽기), w(쓰기), x(실행)                                                                                                                                                                                  |
| `./`                   | 명령 자체는 아니지만 쉘 터미널에서 직접 시스템에 설치된 인터프리터를 사용해 실행 파일을 실행할 수 있음.                                                                                                                                                   |
| `exit`                 | 셸 세션을 종료. 해당 세션을 자동으로 닫음                                                                                                                                                                                      |
| `sudo`                 | ‘superuser do’ 루트 사용자 역할을 할 수 있게 함.                                                                                                                                                                           |
| `shutdown`             | 컴퓨터 전원을 끔. 중지, 재부팅시 사용 가능                                                                                                                                                                                     |
| `htop`                 | 시스템 리소스를 관리할 수 있는 대화형 프로세스 뷰어                                                                                                                                                                                 |
| `unzip`                | .zip 파일 추출                                                                                                                                                                                                    |
| `apt`, `yum`, `pacman` | 패키지 관리자에 액세스하고 Linux 배포판에 따라 하나 또는 다른 패키지 관리자를 사용 Debian 기반(Ubuntu, Linux Mint) : sudo apt install gimp Red Hat 기반(Fedora, CentOS) : sudo yum install gimp Arch 기반(Manjaro, Arco Linux) : sudo pacman -S gimp |
| `echo`                 | 정의된 텍스트 표시(출력)                                                                                                                                                                                                |
| `cat`                  | 터미널에서 직접 파일을 만들고, 보고, 연결 가능.                                                                                                                                                                                  |
| `ps`                   | 현재 셸 세션이 실행 중인 프로세스 확인                                                                                                                                                                                        |
| `kill`                 | 프로세스 종료 명령어                                                                                                                                                                                                   |
| `ping`                 | 네트워크 연결 테스트 명령어. ip 주소 요청시 사용                                                                                                                                                                                 |
| `vim`                  | 오픈 소스 터미널 텍스트 편집기.                                                                                                                                                                                            |
| `history`              | 이전에 사용한 명령어들이 포함된 목록 출력                                                                                                                                                                                       |
| `passwd`               | 사용자 계정의 비밀번호 변경                                                                                                                                                                                               |
| `which`                | 셸 명령의 전체 경로를 출력                                                                                                                                                                                               |
| `shred`                | 파일 내용 복구가 어렵게 만드는 명령어. -u 를 같이 사용하면 파일을 즉시 삭제함                                                                                                                                                                |
| `less`                 | 파일을 앞뒤로 검사할 수 있는 프로그램                                                                                                                                                                                         |
| `tail`                 | 파일의 마지막 줄을 출력. -n 명령어를 사용해 줄 갯수 범위 설정.                                                                                                                                                                        |
| `head`                 | -n 플래그를 사용해 파일의 윗쪽 줄 부터 출력                                                                                                                                                                                    |
| `grep`                 | 정규표현식과 일치하는 줄을 검색하여 인쇄함. ex) grep “linux” long.txt                                                                                                                                                            |
| `whoami`               | 현재 사용중인 사용자 이름 표시                                                                                                                                                                                             |
| `whatis`               | 다른 명령에 대한 한 줄 설명을 인쇄 ex) whatis python, whatis whatis                                                                                                                                                         |
| `wc`                   | <p>텍스트 파일의 “단어 수”를 반환 함 <br>wc long.txt # 37 207 1000 long.txt 라인, 단어, 1000바이트크기, 파일이름 반환.<br>단어 수만 필요하면 wc -w long.txt</p>                                                                                   |
| `uname`                | ‘Unix name’ 약어.유용하게 사용할 수 있는 운영 체제 정보를 인쇄                                                                                                                                                                     |
| `neofetch`             | Linux 배포판의 ASCII 로고 옆에 커널 버전, 셸, 하드웨어 등 시스템 정보를 표시하는 CLI(명령줄 인터페이스) 도구                                                                                                                                        |
| `find`                 | <p>정규 표현식을 기반으로 디렉토리 파일 계층 구조를 검색함</p><ul><li>file [flags] [path] -name [expression]</li><li>find ./ -name “long.txt” # long.txt 파일 검색</li></ul>                                                              |
| `wget`                 | (World Wide Web get) 인터넷에서 파일을 검색합니다.                                                                                                                                                                         |



reference \
&#x20;[https://kinsta.com/blog/linux-commands/](https://kinsta.com/blog/linux-commands/)