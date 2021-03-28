---
layout: post
current: post
thumbnail: ''
cover:
navigation: True
title: '[OS] 컴퓨터 시스템의 동작 원리'
date: 2021-03-12 10:18:00
tags: [os]
class: post-template
subclass: 'post'
author: zangzangs
---




## 컴퓨터 시스템 구조
<br>
<center>
<img src="../..\assets\images\operating_system\chap03_computer_system_structure.png"  style="zoom:60%"/>

<span style="color:gray; size:10"><컴퓨터 시스템 구조></span>
</center>

---

### 1. CPU
중앙처리장치라 불리는 CPU는 인간의 **두뇌**와 같은 역할을 합니다. **중앙처리장치** 라는 말 그대로 중앙에서 사용자가 입력한 명령어를 해석하고 연산한 후 그 결과를 처리합니다.

<br>

### 2. 메모리
 랜덤 액세스 메모리 즉, 램(RAM)은 임의의 영역에 접근하여 읽고 쓰기가 가능한 주기억 장치다. RAM은 어느 위치에 저장된 데이터든지 접근(읽기 및 쓰기)하는 데 동일한 시간이 걸리는 메모리이기에 ‘랜덤(Random, 무작위)’이라는 명칭이 주어진다. 
 
<br>

### 3. Device Controller
- I/O device controller
  - 해당 I/O 장치 유형을 관리하는 일종의 작은 CPU
  - 제어 정보를 위해 control register, status register가 있다.
  - local buffer를 갖는다 (=일종의 data register)
- I/O는 실제 device와 local buffer 사이에서 일어남
- Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림

> device driver(장치구동기)  
> : os 코드 중 각 장치별 처리루틴 -> software  
> device controller(장치제어기)  
> : 각 장치를 통제하는 일종의 작은 cpu -> hardware

<br>

### 4. Interrupt line
CPU로 인터럽트 신호를 보낼 수 있는 하드웨어 라인

<br>

### 5. Mode bit
사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호장치가 필요
Mode bit을 통해 하드웨어적으로 두 가지 모드의 operation 지원

> 1 <span style="color:blue">사용자 모드</span> : 사용자 프로그램 수행  
> 0 <span style="color:red">모니터 모드</span> : OS 코드 수행 (=커널 모드, 시스템 모드)  

- 보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행 가능한 **'특권명령'** 으로 규정
- Interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 변경
- 사용자 프로그램에게 CPU를 넘기기 전에 mode bit을 1로 변경

<br>

### 6. Timer
- 타이머
  - 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트를 발생
  - 타이머는 매 클럭 틱 때마다 1씩 감소
  - 타이머 값이 -이 되면 타이머 인터럽트 발생
  - CPU를 특정 프로그램이 독점하는 것으로부터 보호
  
- 타이머는 time sharing을 구현하기 위해 널리 이용됨
- 타이머는 현재 시간을 계산하기 위해서도 사용

<br>

### 7. DMA Controller

<br>

### 8. PC(Program counter)
CPU의 레지스터중 하나.
pc는 다음번에 실행할 명령어의 주소를 가지고 있다.
하나의 기계어가 종료 이후의 다음 위치를 가리킨다.

<br><br><br>

---

## 인터럽트(Interrupt)
### 인터럽트
- 현대의 운영체제는 인터럽트에 의해 구동된다.
- 인터럽트 당한 시점의 레지스터와 Program counter를 save 한 후 CPU의 제어를 인터럽트 처리 루틴에 넘긴다.

<br>

### 넓은 의미의 interrupt
- 하드웨어 인터럽트
  - 하드웨어가 발생시킨 인터럽트
- 소프트웨어 인터럽트  
    - Exception : 프로그램이 오류를 범한 경우
    - Systeme Call : 프로그램이 커널 함수를 호출하는 경우

<br>

### 인터럽트 관련 용어
- 인터럽트 백터  
  해당 인터럽트 처리 루틴 주소를 가지고 있음
- 인터럽트 처리 루틴(=Interrupt Service Routine, 인터럽트 핸들러)  
  해당 인터럽트를 처리하는 커널 함수

### 동작
1. 인터럽트가 발생하면 CPU는 하던 일을 멈추고 커널 내에서 해당 인터럽트의 처리를 위해 정의된 코드를 찾게된다.  
2. 운영체제는 할 일을 쉽게 찾아가기 위해 인터럽트 벡터(interrupt vector)를 가지고 있다. 인터럽트 백터란 인터럽트 종류마다 번호를 정해서 번호에 따라 처리해야 할 코드가 위치한 부분을 가리키는 자료구조를 말한다.  
3. 인터럽트 백터를 따라가면 실제 처리해야 할 코드는 인터럽트 처리 루틴(Interrupt service routine) 또는 인터럽트 핸들러(Interrupt handler)라고 불리는 다른 곳에 정의된다.  
 4. 인터럽트 처리 루틴을 통해 해당되는 인터럽트 처리를 완료하고 나면 원래 수행하던 작업으로 돌아가 중단되었던 일을 계속해서 수행한다.  

<br>

 ### 시스템 콜(System Call)
사용자 프로그램이 운영체제의 서비스를 받기 위해 커널함수(운영체제)를 호출하는 것


<br>

> 질문: 운영체제한테 CPU가 넘어가는 경우  
> - 인터럽트 발생(Interrupt Line 사용하는 경우)  
>   - 하드웨어 장치들이 I/O가 인터럽트를 요청 할 때  
>   - timer 가 정해진 시간이 지난 후에 CPU에 제어권을 넘긴다.  
>   - 시스템 콜 발생시
> - Exception 발생시



 <br><br><br>


## 동기식 입출력과 비동기식 입출력

- 동기식 입출력(synchronous I/O)
  - I/O 요청 후 입출력 작업이 완료된 후에야 제어가 사용자 프로그램에 넘어감
  - 구현 방법1
    - I/O가 끝날 때까지 CPU를 낭비시킴
    - 매시점 하나의 I/O만 일어날 수 있음
  - 구현 방법2
    - I/O가 완료될 때까지 해당 프로그램에게서 CPU를 빼앗음
    - I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
    - 다른 프로그램에게 CPU를 줌

- 비동기식 입출력(asynchronous I/O)
  - I/O가 시작된 후 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감

> 두 경우 모두 I/O 완료는 인터럽트로 알려준다.


## DMA(Direct Memory Access)
- 빠른 입출력 장치를 메모리에 가까운 속도로 처리하기 위해 사용한다.
- CPU의 중재 없이 device controller가 device의 buffer storage의 내용을 메모리에 block 단위로 직접 전송
- 바이트 단위가 아니라 block 단위로 인터럽트를 발생시킴
