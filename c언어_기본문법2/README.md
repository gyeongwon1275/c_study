### 함수

- c 의 함수는 접근제어자가 없다.
- c 의 함수는 기본적으로 모두 전역함수이다. (cf > static 키워드를 사용하면 지역함수가 된다.) 
- c 의 함수는 어디서든 호출이 가능하다.

```c
int max(int num1, int num2)
{
   /* local variable declaration */
   int result;
 
   if (num1 > num2)
      result = num1;
   else
      result = num2;
 
   return result; 
}

```

### 함수 overloading

- c 에서는 함수 overloading 이 불가능하다.
- c 에서는 함수의 이름이 같으면 같은 함수로 인식한다.

```c
void print(int a)
{
    printf("int : %d\n", a);
}

void print(double a) 
{
    printf("double : %f\n", a); // compile ERROR : conflicting types for 'print'
}
```

### 함수 선언 위치

- c 는 언제나 위 -> 아래로 코드를 읽는다.
- c 에서 함수 정의가 되기 전에 함수를 호출하면 컴파일러는 해당 함수가 매개 변수는 무엇이고, 반환값은 무엇인지 알 수 없다.
- 때문에 컴파일러는 해당 함수의 반환형은 int, 매개변수는 아무거나 올 수 있다고 가정한다. 
- 따라서 컴파일러가 해당 함수를 이후에 찾았을 때 함수의 반환형이 int 가 아니라면 컴파일 에러가 발생한다.

```c

int main()
{
foo(); // compile ERROR : conflicting types for foo
return 0;
}

void foo()
{
printf("foo\n");
}

```

```c

int main()
{
foo(); 
return 0;
}

void foo()
{
printf("foo\n");
return 0;
}

```

- 호출 전에 함수를 정의하면 컴파일 에러가 발생하지 않는다.

```c

void foo()
{
printf("foo\n");
}

int main()
{
foo();
return 0;
}

```

- 위 방식에는 문제가 있을 수 있다. 
- foo 같은 함수가 많아지면 main 함수 위에 모든 함수를 정의해야 한다.
- 이는 코드의 가독성을 떨어뜨린다.
- 함수 끼리 호출하는 경우 순서를 맞추기도 어렵다.

### 함수 선언

- 함수의 구현체 없이 함수 원형만을 선언하는 것
- 함수의 원형은 함수의 이름, 매개변수, 반환형으로 이루어진다.
- 함수 정의는 함수의 원형과 구현체로 이루어진다.
- 함수 정의 보다 함수 선언이 먼저 나와야 한다.
- 일반적으로 함수 선언은 헤더 파일에 작성한다.

```c
// 함수 원형
// 함수의 원형은 함수의 이름, 매개변수, 반환형으로 이루어진다.
// 컴파일러가 함수의 원형을 통해, 함수의 이름, 매개변수, 반환형을 알 수 있다.

int add(int a, int b); 

// 컴파일 과정에서 컴파일러는 함수 호출하는 부분을 비워놨다가 link 과정에서 함수의 구현체를 찾아서 채워넣는다.

int main()
{
    int a = 1;
    int b = 2;
    int c = add(a, b);
    printf("%d\n", c);
    return 0;
}

int add(int a, int b) // 함수 정의
{
    return a + b;
}

```

### 함수 매개변수의 평가순서

- 아래의 add, subtract 함수중 어떤 함수가 먼저 호출될까?

```c
#include <stdio.h>

int add(int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a + b;
}

int subtract(int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a - b;
}

int main(void)

{


int num1 = 1;
int num2 = 2;

printf("%d,%d\n", add(num1, num2), subtract(num1, num2));
      return 0;
}

```

- 컴파일러에 따라 다르다. 
- add 가 먼저 호출될지, subtract 가 먼저 호출될지는 컴파일러에 따라 다르다.
- 평가순서를 강제하려면 한줄에 하나씩 함수를 호출해야 한다.

