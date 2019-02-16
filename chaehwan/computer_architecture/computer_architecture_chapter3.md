# Chapter3. 컴퓨터 산술과 논리 연산

# ALU의 구성 요소

ALU : 수치 및 논리 데이터에 대하여 실제적으로 연산을 수행하는 하드웨어 모듈
-> 실제 연산은 ALU에서 한다. 나머지 구성 요소는 인출/저장/출력

ALU의 내부 요소
* 산술연산장치 : 산술 연산 수행
* 논리연산장치 : 논리 연산들을 수행
* 시프트 레지스터 : 비트들을 좌측 혹은 우측으로 이동
* 보수기 : 데이터에 대하여 2의 보수 취함(음수화)
* 상태 레지스터 : 연산 결과의 상태를 나타내는 플래그를 저장하는 레지스터
    * ex) Z(제로 플래그) : 연산의 결과가 0일때 참이 된다.

레지스터 or 기억장치 -> 
* 처리할 데이터가 입력으로 전달 / 
* 제어 신호: 연산을 수행할 내부 요소의 선택과 ALU 내외로의 데이터 이동 제어
-> ALU -> 
* 결과는 레지스터 중 하나에 저장 / 
* 연산의 결과에 따라 상태 레지스터 내의 해당 플래그들을 세트 
-> 플래그들을 조건 분기 명령어 or 산술 명령어에 사용

# 정수의 표현

컴퓨터가 내부적으로 저장하고 처리하는 데이터는 모두 숫자
-> 그리고 그 숫자 중에서도 이진수를 저장하고 처리

그냥 양의 정수는 간단하게 표현 가능! 문제는 음수와 소수점을 어떻게 표현할 것인가?
* 소수점 : 양의 정수를 표현할 때는 소수점이 최하위 비트에 있다고 가정 (고정소수점 수)
* 음수 : 3가지 방법 존재 / 공통점은 가장 좌측 비트는 부호 비트로 사용 -> 0: 양수, 1: 음수
    * 부호화 크기 표현
        * 맨 좌측 비트가 부호 비트이고, 나머지는 수의 크기 표현
        * ex) 9 -> 0 0001001
        * 단점
            * 덧셈과 뺄셈 수행 시 부호 비트와 크기 부분을 별도로 처리
            * 0에 대한 표현이 2가지 -> +0, -0
    * 보수 표현 : 부호화 크기 표현의 결점을 해결하기 위해 개발
        * 1의 보수 표현
            * 모든 비트를 반전하는 방법
            * 부호화 크기 표현과 마찬가지로 0에 대한 표현이 2가지
        * 2의 보수 표현
            * 같은 길이의 비트들로 표현할 수 있는 수의 개수가 하나 더 많기 때문에 
            일반적으로 사용
            
참고)
컴퓨터의 정수와 실수 표현
출처: https://www.youtube.com/watch?v=YNV3R34i-RQ
진법과 보수(영상 40:20 ~ ) 
출처 : https://www.youtube.com/watch?v=RF04L7KAmKA&index=5&list=PLrFy4sCm2owoj-O71kjLoNc_dMEVzUYlR

부호 비트의 확장
* 처리하는 데이터의 길이가 CPU 연산 과정에서 다른 경우 존재
* ex) 기억장치에 8비트 길이로 저장되어 있는 데이터를 읽어 16비트 레지스터에 저장하는 경우
  -> 8비트가 비기 때문에 16비트로 확장해야 함
   * 부호화 크기 표현 : 부호 비트를 맨 좌측으로 옮기고 그 외의 위치들은 0으로 채우면 됨
   * 2의 보수 : 확장되는 상위 비트들을 부호 비트와 같은 값으로 세트 -> 양수면 0, 음수면 1

# 논리 연산

1) AND 연산
* 두 개의 데이터가 같은 위치에 있는 비트들 간에 AND 연산 수행
* 두 비트들이 모두 1인 경우에는 결과 데이터의 해당 비트가 1이 되고,
어느 하나라도 0이면 그 비트는 0
* 각 비트가 1이면 true, 0이면 false라는 논리적 의미를 가짐

