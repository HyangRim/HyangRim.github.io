---
title:  "운영체제의 개념과 구조. " 

categories:
  - OS
tags:
  - [OS, Operating System, Computer science, 운영체제, 오에스]

toc: true
toc_sticky: true

date: 2022-12-16
last_modified_at: 2022-12-16
---

# 2. 운영체제의 개념과 구조

컴퓨터 시스템에는 4가지 구성요소가 있다. 

- 하드웨어
- OS
- 어플리케이션 프로그램
- 유저

시스템 프로그램과 어플리케이션 프로그램들은 OS를 통해서 하드웨어랑 통신하고 있다. 


OS의 일반적인 정의는 항상 컴퓨터에서 Running(실행)하고 있는 것이며, OS의 가장 핵심적인 요소는 Kernel(커널)이다. 

커널에서 시스템 프로그램과 어플리케이션 프로그램을 조종함. 


Classical 컴퓨터 시스템 구성은

- 한개나 그 이상의 CPU
- 여러개의 디바이스 컨트롤러와 그걸 이어주는 버스.


**bootstrap** 프로그램이란? 컴퓨터를 부팅할 수 있게 해주는 프로그램이다. 
 

CPU가 제일 처음에 로딩해야할 거는 ROM에다가 저장해둠. 그런 걸 불러오는게 그게 bootstrap이라는 부팅용 프로그램. 메모리에다가 운영체제를 로딩하는 일을 해줌. (특히 kernel을 로딩.)

- Interrupts란?

I/O device들이 있는데.  만약에 'A'키보드를 눌렀을 때 그걸 눌렀다고 CPU에 알려주는게 Interrupts이다.

I/O device들이 Idle 상태로 있다가 신호를 받으면 Interrupt해서 받음. 그래서 cpu에서 처리함. 

- CPU하고 IO device가 통신하는 게 Interrupts라고 생각하면 됨.

- 폰 노이만 아키텍쳐?
    - 컴퓨터에 있는 명령어의 집합을 메모리에 로딩하면 그걸 cpu가 하나하나 fetch하고 excute하고. 이런 과정을 거치는 걸 폰 노이만 아키텍쳐라고 함.
    - instruction register, Memory register이 있음.

Storage(저장장치) 구조

Register ← Cache ← Main Memory ← Solid-state disk ← hard disk ← oprical disk ← magnetic tapes

- I/O Structure

CPU안에 thread of execution이 있는데. CPU가 캐시 메모리를 통해서 RAM에서 받아오고, Device가 인터럽트를 하고, 데이터를 주고 request를 보내면서 이런 structure을 가지고 있음. 

OS코드는 대부분 I/O처리를 하는데 대부분임. 즉, 디바이스 컨트롤러를 만드는게 대부분임.

kernel개발은 사실상 지금 이 시대에 와서는 안정화 되었다.

- CPU - 명령을 처리해주는 하드웨어
- Processor - 하나 혹은 더 많은 물리적인 칩
- Core - CPU의 유닛.
- MultiCore
- MultiProcessor

- Symmetric Multiprocessing(SMP)
    - CPU가 하나가 아니라 멀티임.
    - 메모리가 한 개인데, 메모리에 연결되어있는 CPU가 여러개임.
- Multi-core design
    - CPU하나의 칩 안에 Core만 여러개 구성하기. 같은 프로세서 칩에 있음.
- Multiprogramming
    - 메모리에 여러개의 프로그램을 올리는게, Multiprogramming이라고 함
    - 메모리에 여러개의 프로세서가 동시에 돌아가는 있으면 좋은점
    - CPU 사용 효율을 높일 수 있다.
- Multitasking ( = Multiprocessing )
    - 하나의 CPU를 가지고, 시간을 쪼개쓰기. 이런걸 Concurrency (Palleleism)
- CPU scheduling:
    - 램에 여러개의 프로세스가 존재를 함. 한 프로세스가 CPU상에서 돌다가 빠져나감. 그럴 경우에 어떤 프로세스를 선택해야 하는가? 목표는 당연히 CPU 효율을 제일 높게 만듬.
    
- Two separate mode of operations :
    - **user mode** and **kernel mode**
    - 나눠놓는 이유 : 뭔가 이상한 프로그램(악의적, 실수적)으로 인해서 시스템이 크래시 되지 않도록, 막아주는 역할도 운영체제가 해야함.
    - 유저 프로세스는 실행을 하다가 시스템 콜을 함. 그러면 kernel 모드로 들어가서 시스템 콜을 처리함. 그 다음에 유저 모드로 되돌아간 후에, 시스템 콜을 리턴한 걸 받음.
    - 커널 모드에서만 가능한 게 있음..
    
- Virtualization
    - 어떤 싱글 컴퓨터가 있을때, 여러개의 프로세스를 돌릴수 있음.
    - CPU가 있는데, 여러 개의 프로세스가 있으면, 운영체제를 여러 개 돌릴 수 있지 않을까?
    - VMM을 끼우면 됨. Virtual Machine Manager.
    - 하드웨어 위에 OS가 스케줄링 하듯이 OS를 스케줄링 해주는 VMM이 있으면 되지 않을까?
    
- OS in the Variety of Computing Environments
    - Tranditional Computing
    - Mobile Computing
    - Client - Server Computing
    - Peer-to-Peer Computing
    - Cloud Computing
    - Real-Time Embedded Systems.(RTOS)
    
- 운영체제의 역할
    - 유저 인터페이스
    - 프로그램 처리
    - I/O 명령 처리
    - 파일 시스템 관리
    - 커뮤니케이션
    - 에러 감지
    - 리소스 할당
    - 로깅.
    - 보호 및 보안
    
- Multiprocessing을 하려면 싱크로나이징(동기화)를 해야함. 그거 잘 못하면 데드락이 발생함.
    - 그걸 해결하기 위헤 Memory 관리, Virtual 메모리 관리.
    
- OS에 인터페이스 하기위한 방법
    - CLI를 통하는 방법
        - Shells : sh, bash, tcsh등등
    - GUI
        - MacOS, Windows, Linux
    - Touch Screen Interface
        - Android, iPhone
        
    - System calls
        - OS에서는 System call을 씀.
        - API라고들 부름.
        
        ```cpp
        #include <unistd.h>
        
        ssize_t read(int fd, void *buf, size_t count)
        ```
        
        항상 system call을 하긴 힘드니까. 그러니까 대부분은 라이브러리들을 제공함.