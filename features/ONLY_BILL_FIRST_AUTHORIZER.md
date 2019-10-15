# ONLY_BILL_FIRST_AUTHORIZER feature test report

Along with EOSIO v1.8 release, it introduced a set of protocal features that can be acitvated individually. You can check details here: [https://github.com/EOSIO/eos/releases/tag/v1.8.0](https://github.com/EOSIO/eos/releases/tag/v1.8.0)

This article is a test report on one feature of them: ONLY_BILL_FIRST_AUTHORIZER


## Brief

As decribed [here](https://github.com/EOSIO/eos/pull/7089) ONLY_BILL_FIRST_AUTHORIZER, when activated, only bills CPU and NET to the first authorizer of the transaction instead of all authorizers.

What's the point of this feature? As we know EOSIO blockchains charge users NET and CPU costs on transactions like most other blackchains do. Although these costs are transient, it still affect user experiences in a big way. With ONLY_BILL_FIRST_AUTHORIZER, dapp developers can cover these costs on be-half of their users by setup a cosigning service to put their signature always on top in each of their users transaction.

As you can see, ONLY_BILL_FIRST_AUTHORIZER is a partial solution, a more complete solution is proposed here: [https://github.com/EOSIO/spec-repo/blob/master/esr_contract_pays.md](https://github.com/EOSIO/spec-repo/blob/master/esr_contract_pays.md)


## Test ONLY_BILL_FIRST_AUTHORIZER 

To test this feature, you just need to sign the transaction using PAYER's key and put this signature on top of any other signatures.


In our case, we have user's EOS account USER and payer's EOS account PAYER. And we simply sign a transfer action to test it.


### 1. Generate transfer transaction payload:

```
cleos system transfer USER reciver "1 EOS" -p USER -s -j -d > trans.json
```

You will get a payload in `trans.json` like this:

```
```

