
# Deploy eosio.contracts v1.7.0 on Kylin Testnet


## Prerequisites

### 1. Build contracts

Use [eosio.cdt 1.6.1]([https://github.com/EOSIO/eosio.cdt/tree/v1.6.1](https://github.com/EOSIO/eosio.cdt/tree/v1.6.1)) and [eosio 1.8.0](https://github.com/EOSIO/eos/tree/v1.8.0) to build [eosio.contracts v1.7.0](https://github.com/EOSIO/eosio.contracts/tree/v1.7.0) smart contract.

Note: eosio.contracts v.1.7.0 introduced [ricardian-spec v0.2.0](https://github.com/EOSIO/ricardian-spec/tree/v0.2.0) which adds icon support in action ricardian clause. Along with this release, BlockOne team also provided default icons for system contract actions under MIT license. In order to build an abi with accessible action icon url, you need to specify ICON_BASE_URL in contracts/CMakeLists.txt. 

in our case, we use http://ricardian-icons.cryptokylin.io/ as ICON_BASE_URL to build.

#### Checksums:

##### eosio.system:

```
openssl dgst -sha256 eosio.system/eosio.system.wasm
SHA256(eosio.system/eosio.system.wasm)= 6fcd8ef118799f21156acb0e24bcbd66d1dde40e5038fea938aa6cb03ed1505e
```

```
openssl dgst -sha256 eosio.system/eosio.system.abi
SHA256(eosio.system/eosio.system.abi)= 658918579a218447ee4117e25ee124a49e94cad8dc1ab53411c12ef5ce3da64c
```

#### eosio.token:

```
openssl dgst eosio.token/eosio.token.wasm
SHA256(eosio.token/eosio.token.wasm)= f6a2939074d69fc194d4b7b5a4d2c24e2766046ddeaa58b63ddfd579a0193623
```

```
openssl dgst eosio.token/eosio.token.abi
SHA256(eosio.token/eosio.token.abi)= 358aaa14364f5a8726b22a0b24dbd2ba3bd8ff3be9f1604e0f33a4cd709416f0
```

#### eosio.msig:

```
openssl dgst eosio.msig/eosio.msig.wasm
SHA256(eosio.msig/eosio.msig.wasm)= 07e77ebac8d32df58cc182bcfee218a93ccefb2838f23a8ed1593830d98d90db
```

```
openssl dgst eosio.msig/eosio.msig.abi
SHA256(eosio.msig/eosio.msig.abi)= 25c3b222606962cf0dca3f66988c0d539111c0c44e19debdbaace34e47dce3b6
```


## Deploy eosio.system/eosio.token/eosio.msig contracts

### Prepare transaction

Take eosio.system for example:

Generate set code&abi transaction:

```
cleos -u https://api-kylin.eoslaomao.com set contract eosio eosio.system -p eosio -s -j -d > deploy_system.json
```

Update expiration to a future time, set `ref_block_num` and `ref_block_prefix` to 0, you will get a transaction like this:

```
{
  "expiration": "2019-07-19T07:28:47",
  "ref_block_num": 0,
  "ref_block_prefix": 0,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio",
      "name": "setcode",
      "authorization": [{
          "actor": "eosio",
          "permission": "active"
        }
      ],
      "data": "......",
    },{
      "account": "eosio",
      "name": "setabi",
      "authorization": [{
          "actor": "eosio",
          "permission": "active"
        }
      ],
      "data": "......"
    }
  ],
  "transaction_extensions": [],
  "signatures": [],
  "context_free_data": []
}
```

Use the same approach above to generate transaction payload for eosio.token and eosio.msig. Don't forget to use eosio.token and eosio.msig for authority instead.

We have proposed to deplpy eosio.system, eosio.token and eosio.msig contracts on Kylin Testnet, please review and verify ASAP. 

1. deploy eosio.system proposal:

[https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploy1seven](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploy1seven)

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom deploy1seven
```


2. deploy eosio.token proposal:

[https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploytoken](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploytoken)

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom deploytoken
```

3. deploy eosio.msig proposal:

[https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploymsig](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploymsig)

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom deploymsig
```
