---
description: 사용자 관리, 파일 속성 Linux 명령어
---

# User Management Linux Commands

|          |                                                                                                                                                    |
| -------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| adduser  | 새로운 사용자 및 그룹, 정보들 추가                                                                                                                               |
| useradd  | <p>사용자만 추가</p><ul><li>useradd -u [설정할 GID] [생성할 사용자명] : 사용자를 추가하면서 GID를 직접 지</li><li>useradd -g [그룹명] [생성할 사용자명] : 사용자를 생성하면서 그룹명에 포함시킴.</li></ul> |
| passwd   | <p>사용자 비밀번호 변경</p><ul><li>passwd newuser : newuser의 사용자의 비밀번호를 지정(변경)</li></ul>                                                                    |
| usermod  | 사용자 속성을 변경                                                                                                                                         |
| userdel  | 사용자를 삭제                                                                                                                                            |
| chage    | 사용자의 암호를 주기적으로 변경하도록 설정                                                                                                                            |
| groups   | 사용자가 소속된 그룹을 보여줌                                                                                                                                   |
| groupadd | <p></p><p>새로운 그룹을 생성</p><ul><li>그룹추가 : group add [생성할 그룹이름</li><li>그룹 GID 확인 : grep [그룹이름] /etc/group</li></ul>                                    |
| groupmod | <p>그룹의 속성을 변경</p><ul><li>그룹 삭제 : groupdel [삭제할 그룹 이름]</li><li>그룹 이름 변경 : groupmod -n [바뀔그룹이름] [바뀌기전그룹이름]</li></ul>                                 |
| groupdel | 그룹을 삭제                                                                                                                                             |
| gpasswd  | 그룹의 암호를 설정하거나 그룹 관리를 수행                                                                                                                            |

