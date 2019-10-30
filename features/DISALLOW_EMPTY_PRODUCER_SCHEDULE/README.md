# DISALLOW_EMPTY_PRODUCER_SCHEDULE feature

Along with v1.8 release, EOSIO introduced a set of protocal features that can be acitvated individually. You can check details here: [https://github.com/EOSIO/eos/releases/tag/v1.8.0](https://github.com/EOSIO/eos/releases/tag/v1.8.0)

This article is on one specific feature of them: `DISALLOW_EMPTY_PRODUCER_SCHEDULE`

## Brief

As decribed [here](https://github.com/EOSIO/eos/issues/6458), in short, `DISALLOW_EMPTY_PRODUCER_SCHEDULE`, is nothing more than a declaration that tells the community: Block Producers can't，and can't even try to set an empty producer schedule to an EOS network with this feature enabled, which eliminates the possibility to freeze a network by not producing any blocks, even though the possibility is already 0 according to [the code here](https://github.com/EOSIO/eos/blob/a2317d380de2ca4b8753673ee42fdfd6fb1b4819/libraries/chain/controller.cpp#L2733)


## Proposal on EOS Mainnet

EOSLaoMao Team has proposed to activate `DISALLOW_EMPTY_PRODUCER_SCHEDULE` on EOS Mainnet.

Special thanks to EOS Nation and MeetOne Team.

We have included top 35 BPs + 13 highly recoginized technical/governace BPs among the community in our proposals, you can find the full list in file `features/DISALLOW_EMPTY_PRODUCER_SCHEDULE/producer_perm.json`

The payload used for this proposal can be found in `features/DISALLOW_EMPTY_PRODUCER_SCHEDULE/payload.json`

To verify this payload, simply:

```
cleos -u https://api.eoslaomao.com push action eosio activate '{"feature_digest": "68dcaa34c0517d19666e6b33add67351d8c5f69e999ca1e37931bc410a297428"}' -p eosio -s -j -d > activate.json
```

The `feature_digest` of DISALLOW_EMPTY_PRODUCER_SCHEDULE is `68dcaa34c0517d19666e6b33add67351d8c5f69e999ca1e37931bc410a297428`, it could be found via `curl -X POST http://localhost:8888/v1/producer/get_supported_protocol_features -d '{}'` on your BP node with producer api plugin enabled as [described here](https://developers.eos.io/eosio-nodeos/docs/consensus-protocol-upgrade-process#section-special-notes-to-block-producers)

The only difference between `activate.json` you generated and `features/DISALLOW_EMPTY_PRODUCER_SCHEDULE/payload.json` should be `expiration`, `ref_block_num` and `ref_block_prefix`.

Please review and verify this proposal ASAP: [https://bloks.io/msig/eoslaomaocom/disallownobp](https://bloks.io/msig/eoslaomaocom/disallownobp)
