---
title: "[pwn college] dojo 챌린지 세팅"
excerpt: "pwn college dojo 시작 전 세팅"

categories:
    - Pwn College
date: 2022-12-30
---

# 1. 원격 접속을 위한 ssh key 생성

![keygen](/img/keygen.png)
![keygen_result](/img/keygen_result.png)

    ssh-keygen -f key -N ''
    해당 명령어를 입력하면 key 파일과 key.pub 파일이 각각 생성된다.

    key 파일과 key.pub 파일은 각각 private key와 public key 인데,
    현재 접속을 위해 필요한 것은 key.pub의 key 값이다.

    cat key.pub 를 통해 key 값을 볼 수 있고, 이를 복사한다.
    복사가 완료되었다면 pwncollege 사이트에서 로그인한 뒤
    우측 상단의 Settings-SSH Key 메뉴로 접속한다.

![sshkey](/img/sshkey.png)

    그리고 복사해 둔 key 값을 입력란에 붙여넣고 Update 버튼을 누르면 된다.



# 2. 접속

    접속하는 방법은 간단하다.
    ssh -i key hacker@dojo.pwn.college
    해당 명령어를 입력하면 계정에 접속할 수 있다.

![connect](/img/connect.png)

    정상적으로 해당 문제 환경에 접속한 것을 확인할 수 있다.


