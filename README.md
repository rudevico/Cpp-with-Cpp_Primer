# Cpp-with-Cpp_Primer
C++ Primer 5th를 기준으로 공부하는 C++.


## mac에서 진행할 때
* Intel Mac 환경 기준
* macOS Sonoma 14.6.1

```zsh
% gcc -v
Apple clang version 15.0.0 (clang-1500.3.9.4)
Target: x86_64-apple-darwin23.6.0
Thread model: posix
InstallDir: /Library/Developer/CommandLineTools/usr/bin
```
* * *

mac에는 기본적으로 gcc compiler가 깔려있다. 내 경우에는 15.0.0 version이 설치되어 있으며 해당 version에서는 C++11, C++14, C++17, ...등 다양한 표준을 지원한다. 기본적으로 compile할 때 사용하는 표준은 다음과 같은 명령으로 확인할 수 있다.  
```zsh
% g++ -dM -E -x c++ /dev/null | grep __cplusplus
```

다음과 같은 format으로 출력된다.  
```zsh
define __cplusplus [DEFAULT_VERSION]
```
**[DEFAULT_VERSION]**  
* 199711L: C++98/03
* 201103L: C++11
* 201402L: C++14
* 201703L: C++17
* 202002L: C++20

* * *

compile할 때 특정 C++ 표준을 사용하도록 하는 방법:  
```zsh
# g++ -std=[C++ 표준 버전] [C++파일명] -o [실행파일명]
g++ -std=c++11 example.cpp -o example
```
