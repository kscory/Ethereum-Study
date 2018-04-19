# Private Etherum 구축
  - Private Etherum 구축 방법

---

## 들어가기 전에
  ### 1. 준비물
  - `Go Language` : [Go Language](https://golang.org/dl/) 를 다운
  - `Geth` : [geth_git 버전](https://github.com/ethereum/go-ethereum) 혹은 [geth 홈페이지](https://geth.ethereum.org/downloads/) 에서 각자의 OS 맞는 파일 다운

  ### 2. 프라이빗 블록체인 구성 시 생각해야 할 점
  - 커스텀 제네시스 파일 설정을 통해 최초의 이더리움 블록 생성
  - 커스텀 데이터 디렉토리 설정을 통해 블록체인 스토리지 구성
  - 커스템 네트워크 ID 설정을 통해 내가 구축한 프라이빗 블록체인 명명하기
  - 프라이빗 네트워크에서 이용시 추천 사항으로 다른 노드와 연결하기 위해 자동으로 탐색하는 것을 방지하기

## Private Etherum 구축 및 geth 실행
  ### 1. Geth 클라이언트 전용 계좌 생성
  - 네트워크를 구축할 디렉토리에 `geth` 파일과 `attach.exe` 파일을 복사한 후 아래 명령어를 실행 (`.` 이라는 것은 현재 디렉토리를 의미)
    - 그 후 account에 사용할 password 입력
    - 그러면 account 주소를 반환해준다.
    - 참고 : 처음 계좌를 생성하면 Eth 를 25 개 만큼 받은 것을 알 수 있다.
  - 그 후 account 생성이 되었는지 확인 가능

  > 계좌 생성

  ```
  geth --datadir [경로] account new
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/privatesetting/picture/setting1.png)

  > 계좌 확인

  ```
  geth --datadir [경로] account list
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/privatesetting/picture/setting2.png)

  ### 2. genesis 블록을 위한 설정 파일 생성
  - account 를 만든 디렉토리에 `json` 파일을 생성한 후 아래 내용을 넣어준다. (여기서 `genesis.json` 으로 파일명을 만들었다. )
  - __버전마다 계속 바뀌니 참고 할 것__
  - `config` : Geth1.6 이전 버전에서 접속 시 발생하는 문제를 해결하기 위한 설정
    - `chainId` 의 경우 "0" 으로 세팅할 경우 송금시에 오류 발생 가능성 있으니 사용하지 않도록 한다.
  - `coinbase` : 마이닝 작업 수행 후 보상을 받을 어카운트 주소
    - 대부분 마이너가 coinbase 값을 설정하기 때문에 어떤 값을 설정해도 무방
  - `difficulty` : 마이닝 계산 시의 목표값 (난이도)
  - `extraData` : 옵션항목, 최대 32바이트의 공간
  - `gasLimit` : 현재 블록체인에서 블록당 가스 지출의 최댓값
  - `nonce` : 믹스해시와 함께 블록 생성을 위한 마이닝 작업에 사용되는 64비트 해시
  - `mixhash` : 넌스와 함께 사용하여 마이닝 작업 시 충분한 양의 계산을 수행되도록 하는 256 비트 해시
  - `parentHash` : 부모 블럭 헤더의 Keccak256 암호 해시값
    - genesis 블록은 부모가 없으므로 값이 0
  - `timestamp` : 해당 블록의 생성 시간 (유닉스의 time() 함수 값)
    - 블록 생성시 난이도 조절에 사용됨
  - `alloc` : 일정 양의 이더를 특정 어카운트에 미리 할당
    - 이더를 미리 발행하고 사전 판매하기 위해 사용

  > genesis.json

  ```json
  {
  	"config": {
  	"chainId": 20,
  	"homesteadBlock": 0,
  	"eip155Block": 0,
  	"eip158Block": 0
  	},
  	"coinbase" : "0x0000000000000000000000000000000000000000",
  	"difficulty" : "0x02",
  	"extraData" : "",
  	"gasLimit" : "0x2fefd8",
  	"nonce" : "0x0000000000000042",
  	"mixhash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  	"parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
  	"timestamp" : "0x00",
  	"alloc": {
  	"0x[주소]": {"balance": "0xffffffffffffffff"}
  	}
  }
  ```

  ### 3. Genesis 블록 생성
  - 블록체인의 Genesis 블록(최초 블록) 을 아래 명령어를 통해 생성한다.
    - Genesis 블록 파일의 위치, 이름, 데이터저장 경로가 올바른지 확인한다.

  ```
  geth --datadir [주소명] init "[주소명]\genesis.json"
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/privatesetting/picture/setting3.png)

---
