# Lab5 - race condition and critical section

This lab is the homework of [Chapter 26](http://www.cs.wisc.edu/~remzi/OSTEP/threads-intro.pdf)

Format your answers neatly and submit.

### Q1

```
./x86.py -p loop.s -t 1 -i 100 -R dx
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p loop.s -t 1 -i 100 -R dx
ARG seed 0
ARG numthreads 1
ARG program loop.s
ARG interrupt frequency 100
ARG interrupt randomness False
ARG argv 
ARG load address 1000
ARG memsize 128
ARG memtrace 
ARG regtrace dx
ARG cctrace False
ARG printstats False
ARG verbose False

   dx          Thread 0         
    ?   
    ?   1000 sub  $1,%dx
    ?   1001 test $0,%dx
    ?   1002 jgte .top
    ?   1003 halt

```

---
### Q2

```
./x86.py -p loop.s -t 2 -i 100 -a dx=3,dx=3 -R dx -c
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p loop.s -t 2 -i 100 -a dx=3,dx=3 -R dx -c
ARG seed 0
ARG numthreads 2
ARG program loop.s
ARG interrupt frequency 100
ARG interrupt randomness False
ARG argv dx=3,dx=3
ARG load address 1000
ARG memsize 128
ARG memtrace 
ARG regtrace dx
ARG cctrace False
ARG printstats False
ARG verbose False

   dx          Thread 0                Thread 1         
    3   
    2   1000 sub  $1,%dx
    2   1001 test $0,%dx
    2   1002 jgte .top
    1   1000 sub  $1,%dx
    1   1001 test $0,%dx
    1   1002 jgte .top
    0   1000 sub  $1,%dx
    0   1001 test $0,%dx
    0   1002 jgte .top
   -1   1000 sub  $1,%dx
   -1   1001 test $0,%dx
   -1   1002 jgte .top
   -1   1003 halt
    3   ----- Halt;Switch -----  ----- Halt;Switch -----  
    2                            1000 sub  $1,%dx
    2                            1001 test $0,%dx
    2                            1002 jgte .top
    1                            1000 sub  $1,%dx
    1                            1001 test $0,%dx
    1                            1002 jgte .top
    0                            1000 sub  $1,%dx
    0                            1001 test $0,%dx
    0                            1002 jgte .top
   -1                            1000 sub  $1,%dx
   -1                            1001 test $0,%dx
   -1                            1002 jgte .top
   -1                            1003 halt

```
### Q3

```
./x86.py -p loop.s -t 2 -i 3 -r -a dx=3,dx=3 -R dx
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p loop.s -t 2 -i 3 -r -a dx=3,dx=3 -R dx
ARG seed 0
ARG numthreads 2
ARG program loop.s
ARG interrupt frequency 3
ARG interrupt randomness True
ARG argv dx=3,dx=3
ARG load address 1000
ARG memsize 128
ARG memtrace 
ARG regtrace dx
ARG cctrace False
ARG printstats False
ARG verbose False

   dx          Thread 0                Thread 1         
    ?   
    ?   1000 sub  $1,%dx
    ?   1001 test $0,%dx
    ?   1002 jgte .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 sub  $1,%dx
    ?                            1001 test $0,%dx
    ?                            1002 jgte .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1000 sub  $1,%dx
    ?   1001 test $0,%dx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 sub  $1,%dx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 jgte .top
    ?   1000 sub  $1,%dx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 test $0,%dx
    ?                            1002 jgte .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 test $0,%dx
    ?   1002 jgte .top
    ?   1000 sub  $1,%dx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 sub  $1,%dx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 test $0,%dx
    ?   1002 jgte .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 test $0,%dx
    ?                            1002 jgte .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?                            1000 sub  $1,%dx
    ?                            1001 test $0,%dx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 jgte .top
    ?                            1003 halt
```
### Q4

```
./x86.py -p looping-race-nolock.s -t 1 -M 2000 -c
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -t 1 -M 2000 -c
ARG seed 0
ARG numthreads 1
ARG program looping-race-nolock.s
ARG interrupt frequency 50
ARG interrupt randomness False
ARG argv 
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0         
    0   
    0   1000 mov 2000, %ax
    0   1001 add $1, %ax
    1   1002 mov %ax, 2000
    1   1003 sub  $1, %bx
    1   1004 test $0, %bx
    1   1005 jgt .top
    1   1006 halt
```
### Q5

```
./x86.py -p looping-race-nolock.s -t 2 -a bx=3 -M 2000
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -t 2 -a bx=3 -M 2000
ARG seed 0
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 50
ARG interrupt randomness False
ARG argv bx=3
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   1004 test $0, %bx
    ?   1005 jgt .top
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   1004 test $0, %bx
    ?   1005 jgt .top
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   1004 test $0, %bx
    ?   1005 jgt .top
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?                            1006 halt
```
### Q6

```
./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 0
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 0
ARG seed 0
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 4
ARG interrupt randomness True
ARG argv 
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?                                1006 halt
    // -s 1
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 1
ARG seed 1
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 4
ARG interrupt randomness True
ARG argv 
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1006 halt
    //-s 2
daymiradamira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 2
ARG seed 2
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 4
ARG interrupt randomness True
ARG argv 
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?                            1005 jgt .top
    ?                            1006 halt
```
### Q7

```
./x86.py -p looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 1
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 1
ARG seed 0
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 1
ARG interrupt randomness False
ARG argv bx=1
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1006 halt
    //-i 2
    damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 2
ARG seed 0
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 2
ARG interrupt randomness False
ARG argv bx=1
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 mov %ax, 2000
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 mov %ax, 2000
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?                            1006 halt
// -i 3

damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 3
ARG seed 0
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 3
ARG interrupt randomness False
ARG argv bx=1
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   1001 add $1, %ax
    ?   1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?                            1001 add $1, %ax
    ?                            1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 sub  $1, %bx
    ?   1004 test $0, %bx
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1003 sub  $1, %bx
    ?                            1004 test $0, %bx
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1006 halt
    ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?                            1006 halt
```
### Q8

```
./x86.py -p looping-race-nolock.s -a bx=100 -t 2 -M 2000 -i 1
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p looping-race-nolock.s -a bx=100 -t 2 -M 2000 -i 1
ARG seed 0
ARG numthreads 2
ARG program looping-race-nolock.s
ARG interrupt frequency 1
ARG interrupt randomness False
ARG argv bx=100
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace 
ARG cctrace False
ARG printstats False
ARG verbose False

 2000          Thread 0                Thread 1         
    ?   
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1003 sub  $1, %bx
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1001 add $1, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1002 mov %ax, 2000
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1003 sub  $1, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1004 test $0, %bx
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1005 jgt .top
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?                            1000 mov 2000, %ax
    ?   ------ Interrupt ------  ------ Interrupt ------  
    ?   1001 add $1, %ax
...
```
### Q9

```
./x86.py -p wait-for-me.s -a ax=1,ax=0 -R ax -M 2000
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p wait-for-me.s -a ax=1,ax=0 -R ax -M 2000
ARG seed 0
ARG numthreads 2
ARG program wait-for-me.s
ARG interrupt frequency 50
ARG interrupt randomness False
ARG argv ax=1,ax=0
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace ax
ARG cctrace False
ARG printstats False
ARG verbose False

 2000      ax          Thread 0                Thread 1         
    ?       ?   
    ?       ?   1000 test $1, %ax
    ?       ?   1001 je .signaller
    ?       ?   1006 mov  $1, 2000
    ?       ?   1007 halt
    ?       ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?       ?                            1000 test $1, %ax
    ?       ?                            1001 je .signaller
    ?       ?                            1002 mov  2000, %cx
    ?       ?                            1003 test $1, %cx
    ?       ?                            1004 jne .waiter
    ?       ?                            1005 halt
```
### Q10

```
./x86.py -p wait-for-me.s -a ax=0,ax=1 -R ax -M 2000
damira@damira-VirtualBox:~/Загрузки$ ./x86.py -p wait-for-me.s -a ax=0,ax=1 -R ax -M 2000
ARG seed 0
ARG numthreads 2
ARG program wait-for-me.s
ARG interrupt frequency 50
ARG interrupt randomness False
ARG argv ax=0,ax=1
ARG load address 1000
ARG memsize 128
ARG memtrace 2000
ARG regtrace ax
ARG cctrace False
ARG printstats False
ARG verbose False

 2000      ax          Thread 0                Thread 1         
    ?       ?   
    ?       ?   1000 test $1, %ax
    ?       ?   1001 je .signaller
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   ------ Interrupt ------  ------ Interrupt ------  
    ?       ?                            1000 test $1, %ax
    ?       ?                            1001 je .signaller
    ?       ?                            1006 mov  $1, 2000
    ?       ?                            1007 halt
    ?       ?   ----- Halt;Switch -----  ----- Halt;Switch -----  
    ?       ?   1002 mov  2000, %cx
    ?       ?   1003 test $1, %cx
    ?       ?   1004 jne .waiter
    ?       ?   1005 halt
```
---

1. __How do the threads behave?__

2. __What is thread 0 doing?__

3. __How would changing the interrupt interval (e.g., -i 1000, or perhaps to use random intervals) change the trace outcome?__

4. __Is the program efficiently using the CPU?__
