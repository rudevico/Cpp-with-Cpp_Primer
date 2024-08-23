**Table of contents**  
* [Section 1.1 Writing a Simple C++ Program](#section-11-writing-a-simple-c-program)  
* [Section 1.2 A First Look at Input/Output](#section-12-a-first-look-at-inputoutput)
* [Section 1.3 A Word about Comments](#section-13-a-word-about-comments)
* [Section 1.4 Flow of Control](#section-14-flow-of-control)

# Section 1.1 Writing a Simple C++ Program
```zsh
# vi edtor로 cplusplus source file 생성
% vi prog.cc

# 다음과 같이 입력(in vi)
# int main()
# {
#     return 0;
# }
# :wq!

# c++11 표준을 사용하여 prog.cc 파일 compile
% g++ -std=c++11 -o prog prog.cc
% ls
prog	prog.cc

# compile 결과물인 executable file 실행
# 실행 결과가 OS에 리턴됨(이 경우 0)
% ./prog

# 마지막으로 OS에 리턴된 값 즉, prog의 실행 결과 출력
% echo $?
0


```zsh
# 위 예제와 동일하게 진행하지만, return만 -1로 변경
% vi prog1.cc

# 다음과 같이 입력(in vi)
# int main()
# {
#     return -1;
# }
# :wq!

% g++ -std=c++11 -o prog1 prog1.cc
% ls
prog	prog.cc  prog1  prog1.cc

# compile 결과물인 executable file 실행
# 실행 결과가 OS에 리턴됨(이 경우 -1)
% ./prog1

# 마지막으로 OS에 리턴된 값 즉, prog의 실행 결과 출력
# 전통적으로 exit status는 8bit-unsigned-int로 표현하는 것이 관례임.
# 따라서 -1이 255로 변경되어 출력
% echo $?
255
```


# Section 1.2 A First Look at Input/Output
```cpp
// Section 1.3
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0; v2 = 0;
    std::cin >> v1 >> v2;
    std::cout << "The sum of " << v1 << " and " << v2
              << " is " << (v1 + v2) << std::endl;

    return 0;
```

```cpp
// Exercise 1.4
/*
 * Our program used the addition operator, +, to add two numbers.
 * Write a program that uses the multiplication operator, *,
 * to print the product instead.
 */
#include <iostream>

int main()
{
    std::cout << "Enter two numbers:" << std::endl;
    int v1 = 0, v2 = 0;   // variables to hold the input we read
    std::cin >> v1 >> v2; // read input
    std::cout << "The product of " << v1
              << " and " << v2
              << " is " << (v1 * v2) << std::endl;

    return 0;
}
```


# Section 1.3 A Word about Comments
Comment Pairs(blocks) Do Not Nest
```cpp
// Section 1.3
/*
 * commentpairs/* */cannotnest.
 * ‘‘cannot nest’’ is considered source code,
 * as is the rest of the program
 */
int main() {
return 0; }
```

```
comment-pairs-do-not-nest.cc:2:18: warning: '/*' within block comment [-Wcomment]
 * comment pairs /* */ cannot nest.
                 ^
comment-pairs-do-not-nest.cc:2:24: error: unknown type name 'cannot'
 * comment pairs /* */ cannot nest.
                       ^
comment-pairs-do-not-nest.cc:2:35: error: expected ';' after top level declarator
 * comment pairs /* */ cannot nest.
                                  ^
                                  ;
1 warning and 2 errors generated.
```

다음과 같은 상황에서 `main()` 전체를 comment하고 싶다면, /* */로 묶어서 Nested되게 하지 말고 `main()`의 각 line에 single-line comment(`//`)를 붙여주면 된다.  

```cpp
int main()
{
    /*
     * This is a comment block
     * comment block
     */
    printf(“hi”);
```

```cpp
//int main()
//{
//    /*
//     * This is a comment block
//     * comment block
//     */
//    printf(“hi”);
```


# Section 1.4 Flow of Control
sequential execution만으로 풀어낼 수 있는 problem은 사실상 없다고 봐도 무방하다.  
-> 이를 해결하기 위해서 programming languages는 **flow-of-control statements**를 제공한다.  
* **flow-of-control statements**
  - **while** statement
  - **for** statement
  - **if** statement
