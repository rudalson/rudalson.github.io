---
layout: post
title:  "Docker로 tendermint 실행"
categories: blockchain
tags: tendermint docker blockchain
---
tendermint 를 docker 에서 실행하기 위한 과정을 정리하였다.

# docker 에서 실행
## golang image container
```bash
~:>docker run -it --rm golang
root@5b612c38d7fd:/go#
root@5b612c38d7fd:/go# export REPO=github.com/tendermint/tendermint
root@5b612c38d7fd:/go# go get $REPO
package github.com/tendermint/tendermint: no Go files in /go/src/github.com/tendermint/tendermint
root@5b612c38d7fd:/go# ls
bin  src
root@5b612c38d7fd:/go# cd $GOPATH/src/$REPO
root@5b612c38d7fd:/go/src/github.com/tendermint/tendermint# make get_tools
--> Installing tools
./scripts/get_tools.sh
--> Installing mitchellh/gox (51ed453898ca5579fea9ad1f08dff6b121d9f2e8)...
Cloning into 'mitchellh/gox'...
remote: Enumerating objects: 1, done.
remote: Counting objects: 100% (1/1), done.
remote: Total 389 (delta 0), reused 0 (delta 0), pack-reused 388
Receiving objects: 100% (389/389), 106.04 KiB | 0 bytes/s, done.
Resolving deltas: 100% (208/208), done.
/go/src/github.com/mitchellh/gox /go/src/github.com
/go/src/github.com/mitchellh/gox
/go/src/github.com
--> Done

--> Installing golang/dep (22125cfaa6ddc71e145b1535d4b7ee9744fefff2)...
Cloning into 'golang/dep'...
remote: Enumerating objects: 20677, done.
remote: Total 20677 (delta 0), reused 0 (delta 0), pack-reused 20677
Receiving objects: 100% (20677/20677), 12.10 MiB | 1.22 MiB/s, done.
Resolving deltas: 100% (12265/12265), done.
/go/src/github.com/golang/dep /go/src/github.com
/go/src/github.com/golang/dep
/go/src/github.com
--> Done

--> Installing alecthomas/gometalinter (17a7ffa42374937bfecabfb8d2efbd4db0c26741)...
Cloning into 'alecthomas/gometalinter'...
remote: Enumerating objects: 10, done.
remote: Counting objects: 100% (10/10), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 4444 (delta 2), reused 4 (delta 1), pack-reused 4434
Receiving objects: 100% (4444/4444), 5.50 MiB | 1.32 MiB/s, done.
Resolving deltas: 100% (1651/1651), done.
/go/src/github.com/alecthomas/gometalinter /go/src/github.com
/go/src/github.com/alecthomas/gometalinter
/go/src/github.com
--> Done

--> Installing gogo/protobuf (61dbc136cf5d2f08d68a011382652244990a53a9)...
Cloning into 'gogo/protobuf'...
remote: Enumerating objects: 2374, done.
remote: Counting objects: 100% (2374/2374), done.
remote: Compressing objects: 100% (974/974), done.
remote: Total 27489 (delta 1707), reused 1923 (delta 1389), pack-reused 25115
Receiving objects: 100% (27489/27489), 35.38 MiB | 2.92 MiB/s, done.
Resolving deltas: 100% (19505/19505), done.
/go/src/github.com/gogo/protobuf /go/src/github.com
/go/src/github.com/gogo/protobuf
/go/src/github.com
--> Done

--> Installing square/certstrap (e27060a3643e814151e65b9807b6b06d169580a7)...
Cloning into 'square/certstrap'...
remote: Enumerating objects: 215, done.
remote: Counting objects: 100% (215/215), done.
remote: Compressing objects: 100% (136/136), done.
remote: Total 1387 (delta 74), reused 157 (delta 69), pack-reused 1172
Receiving objects: 100% (1387/1387), 2.98 MiB | 505.00 KiB/s, done.
Resolving deltas: 100% (500/500), done.
/go/src/github.com/square/certstrap /go/src/github.com
/go/src/github.com/square/certstrap
/go/src/github.com
--> Done

root@5b612c38d7fd:/go/src/github.com/tendermint/tendermint# make get_vendor_deps
--> Running dep
root@5b612c38d7fd:/go/src/github.com/tendermint/tendermint# make install
CGO_ENABLED=0 go install  -ldflags "-X github.com/tendermint/tendermint/version.GitCommit=`git rev-parse --short=8 HEAD`" -tags 'tendermint' ./cmd/tendermint
root@5b612c38d7fd:/go/src/github.com/tendermint/tendermint#
root@5b612c38d7fd:/go/src/github.com/tendermint/tendermint# cd
root@5b612c38d7fd:~# ll
bash: ll: command not found
root@5b612c38d7fd:~# ls
root@5b612c38d7fd:~# tendermint init
I[6016-01-06|02:58:06.834] Generated private validator                  module=main path=/root/.tendermint/config/priv_validator.json
I[6016-01-06|02:58:06.836] Generated node key                           module=main path=/root/.tendermint/config/node_key.json
I[6016-01-06|02:58:06.837] Generated genesis file                       module=main path=/root/.tendermint/config/genesis.json
root@5b612c38d7fd:~# tendermint node --proxy_app=kvstore
I[6016-01-06|02:58:57.381] Version info                                 module=main software=0.27.4 block=8 p2p=5
I[6016-01-06|02:58:57.418] Starting Node                                module=main impl=Node
I[6016-01-06|02:58:57.422] Started node                                 module=main nodeInfo="{ProtocolVersion:{P2P:5 Block:8 App:1} ID_:dfbfd339d73005f422a68f9c66f4db9e29b63cb5 ListenAddr:tcp://0.0.0.0:26656 Network:test-chain-yBHf0N Version:0.27.4 Channels:4020212223303800 Moniker:5b612c38d7fd Other:{TxIndex:on RPCAddress:tcp://0.0.0.0:26657}}"
E[6016-01-06|02:58:57.422] Couldn't connect to any seeds                module=p2p
I[6016-01-06|02:58:58.432] Executed block                               module=state height=1 validTxs=0 invalidTxs=0
I[6016-01-06|02:58:58.435] Committed state                              module=state height=1 txs=0 appHash=0000000000000000
I[6016-01-06|02:58:59.455] Executed block                               module=state height=2 validTxs=0 invalidTxs=0
I[6016-01-06|02:58:59.459] Committed state                              module=state height=2 txs=0 appHash=0000000000000000
I[6016-01-06|02:59:00.474] Executed block                               module=state height=3 validTxs=0 invalidTxs=0
I[6016-01-06|02:59:00.477] Committed state                              module=state height=3 txs=0 appHash=0000000000000000
^CE[6016-01-06|02:59:01.166] captured interrupt, exiting...               module=main
I[6016-01-06|02:59:01.166] Stopping Node                                module=main impl=Node
I[6016-01-06|02:59:01.166] Stopping Node                                module=main
E[6016-01-06|02:59:01.169] Not stopping BlockPool -- have not been started yet module=blockchain impl=BlockPool
I[6016-01-06|02:59:01.169] Closing rpc listener                         module=main listener="&{Listener:0xc00000c5e8 sem:0xc00007a5a0 closeOnce:{m:{state:0 sema:0} done:0} done:0xc00007a600}"
root@5b612c38d7fd:~#
```

