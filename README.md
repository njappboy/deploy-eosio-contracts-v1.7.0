
# Deploy eosio.contracts v1.7.0 on EOS Mainnet


## Prerequisites

### 1. Prerequisites before building contracts

Use [eosio.cdt 1.6.2]([https://github.com/EOSIO/eosio.cdt/tree/v1.6.2](https://github.com/EOSIO/eosio.cdt/tree/v1.6.1)) and [eosio 1.8.3](https://github.com/EOSIO/eos/tree/v1.8.3) to build [eosio.contracts v1.7.0](https://github.com/EOSIO/eosio.contracts/tree/v1.7.0) smart contract.


#### 1.1 Prepare ICON_BASE_URL to serve action logos

eosio.contracts `v.1.7.0` introduced [ricardian-spec v0.2.0](https://github.com/EOSIO/ricardian-spec/tree/v0.2.0) which adds icon support in action ricardian clause. Along with this release, BlockOne team has also provided default icons for system contract actions under MIT license. In order to build an abi with accessible action icon url, you need to specify ICON_BASE_URL in contracts/CMakeLists.txt. 

in this case, we use `https://raw.githubusercontent.com/CryptoKylin/eosio-ricardian-icons/0.2.0/icons` as ICON_BASE_URL to build. 

Note: `eosio-ricardian-icons` is a repo maintained by CryptoKylin testnet developer group.


#### 1.2 Prepare Ricardian Clauses

Prepare ricardian caluses to make sure regproducer RC and EUA matches the ones on EOS Mainnet. You can find it in file: [contracts/eosio.system/ricardian/eosio.system.clauses.md](https://github.com/EOSLaoMao/eosio.contracts/blob/mainnet/v1.7.0/contracts/eosio.system/ricardian/eosio.system.clauses.md) The EUA section is taken from [EOS-Mainnet governance repo](https://github.com/EOS-Mainnet/governance/blob/master/eosio.system/eosio.system-clause-constitution-rc.md) and regproducer RC from [EOS42/regproduceupodate](https://github.com/eos42/regproduceupodate) 

You can find an v1.7.0 contract with RC updated here: [mainnet/v1.7.0](https://github.com/EOSLaoMao/eosio.contracts/mainnet/v1.7.0/) Please verify it accordingly.



### 2.Build and verify checksums:

##### Build:

clone [mainnet/v1.7.0](https://github.com/EOSLaoMao/eosio.contracts/mainnet/v1.7.0/) and build:

`./build.sh -c [eosio.cdt Path] -e [eosio Path]`

##### Verify checksums:

You can verify the build result under `build` folder once build succeed.

We have put our build under `contracts/1.7.0` for your review, here are checksums we got:

###### eosio.system:

```
openssl dgst -sha256 contracts/1.7.0/eosio.system/eosio.system.wasm
SHA256(contracts/1.7.0/eosio.system/eosio.system.wasm)= 2b10316f971a99ea99079a2104ddc95cc1bae2cddce80088e3c885875d82ac21
```

```
openssl dgst -sha256 contracts/1.7.0/eosio.system/eosio.system.abi
SHA256(contracts/1.7.0/eosio.system/eosio.system.abi)= ec59e8302839d3d161cc8d203eb5216dcacbd2ea29aae2788ccb7f6ff4596c92
```

###### eosio.token:

```
openssl dgst -sha256 contracts/1.7.0/eosio.token/eosio.token.wasm
SHA256(contracts/1.7.0/eosio.token/eosio.token.wasm)= f6a2939074d69fc194d4b7b5a4d2c24e2766046ddeaa58b63ddfd579a0193623
```

```
openssl dgst -sha256 contracts/1.7.0/eosio.token/eosio.token.abi
SHA256(contracts/1.7.0/eosio.token/eosio.token.abi)= 7ed892bbcd7dda2adde2e67e5a43e548530d40c0d7c86f1c950941d391aea7a9
```

###### eosio.msig:

```
openssl dgst -sha256 contracts/1.7.0/eosio.msig/eosio.msig.wasm
SHA256(contracts/1.7.0/eosio.msig/eosio.msig.wasm)= 70fe82b6a8302eba5ce4fe50083abe0d465201d8c0a2f8a3a4c4c092fb0a6570
```

```
openssl dgst -sha256 contracts/1.7.0/eosio.msig/eosio.msig.abi
SHA256(contracts/1.7.0/eosio.msig/eosio.msig.abi)= 3483d8eff9842efdd323d4a198c10c24429a51d656d2669ff83de77511869679
```


## Deploy eosio.system/eosio.token/eosio.msig contracts

### Prepare transaction

Take eosio.system for example:

Generate set code&abi transaction:

```
cleos -u https://api.eoslaomao.com set contract eosio contracts/1.7.0/eosio.system -p eosio -s -j -d > deploy_system.json
```

Update expiration to a future time, set `ref_block_num` and `ref_block_prefix` to 0, you will get a transaction like this:

```
{
  "expiration": "2019-10-19T07:28:47",
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

### Prepare Block Producer permission list

We have included top 30 BPs + some highly recoginized technical BPs among the community in our proposals, you can find the full list here in file `producer_perm.json`.


### Propose

We have proposed to deplpy eosio.system, eosio.token and eosio.msig contracts on EOS Mainnet, please review and verify.

1. Proposal to deploy eosio.system:

[https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=1sevensys](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=1sevensys)

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom 1sevensys
```


2. Proposal to deploy eosio.token:

[https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=1seventoken](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=1seventoken)

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom 1seventoken
```

3. Proposal to deploy eosio.msig:

[https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=1sevenmsig](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=1sevenmsig)

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom 1sevenmsig
```
