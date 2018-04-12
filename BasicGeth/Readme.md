# Basic geth
  - geth 실행하기
  - 채굴하기
  - 계정 관리
  - 송금 하기

---
## geth 실행
  ### 1. geth console 실행
  - 아래 명령어를 입력
    - `--datadir [경로]` : 실행할 경로 지정
    - `--identity [이름]` : 내 프라이빗 노드의 아이덴티티
    - `--rpc` : RPC 인터페이스 사용 가능하도록 설정
    - `--rpcport [포트번호]` : RPC 포트 지정
    - `--rpccorsdomain [클라이언트]` : 접속 가능한 RPC 클라이언트 지정
      - 이 때 `*` 로 지정하면 모두 허용
      - 왠만하면 URL 을 지정해 주는 것이 보안상 좋다.
    - `--port "30303"` : 네트워크 Listening Port 지정 (30303 으로 지정된듯..)
    - `--nodiscover` : 같은 제네시스 블록과 네트워크 ID에 있는 블록에 연결 방지
      - 이를 하지 않는 다면 P2P 노드 연결을 위해 계속해서 ping 이 발생
    - `--rpcapi [API들]` : RPC에 의해 접근을 허락할 API 지정
      - 아래의 경우 db, eth, net, web3 를 지정함
    - `--networkid [네트워크id]` : 네트워크 identifier 설정
    - `console` : 콘솔모드 실행
  - 더 많은 명령어는 [Command Line Options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options) 참고
  - 뒤는 이제 이 console 을 실행한 후 진행!

  > 예시

  ```
  geth --datadir . --identity "Kyung" --rpc --rpcport "8080" --rpccorsdomain "*" --port "30303" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1999 console
  ```

  ### 2. geth attach 실행
  - node 에서 geth 가 실행되고 있을 경우 다른 터미널로 접속하여 json-rpc 을 이용할 수 있다.
  - `geth attach rpc:[주소]` : rpc로 접근
    - 아래에서는 `rpc 포트번호` 를 `8080` 으로 지정한 노드에 접근
  - 이렇게 하면 채굴하면서 코딩하기 편한 장점 존재!!

  > 예시

  ```
  geth attach rpc:8080
  ```

---

## 채굴
  ### 1. 마이닝 계정 지정
  - 마이닝 후 보상받을 계정 지정
  - `miner.setEtherbase(주소)` : 보상받을 계정 지정
    - `miner.setEtherbase(personal.listAccounts[index])` 를 이용하여 주소를 가져올 수 있다.
  - `eth.coinbase` : 보상받을 계정을 확인

  ```
  > miner.setEtherbase(personal.listAccounts[0])
  > eth.coinbase
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining.png)

  ### 2. 마이닝 실행
  - 현재 노드에서 마이닝을 실행
  - `miner.start()` : 마이닝을 실행
    - cf> `miner.start(3)` : 마이닝 쓰레드를 3개로 지정
  - 처음 시작 하게 되면 DAG 파일을 다운받게 된다.

  ```
  > miner.start()
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining2.png)

  ### 3. 마이닝 중지
  - 현재 진행중인 마이닝 중지
  - `miner.stop()` : 마이닝을 중지

  ```
  > miner.stop()
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining3.png)

  ### 4. 생성된 블록 수 조회
  - 현재까지 생성된 블록 번호를 리턴
  - `eth.blockNumber` : 생성된 블록 수 조회

  ```
  > eth.blockNumber
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining4.png)

  ### 5. 현재 채굴중인지 확인
  - 현재 채굴중이라면 true, 아니라면 false를 리턴
  - `eth.mining` : 현재 채굴중인지 확인

  ```
  > eth.mining
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining6.png)


  ### <참고>. DAG 파일 다운
  - 마이닝을 처음 하게 되면 이전에 DAG 를 생성하게 된다.
  - DAG 파일 이란?
    - 노드에 일관성이 유지되도록 도와주는 1GB 크기의 데이터셋
    - POW 알고리즘의 solution time을 표준화하는 방법

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining5.png)

---

## 계좌 관리
  ### 1. 새로운 계좌 생성
  - 이더를 송금하거나 계약을 실행할 수 있는 계좌 생성
  - `personal.newAccount()` : 계좌를 생성
  - cf> `geth account new` 를 이용하여 콘솔을 사용하지 않고도 말들 수 있다.

  ```
  > personal.newAccount()
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/account1.png)

  ### 2. 계좌 확인
  - 현재 보유하고 있는 계좌 주소를 확인할 수 있다.
  - `eth.accounts` or `personal.listAccounts` : 계좌 주소의 리스트를 리턴
    - `eth.account[0]` 과 같이 index를 주어 특정 계좌 주소 리턴 가능

  ```
  > eth.accounts
  > eth.accounts[0]
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/account2.png)

  ### 3. 잔고 확인
  - 생성한 게좌의 잔고를 확인할 수 있다.
  - `eth.getBalace(주소)`
    - 이 때 리턴값은 wei를 리턴하며 eth 로 확인하려면 이를 변환해주어야 한다.
    - `web3.fromWei(value)` 를 이용하여 eth로 변환가능

  ```
  eth.getBalace(eth.accounts[0])
  web3.fromWei(eth.getBalace(eth.accounts[0]))
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/account3.png)

  ### 4. 계좌 lock / unlcok
  - 송금 등을 진행 할때는 계정의 lock이 해제되어 있어야 사용 가능하다.
  - `personal.lockAccount(주소)` : 계정을 락시킴
  - `personal.unlockAccount(주소)` : 계정의 락을 해제
  - `personal.listWallets[index].status` : 특정 계정의 상태를 확인할 수 있다.
  - cf> `web3.personal.unlockAccount(주소)` 이렇게 써도 OK

  ```
  personal.lockAccount(eth.accounts[0])
  personal.unlockAccount(eth.accounts[0])
  personal.listWallets[0].status
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/account4.png)

---

## 송금하기
  ### 4. eth.
  - `eth.getBalace(주소)`
