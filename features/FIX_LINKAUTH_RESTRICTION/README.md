# FIX_LINKAUTH_RESTRICTION feature

Along with v1.8 release, EOSIO introduced a set of protocal features that can be acitvated individually. You can check details here: [https://github.com/EOSIO/eos/releases/tag/v1.8.0](https://github.com/EOSIO/eos/releases/tag/v1.8.0)

This article is on one specific feature of them: `FIX_LINKAUTH_RESTRICTION`

## Brief

As decribed [here](https://github.com/EOSIO/eos/issues/6672), in short, `FIX_LINKAUTH_RESTRICTION`, is just a bug fix which forbids the five special names for `eosio::linkauth` action: `updateauth`, `deleteauth`, `linkauth`, `unlinkauth`, and `canceldelay`


## Proposal on EOS Mainnet

EOSLaoMao Team has proposed to activate `FIX_LINKAUTH_RESTRICTION` on EOS Mainnet.

We have included top 35 BPs + some highly recoginized technical/governace BPs among the community in our proposal, you can find the full BP list in file `features/FIX_LINKAUTH_RESTRICTION/producer_perm.json`

The payload used for this proposal can be found in `features/FIX_LINKAUTH_RESTRICTION/payload.json`

To verify this payload, simply:

```
cleos -u https://api.eoslaomao.com push action eosio activate '{"feature_digest": "e0fb64b1085cc5538970158d05a009c24e276fb94e1a0bf6a528b48fbc4ff526"}' -p eosio -s -j -d > activate.json
```

The `feature_digest` of FIX_LINKAUTH_RESTRICTION is `e0fb64b1085cc5538970158d05a009c24e276fb94e1a0bf6a528b48fbc4ff526`, it could be found via `curl -X POST http://localhost:8888/v1/producer/get_supported_protocol_features -d '{}'` on your BP node with producer api plugin enabled as [described here](https://developers.eos.io/eosio-nodeos/docs/consensus-protocol-upgrade-process#section-special-notes-to-block-producers)

The only difference between `activate.json` you generated and `features/FIX_LINKAUTH_RESTRICTION/payload.json` should be `expiration`, `ref_block_num` and `ref_block_prefix`.

Please review and verify this proposal ASAP: [https://bloks.io/msig/eoslaomaocom/fixlinkauth](https://bloks.io/msig/eoslaomaocom/fixlinkauth)