### 요약
```bash
docker run -it --rm golang
export REPO=github.com/tendermint/tendermint
go get $REPO
cd $GOPATH/src/$REPO
make get_tools
make get_vendor_deps
make install
tendermint init
tendermint node --proxy_app=kvstore
```

```bash
#!/usr/bin/env bash

# XXX: this script is meant to be used only on a fresh Ubuntu 16.04 instance
# and has only been tested on Digital Ocean

# get and unpack golang
curl -O https://storage.googleapis.com/golang/go1.10.linux-amd64.tar.gz
tar -xvf go1.10.linux-amd64.tar.gz

apt install make

## move go and add binary to path
mv go /usr/local
echo "export PATH=\$PATH:/usr/local/go/bin" >> ~/.profile

## create the GOPATH directory, set GOPATH and put on PATH
mkdir goApps
echo "export GOPATH=/root/goApps" >> ~/.profile
echo "export PATH=\$PATH:\$GOPATH/bin" >> ~/.profile

source ~/.profile

## get the code and move into it
REPO=github.com/tendermint/tendermint
go get $REPO
cd $GOPATH/src/$REPO

## build
git checkout master
make get_tools
make get_vendor_deps
make install
```

## tendermint single container
```bash
~:> docker run -it --rm -v "/Users/kmpak/tmp/tendermint1:/tendermint" tendermint/tendermint init
Unable to find image 'tendermint/tendermint:latest' locally
latest: Pulling from tendermint/tendermint
ff3a5c916c92: Pull complete
3c63001644b1: Pull complete
b928d43effa5: Pull complete
Digest: sha256:6a6691c448554392ab05c0b5754489e02628d9539d3887458bdc4f69251697cd
Status: Downloaded newer image for tendermint/tendermint:latest
I[7016-01-07|00:58:03.880] Generated private validator                  module=main path=/tendermint/config/priv_validator.json
I[7016-01-07|00:58:03.882] Generated node key                           module=main path=/tendermint/config/node_key.json
I[7016-01-07|00:58:03.884] Generated genesis file                       module=main path=/tendermint/config/genesis.json
~:> docker run -it -p 26657:26657 --rm -v "/Users/kmpak/tmp/tendermint1:/tendermint" tendermint/tendermint node --proxy_app=kvstore
I[7016-01-07|00:58:57.859] Version info                                 module=main software=0.27.4 block=8 p2p=5
I[7016-01-07|00:58:57.890] Starting Node                                module=main impl=Node
I[7016-01-07|00:58:57.899] Started node                                 module=main nodeInfo="{ProtocolVersion:{P2P:5 Block:8 App:1} ID_:1a164d4db5c654ed19f0774f082911cd5e6b05a2 ListenAddr:tcp://0.0.0.0:26656 Network:test-chain-iqFzFK Version:0.27.4 Channels:4020212223303800 Moniker:b0d655eddf54 Other:{TxIndex:on RPCAddress:tcp://0.0.0.0:26657}}"
E[7016-01-07|00:58:57.899] Couldn't connect to any seeds                module=p2p
I[7016-01-07|00:58:58.898] Executed block                               module=state height=1 validTxs=0 invalidTxs=0
I[7016-01-07|00:58:58.899] Committed state                              module=state height=1 txs=0 appHash=0000000000000000
I[7016-01-07|00:58:59.914] Executed block                               module=state height=2 validTxs=0 invalidTxs=0
I[7016-01-07|00:58:59.915] Committed state                              module=state height=2 txs=0 appHash=0000000000000000
I[7016-01-07|00:59:00.928] Executed block                               module=state height=3 validTxs=0 invalidTxs=0
I[7016-01-07|00:59:00.928] Committed state                              module=state height=3 txs=0 appHash=0000000000000000
I[7016-01-07|00:59:01.943] Executed block                               module=state height=4 validTxs=0 invalidTxs=0
I[7016-01-07|00:59:01.944] Committed state                              module=state height=4 txs=0 appHash=0000000000000000
I[7016-01-07|00:59:02.956] Executed block                               module=state height=5 validTxs=0 invalidTxs=0
I[7016-01-07|00:59:02.957] Committed state                              module=state height=5 txs=0 appHash=0000000000000000
I[7016-01-07|00:59:03.974] Executed block                               module=state height=6 validTxs=0 invalidTxs=0
I[7016-01-07|00:59:03.975] Committed state                              module=state height=6 txs=0 appHash=0000000000000000
I[7016-01-07|00:59:04.989] Executed block                               module=state height=7 validTxs=0 invalidTxs=0
I[7016-01-07|00:59:04.990] Committed state                              module=state height=7 txs=0 appHash=0000000000000000
I[7016-01-07|00:59:06.006] Executed block                               module=state height=8 validTxs=0 invalidTxs=0
I[7016-01-07|00:59:06.007] Committed state                              module=state height=8 txs=0 appHash=0000000000000000
```