```c
#include <stdio.h>

int add(int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a + b;
}

int subtract(int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a - b;
}

int main(void)

{
int num1 = 1;
int num2 = 2;
int add_result = add(num1, num2);
int subtract_result = subtract(num1, num2);

printf("%d,%d\n", add_result, subtract_result);
      return 0;
}

```

### 평가순서

- `;` `||` `&&` => sequence point
- `;` `||` `&&` 를 만나면 평가순서를 강제할 수 있다. 
- 연산자 우선순위는 평가순서를 강제할 수 없다.
   
```c

#include <stdio.h>

int add (int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a + b;
}

int subtract(int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a - b;
}

int multiply(int a, int b)
{
    printf("a = %d, b = %d\n", a, b);
    return a * b;
}

int main(void) {
   // add -> subtract -> multiply 순서로 호출되지 않을 수 있다. 
   int result = add(1, 2) + subtract(3, 4) + multiply(5, 6);

   // 평가순서를 강제하려면
   int add_result = add(1, 2);
   int subtract_result = subtract(3, 4);
   int multiply_result = multiply(5, 6);

   int result_2 = add_result + subtract_result + multiply_result;

   return 0;

}

```c

#include <stdio.h>

int main(void)
 {
   int i = 0;
   int j = 0;
   int k = 0;

   if(++i || ++j && ++k)
   {
       printf("true\n");
   }

   printf("i = %d, j = %d, k = %d\n", i, j, k); // i = 1, j = 0, k = 0
 }

### 범위

- 블록 범위
- 파일 범위
- 함수 범위
- 함수 선언 범위

블록 범위

- 블록 범위는 블록 내부에서만 변수를 사용할 수 있다.
- 블록 안에 블록이 있을 경우 안쪽 블록에서는 바깥쪽 블록의 변수를 사용할 수 있다.

- C 언어에서 변수는 함수 호출 전에 선언되어야 한다.

```c
#include <stdio.h>

int main(void) {
      int num = 10;
      printf("num = %d\n", num);
      
      int num2 = 20; // compile error
      int result = num + num2; // compile error

      printf("result = %d\n", result);
      return 0;
}
```

compile possible

```c
#include <stdio.h>

int main(void) {
      int num = 10;
      printf("num = %d\n", num);
// 새로운 block 에서 변수를 선언했기 때문에 compile 가능하다.
      {
            int num2 = 20;
            int result = num + num2;
            printf("result = %d\n", result);
      }

      return 0;
}
```

file scope variables

- block 에 속하지 않고, 파일 전체에서 사용할 수 있는 변수를 파일 범위 변수라고 한다.
- file scope variables (also known as "global variables") 
- 함수 밖에서 선언된 변수
- 선언된 파일 전체에서 사용할 수 있다.

```c
#include <stdio.h>

int global_variable = 10;

void printValue() {
    printf("global_variable: %d\n", global_variable);
}

int main() {
    printValue();
    return 0;
}
```

global variable 의 메모리 내 위치

- global variable 은 data segment 에 저장된다.
- data segment 는 프로그램이 메모리에 로드될 때 생성되고, 메모리의 일부를 할당받는다.
- global variable 과 static variable 은 data segment 에 저장된다.
- global variable 의 메모리 위치는 프로그램이 로드될 때 결정된다.
- global variable 의 메모리 위치는 선언된 순서와는 무관하다.
- global variable 의 메모리 위치는 컴파일러나 운영체제에 따라 달라질 수 있다.
- 참고로 c 파일을 실행했을 때 code -> data segment -> heap -> stack 순으로 메모리가 할당된다.

cf > static keyword

- c 에서 static 은 storage class specifier 이다.
- static 을 변수에 적용하면, static storage duration 을 가지고, 함수 호출 사이에 값이 유지된다.
- static 을 함수에 적용하면, 함수의 scope 를 파일 내에서만 사용할 수 있게 한다.
- 즉, static 은 변수와 함수의 scope 를 제한하고, 지속적인 저장 기간을 가진다.
- static 은 파일 범위 변수를 만들기 위해 사용된다.

function scope

