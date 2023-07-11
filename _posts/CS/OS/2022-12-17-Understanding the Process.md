---
title:  "프로세스의 이해" 

categories:
  - OS
tags:
  - [OS, 오에스, 운영체제, 프로세스, Operating System]

toc: true
toc_sticky: true

date: 2022-12-17
last_modified_at: 2022-12-17
---


# 3. 프로세스의 이해

Process는 실행중인 프로그램임. 

운영체제 입장에서는 작업의 단위가 프로세스이다.

프로세스는 실행이 되기 위해선

- CPU
- Memory
- Files
- IO devieces.

이렇게 총 4개를를 쓸 수 있어야함.

여러개의 섹션으로 나눠져있는 프로세스들의 메모리 레이아웃을 보면

- Text section:
    - the executable code
- Data section:
    - 전역변수
- Heap section:
    - 할당으로 메모리가 된 것.
- Stack section:
    - 함수나, function parameters, return addresses, local variables

프로세스의 메모리가 0 ~ max까지라고 할 때

text , data , heap, stack 의순서대로 할당됨. stack은 max에서 밑으로, heap은 0에서 max쪽으로 향하며 메모리가 쌓임. 

![Imgur](https://i.imgur.com/ycVMy8C.png)

- OS가 프로세스의 상태를 어떻게 관리하는가. (5개의 상태)
    - New : 프로세스가 만들어짐
    - Running : Process가 실행되고 있음.
    - Waiting : 다른 프로세스가 CPU를 점유하고 있어서 대기하고 있는 상태. (일 안 끝남)
        - 주로 IO가 끝날 때 까지 대기.
    - Ready : 대기 하고 있다가, 다른 애가 끝났다. 그렇다고 바로 점유 못함. 레디큐로 가서 나 점유 가능하다. 그런 걸 알려줘야함.
    - Terminated : 프로세스의 일이 다 끝난 상태.
    
- PCB(Process Control Block) or TCB (Task Control Block)라는 2가지 형태가 있는데. 

PCB나, TCB라는 구조체를 만들어서 여기다가 프로세스가 가져야하는 모든 정보를 다 집어넣음.

- 각각 프로세스는 PCB를 가지고 OS안에서 핸들링함.
- PCB가 가지고 있는 정보는 굉장히 많음. 그 중에서도 있는것
    - Process State : run / ready / wait .....
    - Program counter → PC ( 현재 메모리를 가리키고 있는게 어디인가. ex) 0x0004EA2F 주소)
    - CPU registers : 레지스터 정보들을 가지고 있어야함.
    - CPU-scheduling information
    - Memory-management information (malloc 얼마나 했는지 등)
    - Accounting information (어떤 유저가 Creation 했는지)
    - I/O status information (어떤 파일을 오픈했고, 어떤 자원을 오픈해서 락을 걸어놨는지.)
    

이런 PCB는 프로세스마다 가짐. 

- 프로세스는 Single thread of execution을 가짐. CPU가 프로세스를 바꾸고 싶을때는, 다른 프로세스의 PCB를 가져오면 댐.
- Single thread의 컨트롤은 한 번에 한 테스크밖에 못하기 때문에
- 여러 개를 동시에 실행하는 걸 Multi-processing, Multi-tasking임.
- 프로그램 한 개 안에서도 여러 개의 스레드가 실행될 필요가 많아짐. 그래서 thread라는 단위가 생김. 이 문장 위의 thread는 선형적으로 한 줄로 프로그램이 실행된다는 'thread'를 의미함.
- Process를 여러 개 하는 것보다 thread를 여러개 하는게 더 좋은 점이 많음. 그래서 멀티스레드로 보통함.

그래서 Multiprogramming의 목적은 동시에 여러 개의 프로세스를 실행시켜보자! (at the same time. simultaneously, concurrently)

- CPU 사용 효율을 높이자.
- Time sharing을 하는 이유는 CPU Core를 프로세스간의 아주 자주 변경을 해서 사용자 입장에서 각 프로그램이 동시에 도는 것 처럼 보이게 하기
- CPU를 스케줄링 해줘야함 : Scheduling Queues
    - 어떤 프로세스가 레디 큐에다가 신청을 넣음. CPU가 사용이 끝나면 레디 큐 맨 뒤에다가 대기를 시킴.
    
- 만약 IO가 있으면 wait queue로 가고, 끝나면 ready queue로 감.

- Context Switch (문맥 교환) : OS가 교환해줌
    - 프로세스가 사용되고 있는 상태(context)는 PCB에 저장되어 있음.
    - interrupt가 일어났을때 지금 context를 저장해 놓고, 그게 다 끝나면 그 context를 다시 불러옴.
    
- Context Switch란
    - 현재의 PCB를 저장하고, 다른 프로세스의 PCB를 불러오기 등..
    
- OS는 제공한다
    - 프로세스 생성
    - 프로세스 termination
    
- 프로세스가 새로운 프로세스를 생성할 수 있음.
    - the creating process : a parent process.
    - a newly created process : a child process
    - fork()라는 함수로 만듬. 이런 프로세스를 만들 수 있는 첫 번째 프로세스 (init)
    
- 실행할 때 두 가지 가능성이 있음
    - Parent가 있고 fork해서 부모도 게속 실행하고, 자식도 실행하고. 그런 방식
    - Parent 가 있고 fork해서 Child가 끝날 때까지 wait하는 방식.
    
- Address-space 측면에서도 두 가지 경우가 있음
    - 만약에 CHILD가 똑같은 일을 하는 거면 따로 공간을 둘 필요가 없으니까, PCB만 두 개 두어서, 주소공간을 PARENT와 같이 쓰는 거랑
    - 다른 일을 하면 new program을 로드하는 것.
    
- A process terminates
    - 그냥 final statement(return)을 실행함
    - 강제로 끝내고 싶으면 exit()를 씀. (OS의 SYSTEM CALL)
    - OS입장에선 메모리 회수, 파일 회수, 그런거 다 회수하기.
    
- Zombie and Orphan
    - Zombie process : fork해서 child를 생성했는데 wait를 호출 안하고 부모는 부모일을 계속하면 얘는 그냥 좀비처럼 남아있는 것.
    - Orphan process : parent process가 wait를 하지 않고 종료해버리면, child는 그대로 남아있으니 고아가 됨