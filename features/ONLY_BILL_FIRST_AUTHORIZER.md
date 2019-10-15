# ONLY_BILL_FIRST_AUTHORIZER feature test report

Along with v1.8 release, EOSIO introduced a set of protocal features that can be acitvated individually. You can check details here: [https://github.com/EOSIO/eos/releases/tag/v1.8.0](https://github.com/EOSIO/eos/releases/tag/v1.8.0)

This article is a test report on one specific feature of them: ONLY_BILL_FIRST_AUTHORIZER

NOTE: please make sure to test it on a `ONLY_BILL_FIRST_AUTHORIZER` activated EOSIO network.


## Brief

As decribed [here](https://github.com/EOSIO/eos/pull/7089), `ONLY_BILL_FIRST_AUTHORIZER`, when activated, the network will only bill CPU and NET to the first authorizer of the transaction instead of all authorizers.

What's the point of this feature? As we know EOSIO blockchains charge users NET and CPU costs on transactions like most other blackchains do. Although these costs are transient, it still affect user experiences in a big way. With `ONLY_BILL_FIRST_AUTHORIZER` activated, dApp developers can cover these costs on behalf of their users by setup a cosigning service to put their signature always on top in each of their users transaction.

As you can see, `ONLY_BILL_FIRST_AUTHORIZER` is a partial solution, a more complete solution is proposed here: [https://github.com/EOSIO/spec-repo/blob/master/esr_contract_pays.md](https://github.com/EOSIO/spec-repo/blob/master/esr_contract_pays.md)


## How to test ONLY_BILL_FIRST_AUTHORIZER feature

To test this feature, you just need to sign the transaction using PAYER's key and put this signature on top of any other required signatures.


In our case, we have user's EOS account `USER` and payer's EOS account `PAYER`. And we simply sign a transfer action to test it.


### 1. Generate transfer transaction payload

```
cleos transfer USER reciver "1 EOS" -p USER -s -j -d > trans.json
```

You will get a payload in `trans.json` like this:

```
{
  "expiration": "2019-10-15T06:02:59",
  "ref_block_num": 486,
  "ref_block_prefix": 2948179210,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio.token",
      "name": "transfer",
      "authorization": [{
          "actor": "USER",
          "permission": "active"
        }
      ],
      "data": "0000000084ab32dd0000000088ab32dd102700000000000004454f530000000000"
    }
  ],
  "transaction_extensions": [],
  "signatures": [],
  "context_free_data": []
}
```

Note that `signatures` is empty cause we used `-s` to skip signing.

Next:

1. Change `expiration` to a future time within 1 hour, e.g. 20 minutes later.
2. Add `PAYER` to `authorization` inside `actions` as the first authorizer.
3. Set both `ref_block_num` and `ref_block_prefix` to 0;

You will get a new `trans.json` like this:

```
{
  "expiration": "2019-10-15T06:22:59",
  "ref_block_num": 0,
  "ref_block_prefix": 0,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio.token",
      "name": "transfer",
      "authorization": [{
          "actor": "PAYER",
          "permission": "active"
        },{
          "actor": "USER",
          "permission": "active"
        }
      ],
      "data": "0000000084ab32dd0000000088ab32dd102700000000000004454f530000000000"
    }
  ],
  "transaction_extensions": [],
  "signatures": [],
  "context_free_data": []
}
```

Note that `expiration`, `ref_block_num` and `ref_block_prefix` has been updated, and new actor `PAYER` added as first `authorization`.


### 2. Sign transfer transaction payload

Now lets sign this transfer payload using both PAYER and BUYER's private key.

Sign with PAYER's private key:

```
cleos sign trans.json -k PRIVATE_KEY_OF_PAYER
```

You will get an output similar to this:

```
{
  "expiration": "2019-10-15T06:22:59",
  "ref_block_num": 0,
  "ref_block_prefix": 0,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio.token",
      "name": "transfer",
      "authorization": [{
          "actor": "BUYER",
          "permission": "active"
        },{
          "actor": "USER",
          "permission": "active"
        }
      ],
      "data": "0000000084ab32dd0000000088ab32dd102700000000000004454f530000000000"
    }
  ],
  "transaction_extensions": [],
  "signatures": [
    "SIG_K1_Jx4oc4Dt4tTRKJfx25wQPRnLnpU24kHAhaEQHoQkRt2RMyxP2Dycnm7xcuPCHmkoGSDDNKjFGcNQ4pNgi71pnxtR5eTckL"
  ],
  "context_free_data": []
}
```

Note that signature generated in `signatures`: `SIG_K1_Jx4oc4Dt4tTRKJfx25wQPRnLnpU24kHAhaEQHoQkRt2RMyxP2Dycnm7xcuPCHmkoGSDDNKjFGcNQ4pNgi71pnx`, this is the signature generated using PAYER's private key.

You can generate the USER's signaure using the same approach.

Put these two signatures back to `trans.json`, you will get a new `trans.json` with two valid signatures, similar to this:

```
{
  "expiration": "2019-10-15T06:22:59",
  "ref_block_num": 0,
  "ref_block_prefix": 0,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio.token",
      "name": "transfer",
      "authorization": [{
          "actor": "PAYER",
          "permission": "active"
        },{
          "actor": "USER",
          "permission": "active"
        }
      ],
      "data": "0000000084ab32dd0000000088ab32dd102700000000000004454f530000000000"
    }
  ],
  "transaction_extensions": [],
  "signatures": [
    "SIG_K1_Jx4oc4Dt4tTRKJfx25wQPRnLnpU24kHAhaEQHoQkRt2RMyxP2Dycnm7xcuPCHmkoGSDDNKjFGcNQ4pNgi71pnxtR5eTckL",
    "SIG_K1_KWKYahe6JYBevPntzHHrcCfQnj5vcDmW3Bsy2F3S7WA5Y1XzUgZFsL7di4FJjWd4zTJQiQUieJz8mbKAfiwRZ3QQZmP1Ds"
  ],
  "context_free_data": []
}
```


### 3. push transfer transaction payload

Before you push this transaction, please log the current resource state of both USER and PAYER account for later usage:

```
cleos get account USER > before
cleos get account PAYER >> before
```

Use this `trans.json` as payload, push a transaction:

```
cleos push transaction -s trans.json
```

If `expiration` and both signatures are valid, this transaction should succeed with a transaction id returned.

### 4. Check resource usage

Now lets log new resource state:


```
cleos get account USER > after
cleos get account PAYER >> after
```

Check the diff between `before` and `after`:

```
diff before after
```

You should be able to see that PAYER's NET and CPU usage accumulated, while USER's NET and CPU usage still the same.