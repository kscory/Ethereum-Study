# Solidity : Inheritance & import & interface & library
  - Inheritance
  - import
  - library
  - interface

---

## Inheritance & import & library
  ### 1. Inheritance
  - 솔리디티에서 컨트랙트의 상속을 지원하며 `is` 의 형태로 지원한다.
  - 참고> 하나의 긴 로직을 만드는 것보다 여러 로직으로 나누어 관리하는게 편할 때 상속을 이용한다.(어느 언어나 동일)

  > BabyDoge 는 Doge 를 상속받아 catchphrase 라는 함수를 사용할 수 있도록 했다.

  ```javascript
  contract Doge {
      function catchphrase() public returns (string) {
          return "So Wow CryptoDoge";
      }
  }

  contract BabyDoge is Doge {
      function anotherCatchphrase() public returns (string) {
          return "Such Moon BabyDoge";
      }
  }
  ```

  ### 2. import
  - 여러 Solidity 파일로 나눠서 저장했을 경우 `import` 를 통해 파일을 가져올 수 있다.
  - import 한 파일 내의 contract 를 가져올 수 있으며, 보통 한가지 컨트랙트를 가지고 있으므로 상속을 진행한다.

  > 동일 디렉토리 내의 contract를 호출 후 상속

  ```javascript
  import "./dogContract.sol";

  contract PuddleContract is DogContract {

  }
  ```

  ### 3. library
  - 라입러리란 솔리디티에서 특별한 종류의 컨트랙트
  - 기본(native) 데이터 타입에 함수를 붙일 때 자주 사용
  - 내부에 정의된 함수를 바로 사용할 수 있다.
  - `using` 을 통해 라이브러리를 사용한다 명시

  > 라이브러리 예시 (safemath.sol 이라 가정)

  ```javascript
  library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
      if (a == 0) {
        return 0;
      }
      uint256 c = a * b;
      assert(c / a == b);
      return c;
    }
  }
  ```

  > 라이브러리 사용

  ```javascript
  import "./safemath.sol";

  using SafeMath for uint256;

  uint256 a = 5;
  uint256 c = a.mul(2); // 5 * 2 = 10
  ```

---

## interface
  ### 1. interface 란?
  - __이더리움 블록체인__ 상에 있는 컨트랙트와 상호작용을 하기 위해서 사용됨
  - interface 를 이용하기 위해서는 원하는 컨트랙트의 함수가 `external` 혹은 `public` 으로 선언되어 있어야만 한다.

  ### 2. interface 작성
  - 컨트랙트와 동일하게 `contract 인터페이스` 으로 선언 (인터페이스를 위한 컨트랙트 명은 기존 컨트랙트 명과 달라도 됨)
  - 컨트랙트 내에 상호작용하고자 하는 함수만을 선언한다.
  - 함수 몸체를 정의하지 않고 __선언만__ 한다. (뼈대만 선언함)

  > 블록체인상의 컨트랙트라 가정

  ```java
  contract LuckyNumber {
      mapping(address => uint) numbers;

      function setNum(uint _num) public {
          numbers[msg.sender] = _num;
      }

      function getNum(address _myAddress) public view returns (uint) {
          return numbers[_myAddress];
      }
  }
  ```

  > getNum 함수만을 가져오는 interface 선언

  ```java
  // 인터페이스 컨트랙트 작성
  contract NumberInterface {
      // 몸체를 제외한 함수 인터페이스 작성
      function getNum(address _myAddress) public view returns (uint);
  }
  ```

  ### 3. interface 사용
  - 사용할 컨트랙트의 주소를 가져옴 (이더리움 상의 주소)
  - 만든 interface Contract 에 주소를 연결 (이 작업을 수행하면 원하는 컨트랙트를 가리키게 된다.)
  - 함수를 이용

  > 2번에서 선언한 interface를 이용

  ```javascript
  contract MyContract {
      // 사용할 컨트랙트의 주소를 가져옴
      address NumberInterfaceAddress = 0xab38...
      // 만든 interface Contract 에 주소를 연결
      NumberInterface numberContract = NumberInterface(NumberInterfaceAddress)

      function someFunction() public {
          // 정의된 함수를 이용
          uint num = numberContract.getNum(msg.sender);
      }
  }
  ```
---