- function scope 변수는 함수안에서 선언된 변수를 말한다.
- 지역변수라고도 한다.
- 함수가 호출될 때마다 생성되고, 함수가 종료되면 소멸된다.



For example:

```c
#include <stdio.h>

void printValue(int local_variable) {
    printf("local_variable: %d\n", local_variable);
}

int main() {
    int local_variable = 10;
    printValue(local_variable); // local_variable: 10
    return 0;
}
```

위 코드에서 local_variable 은 main 함수 안에서 선언되었고, printValue 함수의 인자로 전달되었다. printValue 함수 안에서는 같은 이름의 지역변수가 선언되었지만, main 함수에서 선언된 변수와는 별개의 변수이다. printValue 함수 안의 지역변수는 printValue 함수 내에서만 접근할 수 있고, 함수가 종료되면 소멸된다.


## 배열

```c
#include <stdio.h>

int main(void) {
    int array[5] = {1, 2, 3, 4, 5};
    printf("array[0] = %d\n", array[0]);
    printf("array[1] = %d\n", array[1]);
    printf("array[2] = %d\n", array[2]);
    printf("array[3] = %d\n", array[3]);
    printf("array[4] = %d\n", array[4]);
    return 0;
}
```
- C 에서는 배열을 값형으로 만들 수 있다.
- 배열은 같은 타입의 변수들의 집합이다.
- 배열은 stack 메모리에 연속된 공간을 할당받는다.
- 배열의 각 요소는 배열의 시작 주소로부터 요소의 크기만큼 떨어진 주소에 저장된다.

참고로 C code 를 compile 하면, exe 파일이 나온다. 
exe 파일에는 code segment(assembly code), data segment, heap, stack 이 있다.
이런식으로 메모리에 올려놔야 CPU 에서 실행할 수 있다.


### stack memory

- 함수에서 사용하는 지역변수 등을 임시로 저장하는 메모리 영역이다.
- stack memory 의 크기는 프로그램 build 시 결정된다.
- stack memory 의 위치는 프로그램이 실행될 때 결정된다.
- 함수가 호출될 때마다 함수가 필요로 하는 공간을 stack memory 에서 할당받는다.
- 함수가 반환되면 할당받은 공간을 반환한다. => 때문에 빈공간이 없다. 
- 함수 밖에서 선언된 변수를 argument 로 전달하면, 함수가 호출될 때 stack 에 복사본을 만들어서 함수안에서 사용한다. 
- new 로 만든 data 는 heap memory 에 저장된다.
    - stack memory 와 다르게, heap memory 는 프로그램이 필요한 크기만큼 동적으로 할당받을 수 있다.
    - 이는 heap memory 에 빈공간을 만들게 된다.
    - 운영체제는 빈공간을 찾아서 할당해야 하기 때문에, heap memory 에서 할당받는 속도는 stack memory 보다 느리다.


- stack 의 크기는 한정되어 있음
- 대략 1MB 정도
- 1MB 를 넘어가면 stack overflow 발생
- 1Mb 를 초과하는 큰 data 를 stack 에 저장하려고 하면, stack overflow 가 발생한다.
- 특히 재귀함수를 사용할 때, stack overflow 가 발생할 수 있다.
    - 단순하게 생각하면 함수 호출할 때마다 stack frame 이 쌓이기 때문에, 만약 재귀함수가 무한히 호출된다면, stack overflow 가 발생할 수 있다.

#### 함수의 stack memory 사용량은 고정

- 함수는 누가 자신을 호출할 지 모름
- 함수를 정의할 때, stack memory 크기를 결정함

```c

#include <stdio.h>

int do_some_work(int a, int array[])

int main(void) {
    int a = 10;
    int array[5] = {1, 2, 3, 4, 5};
    do_some_work(a, array);
    return 0;
}
```

- 위에서 do_some_work 는 stack memory 에 array item 크기만큼의 공간을 할당받아야 할까?? 
- 아니다! do_some_work 함수는 array 의 주소만 argument 로 받아서 사용한다. 
- int, float, double, char, pointer 등의 기본 타입은 함수 호출시 복사본을 만들어서 전달한다.
- array 는 주소를 전달하기 때문에, 함수 호출시 복사본을 만들지 않는다.
- 주소만 받아서 원본을 사용하기 때문에, stack memory 에 array 크기만큼 공간을 할당받지 않는다.

