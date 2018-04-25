# Basic Solidity 1
  - Version Pragma
  - Contract 선언
  - Solidity Type
  - keccak256
  - 형 변환

---
## Version Pragma & Contract
  ### 1. Version Pragma
  - 솔리디티 소스 코드는 항상 `version pragma` 로 시작
    - 프로그램이 호환성을 가진 솔리디티 컴파일러 버전을 명시
    - 새로운 컴파일러 버전이 나와도 기존 코드가 깨지지 않도록 예방
  - 버전 프래그마의 의미
    - `^0.4.19` : 0.x.x 와 호환
    - `=1.2.7` or `1.2.7` : 1.2.7 만 호환
    - `<=1.2.0` : 1.2.0 이하
    - etc..

  ```c
  pragma solidity ^0.4.19;
  ```

  ### 2. Contract
  - 솔리디티 코드는 컨트랙트 안에 있다.
  - __컨트랙트__ 는 이더리움 application 의 기본 요소로 모든 변수와 함수는 어느 한 컨트랙트에 속해있다.

  ```c
  pragma solidity ^0.4.19;
  contract HelloWorld{
    // 로직 작성
  }
  ```

## Solidity Type
  ### 0. 상태 변수
  - 상태변수는 컨트랙트 저장소에 영구적으로 저장, 즉 블록체인에 기록됨
  - 이는 데이터베이스에 데이터를 쓰는 것과 동일
  - `public`, `private`, `internal` 중 하나로 변수의 __가시성__ 지정 가능
    - `public` : 상태 변수를 조회하는 `getter` 함수를 자동으로 생성
    - `private` : 컨트랙트 안에 정의된 함수에서만 사용 가능
    - `internal` : 속해있는 컨트랙트와 상속받은 컨트랙트 안에 정의된 함수에서 사용 가능
  - ex> `myUnsignedInteger` 에 uint를 생성하여 100 이라는 값을 배정

  ```c
  contract Example {
    uint myUnsignedInteger = 100;
  }
  ```

  ### 1. 정수형
  - `uint` : 부호가 없는 정수형
    - `uint32` : 32비트의 정수형
    - `uint256`: 256비트의 정수형(일반 uint는 이와 동일)
    - etc...
  - `int` : 정수형
  - 정수형 뒤에 사용할 비트수는 일반적으로 사용할때는 상관 없지만 구조체 안에서는 이르 해야만 가스소모를 줄일 수 있다.
  - 또한 정수형은 연산이 가능하다.
    - 비교연산 : `<=`, `<`, `==`, `!=`, `>=`, `>`
    - 비트연산 : `&`, `|`, `^`, `~`
    - 산술 연산 : `+`, `-`, `*`, `%`, `**`, `<<`, `>>`

  ```c
  contract HelloWorld {
      int down = -3;          // 정수형
      uint age = 16;          // 부호가 없는 정수형
      uint8 grade = 1;        // 바이트 수 지정
      uint total = 10 ** 16;  // 연산 가능
  }
  ```

  ### 2. 부울형
  - `bool` : 논리 자료형 (true, false 존재)
  - 다른 언어와 동일하게 여러 연산자의 리턴값으로 존재
    - `!`, `&&`, `==`, `!=` 등

  ### 3. address
  - 솔리디티의 특징 중 하나.
  - 이더리움 주소값 (20바이트)이며 특정 함수 호출 가능
  - 정수의 비교연산 모두 사용 가능
  - 내장된 특정 함수 호출 가능

  변수 or 함수 | Type | 설명
  :----: | :----: | :----:
  balance | uint256 | 주소의 잔액(Wei)
  transfer(uint256 amount) | | 주어진 주소로 amount 만큼의 Wei를 전송 </br>(실패시 예외처리)
  send(uint25 amount) | bool | 주어진 주소로 amount 만큼의 Wei를 전송 </br>(실패시 false 반환, 실패 로직 구현 가능)
  call(...) | bool | 임의의 컨트랙트에 있는 함수를 부를 때 사용 </br>(EVM의 CALL 을 부름 => ex> `value()`, `gas()` 값 지정 가능)
  callcode(...) | bool | EVM의 CALLCODE를 부름
  delegatecall(...) | bool | EVM의 DELEGATECALL 을 부름 </br>(call과 다르게 `storage` 와 `balance` 등 사용 가능하지만 `value()` 옵션 사용 불가)

  ### 4. string
  - UTF-8 인코딩 문자열을 저장학 위한 동적 크기 배열
  - 다른 언어와 비슷하게 문자열 길이, 문자열 특정문자확인, 문자열 비교함수, 자르기 등의 API 제공
    - 그러나 GAS 소모량 때문에 대부분 DApp 에서 문자열 연산을 수행하며 컨트랙트에서는 수행하지 않는다.

  ```c
  string greeting = "Hello world!";
  ```

  ### 5. 구조체
  - 다양한 자료형의 데이터를 묶을 수 있다.
  - `struct` 로 선언

  ```c
  // 구조체 생성
  struct Person {
      string name;
      uint age;
      address wallet;
  }

  function birthday() {
      // 구조체 인스턴스 생성
      Person p = Person("James", 26, 0x7Df34...);
      p.age += 1;
  }
  ```

  ### 6. 배열
  - 배열에는 `정적`, `동적` 배열의 두 종류 배열 존재하며 동적 배열의 경우 `스토리지`, `메모리` 배열의 두가지 존재 (메모리와 스토리지는 나중에 다시 설명)
  - `정적 배열` 은 배열의 크기를 변경 할 수 없으며 아래 연산을 지원
    - 인스턴스 선언 : `uint8[3] scores = [1,2,3];`
    - 인덱스 접근 : `scores[1];`
    - 배열 원소 변경 : `scores[1] = 4;`
    - 길이 조회 : `scores.length;`
  - `동적 스토리지 배열` 은 선언 시 배열의 크기를 지정하지 않으며 배열의 크기를 조정할 수 있다.(ex> push)
    - 인스턴스 선언 : `uint[] scores;`
    - 인덱스 접근 : `scores[1];`
    - 배열 원소 변경 : `scores[1] = 4;`
    - 길이 조회 : `scores.length;`
    - 크기 조정 : `scores.length = 20;`
    - 배열에 원소 추가 : `scores.push(30);`
  - `동적 메모리 배열` 은 스마트 컨트랙트가 실행될 때 동적으로 크기가 결정되며, 배열의 크기가 한번 결정되면 변경 불가능하다.
    - 인스턴스 선언 : `uint[] scores = new uint[](10);`
    - 인덱스 접근 : `scores[1];`
    - 배열 원소 변경 : `scores[1] = 4;`
    - 길이 조회 : `scores.length;`
  - `public` 등 으로 배열 선언 가능하며 이는 `getter` 메소드를 자동적으로 생성
  - 구조체를 배열로 선언 가능

  ```c
  // 정적 배열:
  uint[2] fixedArray;
  string[5] stringArray;

  // 동적 스토리지 배열:
  uint[] dynamicArray;

  // 동적 메모리 배열
  uint[] staticArray = new uint[](5);

  // 구조체 배열
  Person[] people;

  // Public 배열
  Person[] public people;
  ```

---
## Keccak256 & 형 변환
  ### 1. Keccak256
  - 이더리움은 SHA3의 한 버전인 keccak256를 내장 해시 함수로 가지고 있다.
  - 해시 함수는 기본적으로 입력 스트링을 랜덤 256비트 16진수로 매핑
  - 이를 이용해서 `string type` 이 일치하는지 확인 가능

  ```c
  keccak256("aaaab"); //6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5

  keccak256("aaaac"); //b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9

  if(keccak256("kitty") == keccak256("zombie")){
      uint feed = 0;
  }
  ```

  ### 2. 형 변환
  - 특정 형으로 감쌀 수 있다.
  - 바이트 개수가 다른 경우 이를 사용해야 한다.

  ```c
  uint8 a = 5;
  uint b = 6;

  uint8 c = a * b; // a * b가 uint8이 아닌 uint를 반환하기 때문에 에러 발생

  uint8 c = a * uint8(b); // b를 uint8으로 형 변환
  ```
