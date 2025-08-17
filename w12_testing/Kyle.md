
# 11.1 Mistake: 

## The Question
Find the mistake(s) in the following code:
```c
unsigned int i;
for (i = 100; i >= 0; --i)
printf("%d\n", i);
```

## Questions I Will Ask Before Going into Solution
- What version of C are we using? C89? C99? or C11+?
- What is the original intention and correct aim of the code?

## Approach
1. 변수의 data type을 확인하고 이러한 data type이 이후 코드에서 문제를 일으키지 않는지 검토한다.

    우선 c에서 그냥 int는 최소 16비트이고 short 의 크기 이상인 정수형이다. 표준에서의 short와 최소 용량이 같다.
    하지만 실무에서는 보통 32비트이다. 따라서 unsigned int라면 32비트 내에서 0부터 2^32 - 1까지일 것. 

    따라서 for문에서 iteration을 지속하는 조건이 i>=0이라면, i가 -1이 되게 되고, 이 순간 i는 underflow가 발생하여
    다시 unsigned int에서의 최대값 2^32 - 1이 될 것이다. 

    이렇게 되면 for문은 영원히 멈추지 않고 지속되는 무한 반복문이 될 것이다.

~~
2. for문의 문법이 올바른지 고민한다.
    언어에 따라 for문의 문법이 다르게 동작할 수 있는데 대표적으로 주의해야할 것이 Java의 dangling else일 것이다.
    이 경우 C를 기준으로 하므로 for문에 중괄호가 쓰여 block을 구성하지 않았다면 반복실행되어야 할 모든 코드는 한 줄 안에 담겨야 할 것이다.

    그러나 문자열을 출력하는 printf()함수가 for문과 다른 줄에 있음에도 중괄호를 통해 묶여있거나 하지 않으므로,
    이 코드는 for문이 무한 반복하지 않더라도 최종적으로 단 1회만 i를 출력하고 종료될 것이다.

결과적으로 위 코드는 그대로 실행할 경우 무한 반복으로 인한 메모리 오류를 발생시킬 것이다.
개발의도의 변화를 최소화한 올바른 코드는 아래와 같다.
```c
unsigned int i;
for (i = 100; i > 0; --i) {
    printf("%d\n", i);
}
```!!
~~

## Correct Approach

unsigned int로 인해 반복문이 무한히 실행되는 것은 맞다. 그러나 두 번째 오류는 for문의 문법에 있지 않고 사용된 format string에 있다. 부호없는 정수이므로 %d가 아니라 %u를 사용해야하며, %u가 아닌 %d를 부호없는 정수를 출력하는데 사용할 경우의 오류는 정의되어있지 않다. for문 뒤에 중괄호가 없을 경우 줄바꿈 여부와 무관하게 그저 단 하나의 실행문 만이 실행되는 정도의 차이가 있을 뿐, 위 코드의 문법이 잘못된 것은 아니다. 컴파일러는 인간의 가독성을 위한 줄바꿈이나 들여쓰기는 고려하지 않음을 잊지 말 것.

해법 1: 부호있는 정수 사용하기.
```c
for (int i = 100; i >= 0; --i)
    printf("%d\n", i);
```

해법 2(교재): 부호없는 정수 유지한 채 등호 제거하기.
```c
for (unsigned int i = 100; i > 0; --i)
    printf("%u\n", i);

```
