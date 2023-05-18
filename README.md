# 💻 Operating System Project Repository

This is a Course Project of Operating System (2023-Spring), Konkuk Univ.

|Date|Topic(Link)|Description|
|------|---|---|
|2023. 03. |Adding a system call with a test application|Xv6 Starter!|
|2023. 04. |Implement Lottery Scheduling on Xv6|Project 1|

(🔨 will be updated soon)

---
## Add a new System call in xv6
1. syscall.h 에서 새로운 system call 호출 번호를 할당한다.
2. sysproc.c 에 새로 추가할 system call 의 실제 함수를 구현한다.
3. system call dispatcher 에 system call 을 추가하기 위해, syscall.c 에 새로운 system call 을 위한 주소를 추가한다
4. User Program 을 만들어서, 새롭게 생성한 system call 을 test 한다. 이를 위해 해당 system call 의 interface 를 제공하는 라이브러리 함수를 작성해야 한다. 이는 usys.S 파일과 user.h 을 수정한다.
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

5. Makefile 에 User program 을 추가한 뒤, 테스트를 진행한다.
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
