# Basic Publish
  - Smart Contract 배포하기

---
## 배포 전 수행 사항
  ### 1. 스마트 컨트랙트 작성
  - 스마트 컨트랙트에 대한 내용은 나중에 다시 !!
    - 참고로 블록내용을 바꿔야 하는 set의 경우에만 가스비용이 들어간다.
  - 임시 코드 내용
    - `get` : storedData 변수값을 불러옴
    - `set` : storedData 에 변수값을 저장

  ```sol
  pragma solidity ^0.4.18;
      contract SimpleStorage {
          uint storedData;
          function set(uint x) {
              storedData = x;
          }
          function get() constant returns (uint) {
              return storedData;
          }
      }
  ```

  ### 2. 구현한 소스 코드 컴파일
  - 소스 코드를 컴파일해서 EVM 바이트 코드를 만들어 낸다.
  - 소스코드 컴파일러를 사용할 수 있지만 __remix__ 에서 제공하는 컴파일러 사용 (프라이빗에서 진행할 예정이므로 컴파일까지만)
  - 컴파일 순서
    - 코드 작성
    - __Start to complie__ 수행
    - Detail 확인

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicPublishing/picture/complie1.png)

  ### 3. 컴파일된 데이터 확인
  - Detail 에서 확인 가능
  - 컴파일된 `Byte Code`를 확인
  - `ABI` 를 확인
    - 참고) ABI : (geth 콘솔에서 사용할) 스마트 컨트랙트에서 사용될 인터페이스 부분을 의미
  - (이 변수들을 저장해놓고 쓰는 것이 좋지만 여기서는 그냥 진행함)

  ```javascript
  // Byte Code 예시
  0x608060405234801561001057600080fd5b5060df8061001f6000396000f3006080604052600436106049576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806360fe47b114604e5780636d4ce63c146078575b600080fd5b348015605957600080fd5b5060766004803603810190808035906020019092919050505060a0565b005b348015608357600080fd5b50608a60aa565b6040518082815260200191505060405180910390f35b8060008190555050565b600080549050905600a165627a7a72305820553ba2cfafac5750eb70a086a37a42aa0d24f994151c24b0fe7763133dc072890029

  // ABI 예시
  [
  	{
  		"constant":false,
  			"inputs":[{"name":"x","type":"uint256"}],
  			"name":"set",
  			"outputs":[],
  			"payable":false,
  			"stateMutability":"nonpayable",
  			"type":"function"
  	},
  	{
  	 	"constant":true,
  	 	"inputs":[],
  	 	"name":"get",
  	 	"outputs":[{"name":"","type":"uint256"}],
  	 	"payable":false,
  	 	"stateMutability":"view",
  	 	"type":"function"
  	}
  ]
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicPublishing/picture/complie2.png)

---
## 스마트 컨트랙트 배포
  ### 2. 스마트 컨트랙트 객체 생성
  - 실제 스마트 컨트랙트의 주소를 생성하는 것이 아니라 클래스를 정으한 것과 유사(즉, __객체__ 를 생성한다는 의미)
  - `eth.contract([ABI])` : geth 콘솔에서 제공하는 스마트 컨트랙트 객체를 생성하는 함수
    - ABI 는 위에서 생성한 ABI를 넣어주면 된다.
  - 객체를 만들면 아래 그림과 같이 나오게 된다.

  > 예시

  ```javascript
  simplestorageContract = eth.contract([{"constant":false,"inputs":[{"name":"x","type":"uint256"}],"name":"set","outputs":[],"payable":false,"stateMutability":"nonpayable","type":"function"},{"constant":true,"inputs":[],"name":"get","outputs":[{"name":"","type":"uint256"}],"payable":false,"stateMutability":"view","type":"function"}])
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicPublishing/picture/publish1.png)

  ### 3. 스마트 컨트랙트 배포
  - 생성한 스마트 컨트랙트 객체를 실제로 블럭에 포함시켜, 채굴될 수 있도록 하는 과정
  - `[스마트컨트랙트 객체].new([생성자], object)`
    - object는 [송금 예제](https://github.com/Lee-KyungSeok/Ethereum-Study/tree/master/BasicGeth) 참고
    - object 의 값 중에 `data` 영역에 remix 에서 만든 ByteCode를 넣도록 한다.
    - 참고> 생성자 예시 : `contract= CompiledContract.new( \_greeting, {from:[Address], data:[DATA]` ()
  - 스마트컨트렉트를 배포하게 되면 리턴값으로 abi, address, txid 를 리턴한다.

  > 예시 (예시 컨트랙트 코드는 생성자가 존재하지 않음)

  ```javascript
  simplestorage = simplestorageContract.new({
    from: eth.accounts[0],
    data: '0x608060405234801561001057600080fd5b5060df8061001f6000396000f3006080604052600436106049576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806360fe47b114604e5780636d4ce63c146078575b600080fd5b348015605957600080fd5b5060766004803603810190808035906020019092919050505060a0565b005b348015608357600080fd5b50608a60aa565b6040518082815260200191505060405180910390f35b8060008190555050565b600080549050905600a165627a7a72305820553ba2cfafac5750eb70a086a37a42aa0d24f994151c24b0fe7763133dc072890029'
  })
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicPublishing/picture/publish2.png)

  ### 4. 채굴 후 변화
  - 위 그림에서 Address 가 undefined 라는 것을 알 수 있다. 즉, 해당 트랜잭션이 아직 채굴되지 않은 상태라는 뜻
  - 채굴이 완료되면 `Address` 가 생기고 이 `Address` 를 이용하여 스마트 컨트랙트를 사용할 수 있게 된다.
    - 원래 채굴 후 만든 컨트랙트를 입력하면 나온다. (ex> simplestorage)
    - 그러나 버그가 가끔 발생하므로 `eth.getTransactionReceipt([TXID])` 에서 주소 확인하도록 한다. (작성하면서 버그떠서... 해시가 조금 다를 수 있음 참고 바람)

  ```javascript
  eth.getTransactionReceipt("0x8d10911325055ce21152b2011831cb4abd7de5e482987da3bc6a12642c5890f0")
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicPublishing/picture/publish3.png)

---
## 스마트 컨트랙트 사용
  ### 1. 컨트랙트 객체 생성
  - 만들어진 주소를 사용하여 새로운 스마트 컨트랙트 객체를 생성한다.
  - 이 객체를 이용하여 스마트 컨트랙트를 실행할 수 있다.
  - `eth.contract([ABI]).at([ADDRESS])`

  ```javascript
  myContract= eth.contract(abi).at("0x0e9b6363773af3042afaa7530bc2780495c1ad11")
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicPublishing/picture/publish4.png)

  ### 2. 함수 실행 (get)
  - dd

  ### 3. 함수 실행 (set)
  - dd
