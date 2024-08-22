**Table of contents**  
* [Section 1.1 Writing a Simple C++ Program](#section-11-writing-a-simple-c-program)  
* [Section 1.2 A First Look at Input/Output](#section-12-a-first-look-at-inputoutput)


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



# A Word about Comments



# Flow of Control
