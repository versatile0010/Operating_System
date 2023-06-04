# 💻 Operating System Project Repository

This is a Course Project of Operating System (2023-Spring), Konkuk Univ.

|Date|Topic(Link)|Description|
|------|---|---|
|2023. 04. |Adding a system call with a test application|Project 0: Xv6 Starter|
|2023. 05. |Implement Lottery Scheduler on Xv6|Project 1|
|2023. 06. 15|Implement Kernel Thread|Project 2|

---
## 📝Proj0: Add a new System call in xv6
1. syscall.h 에서 새로운 system call 호출 번호를 할당한다. 🟢
2. sysproc.c 에 새로 추가할 system call 의 실제 함수를 구현한다. 🟢
3. system call dispatcher 에 system call 을 추가하기 위해, syscall.c 에 새로운 system call 을 위한 주소를 추가한다 🟢
4. User Program 을 만들어서, 새롭게 생성한 system call 을 test 한다. 이를 위해 해당 system call 의 interface 를 제공하는 라이브러리 함수를 작성해야 한다. 이는 usys.S 파일과 user.h 을 수정한다. 🟢
```
// usys.S
...
SYSCALL(newSystemcall)
...
```

```
// user.h
...
int newSystemcall(innt);
...
```

5. Makefile 에 User program 을 추가한 뒤, 테스트를 진행한다. 🟢
---

## About XV6

### Overview
- XV6 는 여러 프로세스를 관리하며, 관련 코드는 proc.h 와 proc.c 에서 확인가능하다. 각각의 프로세스는 스케줄러에 의해 관리되는 다양한 상태(RUNNABLE, RUNNING, SLEEPING)를 가진다.
- proc.c 내의 scheduler() 함수를 통해 XV6 의 스케줄링을 수정할 수 있다.
- XV6 는 각 프로세스에 대하여 독립적인 가상 주소 공간을 제공하기 위해 페이지 테이블을 사용하며, 이는 vm.c 에서 확인할 수 있다.
- XV6 가 제공하는 파일 시스템은 fs.c , file.c , sysfile.c 에서 확인할 수 있다.

### Main
- main.c 에서 XV6 의 초기화(메모리 설정, 스케줄링 설정 등) 및 시작을 담당한다.

### Virtual Memory
- vm.c 에서 가상 메모리 관리 관련 코드를 포함한다. (Address translation)

### Process Management
- proc.c 에서 process 생성, 스케줄링, 상태 관리, life cycle 관련 코드가 포함되어있다.

### Traps, Interrupts
- trap.c 와 trapasm.S 에서 interrupt 와 trap 처리 관련 코드가 포함된다. system call, timer interrupt, device interrupt, exception 관련하여 처리한다.

### File System
- fs.c, file.c, sysfile.c 에서 xv6 의 파일 시스템을 구현하고 있다. 파일, 디렉토리 생성, 읽기, 쓰기, 삭제 등의 작업을 수행한다.

### System Calls
- syscall.c, sysproc.c 에서 시스템 호출의 구현을 담당한다. system call 을 통해 user program 이 kernel 기능에 접근할 수 있게 된다.

### Lock
- spinlock.c 와 sleeplock.c 에서 lock 을 구현하고 있다.

### Device Drivers
- ide.c, kbd.c, console.c 등에서 여러 하드웨어 장치 드라이버를 구현한다.

---

## 📝Proj1: Lottery Scheduler
- process 에 random 하게 cpu 자원을 분배하는 확률적인 스케줄러이다.
- 각각의 process 는 ticek 을 할당받고, 무작위로 ticket 을 선정하여 스케쥴할 process 를 선정한다.
- Process with Higher Ticket Gets Higher Chance to Win (If Sum of Tickets >= Random Number, Process Executes)

![image](https://github.com/versatile0010/Operating_System/assets/96612168/e86069e8-6823-4b9d-a28f-970af47d49e2)


## How to implement Lottery Scheduler on XV6 ?
1. 먼저 티켓 필드를 추가해야 한다. proc.h 의 proc 구조체에 tickets 필드를 추가한다. 🟢
2. 새로운 프로세스가 할당될 때, ticket 이 적절히 초기화 되도록 proc.c 파일 내의 fork() 함수를 수정해야한다. 🟢
3. Lottery Scheduler 을 구현하기 위해 proc.c 내의 scheduler() 함수를 수정해야한다. 🟢
4. 랜덤으로 티켓을 선택할 수 있도록, rand.c, rand.h 를 구현한 다음 scheduler() 함수 내에서 이를 사용하도록 수정한다. 🟢
5. 이제 랜덤으로 티켓이 선택되면, 스케줄할 프로세스가 정해진 해당 프로세스를 실행한다. (schduler() 내의 기존 코드를 사용하면 된다.) 🟢
6. Ticket 의 숫자를 조절할 수 있는 system call 을 구현한다. 🟢
7. Lottery scheduler 을 잘 설계했는 지 확인할 수 있는 Test code 을 실행시켜서 점검한다. 🟢

---

과제 요구사항
```
1. process의 티켓 값을 설정하는 시스템 콜 추가 🟢
2. process의 티켓 값을 구할 수 있는 시스템 콜 추가 🟢
3. 외부 파일을 추가하지 말고, rand.c, rand.h 를 직접 구현할 것. (완벽하게 Random 하지는 않아도 됨) 🟢
4. 티켓의 초기값은 10 으로 설정할 것 🟢
```
---

##  📝Proj2. Implement Kernel Threads
- Add two new System calls
```
1. int clone(void(*fcn)(void *, void *), void *arg1, void *arg2, void *stack)
: arg1, arg2 는 thread 의 인자이고, stack 은 user space memory 이다. 초기 크기는 4KB 로 정의하고,
반환 값은 thread_id 혹은 -1(실패 시)
2. int join(void **stack)
: thread stack 의 주소를 인자로 넘겨주어 stack 을 해제한다. 반환 값은 thread_id 혹은 -1(실패 시)
```

- Implement two wrapper function
```
1. int thread_create(void(*fcn)(void *, void *), void *arg1, void *arg2);
2. int thread_join()

```
- OS and Kernel Thread
```
OS 는 system resource 를 관리하고 user 와 system 간 interface 를 제공하는 software 이다.
kernel thread 를 통해 os 의 병렬처리를 빠르게 수행할 수 있다.
```


(🔨 will be updated soon- 05.26.)












