---
cover: ../.gitbook/assets/Gitbook Banner large 6 (1) (1) (10) (20).png
coverY: 0
---

# Useful CLI Commands

Get standard debug info from the `juno` daemon:

```bash
chtd status
```

Check if your node is catching up:

```bash
# Query via the RPC (default port: 26657)
curl http://localhost:26657/status | jq .result.sync_info.catching_up
```

Get your node ID:

```bash
chtd tendermint show-node-id
```

{% hint style="info" %}
Your peer address will be the result of this plus host and port, i.e. `<id>@<host>:26656` if you are using the default port.
{% endhint %}

Check if you are jailed or tombstoned:

```bash
chtd query slashing signing-info $(chtd tendermint show-validator)
```

Set the default chain for commands to use:

```bash
chtd config chain-id morocco-1
```

Get your `valoper` address:

```bash
chtd keys show <your-key-name> -a --bech val
```

See keys on the current box:

```bash
chtd keys list
```

Import a key from a mnemonic:

```bash
chtd keys add <new-key-name> --recover
```

Export a private key (warning: don't do this unless you know what you're doing!)

```bash
chtd keys export <your-key-name> --unsafe --unarmored-hex
```

Withdraw rewards (including validator commission), where `chronicvaloper1...` is the validator address:

```bash
chtd tx distribution withdraw-rewards <chronicvaloper1...> --from <your-key>  --commission
```

Stake:

```bash
chtd tx staking delegate <chronicvaloper1...> <AMOUNT>ucht --from <your-key>
```

Find out what the JSON for a command would be using `--generate-only`:

```bash
chtd tx bank send $(chtd keys show <your-key-name> -a) <recipient addr> <AMOUNT>ucht --generate-only
```

Query the results of a gov vote that has ended, from a remote RPC (NB - you have to specify a height before the vote ended):

```bash
 chtd q gov votes 1 --height <height-before-vote-ended> --node https://rpc-archive.chronicnetwork.io:443
```

Query the validator set (and jailed status) via CLI:

```bash
chtd query staking validators --limit 1000 -o json | jq -r '.validators[] | [.operator_address, (.tokens|tonumber / pow(10; 6)), .description.moniker, .jail, .status] | @csv' | column -t -s"," | sort -k2 -n -r | nl
```

Get contract state:

```bash
chtd q wasm contract-state all <contract-address>
```