#### 함수에서 배열을 매개변수로 받아 사용할 때

- 원본 배열이 변경됨

```c
do_some_work(int array[]) {
    array[0] = 10; // 원본 배열이 변경됨
}
```

#### 배열의 초기화

- 별도 value 로 초기화 하지 않으면, 쓰레기값이 들어있음

```c
int array[5]; // 쓰레기값이 들어있음
```

- best practice 는 0 으로 초기화

```c
int array[5] = {0,}; // 모든 요소를 0으로 초기화
```

#### C 에서 배열 사용의 위험성

- 초기회되지 않은 배열을 사용하면, 쓰레기값이 들어있기 때문에, 예상치 못한 동작이 발생할 수 있다.
- 배열의 크기를 벗어나는 index 를 사용하면, 예상치 못한 동작이 발생할 수 있다. ( buffer overflow )
- 다른 배열 또는 변수가 사용하고 있던 메모리에 의도치 않게 덮어 씌울 수 있다. 

Memory stomp, also known as buffer overflows, is a common programming error in C where a program writes data to a memory buffer that is larger than the space allocated for that buffer. This can cause the program to overwrite adjacent memory locations, potentially causing the program to crash, produce incorrect results, or even open up security vulnerabilities.


### C build 과정

- build 란? 
    - 사람이 읽기 쉬운 코드를 컴퓨터에서 실행할 수 있는 파일로 만드는 과정

- C build 단계
1. 전처리 (preprocessing)
C에서 전처리는 코드가 프로그램으로 컴파일되기 전에 이루어지는 단계입니다. 전처리 과정에서 전처리 프로그램은 헤더 파일을 포함하여 매크로를 해당 값으로 바꾸고 주석을 제거하는 등의 작업을 수행하여 코드를 수정합니다. 그 결과 컴파일러가 작업할 수 있는 버전에 더 가까운 원본 코드의 수정된 버전이 생성됩니다. 이렇게 수정된 코드는 추가 처리를 위해 컴파일러로 전달됩니다.


2. 컴파일 (compile)

컴파일은 사람이 읽을 수 있는 소스 코드를 기계가 실행할 수 있는 코드로 변환하는 과정입니다. C와 같은 고급 프로그래밍 언어로 프로그램을 작성할 때는 컴퓨터의 CPU에서 실행할 수 있는 형태로 코드를 변환해야 합니다. 이 과정에는 여러 단계가 포함되며, 그 중 하나가 컴파일입니다.

컴파일 프로세스는 C와 같은 프로그래밍 언어로 작성된 소스 코드로 시작되며, 소스 코드는 컴파일러를 사용하여 컴퓨터가 이해하고 실행할 수 있는 일련의 이진 명령어인 머신 코드로 변환됩니다.

컴파일 과정에서 컴파일러는 구문 검사, type checking 및 최적화와 같은 여러 작업을 수행합니다. 구문 검사는 코드가 프로그래밍 언어의 규칙을 따르는지 확인하고,  type checking는 코드에 사용된 데이터 유형이 일관성을 유지하는지 확인합니다. 최적화는 실행 코드의 크기를 줄이고, 속도를 개선하고, 필요한 메모리 양을 줄임으로써 결과 실행 코드의 성능을 개선하는 작업을 포함합니다.

컴파일 프로세스가 완료되면 결과 머신 코드는 일반적으로 컴퓨터에서 실행할 수 있는 실행 파일에 저장됩니다. 이 실행 파일에는 컴퓨터가 프로그램을 실행하는 데 필요한 instructions이 포함되어 있으므로 필요한 하드웨어와 운영 체제가 있는 모든 컴퓨터에서 프로그램을 실행할 수 있습니다.

3. 어셈블 (assemble)

