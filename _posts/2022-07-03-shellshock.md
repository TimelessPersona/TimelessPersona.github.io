---
title: "[pwnable.kr] shellshock"
excerpt: "shellshock 풀이"

categories:
    - pwnable
date: 2022-07-03
---

# 1. 문제

    ssh 명령어를 통해 접속하면 아래와 같은 화면이 보인다.

![shellshock 접속화면](/img/shellshock-1.JPG)

    ls 명령어로 확인해 본 결과 bash, flag, shellshock, shellshock.c 파일이 보인다.
    shellshock.c 의 소스코드를 확인해 보자.

![shellshock.c](/img/shellshock-2.JPG)

    shellshock 프로그램은 같은 경로의 bash를 실행시키고 shock_me 라는 문자열을 출력한다.

# 2. shellshock

    bash를 실행시키는 프로그램. 그런데 이것으로 어떻게 flag를 열어볼 수 있을까?
    정답은 이름에 있다.
    shellshock는 굉장히 심각한 취약점 중 하나였다. 환경 변수를 설정할 때 명령어가 실행되는 취약점을 이용하여
    쉘에서 악의적 명령을 실행하게 하는 방식이었다.

    이러한 shellshock의 방식을 생각해 보면 이 문제에서 실행하는 bash는 shellshock 취약점 패치가 진행되지 않은 bash shell일 것이다.
    그리고 같은 방식으로 환경변수 등록 시 명령어를 추가로 삽입하여 flag를 읽어내는 방식으로 풀어낼 수 있을 것이다.

# 3. 풀이

    해당 bash shell이 shellshock가 적용 가능한 취약한 shell인지 확인하는 명령어 문장이 있다.
    $ env x='() { :;}; echo vulnerable' bash -c "echo this is a test"
    실행 결과가 this is a test 라면 안전한 shell이고 vulnerable 이 추가되어 출력된다면 취약한 shell이다.

    해당 명령어가 동작하는 원리는 원래 함수형 환경변수가 아닌 일반 변수가
    변수='(){ :;}; 공격코드' 같은 모양의 경우 함수로 인식되어 실행되는 것이다.

    따라서 우리는 이 것을 이용해 명령어를 작성하면 된다.
    고려해야할 것은 두 가지이다. flag를 읽어야 하므로 /bin/cat flag라는 명령어를 공격코드로 넣어주어야 하고,
    shellshock 프로그램이 셸쇼크에 취약한 bash shell을 실행하므로 shellshock 실행 구문도 넣어주어야 한다.
    따라서 최종적으로 작성된 명령어는 아래와 같은 형태가 된다.

![finalcode](/img/shellshock-3.JPG)

    flag가 출력되는 것을 확인할 수 있다.