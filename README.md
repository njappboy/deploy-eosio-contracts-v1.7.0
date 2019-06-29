
# Deploy eosio.contracts v1.7.0 on Kylin Testnet


## Prerequisites

### 1. Build contracts

Use [eosio.cdt 1.6.1]([https://github.com/EOSIO/eosio.cdt/tree/v1.6.1](https://github.com/EOSIO/eosio.cdt/tree/v1.6.1)) and [eosio 1.8.0](https://github.com/EOSIO/eos/tree/v1.8.0) to build [eosio.contracts v1.7.0](https://github.com/EOSIO/eosio.contracts/tree/v1.7.0) smart contract.

Note: eosio.contracts v.17.0 introduced [ricardian-spec v0.2.0](https://github.com/EOSIO/ricardian-spec/tree/v0.2.0) which adds logos in action ricardian contracts. along with this release, BlockOne team provided default logos for system contract actions under MIT license. In order to build an abi with accessible action icon url, you need to specify ICON_BASE_URL in contracts/CMakeLists.txt. 

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

## Deploy eosio.system contract

### Prepare deploy transaction and propose

generate set code&abi transaction:

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

We have proposed this transaction on Kylin Testnet : [https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploy1seven](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deploy1seven), please review and verify ASAP. 

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom deploy1seven
```