2) OR 연산
* 두 개의 데이터가 같은 위치에 있는 비트들 간에 OR 연산 수행
* 두 비트들 중 어느 하나만 1이면 결과 데이터의 해당 비트가 1이 되고,
두 비트들이 모두 0이면 그 비트는 0이 됨

3) XOR 연산
* 두 비트들이 서로 다른 값을 가지면 결과 데이터의 해당 비트가 1이 되고, 같은 값을 가지면
그 비트는 0이 됨

4) NOT 연산
* 데이터 단어의 모든 비트들이 반전
* 1 -> 0, 0 -> 1

5) 선택적 세트 연산(Selective-set)
* 데이터의 특정 비트들을 1로 세트해주기 위한 동작
* 데이터가 저장된 A 레지스터와 세트될 비트들의 위치를 지정해주는 B 레지스터 간에 OR 연산 수행
* B 레지스터의 비트들 중에서 값이 1인 비트들과 같은 위치에 있는 A 레지스터들의 비트들이 1로 세트됨
* B 레지스터의 비트들 중에서 값이 0인 비트들과 같은 위치에 있는 A 레지스터들의 비트들은 변화가 없음

6) 선택적 보수 연산(Selective-complement)
* 데이터 내의 특정 비트들의 값을 보수화(1->0, 0->1)하기 위한 동작
* 데이터가 저장된 A 레지스터와 반전시킬 비트들의 위치를 지정해주는 B 레지스터 간에 XOR 연산 수행
* B 레지스터의 비트들 중에서 값이 1인 비트들과 같은 위치에 있는 A 레지스터의 비트들은 반대되는 값으로 바꿈
* B 레지스터의 비트 값이 0인 위치는 변화가 없음

7) 마스크 연산(masking)
* 데이터 내의 특정 비트들의 값을 0으로 리셋
* 데이터가 저장된 A 레지스터와 반전시킬 비트들의 위치를 지정해주는 B 레지스터 간에 AND 연산 수행
* B 레지스터의 비트들 중에서 값이 0인 비트들과 같은 위치에 있는 A 레지스터의 비트들은 0으로 리셋됨
* B 레지스터의 비트들 중에서 값이 1인 비트들과 같은 위치에 있는 A 레지스터의 비트들은 변화가 없음

8) 삽입 연산
* 데이터 내의 특정 비트들의 값을 새로운 값으로 대체시키기 위한 동작
* 2단계
    * 1단계 : 삽입하고자 하는 비트들에 대하여 마스크 연산 수행 -> 0으로 리셋
    * 2단계 : 새로 삽입할 비트들과 그 결과 데이터 간에 OR 연산을 수행

9) 비교 연산
* A와 B 레지스터의 내용을 비교
* 두 레지스터에서 대응되는 비트들의 값이 같으면 A 레지스터의 값을 0으로 세트
* 서로 다르면 A 레지스터의 해당 위치의 비트를 1로 세트
* XOR 연산을 통해 구현 가능

# 시프트 연산

1) 논리적 시프트

레지스터 내의 데이터 비트들을 왼쪽 혹은 오른쪽으로 한 칸씩 이동시키는 것
* 유의사항 : 최하위 비트는 0이 들어오고, 최상위 비트는 버리게 됨
* 우측 시프트 연산 : 원래 수를 2로 나눈 값
* 좌측 시프트 연산 : 원래 수에 2를 곱한 결과
* 직렬 데이터 전송 가능

2) 순환 시프트

논리적 시프트와 근본적으로 같지만, 최상위 혹은 최하위 비트를 버리지 않고 
반대편 끝에 있는 비트 위치로 들어가게 한다는 것
* 직렬 데이터 전송 가능

3) 산술적 시프트

레지스터에 저장된 데이터가 부호를 가진 정수인 경우 부호 비트를 고려하여 수행되는 시프트
부호 비트는 그대로 두고, 수의 크기를 나타내는 비트들만 시프트