## tendermint multi container
### init
user 디렉토리 밑에 각컨테이너의 설정을 담기위해 tendermint1~tendermint3 까지 디렉토리를 생성한다. 이후 tendermint init 으로 default 설정 및 data 디렉토리를 생성한다.
```
docker run -it --rm -v "/Users/kmpak/tmp/tendermint1:/tendermint" tendermint/tendermint init
docker run -it --rm -v "/Users/kmpak/tmp/tendermint2:/tendermint" tendermint/tendermint init
docker run -it --rm -v "/Users/kmpak/tmp/tendermint3:/tendermint" tendermint/tendermint init

docker run -it --rm -v "/Users/kmpak/tmp/tendermint1:/tendermint" tendermint/tendermint show_node_id
docker run -it --rm -v "/Users/kmpak/tmp/tendermint2:/tendermint" tendermint/tendermint show_node_id
docker run -it --rm -v "/Users/kmpak/tmp/tendermint3:/tendermint" tendermint/tendermint show_node_id
```
init 이후 show_node_id 명령어로 노드에 대한 address( ?) 값을 구해온다. 이걸로 아래의 config 파일에 입력하기 위해서다.


### config
tendermint1 ~ 3 까지 config 디렉토리에서 몇몇 설정을 맞춰준다.

`config.toml`
```
moniker = "tm1"
...


[rpc]


# TCP or UNIX socket address for the RPC server to listen on
laddr = "tcp://0.0.0.0:26657"
...


persistent_peers = "d62c6985171a7121a9cff42975f5e57d30c3d4f6@tm1:26656,3ec7e8d3780c654f177c9cd4c87907bc391fb055@tm2:26656,444b7ce1fa4eb64265df62bd6ab16a50bbc1b927@tm3:26656"
```

