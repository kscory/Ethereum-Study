# Basic geth
  - geth 실행하기
  - 계정 관리
  - mining 실습

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

  > 예시

  ```
  geth --datadir . --identity "Kyung" --rpc --rpcport "8080" --rpccorsdomain "*" --port "30303" --nodiscover --rpcapi "db,eth,net,web3" --networkid 1999 console
  ```
---

## 채굴하기
  ### 1. 마이닝 계정 지정
  - `miner.setEtherbase(주소)` 명령어 입력
  - `miner.setEtherbase(personal.listAccounts[index])` 를 이용하여 주소를 가져올 수 있다.

  ```
  > miner.setEtherbase(personal.listAccounts[0])
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining.png)

  ### 2. 마이닝 실행
  - `miner.start()` 명령어 입력
    - cf> `miner.start(3)` : 마이닝 쓰레드를 3개로 지정
  - 처음 시작 하게 되면 DAG 파일을 다운받게 된다.

  ```
  > miner.start()
  ```

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining2.png)

  ### 3. 마이닝 중지
  - `miner.stop()` 명령어 입력

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining3.png)

  ```
  > miner.stop()
  ```

  ### 4. DAG 파일 다운
  - 마이닝을 처음 하게 되면 이전에 DAG 를 생성하게 된다.
  - DAG 파일 이란?
    - 노드에 일관성이 유지되도록 도와주는 1GB 크기의 데이터셋
    - POW 알고리즘의 solution time을 표준화하는 방법

  ![](https://github.com/Lee-KyungSeok/Ethereum-Study/blob/master/BasicGeth/picture/mining4.png)

---