4) C플래그를 포함한 시프트 연산

실제 CPU에서는 일반적으로 시프트 연산에 올림수(C) 플래그가 포함됨
C 플래그를 포함한 좌측-시프트 연산에서는 최상위 비트가 버려지지 않고
C 플래그로 이동
(원래 이때 C 플래그의 값은 지워짐)

# 정수의 산술 연산

1) 덧셈

2의 보수로 표현된 수들간의 덧셈 방법 : 먼저 두 수를 더하고, 만약 올림수가 발생 시 버림
덧셈 결과값의 부호가 1이면, 음수를 의미

병렬 가산기 : 정수의 산술 연산 중 덧셈을 수행
* 데이터 비트 수만큼의 전가산기들로 구성
* 전가산기 - 올림수 비트를 전송하는 선에 의해 연결
    * 하위 단계에서 발생한 올림수가 상위 단계 전가산기의 올림수 입력

오버플로우 : 덧셈에서 수의 표현 범위를 초과하여 전혀 틀린 결과를 산출하는 것
ex) 2의 보수로 표현된 4비트 데이터의 표현 범위 : -8 ~ + 7 -> 이 범위 초과 시 오버플로우
   * ex1) (+6) + (+3) = +9 
        0110
        0011
     -> 1001 => -7 (오버플로우)
   * ex2) (-7) + (-6) = -13
        1001
        1010
   -> 1 0011 => +3 (오버플로우) 

오버플로우는 덧셈 과정에서 최상위 비트들 및 그 다음 비트들의 덧셈 과정에서 발생하는 올림수들이
서로 다른 경우에 발생
-> 두 올림수 간의 exclusive-OR를 수행하여 검출 가능
    오버플로우 여부(V) = C4(4비트 데이터의 최상위 비트) XOR C3(4비트 데이터의 최상위 비트의 다음 비트)

참고) 캐리와 오버플로우
출처 : http://blog.naver.com/PostView.nhn?blogId=hjyang0&logNo=183698525

2) 뺄셈

뺄셈은 덧셈을 이용해서 수행 가능하다.

A + (-B) = A + (-B)
A - (-B) = A + (+B)

* 정수들의 뺄셈은 다음과 같이 덧셈을 이용하여 수행 가능
* 피감수(A)로부터 감수(B)를 빼려면 B를 음수화한 다음 A에 더하면 됨
* 2의 보수로 표현된 수들의 뺄셈도 같은 방법으로 처리 가능
* 이런 이유로 뺄셈은 덧셈을 이용하여 수행되고 일반적으로 ALU에 뺄셈을 위한
회로를 별도로 두지 않고 가산기를 이용하여 뺄셈을 수행
* 대신 B는 가산기에 들어가지 전에 보수기를 거침
(덧셈인 결국 신호를 0으로 주어 그대로 통과)

cf) 57 - 34 = 57 - (100 - 66) = 57 + 66 - 100 = 23

3) 곱셈

4) 나눗셈


# 부동소수점 수의 표현

앞서 설명한 2진수 표현 방법의 문제 : 정해진 비트들을 가지고 표현해야 하므로
표현 가능한 수의 한계가 있어서, 아주 큰 수나 아주 작은 수는 표현할 수 없음

10진수의 경우 과학적 표기(scientific notation)를 사용하여 그러한 문제 극복
ex) 274,000,000,000,000 -> 2.74 X 10^14
ex2) 0.000000000000274 -> 2.74 X 10^-12
10진 소수점의 위치를 적절히 이동시키고, 소수점의 위치는 지수를 이용하여 표현
이렇게 소수점의 위치를 필요에 따라 이동시키는 표현 방법 -> '부동소수점 표현'
그리고 그렇게 표현된 수 -> '부동소수점 수'
이 방법을 통해 매우 큰 수와 매우 작은 수 간결하게 표현 가능

N = (-1)^S * M * B^E

S : 수의 부호
M : 가수
B : 기수 (10진수 -> 10, 2진수 -> 2)
E : 지수

