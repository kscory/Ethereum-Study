# Solidity : Ownable & SafeMath (OpenZeppelin)
  - [Ownable](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/ownership/Ownable.sol)
  - [SafeMath](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol)

---

## Ownable.sol
  ### 1. Ownable 컨트랙트란?
  - __생성자__ : 컨트랙트가 생성되면 컨트랙트의 생성자가 `owner` 에 `msg.sender`(컨트랙트를 배포한 사람)를 대입
  - __제어자__ : 특정한 함수들에 대해서 오직 소유자만 접근할 수 있도록 제한 가능한 `onlyOwner` 제어자 제공 (by require)
  - __함수__ : 새로운 소유자에게 해당 컨트랙트의 소유권을 옮길 수 있는 함수 제공
  - __이벤트__ : 함수의 소유권이 옮겨 졌을 때 사용할 수 있는 event 함수 제공

  > Ownable 컨트랙트 코드

  ```javascript
  contract Ownable {
    address public owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    function Ownable() public {
      owner = msg.sender;
    }

    modifier onlyOwner() {
      require(msg.sender == owner);
      _;
    }

    function transferOwnership(address newOwner) public onlyOwner {
      require(newOwner != address(0));
      emit OwnershipTransferred(owner, newOwner);
      owner = newOwner;
    }
  }
  ```

---

## SafeMath.sol
  ### 1. SafeMath 란
  - 숫자 연산에 있어 __오버플로우__ 와 __언더플로우__ 를 막기 위한 라이브러리 컨트랙트 (by assert)
    - 오버플로우 : 8비트에서 `11111111`에 `1`을 더하면 `00000000` 으로 돌아가는 현상과 같은 것
    - 언더플로우 : 그와 반대로 8비트에서 `0` 에서 1을 뺀 경우 `11111111` 이 되는 현상
  - 4개의 함수 존재
    - `add` / `sub` / `mul` / `div`
  - 그러나 `uint256` 만 존재하므로 `uint16`, `uint32` 와 같은 라이브러리를 추가적으로 만들어서 적용해야 한다. ([newSafeMath.sol](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/ownable/NewSafeMath.sol) 참고)

  > SafeMath.sol 코드

  ```javascript
  library SafeMath {

    function mul(uint256 a, uint256 b) internal pure returns (uint256 c) {
      if (a == 0) {
        return 0;
      }
      c = a * b;
      assert(c / a == b);
      return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
      // assert(b > 0); // Solidity automatically throws when dividing by 0
      // uint256 c = a / b;
      // assert(a == b * c + a % b); // There is no case in which this doesn't hold
      return a / b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
      assert(b <= a);
      return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256 c) {
      c = a + b;
      assert(c >= a);
      return c;
    }
  }
  ```
---

## 참고
  ### 1. OpenZeppelin
  - OpenZeppelin 에는 많은 검증된 솔리디티 코드가 존재
  - `ERC20`, `ERC721` 등의 기본 코드 제공
  - [OpenZeppelin 으로 가기](https://github.com/OpenZeppelin)
