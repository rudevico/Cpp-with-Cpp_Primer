# Table of contents
* [Section 2.1 | Primitive Built-in Types](#section-21--primitive-built-in-types)
* [Section 2.2 | Variables](#section-22--variables)
* [Section 2.3 | Compound Types](#section-23--compound-types)
* [Section 2.4 | `const` Qualifier](#section-24--const-qualifier)
* [Section 2.5 | Dealing with Types](#section-25--dealing-with-types)
* [Section 2.6 | Defining Our Own Data Structures](#section-26--defining-our-own-data-structures)


# Section 2.1 | Primitive Built-in Types
C++의 primitive built-in types는 크게 arithmetic types(연산이 가능한)과 special type(`void`)로 구분할 수 있다.  

* __Arithmetic Types__
  - integral types  
    `bool`, `char`, `short`, `int`, `long long`, ...
  - __floating-point types__  
    `float`, `double`, `long double`
* __`void`__  
  most commonly as the return type for functions that do not return a value.

## 2.1.1. Arithmetic Types
### Signed and Unsigned Types
`bool`과 extended character types를 제외한 모든 integral types는 `signed`와 `unsigned`로 구분될 수 있다. `signed`는 0을 포함한 음/양수를, `unsigned`는 0 이상의 값을 표현할 수 있다.  
`int`, `short`, `long` 등은 `signed int`, `signed short`, `signed long`과 동일하다. 이때 `signed`를 붙이지 않은 경우를 plain `int` plain `short` 등이라고 할 수 있다 즉, 대부분의 경우에서 plain은 `signed`를 의미한다.  
하지만 `char`의 경우는 다르다. compiler에 따라 plain을 `signed`로 취급하는 경우도 있고, plain을 `unsigned`로 취급하는 경우도 있기 때문에 주의가 필요하다.  

> 어떤 type을 사용해야 하는지를 정해야 되는 상황에서 참고할 수 있는 advice가 존재한다.  
1. value가 negative가 아닌 것이 확실할 때는 `unsigned type`을 사용한다.
2. arithmetic 즉 산술이 목적일 때는 `int`나 `long long`을 사용한다.  
   (`long` often has the same size as `int`.)
3. arithmetic expressions에는 plain `char` 또는 `bool`을 사용하지 마라. 이 두 타입은 characters나 truth values를 hold할 때만 사용할 것을 권장한다.  
   만약 value의 상한이나 하한이 작은 상황이 보장되면 즉, `int`의 4byte라는 저장 공간이 과도한 상황이라면 `short`를 사용한다.
   만약 `short`의 2byte도 과도한 경우라도 그 정도의 낭비는 감수하고 그냥 `short`를 사용하는 것을 권장하지만, 조금의 메모리 낭비도 줄여야 되는 상황이라면 1byte의 `char`을 사용해도 된다. 다만 `char`의 경우 compiler에 따라 plain이 signed와 unsigned 무엇으로 해석될 지에 차이가 존재하기 때문에 혼란을 방지하고자 앞에 signed/unsigned를 반드시 지정해준다.
4. floating-point computations에는 그냥 `double`만 쓸 것을 권장한다. `float`은 precision이 부족한 경우도 빈번하게 생기고, 두 type의 연산 속도 차이가 유의미하지 않다(심지어 `double`이 더 빠른 경우도 있다).  
   `long double`은 대부분의 경우에서 필요한 수준의 precision이 아니고, considerable run-time cost 문제가 발생한다.


## 2.1.2. Type Conversions
기존에 다른 언어를 배울 때는 type casting이 implicit과 explicit으로 나뉜다고 배웠었는데, 엄밀한 영어 표현으로 구분하자면 implicit한 경우를 type conversion, explicit한 경우를 type casting이라고 한다. 지금은 type conversion(implicit)에 대해서만 다루고, type casting(explicit)의 경우는 4.11에서 다룬다.  
> Type conversions happen automatically when we use an object of one type where an object of another type is expected.

```cpp
bool b = 42;          // b is true(bool의 경우 0이면 false, 아니면 true)
int i = b;            // i has value 1
i = 3.14;             // i has value 3
double pi = i;        // pi has value 3.0
unsigned char c = -1; // assuming 8-bit chars, c has value 255
signed char c2 = 256; // assuming 8-bit chars, the value of c2 is undefined
/* out of range value를 object에 assign할 때 signed/unsiged 여부에 따라 결과가 다르다.
 * unsigned object인 경우에는 under(over) flow를 발생시킨다.
 * signed object의 경우에는 undefined 처리한다.
 * 이때 undefined가 어떻게 작동할지(crash or runtime error or ...)는 시스템에 따라 다르다.
*/
```

### Expressions Involving Unsigned Types
unsigned와 signed를 연산하는 경우에는 연산 전에 signed가 먼저 implicit하게 unsigned로 convert된 뒤에 연산이 수행된다 by compiler.  
```cpp
unsigned int u = 10;
int i = -42;
std::cout << (i + i) << std::endl; // prints -84
std::cout << (u + i) << std::endl; // if 32-bit ints, prints 4294967264
/* 1. signed int를 unsigned int로 convert
 * 2. unsigned int -42는 wrpas around(underflow)된다.
 * -> (2^32 -1) - 41 = 4294967254
 * 3. 위 값(converted i)에 10을 더하면 4294967264.
*/
```

```cpp
unsigned int u1 = 42, u2 = 10;
std::cout << u1 - u2 << std::endl; // ok: result is 32
std::cout << u2 - u1 << std::endl; // ok: but the result will wrap around
/* Regardless of whether one or both operands are unsigned,
 * if we subtract a value from an unsigned, we must be sure
 * that the result cannot be negative
*/
```

The fact that an unsigned cannot be less than zero also affects how we write loops.  
```cpp
// 정상적으로 종료되는 loop
for (int i = 10; i >= 10; --i)
    std::cout << i << std::endl;

// WRONG: u can never be less than 0; the condition will always succeed
// 무한 loop에 빠지는 경우
for (unsigned u = 10; u >= 0; --u)
    std::cout << u << std::endl;
```

unsigned에 대해서 0까지만 출력하고 loop을 멈추게 하려면 `while` statement를 사용한다.  
```cpp
unsigned u = 11; // start the loop one past the first element we want to print
while (u > 0) {
    --u;         // decrement first, so that the last iteration will print 0
    std::cout << u << std::endl;
```

```cpp
// Exercise 2.3: What output will the following code produce?
#include <iostream>

int main() {
    unsigned u = 10, u2 = 42;
    std::cout << (u2 - u) << std::endl; // prints 32
    std::cout << (u - u2) << std::endl; // assuming 32-bit, prints 4294967264

    int i = 10, i2 = 42;
    std::cout << (i2 - i) << std::endl; // prints 32
    std::cout << (i - i2) << std::endl; // prints -32
    std::cout << (i - u) << std::endl;  // prints 0
    std::cout << (u - i) << std::endl;  // prints 0
    /* unsigned는 grater than or equal to 0이기 때문에
     * 두 경우 모두 wrap arround없이 0을 출력한다.
    */

    return 0;
}
```

## 2. 1. 3. Literals
A value, such as 42, is known as a literal because its value self-evident. Every literal has a type. The form and value of a literal determine its type.  

### Integer and Floating-Point Literals
integer literal의 경우 다음과 같이 구분할 수 있다.  
* begin with nothing  
  `20 // decimal 10진수`
* begin with 0(zero)  
  `024 // octal 8진수`
* begin with 0x or 0X  
  `0x14 // hexadecimal 16진수`
* floating-point literals have type `double`

**Literal is never a negative number.**  
`-42`는 `42`라는 literal을 `-`라는 operator를 통해서 negation한 것이다. 즉 `-42`는 literal이 아니다.

### Character and Chararcter String Literals
* `'a' // character literal, size is 1`
* `"A" // string literal, size is 2 because compiler appends a null character`

### Escape Sequences
### Specifying the Type of a Literal
### Boolean and Pointer Literals
* `bool test = false; // The words true and false are literals of type bool`
* `int* ptr = nullptr // The word nullptr is a pointer literal`


# Section 2.2 | Variables
# Section 2.3 | Compound Types
# Section 2.4 | `const` Qualifier
# Section 2.5 | Dealing with Types
# Section 2.6 | Defining Our Own Data Structures