어셈블리는 어셈블리 언어 코드를 기계어 코드로 변환하는 프로세스입니다. 어셈블리 언어는 컴퓨터는 더 쉽게 이해할 수 있지만 사람이 이해하기 어려운 코드를 작성하는 데 사용되는 저수준 프로그래밍 언어입니다. 어셈블리 언어 코드는 컴퓨터의 CPU가 실행할 수 있는 명령을 나타내는 약어법(mnemonics)을 사용하여 작성됩니다.

어셈블리 과정에는 어휘 분석, 구문 분석, 코드 생성 등 여러 단계가 포함됩니다. 어휘 분석은 소스 코드를 토큰 또는 키워드, 식별자, 연산자 등의 개별 의미 단위로 분해하는 작업을 포함합니다. 구문 분석은 코드의 구조를 분석하여 코드가 어셈블리 언어 구문 규칙을 따르는지 확인하는 작업입니다. 코드 생성은 어셈블리 코드를 CPU에서 실행할 수 있는 기계어 코드로 변환하는 작업을 포함합니다.

어셈블리 프로세스 중에 어셈블러 프로그램은 번역된 기계어 코드와 코드의 메모리 내 위치 및 코드에 사용된 기호에 대한 정보를 포함하는 객체 파일을 생성합니다. 그런 다음 이 오브젝트 파일을 다른 오브젝트 파일과 링크하여 실행 프로그램을 생성할 수 있습니다.

어셈블리는 일반적으로 장치 드라이버 또는 운영 체제 코드 작성과 같은 저수준 프로그래밍 작업에 사용됩니다. 어셈블리 언어는 컴퓨터 하드웨어에 대한 높은 수준의 제어 기능을 제공하므로 고도로 최적화되고 효율적인 코드를 작성할 수 있습니다. 하지만 C나 Python과 같은 고급 프로그래밍 언어에 비해 작성하고 이해하기 어렵습니다.

4. 링크 (link)

C에서 링크는 컴파일러 또는 어셈블러에서 생성된 객체 파일을 하나의 실행 파일 또는 라이브러리로 결합하는 프로세스입니다. C로 프로그램을 작성할 때 소스 코드의 일부가 아닌 외부 라이브러리나 시스템 파일에서 제공하는 함수와 라이브러리를 사용하는 경우가 많습니다. 이러한 함수와 라이브러리는 개별적으로 컴파일 및 어셈블되므로 완전한 실행 프로그램을 만들기 위해 서로 연결해야 하는 객체 파일이 생성됩니다.

링크 프로세스에는 오브젝트 파일과 라이브러리 간의 참조를 확인하여 필요한 모든 symbol를 사용할 수 있고 올바르게 정의되었는지 확인하는 작업이 포함됩니다. 또한 메모리에서 실제 위치를 반영하도록 심볼의 주소를 조정하는 재배치 작업도 포함됩니다. 또한 연결에는 오브젝트 파일과 라이브러리의 성능을 개선하기 위한 최적화 및 기타 수정 작업이 포함될 수 있습니다.

링크에는 정적(static) 링크와 동적(dynamic) 링크의 두 가지 유형이 있습니다. 정적 링크는 최종 실행 파일에 필요한 모든 객체 파일과 라이브러리를 포함합니다. 이렇게 하면 추가 외부 파일 없이도 호환되는 모든 시스템에서 배포 및 실행할 수 있는 단일 독립 실행형 실행 파일이 생성됩니다. 반면 동적 링크는 런타임에 외부 라이브러리 및 시스템 파일에 의존하는 더 작은 실행 파일을 생성하는 방식입니다. 이렇게 하면 실행 파일의 크기가 줄어들고 라이브러리를 업데이트하고 버그를 수정하기가 더 쉬워집니다.

링커는 일반적으로 빌드 프로세스의 마지막 단계로 C 컴파일러에 의해 자동으로 호출됩니다. 링크의 결과는 대상 시스템에서 실행할 수 있는 실행 파일과 실행 파일이 의존하는 모든 필수 라이브러리 파일입니다.