`genesis.json`
```json
{
  "genesis_time": "2019-01-11T04:52:40.600691343Z",
  "chain_id": "tm-chain",
  "consensus_params": {
    "block_size": {
      "max_bytes": "22020096",
      "max_gas": "-1"
    },
    "evidence": {
      "max_age": "100000"
    },
    "validator": {
      "pub_key_types": [
        "ed25519"
      ]
    }
  },
  "validators": [
    {
      "address": "CBF99C9ED817A86663A9741D24283EBEC446F8DF",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "kHSRLUsAmAjdPqP4EU7dipQoAAXABWIw2yMcM6731GA="
      },
      "power": "10",
      "name": "tmnode1"
    },
    {
      "address": "22BFED324E1F8CD3021669D557101D9891180ADD",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "C4KlwOzmL2IBMq5/U0Qj3M5IEP4MaM+IwRqjEewJKsM="
      },
      "power": "1",
      "name": "tmnode2"
    },
    {
      "address": "0C232C5EE2246807A7FCE36FA0C1C24739D494FC",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "j5JRtifiVtj7duuJZd42VjBhg8L7Vu8O8umopaJfags="
      },
      "power": "1",
      "name": "tmnode3"
    }
  ],
  "app_hash": ""
```
이 부분은 tendermint 설정을 어느정도 이해하고 있어야 하는데 docker 를 떠나서 멀티노드 구성에 대한 설정을 이해해야 할 수 있는 작업이다.

같은 Network에서 통신하는 node들은 같은 `genesis.json` 파일을 참조해야한다. 

### user-defined bridge networks
멀티노드 설정 작업이 끝나면 이후 docker container 간 원활한 네트웍 통신을 위해 network group 를 만든다. [Networking with standalone containers](https://docs.docker.com/network/network-tutorial-standalone/) 참조
```bash
docker network create --driver bridge tmnet
```

### run
config과 네트웍 그룹으로 실행을 시킨다. port는 컨테이너 안에서는 모두 `26657` port 이겠지만 host에서 달리 접근하기 위해서는 `26617`, `26627`, `26637` 로 매핑시켰다.
```
docker run -it -p 26617:26657 -h tm1 --name tm1 --network tmnet --rm -v "/Users/kmpak/tmp/tendermint1:/tendermint" tendermint/tendermint node --proxy_app=kvstore

docker run -it -p 26627:26657 -h tm2 --name tm2 --network tmnet --rm -v "/Users/kmpak/tmp/tendermint2:/tendermint" tendermint/tendermint node --proxy_app=kvstore

docker run -it -p 26637:26657 -h tm3 --name tm3 --network tmnet --rm -v "/Users/kmpak/tmp/tendermint3:/tendermint" tendermint/tendermint node --proxy_app=kvstore
```

### transaction 발생
host terminal 에서 트랜잭션을 날려보면 된다. 가령 tm1 에 붙고 싶을 때는 `26617` 로..... tm3에 붙고 싶으면 `26637`로 접근하면 된다.
```bash
~:> curl -s localhost:26627/status
{
  "jsonrpc": "2.0",
  "id": "",
  "result": {
    "node_info": {
      "protocol_version": {
        "p2p": "5",
        "block": "8",
        "app": "1"
      },
      "id": "3ec7e8d3780c654f177c9cd4c87907bc391fb055",
      "listen_addr": "tcp://0.0.0.0:26656",
      "network": "tm-chain",
      "version": "0.27.4",
      "channels": "4020212223303800",
      "moniker": "tm2",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://0.0.0.0:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "62FD6EEA733C6049C2E0E2CB0A22A093A1A9BA19F30214ED5D0E79305A3FD522",
      "latest_app_hash": "0000000000000000",
      "latest_block_height": "2",
      "latest_block_time": "2019-01-11T05:03:48.581552326Z",
      "catching_up": false
    },
    "validator_info": {
      "address": "22BFED324E1F8CD3021669D557101D9891180ADD",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "C4KlwOzmL2IBMq5/U0Qj3M5IEP4MaM+IwRqjEewJKsM="
      },
      "voting_power": "1"
    }
  }
~:> curl -s 'localhost:26617/abci_query?data="abcd"'
{
  "jsonrpc": "2.0",
  "id": "",
  "result": {
    "response": {
      "log": "exists",
      "key": "YWJjZA==",
      "value": "YWJjZA=="
    }
  }
}
```

### 마무리 - 뒷정리
docker container를 모두 내렸다면 생성했던 네트웍 그룹을 제거해준다.
```
docker network rm tmnet
```
