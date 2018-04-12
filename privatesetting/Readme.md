# Private Etherum 구축
  - Private Etherum 구축 방법
  - ㅇ

---

## Private Etherum 구축
  ### 0. 준비물
  - `Go Language` : [Go Language](https://golang.org/dl/) 를 다운
  - `Geth` : [go-Etherum](https://github.com/ethereum/go-ethereum) 홈페이지에서 각자에 맞는 버전 다운

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

  ![](https://github.com/Lee-KyungSeok/Etherum-Study/blob/master/privatesetting/picture/setting1.png)

  > 계좌 확인

  ```
  geth --datadir [경로] account list
  ```

  ![](https://github.com/Lee-KyungSeok/Etherum-Study/blob/master/privatesetting/picture/setting2.png)

  ### 2. genesis 블록을 위한 json 파일 생성
  - account 를 만든 디렉토리에 `json` 파일을 생성한 후 아래 내용을 넣어준다.
    - 여기서 `genesis.json` 으로 파일명을 만들었다.
    - 아래 내용 중 `[주소]` 에 위에서 account를 생성했을 때 나온 주소를 넣어준다. (`0x` 를 반드시 붙혀 16진수라는 것을 표시해야 함에 주의)

  > genesis.json

  ```json
  {
  	"config": {
  	"chainId": 0,
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
  - 블록체인의 Genesis 블록(맨 처음 블록) 을 아래 명령어를 통해 생성한다.
    - Genesis 블록 파일의 위치, 이름, 데이터저장 경로가 올바른지 확인한다.

  ```
  geth --datadir [주소명] init "[주소명]\genesis.json"
  ```

  ![](https://github.com/Lee-KyungSeok/Etherum-Study/blob/master/privatesetting/picture/setting3.png)
