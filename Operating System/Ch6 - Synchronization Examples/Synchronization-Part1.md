# Synchronization Tools
## 배경
### Cooperating processes
데이터를 공유하거나 logical address를 공유해 각자의 프로세스에 서로 영향을주고, 받는 상태
- 각 프로세스(스레드)가 Concurrent access할 때 data inconsistency가 발생할 수 있다.
- 그러므로 우리는 data consistency를 유지하기 위해서 프로세스를 순차적으로 실행할 필요가 있다

<br/>

### 프로세스(스레드)간 공유되어지는 Integrity of data
- Concurrent execution
    - Instruction stream중에 어디서 interrupt가 발생할지 모른다.
    - 다른 프로세스가 할당되어 공유 데이터를 사용할 때
- Parallel execution
    - 두 개 이상의 instruction streams를 가질 때
    - 각자의 다른 코어에서 각각의 프로세스를 parallel하게 수행할 때

### 두 프로세스가 concurrent하게 수행될 때 왜 Data Inconsistency가 생길까?
다음 코드는 두 개의 스레드가 concurrent하게 수행할 때 Data Inconsistency가 발생하는 예시이다.
```cpp
int count = 0;

void *run(void *param) {
    int i;
    for(i = 0; i < 10000; i++)
        count++;
    pthread_exit(0);
}

int main() {
    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, run, NULL);
    pthread_create(&tid2, NULL, run, NULL);
    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);
    printf("%d\n", sum);
}

// expected result : 20000
// actual result : 16329, 20000, 19090, 19920, etc..
```

다음 machine language를 보면 답을 알 수 있다.
- 스레드1

    mov reg1 %count

    add reg1

    mov %count reg1

- 스레드2

    mov reg2 %count

    add reg2

    mov %count reg2

위 두 개의 machine language를 통해서 count가 값이 증가하는 것을 볼 수 있다. 저 세 개의 instruction이 모두 수행이 된후 context switching이 발생하면 아무 문제없이 예상대로 20000이 나올 것 이다.

하지만 context swiching이 정직하게 세 개의 instruction을 마치고 발생한다는 보장은 없다. 예시로,

- 스레드1 reg = 0, reg = 1에서 인터럽트 발생!
- 스레드2 reg = 0, reg = 1, count = 1 인터럽트 발생!
- 스레드1 count = 1

위와 같은 이유로 예상된 값이 안나오는 것이다.


### Race Condition
위에서 설명한 바와 같이
- 여러개의 프로세스(스레드)가 존재하고
- 공유된 데이터가 존재해 이 공간에 모두 접근이 가능할 때
- **결과**는 특정 순서에 따라 공유 데이터에 access가 일어나느냐에 따라 결정된다.


이런 상태를 Race Condition(경쟁 상태)라고 한다.

이를 방지하기 위해서는 어떤 방법으로 **process synchronization(프로세스 동기화)**를 해야한다.


### Critical Section Problem
- 시스템이 n개의 프로세스로 구성되어 있다고 하자
    - 각 프로세스는 critical section이라고 불리는 부분의 코드를 가지고 있다.
    - 이 ciritical section은 적어도 하나의 다른 프로세스와 데이터를 공유로 접근할 수 있는 로직이 있다.

- 여기서 중요한 점은
    - 하나의 프로세스가 해당 critical section에서 작업하고 있을 때
    - 다른 프로세스의 접근을 못하게 막는다.

위와 같은 방법으로 프로세스를 synchronize하게 사용할 수 있다.

### Critical sections of Codes
1. entry-section: critical section에 진입하는 section.
    - critical section에 진입하기 위한 permission을 받고 들어간다.
2. critical-section: entry-section진입 이후의 seciton.
3. exit-section: critical section에서 나가는 section.
4. remainder-section: 나머지 section.

### 세 가지 solution 요구사항
1. Mutual Exclusion(상호 배제)
    - P1이 critical section에서 작업 중 이다.
    - 다른 어떤 프로세스가 critical section에서 작업할 수 없다.
2. Progress(avoid deadlock)
    - 아무도 critical section을 점유하지 않고 있을 때, 몇 개의 프로세스가 critical section에 접근하려고 한다면,
    - 모든 프로세스가 무기한으로 대기한다.
3. Bounded Waiting(avoid starvation)
    - 다른 프로세스가 점유할 수 있는 횟수에 대한 제한이 있어야한다.
    - 우선순위가 계속 밀리면 starvation이 발생할 가능성이 있다.

### single-core에서 해결 방법
- critical section 사용 중에는 interrupt 발생을 금지한다.
- 매우 간단하지만 multi-core로 갈 수 록 시스템 성능이 매우 저하된다.

### Preemptive and non-preemptive 접근 방법
- Non-preemptive kernel
    - exit될 때 까지 커널 모드를 계속 실행한다.
- Preemptive kernel
    - 커널 모드에서도 다른 프로세스에 선점 되도록 허가한다.
    - 설계가 어렵지만 reponsive하기 때문에 많이 사용한다.
    