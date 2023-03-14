### C89 표준

-   1989년 에 ANSI 에 의해 채택된 첫 표준
-   대부분 컴파일러가 지원하는 표준
-   많은 임베디드 시스템은 C89 만 지원
-   소형 하드웨어는 window 같은 범용 운영체제 지원하지 않고 전용 운영체제만 지원함 소형 하드웨어 운영체제 용으로 [컴파일](https://www.uknew.co/%EC%BB%B4%ED%8C%8C%EC%9D%BC)이 가능하면서 하드웨어에 직접 접근하기 위해 C 89 사용


cf > 컴파일
- 개발자가 작성한 코드를 binary file 로 변환하는 과정

### #include

- `#include` 이 위치한 곳에 다른 파일을 복사해서 그대로 삽입하도록 전처리기에 명령하는 지시문
- c header file 을 포함시키는데 주로 사용 

cf > [전처리기](https://www.tutorialspoint.com/cprogramming/c_preprocessors.htm)

- 컴파일러의 일부가 아니라 컴파일 과정의 별도 단계
- C 전처리기는 단순히 텍스트 대체 도구이며 실제 컴파일 전에 필요한 전처리를 지시하는 것을 의미


```c
#include <stdio.h>
```

- 전처리기에게 `#include <stdio.h>` 가 표시된 부분에 stdio.h 파일을 넣으라고(include) 명령
- 컴파일 전에 전처리기가 stdio.h 파일을 찾아서 해당 파일의 내용을 `#include <stdio.h>` 위치에 복사

cf > `<stdio.h>`

- Standard Input Output, 즉 표준 입력 또는 출력을 위한 헤더 파일
- printf, scanf 등 콘솔 입/출력 할 때, 외부 파일을 읽기 위해 사용함

example 
    
    ```c
    #include <stdio.h>

    int main(void)
    {
        printf("Hello World!\n");
        return 0;
    }
    ``` 

[참고](https://www.techonthenet.com/c_language/directives/include.php)

cf > stream

정의

- 데이터의 흐름
- 데이터를 조각조각 나눠서 관리하는 것 

사용하는 이유

- 메모리에 한번에 모든 데이터를 저장할 수 없기 때문에, 데이터를 조각조각 나눠서 가져오기 위함
- 메모리는 커봤자 32GB 정도밖에 안됨, 하지만 데이터는 수 TB 정도 될 수 있음
- 네트워크 측면에서는 한번에 보낼 수 있는 데이터의 양이 한정되어 있어, 조각조각 나눠서 보내야 하기 때문에 stream 을 사용함

참고자료
- https://study.com/academy/lesson/streams-in-computer-programming-definition-examples.html
- https://docs.oracle.com/cd/E19455-01/805-7478/6j71mbrcf/index.html
- https://www.quora.com/What-are-streams-in-programming


### int main(void)

- C 프로그램의 진입점
- C 코드를 빌드해서 나온 실행파일을 실행하면, main 함수가 자동적으로 실행됨
- 프로그램에 문제가 없으면 0을 반환하고, 문제가 있으면 1을 반환
- void 를 지정해 주는 이유는 main 함수에서 argument 를 받지 않기 때문

ex > 

```c
#include <stdio.h>

int main(void)
{
    printf("Hello World!\n");
    return 0;
}
```


### printf()

- 화면에 출력하기 위한 함수
- printf = print formatted
- printf 함수는 화면에 출력할 내용을 지정하는 형식 문자열(format string)과 출력할 내용을 지정하는 인자(argument)를 함께 사용

ex > 

```c

#include <stdio.h>

int main(void)
{
    const char* name = "gyeongwon";
    const int age = 30;

    printf("Hello %s=%d",name,number);
    return 0;
}
```

cf > eof

- end of file
- 파일의 끝을 의미

필요한 이유

- 파일의 끝을 알아야, 파일을 읽는 것을 중단할 수 있기 때문

- newline character 로 끝나지 않은 라인은 실제 라인이 아님

- 컴퓨터에게 너 여기까지 실행해! 라고 알려주기 위해

- [참고](https://stackoverflow.com/questions/729692/why-should-text-files-end-with-a-newline
)

### C 는 절차적 언어

- 데이터보다 프로세스에 중점이 맞춰져 있음
- 함수는 모두 전역 함수


### unsigned 와 signed

- C 에서는 부호 없는 자료형을 나타내는 unsigned 와 부호 있는 자료형을 나타내는 signed 가 있음
- 부호 없는 자료형은 0 이상의 값만 표현할 수 있음

#### char

- 단일 문자를 표현하기 위한 자료형
- char 는 1byte(= 8bit) 의 메모리를 사용
```c
char c = 'a';
```

#### short

- short 는 2byte(= 16bit) 의 메모리를 사용

```c
short num = -30000;
unsigned short unsigned_num = 65535;
signed short signed_num = -32767;
```
#### int 

- int 는 4byte(= 32bit) 의 메모리를 사용
- 2^32 = 4294967296
- 2^31 = 2147483648
- 범위는 signed : -2147483648 ~ 2147483647
- 범위는 unsigned : 0 ~ 4294967295

```c
int num = -30000;
unsigned int unsigned_num = 4294967295;
signed int signed_num = -2147483648;
``` 

#### long

- long 은 8byte(= 64bit) 의 메모리를 사용

```c
long num = -30000;
unsigned long unsigned_num = 4294967295;
```

#### float

- float 은 4byte(= 32bit) 의 메모리를 사용
- 소수점을 표현하기 위한 자료형
- unsigned float 은 없음
- 관련 header file : float.h

```c
float num = 3.14f;
```

#### double

- double 은 8byte(= 64bit) 의 메모리를 사용
- 소수점을 표현하기 위한 자료형
- unsigned double 은 없음
- 관련 header file : float.h
```c
double num = 3.14;
```
#### long double

- long double 은 16byte(= 128bit) 의 메모리를 사용
- 소수점을 표현하기 위한 자료형
- unsigned long double 은 없음
- 관련 header file : float.h

```c
long double num = 3.14;
```

#### bool

- c 에서는 bool 이라는 자료형이 없음
- 0 은 false, 0 이 아닌 값은 true 로 간주

```c
int is_true = 1;
int is_false = 0;
```

#### enum

- enum 은 열거형을 의미

cf > enum 을 사용하는 이유

- enum 을 사용하면 코드의 가독성이 높아짐
- enum 을 사용하면 코드의 유지보수가 쉬워짐
- enum 을 사용하면 코드의 실수를 줄일 수 있음

Enums, short for enumerated types, are a data type in programming that consists of a set of named values. They provide several benefits, including:

1.  Improved readability: Enums allow you to use descriptive, self-explanatory names for values instead of using hardcoded integers or string literals. This improves the readability and maintainability of your code.
    
2.  Type safety: Enums provide type safety by limiting the possible values that a variable can take. This can help prevent bugs caused by using an incorrect value.
    
3.  Better documentation: Enums provide a clear and concise way to document the valid values for a variable, making it easier for other developers to understand the intended behavior of your code.
    
4.  Better performance: Using enums instead of strings or integers can result in better performance, as the compiler can optimize the storage of enums more efficiently.
    
5.  Improved organization: Enums allow you to group related values together, making it easier to manage and organize your code.
    

```c
enum weekdays {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
};

int main() {
    enum weekdays today = WEDNESDAY;
    printf("Today is %d\n", today); // 2
    return 0;
}

```

#### 변수 선언

- 변수 선언 및 초기화는 block 의 시작 부분에서 처리되어야 함

```c
int get_sum(int a, int b)
{
    return a + b;
}
 
int get_difference(int a, int b)
{
    return a - b;
}
 
int main(void)
{
    int a = 3;
    int b = 10;
    int sum = get_sum(a, b);
    printf("sum: %d\n", sum);
    
    int difference = get_difference(a, b); // 변수를 선언 후 초기화를 중간에 했기 때문에 complie error 가 발생한다.
    printf("difference: %d", difference);
 
    return 0;
}
```

- 변수 초기화를 시작부분에서 하지 않으려면, 변수만 선언하고 초기화는 나중에 해야 함 

```c

int get_sum(int a, int b)
{
    return a + b;
}

int get_difference(int a, int b)
{
    return a - b;
}

int main(void)
{
    int a;
    int b;
    int sum;
    int difference;
    
    a = 3;
    b = 10;
    sum = get_sum(a, b);
    printf("sum: %d\n", sum);
    
    difference = get_difference(a, b);
    printf("difference: %d", difference);
 
    return 0;
}
```

#### 연산자 우선순위

연산자 우선순위는 표현식에서 연산이 수행되는 순서를 나타냅니다. 여러 작업을 하나의 표현식으로 결합할 때 작업이 수행되는 순서를 결정합니다. 예를 들어 2 + 3 * 4 식에서 곱셈 연산자의 우선 순위가 높기 때문에 더하기 전에 곱셈을 수행합니다. 일반적으로 우선 순위가 높은 작업이 우선 순위가 낮은 작업보다 먼저 수행됩니다.

연산자에는 할당된 우선 순위 수준이 있으며 우선 순위가 높은 연산자가 우선 순위가 높습니다. 두 개 이상의 연산자가 동일한 표현식에 나타나면 우선 순위가 높은 연산자가 먼저 수행됩니다. 두 개 이상의 연산자가 동일한 우선 순위 수준을 갖는 경우 식에서 왼쪽에서 오른쪽으로 수행됩니다.

우선 순위는 프로그래밍 언어마다 다를 수 있지만 몇 가지 일반적인 연산자 우선 순위 수준은 다음과 같습니다.

Parentheses ()
Unary operators (e.g. !, ~, ++, --)
Multiplication and division
Addition and subtraction
Relational operators (e.g. <, >, <=, >=, ==, !=)
Logical operators (e.g. &&, ||)
Assignment operators (e.g. =, +=, -=, *=, /=, %=)

모호성을 피하고 원하는 작업 순서가 수행되도록 하기 위해 괄호를 사용하여 복잡한 표현식에서 작업 순서를 명시하는 것이 좋습니다.

### sizeof

The sizeof operator in C is used to determine the size of a data type, object or expression in bytes. The size of a data type or object can be useful in determining the amount of memory that needs to be allocated or used.

The sizeof operator can be used in two ways:

With a data type: sizeof(int) returns the size of an int data type in bytes.
With an expression or object: sizeof(a) returns the size of the object a in bytes.
The sizeof operator evaluates to a constant value at compile-time, so it can be used in constant expressions. For example, to allocate memory for an array of 10 integers, the following code can be used:


```c
int arr[10];
int *ptr = (int*)malloc(10 * sizeof(int));
```

```c
char ch = 'a';
int num  = 10;
char char_array[30];

size_t size_char = sizeof(ch); // 1
size_t size_int = sizeof(num); // 4
size_t size_float = sizeof(float); // 4
size_t size_array = sizeof(char_array); // 30
```

It's important to note that the result of the sizeof operator is implementation-defined and may vary depending on the platform or compiler being used.

### 복습퀴즈

<details>
<summary>C 프로그램의 진입점은 무엇인가요</summary>
main 함수
</details>

<details>
<summary>printf 함수는 어떤 역할을 하는 함수인가요?</summary>
화면에 출력하기 위한 함수
</details>

<details>
<summary>C에서 함수를 선언할 때 argument 에 void를 사용하는 것은 무엇을 의미하나요?</summary>

argument 를 받지 않는다는 의미
</details>

<details>
<summary>C에서 함수를 선언할 때 argument 에 void를 사용하지 않는 것은 무엇을 의미하나요?</summary>
argument 를 받는다는 의미
</details>

