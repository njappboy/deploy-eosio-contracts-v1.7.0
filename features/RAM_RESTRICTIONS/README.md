# RAM_RESTRICTIONS feature

Along with v1.8 release, EOSIO introduced a set of protocal features that can be acitvated individually. You can check details here: [https://github.com/EOSIO/eos/releases/tag/v1.8.0](https://github.com/EOSIO/eos/releases/tag/v1.8.0)

This article is on one specific feature of them: `RAM_RESTRICTIONS`

## Brief

As decribed [here](https://github.com/EOSIO/eos/pull/7131),  the RAM_RESTRICTIONS protocol feature which, when activated makes the following changes to the rules regarding RAM billing within an action:

For an unprivileged contract responding to a notification:

Not allowed to schedule a deferred transaction in which the RAM costs are paid by an account other than receiver.
Allowed to execute database operations that increase RAM usage of an arbitrary account other than receiver as long as the action's net effect on RAM usage for that account is to not increase it.

For an unprivileged contract executing an action (but not as a response to a notification):

Not allowed to schedule a deferred transaction in which the RAM costs are paid by an account other than receiver unless that account authorized the action.

Allowed to execute database operations that increase RAM usage of an arbitrary account other than receiver as long as either that account authorized the action or the action's net effect on RAM usage for that account is to not increase it.

## Proposal on EOS Mainnet

EOSLaoMao Team has proposed to activate `RAM_RESTRICTIONS` on EOS Mainnet.

We have included top 35 BPs + some highly recoginized technical/governace BPs among the community in our proposal, you can find the full BP list in file `features/RAM_RESTRICTIONS/producer_perm.json`

The payload used for this proposal can be found in `features/RAM_RESTRICTIONS/payload.json`

To verify this payload, simply:

```
cleos -u https://api.eoslaomao.com push action eosio activate '{"feature_digest": "4e7bf348da00a945489b2a681749eb56f5de00b900014e137ddae39f48f69d67"}' -p eosio -s -j -d > activate.json
```

The `feature_digest` of RAM_RESTRICTIONS is `4e7bf348da00a945489b2a681749eb56f5de00b900014e137ddae39f48f69d67`, it could be found via `curl -X POST http://localhost:8888/v1/producer/get_supported_protocol_features -d '{}'` on your BP node with producer api plugin enabled as [described here](https://developers.eos.io/eosio-nodeos/docs/consensus-protocol-upgrade-process#section-special-notes-to-block-producers)

The only difference between `activate.json` you generated and `features/RAM_RESTRICTIONS/payload.json` should be `expiration`, `ref_block_num` and `ref_block_prefix`.

Please review and verify this proposal ASAP: [https://bloks.io/msig/eoslaomaocom/fixlinkauth](https://bloks.io/msig/eoslaomaocom/fixlinkauth)
