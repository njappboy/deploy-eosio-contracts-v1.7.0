# ONLY_LINK_TO_EXISTING_PERMISSION feature

Along with v1.8 release, EOSIO introduced a set of protocal features that can be acitvated individually. You can check details here: [https://github.com/EOSIO/eos/releases/tag/v1.8.0](https://github.com/EOSIO/eos/releases/tag/v1.8.0)

This article is on one specific feature of them: `ONLY_LINK_TO_EXISTING_PERMISSION`

## Brief

As decribed in issue [#6831](https://github.com/EOSIO/eos/pull/6831) and [#6333](https://github.com/EOSIO/eos/issues/6333), The eosio::linkauth native action was meant to disallow linking an action to a non-existing permission. Due to a bug in apply_eosio_linkauth, it is possible for a user to link an action to a non-existing permission as long as a permission with the same name exists within some account.

There are no critical security issues due to this bug. A user has no incentive to link an action to a non-existing permission. If they do so anyway, for example by accident, the result would be that the linked action could not be authorized by that account until it was unlinked. To unlink, the non-existing permission would first need to be created.

Nevertheless, it is desirable to fix this bug to avoid the above inconvenience due to an accident. A subjective mitigation against this could first be deployed (this is tracked in [#6290](https://github.com/EOSIO/eos/issues/6290) before the objective fix is activated as part of a consensus upgrade feature.

## How to test it

Prepare 3 accounts, account A, B and C.

First, create a customized permission called 'perma' on account A:

```
cleos set account permission A perma '{"threshold": 1, "keys": [], "accounts": [{"weight": 1, "permission": {"actor": "A", "permission": "active"}}]}' -p A
```

For account B, try to set `eosio.token`'s `transfer` action's permission to a non-existing permission B@perma. Note that `perma` is the same permission name on account A.

```
cleos set action permission B eosio.token transfer perma -p B
```

Before `ONLY_LINK_TO_EXISTING_PERMISSION` activated, the prior operation will be executed successfully due to it only check the existance of permission name without checking the actual account has this permission or not.

After `ONLY_LINK_TO_EXISTING_PERMISSION` activated, try the prior operation on account C, you will get an error like this:

```
Error 3090005: Irrelevant authority included
Please remove the unnecessary authority from your action!
Error Details:
the owner of the linked permission needs to be the actor of the declared authorization
```

Note: After `ONLY_LINK_TO_EXISTING_PERMISSION` activated, accounts already have non-existing permissions set before activation will not be affected. They only need to create these permissions first before they can use it.

## Proposal on EOS Mainnet

EOSLaoMao Team has proposed to activate `ONLY_LINK_TO_EXISTING_PERMISSION` on EOS Mainnet.

Special thanks to EOS Nation and MeetOne Team.

We have included top 35 BPs + 13 highly recoginized technical/governace BPs among the community in our proposals, you can find the full list in file `features/ONLY_LINK_TO_EXISTING_PERMISSION/producer_perm.json`

The payload used for this proposal can be found in `features/ONLY_LINK_TO_EXISTING_PERMISSION/payload.json`

To verify this payload, simply:

```
cleos -u https://api.eoslaomao.com push action eosio activate '{"feature_digest": "1a99a59d87e06e09ec5b028a9cbb7749b4a5ad8819004365d02dc4379a8b7241"}' -p eosio -s -j -d > activate.json
```

The `feature_digest` of ONLY_LINK_TO_EXISTING_PERMISSION is `1a99a59d87e06e09ec5b028a9cbb7749b4a5ad8819004365d02dc4379a8b7241`, it could be found via `curl -X POST http://localhost:8888/v1/producer/get_supported_protocol_features -d '{}'` on your BP node with producer api plugin enabled as [described here](https://developers.eos.io/eosio-nodeos/docs/consensus-protocol-upgrade-process#section-special-notes-to-block-producers)

The only difference between `activate.json` you generated and `features/ONLY_LINK_TO_EXISTING_PERMISSION/payload.json` should be `expiration`, `ref_block_num` and `ref_block_prefix`.

Please review and verify this proposal ASAP: [https://bloks.io/msig/eoslaomaocom/onlylnkexist](https://bloks.io/msig/eoslaomaocom/onlylnkexist)