ex) 2.74 * 10^14 -> M = 2.74, E = +14, B = 10
그런데
디지털 컴퓨터 -> 2진수 체계 -> 2진 부동소수점 수 표현
(기수만 2가 됨)

가수가 정밀도를 결정하고, 지수가 표현 가능한 수의 범위를 결정
(데이터의 길이가 고정된 상태에서 어느 한 필드의 비트 수를 증가시키면 다른 필드의
비트 수가 감소하므로 적절히 조정하는 것이 필요)

결국 수의 표현 범위와 정밀도를 동시에 늘릴 수 있는 방법은 데이터 표현에 더 많은 비트들을
사용하는 것 

단일 정밀도 수 : 32비트로 표현된 부동소수점 수
복수 정밀도 수 : 64비트로 표현된 부동소수점 수

사실 하나의 수에 대해 여러 개의 표현 가능

0.1101 * 2^5
11.01 * 2^3
0.001101 * 2^7

이로 인한 혼란을 막는 방법은 한 가지 표준 형식을 정하여 사용하는 것
-> '정규화된 표현'으로 나타내는 것
+(-) 0.1bbb..b * 2^E (b는 임의의 값(0 or 1)을 가진 비트)
정규화된 표현 : 소수점의 바로 오른 편에 있는 비트가 반드시 '1'이 되도록 위치를 조정하는 것
그에 따라 지수가 결정 -> 위의 예제라면 0.1101 * 2^5

ex)
데이터 표현
S(부호) E(지수)      M(가수)
0    | 00000101 | 11010000000000000000000

* 정규화된 표현의 경우 2진 소수점 아래의 첫 번째 비트는 항상 1이므로
이 비트는 반드시 저장할 필요 없음 -> 나중에 처리해주기만 하면 됨
-> 한 비트 더 저장 가능
* 부동소수점 표현에서의 문제 : 정수 0에 대한 표현
* 정수 0은 지수 0일 때가 아님 -> 만약 E의 값이 아주 큰 음수라면
2^E의 절대값이 거의 0에 가까워질 때 것 -> 지수를 바이어스된 수로 표현
(바이어스된 수 : 어떤 수에 바이어스 값이 더해진 수
ex) 바이어스 : 3 -> 1000 + 11 = 1011)
바이어스가 128이면 그 수만큼 이진수로 지수부에 더해주면 됨
반대로 실제 지수값을 찾으려면 이진수로 바이어스값을 빼주면 됨

IEEE(미국전기전자공학회) 표준 (호환성 향상 목적)
* 32 비트 단일 정밀도 형식
    * 지수부 : 8비트
    * 부호 비트 : 1비트
    * 가수부 : 23비트
    * 소수점 위의 1은 나타내지 않고 소수점 아래 부분만 표현
      (hidden bit)
    * 바이어스 값 : 127
* 64 비트 복수 정밀도 형식
    * 지수부 : 11비트
    * 부호 비트 : 1비트
    * 가수부 : 52비트

참고) 부동소수점에 대한 이해
http://thrillfighter.tistory.com/349

# 부동소수점 산술 연산

1) 덧셈과 뺄셈

* 덧셈과 뺄셈이 더 복잡
    * why? 먼저 두 수의 지수를 일치시켜주어야 하기 때문
      (두 수의 소수점을 같게 한다는 것)
    * 연산을 수행한 후에는 다시 정규화 진행
* 절차
    * 두 수의 소수점 위치를 일치시킨다 
    * 가수들 간에 더하기(or 빼기)를 수행한다.
    * 결과를 정규화한다.
* 파이프라인 단계로 나누어 처리 가능

2) 곱셈과 나눗셈

* 덧셈과 뺄셈과 달리 소수점 위치조정이 필요 없음
* 절차
    * 가수들을 곱한다.
    * 지수들을 더한다.
    * 결과값을 정규화한다.
* 발생할 수 있는 문제들
    * 지수 오버플로우
    * 지수 언더플로우
    * 가수 언더플로우
    * 가수 오버플로우