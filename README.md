# 2021 마스터즈 코스 테스트
 
### 개요 및 주의 사항
 - 단계별(step1 - 3)로 루빅스 큐브 구현
 - 지정된 코딩 요구사항을 적용하되, 특별히 명시되지 않은 부분은 자유롭게 구현한다.
 
 
### 코딩 요구사항
 - [ ] 단계별로 구현한 코드의 동작 방식을 README.md에 정리
 - [ ] 단계별로 각각 step-1, step-2, step-3 라는 이름의 브랜치 작성
 - [ ] 커밋 시점과 단위, 커밋 메세지 고려하여 작성 [[참고](https://meetup.toast.com/posts/106)]

### 1단계: 단어 밀어내기 구현하기
1. 입력: 단어 하나, 정수 숫자 하나( -100 <= N < 100) , L 또는 R을 입력받는다. L 또는 R은 대소문자 모두 입력 가능하다.
    - INPUT : Char, Integer, 'L'/'l' OR 'R'/'r' 
    - Exception Handling
        * User Input Error
        * Integer Boundary Error
2. 주어진 단어를 L이면 주어진 숫자 갯수만큼 왼쪽으로, R이면 오른쪽으로 밀어낸다.
    - subString()
    - Exception Handling
        * IndexOutOfBoundsException
3. 밀려나간 단어는 반대쪽으로 채워진다.


### 1단계 : Pseudo Code
1. 사용자 입력(User Input)
    - BufferedReader로 사용자 입력 받기 ( word, N, direction )
    - Exception Handling
        * InputMismatchException, IOException, NullPointerException [[참고](https://docs.oracle.com/javase/7/docs/api/java/util/Scanner.html)]
             
2. N 정제(Purifying Integer input, N)
    - IF N == 0 
        * Doesn't shift
    - IF N < 0
        * 'R'/'r' ↔ 'L'/'l'
        * N = -N
    - IF N >= word.length
        * N = N % word.length

3. 방향에 따른 정제(Direction matters)
    - IF direction == 'R' or 'r'
        * N = word.length - N

4. String Shift
    -  word.substring(N) + word.substring(0, N)
    
### 2단계: 평면 큐브 구현하기

#### 요구 사항
- 너무 크지 않은 함수 단위로 구현하려고 노력할 것
    - [ ] 기능 단위 모듈화 신경쓰기
- 전역 변수의 사용을 자제할 것
    - [ ] 최대한 캡슐화, 정보 은닉
- 객체와 배열을 적절히 활용할 것
    - [ ] Cube 객체 
    - [ ] Cube 객체 필드에 배열 형태의 cube 정보

#### 작동 방식
사용자 입력을 받아서 아래의 동작을 하는 프로그램을 구현하시오

1. 처음 시작 시 초기 상태를 출력

2. 간단한 프롬프트 표시
    - CLI에서 키보드 입력받기 전에 표시해주는 간단한 글자들 - 예: CUBE>
    
3. 사용자 명령어 입력
    - 한 번에 여러 문자를 입력받은 경우 순서대로 처리해서 매 과정을 화면에 출력


### 2단계 : Pseudo Code

1. 큐브 현재 상태 출력
    - Cube cube = new Cube();
    - cube.printCube()
        - Cube 현재 상태 출력

2. 사용자 입력(User Input)
    - `CUBE>` prompt 출력
    - BufferedReader로 사용자 입력 받기 ( word, N, direction )
    - Exception Handling
        * InputMismatchException, IOException, NullPointerException [[참고](https://docs.oracle.com/javase/7/docs/api/java/util/Scanner.html)]

3. 명령 수행
    - Cube field
        - char[][] cubeInfo = new char[3][3];
        - 유저 입력에 따라 step-1에서 만든 기능으로 명령 수행     
```java 
// 큐브 예시
   R R W
   G C W
   G B B
```
   - 명령어
``` java
  가장 윗줄 (word = cubeInfo[0][0] + cubeInfo[0][1] + cubeInfo[0][2])
      - U : 왼쪽으로 한 칸 밀기 RRW -> RWR
        💠 word 1 L
      - U' : 오른쪽으로 한 칸 밀기 RRW -> WRR
        💠 word 1 R
  가장 오른쪽줄 (word = cubeInfo[0][2] + cubeInfo[1][2] + cubeInfo[2][2])
      - R : 위로 한 칸 밀기 WWB -> WBW
        💠 word 1 L
      - R': 아래로 한 칸 밀기 WWB -> BWW
        💠 word 1 R
  가장 왼쪽줄 (word = cubeInfo[0][0] + cubeInfo[1][0] + cubeInfo[2][0])
      - L : 아래로 한 칸 밀기 RGG -> GRG (L의 경우 R과 방향이 반대임을 주의한다.)
        💠 word -1 L
      - L': 위로 한 칸 밀기 RGG -> GGR
        💠 word -1 R
  가장 아랫줄 (word = cubeInfo[2][0] + cubeInfo[2][1] + cubeInfo[2][2])
      - B : 오른쪽으로 한 칸 밀기 GBB -> BGB (B의 경우도 U와 방향이 반대임을 주의한다.)
        💠 word -1 L
      - B': 왼쪽으로 한 칸 밀기 GBB -> BBG
        💠 word -1 R
  프로그램 종료
      - `Q` : Bye~를 출력 후 종료
```
### 생각할 점
```java 
// 큐브 예시
   A B C
   D E F
   G H I
```

- 어떤 식으로 큐브 정보를 저장?
    1. 가로 혹은 세로 기준으로 저장 
        - 가로 기준 {"ABC", "DEF", "GHI"}
        - 세로 기준 {"ADG", "BEF", "CFI"}
        - (+) 정보 업데이트 용이
        - (-) 가로 기준이면 세로 명령 업데이트 힘들다, vice versa
    2. 가로 기준, 세로 기준 모두 저장 ❎
        - (-) 한 번 업데이트 시, 두 정보 다 업데이트 해주어야 함
        - (-) 메모리 * 2
        - (+) 명령 처리 용이
    3. 2D array에 저장 ✅
        - (+) 업데이트 용이
        - (-) String concat time-complicity..? maybe..?
        - 한 명령어에 한 칸만 움직이는거면, step-1에서 구현한 함수를 쓰지 않는게 효율적일 듯..?
        - 이후 확장성(한 칸 이상 움직이는 명령을 추가할 가능성)을 생각해서, step-1에서 구현한 함수 사용하는 방향으로 
        