cf> symbol

C에서 링크의 맥락에서 심볼은 프로그램에서 변수, 함수 또는 기타 엔티티를 참조하는 데 사용되는 이름입니다. 심볼은 프로그램의 다른 부분 간 또는 다른 프로그램 간의 통신을 가능하게 하는 데 사용됩니다.

링커는 링크 프로세스 중에 한 파일에 정의된 심볼을 다른 파일에 사용된 심볼과 일치시켜 객체 파일과 라이브러리 간의 참조를 확인합니다. 이 프로세스에는 심볼 이름을 정의 또는 참조에 매핑하는 데이터 구조인 심볼 테이블을 생성하는 작업이 포함됩니다. 심볼 테이블에는 이름, 유형, 크기, 메모리 내 위치 등 각 심볼에 대한 정보가 포함되어 있습니다.

심볼은 외부 심볼 또는 내부 심볼의 두 가지 방식으로 정의할 수 있습니다. 외부 심볼은 하나의 객체 파일 또는 라이브러리에 정의되어 다른 객체 파일에서 사용됩니다. 이러한 심볼은 일반적으로 다른 소스 파일이나 라이브러리에 정의된 함수나 변수에 대한 액세스를 제공하는 데 사용됩니다. 반면 내부 심볼은 단일 오브젝트 파일 또는 라이브러리 내에 정의되며 외부에서 볼 수 없습니다.

심볼이 한 파일에서 참조되지만 해당 파일에 정의되어 있지 않은 경우 링커는 다른 객체 파일과 라이브러리를 검색하여 심볼의 정의를 찾아야 합니다. 심볼이 하나의 객체 파일 또는 라이브러리에만 정의되어 있는 경우 링커는 올바른 위치에 대한 참조를 확인할 수 있습니다. 그러나 심볼이 여러 파일에 정의되어 있는 경우 링커는 하나의 정의를 선택하여 사용하고 다른 정의는 폐기해야 합니다. 이 프로세스를 심볼 확인이라고 합니다.

심볼은 프로그램의 여러 부분이 서로 통신하고 링커가 서로 다른 파일 및 라이브러리 간의 참조를 확인할 수 있도록 하기 때문에 링크에서 중요한 역할을 합니다.

cf > dynamic link

동적 링크는 프로그램 실행 파일에 라이브러리를 정적으로 링크하는 대신 실행 프로그램을 런타임에 공유 라이브러리와 링크하는 C 프로그래밍의 링크 방식입니다. 이를 통해 여러 프로그램이 라이브러리의 단일 복사본을 공유할 수 있으므로 프로그램에 필요한 메모리와 디스크 공간을 줄일 수 있습니다.

동적 링크에서 실행 파일에는 런타임에 공유 라이브러리를 로드하고 라이브러리의 함수 또는 변수에 대한 참조를 확인하는 작은 스텁만 포함됩니다. 공유 라이브러리는 메모리에 로드되고 필요에 따라 프로그램에 링크되어 런타임에 필요한 코드와 데이터를 제공합니다.

동적 링크는 정적 링크에 비해 몇 가지 장점이 있습니다. 컴파일 시에는 사용할 수 없는 라이브러리를 프로그램에서 사용할 수 있으며 실행 파일의 크기가 줄어들어 배포 및 업데이트가 더 쉬워집니다. 또한 동적 링크는 라이브러리가 필요할 때만 로드되므로 프로그램 시작 시간과 메모리 사용량을 개선할 수 있습니다.

하지만 동적 링크에는 몇 가지 단점도 있습니다. 프로그램마다 동일한 라이브러리의 다른 버전이 필요한 경우 버전 관리 문제가 발생할 수 있습니다. 또한 공유 라이브러리가 업데이트되거나 제거되면 해당 라이브러리에 의존하는 모든 프로그램에 문제가 발생할 수 있습니다.

동적 링크는 공유 라이브러리가 여러 프로그램에서 널리 사용되고 공유되는 Linux 및 Windows와 같은 최신 운영 체제에서 일반적으로 사용됩니다.