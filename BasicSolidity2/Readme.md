# Solidity Function & Event
  - function
  - event

---

## function
  ### 1. 함수의 선언
  - `function` 을 이용해 선언
  - 인자명을 보통 `_` (언더바) 로 시작해서 전역변수와 구별하는 것이 관례

  ```javascript
  contract Greeter {
      function createInfo(string _name, uint _age, string _phone){

      }
  }
  ```

  ### 2. 함수의 접근 제어자
  - 해당 함수를 부를 수 있는 어카운트 범위를 제한 (어카운트는 컨트랙트 어카운트(CA), 외부소유어카운트(EOA) 가 존재)
  - 함수의 가시성 종류
    - `public` : 모든 곳에서 호출 가능하며 어카운트에 대한 제한도 없다. </br>호출에 사용된 매개변수가 항상 메모리에 기록되기 때문에 가스가 많이 소모될 수 있다.
    - `private` : 함수를 선언한 컨트랙트에서만 호출 가능
    - `external` : 외부소유어카운트에서만 호출 가능, 호출에 사용된 매개변수들은 calldata 영역에 기록
    - `internal` : 함수를 선언한 컨트랙트 및 상속받은 컨트랙트에서만 호출 가능
  - tip> 기본적으로는 `private` 으로 선언하고, 공개할 함수만 `public` 으로 선언하도록 한다.

  ```javascript
  contract Greeter {
      function createInfo(string _name, uint _age, string _phone) public {

      }
  }
  ```

  ### 3. 함수의 반환값
  - `returns` 를 선언하여 반환값 종류를 선언
  - 함수 내부에 `return` 을 활용하여 특정 값을 반환

  ```javascript
  contract Greeter {
      function createInfo(string _name, uint _age, string _phone) public returns(string) {
          return "Success";
      }
  }
  ```

  ### 4. 함수 상태 제어자
  - 함수 제어자는 컨트랙트에 정의된 상태 변수를 읽거나 쓰는 연산을 함수 내에서 수행할 수 있는지를 제한하기 위한 용도로 사용
  - 이를 적절히 사용해야 gas 소모량을 줄일 수 있다. (pure 와 view 는 가스를 소모하지 않는다.)
  - 함수 제어자 종류
    - `pure` : 블록체인 네트워크에 기록된 데이터에 아예 접근하지 않으며, 파라미터로 주어지지 않은 상태 변수는 읽거나 쓸 수 없다.
    - `view` : 데이터를 보기만 하고 데이터를 수정할 수 없다.
    - `payable` : 계약 계정에 외부에서 이더를 송금 받을 수 있도록 한다.
    - `non-payable` (default) : 특별히 선언하지 않으면 지정되며 이더 송금이 불가한 payable 이다.

  ```javascript
  contract exampleContract {
      uint private year = 2018;
      uint256 balance = 0;

      // view
      function getCurrentYear() public view returns (uint) {
          return year;
      }

      // pure
      function isLeapYear(uint _year) public pure returns (bool) {
          return ((_year % 4 == 0) && (_year % 100 != 0)) || (_year % 400 == 0);
      }

      // payable
      function buySomething() external payable {
        transferThing(msg.sender); // 송금하는건 나중에..
      }
  }
  ```

---

## Event
  ### 1. Event
  - 컨트랙트가 블록체인 상에서 앱의 사용자 단에서 무언가 액션이 발생했을 때 의사소통하는 방법
  - 특정 이벤트가 일어나는지 "귀를 기울이고" 그 이벤트가 발생하면 행동을 취함

  ```javascript
  // 이벤트를 선언
  event IntegersAdded(uint x, uint y, uint result);

  function add(uint _x, uint _y) public {
    uint result = _x + _y;
    // 이벤트를 실행하여 앱에게 add 함수가 실행되었음을 알림
    IntegersAdded(_x, _y, result);
    return result;
  }
  ```

  - 이에 대한 이벤트를 자바스크립트로 구현하게 되면 다음과 같이 지정할 수 있다.

  ```javascript
  YourContract.IntegersAdded(function(error, result) {
      // 결과와 관련된 행동을 취함
  })
  ```

---

## msg.sender & require
  ### 1. msg.sender
  - 현재 함수를 호출한 사람(혹은 스마트 컨트랙트)의 주소를 가리키는 전역 변수
  - 컨트랙트는 누군가가 항상 실행하므로 `msg.sender` 는 반드시 존재한다.

  > mapping 을 이용한 예제

  ```javascript
  mapping (address => uint) favoriteNumber;

  function setMyNumber(uint _myNumber) public {
    // `msg.sender`에 대해 `_myNumber`가 저장되도록 `favoriteNumber` 매핑을 업데이트
    favoriteNumber[msg.sender] = _myNumber;
  }

  function whatIsMyNumber() public view returns (uint) {
    // sender의 주소에 저장된 값을 로드
    // sender가 `setMyNumber`을 아직 호출하지 않았다면 반환값은 `0`
    return favoriteNumber[msg.sender];
  }
  ```

  ### 2. require
  - 특정 조건이 참이 아닐 때 함수가 에러 메시지를 발생하고 실행을 멈춤

  ```javascript
  function sayHiToVitalik(string _name) public returns (string) {
    // _name이 "Vitalik"인지 비교한다. 참이 아닐 경우 에러 메시지를 발생하고 함수를 벗어남
    require(keccak256(_name) == keccak256("Vitalik"));
    // 참이면 함수 실행
    return "Hi!";
  }
  ```
