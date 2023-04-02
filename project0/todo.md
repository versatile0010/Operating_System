# Write a new system call 🔥

- hello()
  - Argument ( char *string) 
  - Input (in qemu, the name of test app is "test")
  - Output : "Hello Konkuk"


### Adding a new System Call 
- sysproc.c 에 새로운 system call 인 hello() 을 구현한다. ✔️
- syscall.h 에 hello() 를 등록한다. ✔️ SYS_hello 22
- user.h 에 hello() 를 등록한다. ✔️
- usys.S 에 hello() 를 등록한다. ✔️

### User Program (test)
- test.c 파일을 추가한다.
- Makefile 에 test.c 를 추가한다.
