---
title:  "프로세스의 생성" 

categories:
  - OS
tags:
  - [OS, 오에스, 운영체제, 프로세스, Operating System]

toc: true
toc_sticky: true

date: 2022-12-19
last_modified_at: 2022-12-19
---


# 4. 프로세스의 생성

- In UNIX-like O/S
    - 새로운 프로세스는 fork()라는 걸로 생성이 됨.
    - Child process는 부모 프로세스의 주소공간을 복사함.
- 부모, 자식 프로세스는 여전히 됨.
    - 포크 이후의 명령어에서 계속 실행을 하겠다.
- 차이점 이라면
    - fork()의 리턴 코드는 0이면, 차일드 프로세스고
    - pid가 0이 아닌걸 리턴하면 parent 프로세스임.
    
- fork()를 한 후에 뭘 하는거
    - address space를 그대로 복제하고, parent는 자기 할 일을 그대로 계속 하거나
    - child가 run하는 동안에 wait()할 수도 있음.
    - child는 address space 자체를 그대로 복사해서, child는 parent에 영향을 못 끼침.
    

execlp란? 프로세스를 새로만드는 주 용도는 전혀 다른 일을 하고 싶은데, 이 프로그램이 Launcher프로그램임. 자기가 실행할 때 새로운 버전이 있는지 확인하고, 업그레이드함.