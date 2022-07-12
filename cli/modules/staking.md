---
description: >-
  The staking module provides a set of subcommands to query the staking state
  and send staking transactions.
---

# staking

## Available Commands

| Name | Description |
| :--- | :--- |
| [validator](staking.md#chtd-query-staking-validator) | Query a validator |
| [validators](staking.md#chtd-query-staking-validators) | Query for all validators |
| [delegation](staking.md#chtd-query-staking-delegation) | Query a delegation based on address and validator address |
| [delegations](staking.md#chtd-query-staking-delegations) | Query all delegations made from one delegator |
| [delegations-to](staking.md#chtd-query-staking-delegations-to) | Query all delegations to one validator |
| [unbonding-delegation](staking.md#chtd-query-staking-unbonding-delegation) | Query an unbonding-delegation record based on delegator and validator address |
| [unbonding-delegations](staking.md#chtd-query-staking-unbonding-delegations) | Query all unbonding-delegations records for one delegator |
| [unbonding-delegations-from](staking.md#chtd-query-staking-unbonding-delegations-from) | Query all unbonding delegatations from a validator |
| [redelegations-from](staking.md#chtd-query-staking-redelegations-from) | Query all outgoing redelegatations from a validator |
| [redelegation](staking.md#chtd-query-staking-redelegation) | Query a redelegation record based on delegator and a source and destination validator address |
| [redelegations](staking.md#chtd-query-staking-redelegations) | Query all redelegations records for one delegator |
| [pool](staking.md#chtd-query-staking-pool) | Query the current staking pool values |
| [params](staking.md#chtd-query-staking-params) | Query the current staking parameters information |
| [historical-info](staking.md#chtd-query-staking-historical-info) | Query historical info at given height |
| [create-validator](staking.md#chtd-tx-staking-create-validator) | Create new validator initialized with a self-delegation to it |
| [edit-validator](staking.md#chtd-tx-staking-edit-validator) | Edit existing validator account |
| [delegate](staking.md#chtd-tx-staking-delegate) | Delegate liquid tokens to an validator |
| [unbond](staking.md#chtd-tx-staking-unbond) | Unbond shares from a validator |
| [redelegate](staking.md#chtd-tx-staking-redelegate) | Redelegate illiquid tokens from one validator to another |

### chtd query staking validator <a id="chtd-query-staking-validator"></a>

#### Query a validator by validator address <a id="query-a-validator-by-validator-address"></a>

Query information for validator address `<chronicvaloper...>`:

```text
chtd query staking validator <chronicvaloper...>
```

Will return something similar to:

```text
commission:
  commission_rates:
    max_change_rate: "0.010000000000000000"
    max_rate: "0.200000000000000000"
    rate: "0.110000000000000000"
  update_time: "2021-07-01T09:06:42.110582713Z"
consensus_pubkey:
  '@type': /cosmos.crypto.ed25519.PubKey
  key: +vZPP6QFMUUkCO+MyMdOZGUzuNLAB98ruw6Rjfvnk60=
delegator_shares: "10647850539.181674918343698393"
description:
  details: ""
  identity: FEE30F35994C320D
  moniker: nullmames
  security_contact: ""
  website: ""
jailed: false
min_self_delegation: "1"
operator_address: chronicvaloper1ludczrvlw36fkur9vy49lx4vjqhppn30ggunj3
status: BOND_STATUS_BONDED
tokens: "10331782033"
unbonding_height: "631804"
unbonding_time: "2021-08-25T00:26:38.283926951Z"
```

### chtd query staking validators <a id="chtd-query-staking-validators"></a>

#### Query all validators <a id="query-all-validators"></a>

The following will return information for ALL validators:

```text
chtd query staking validators
```

The returned values will be similar to those from [`chtd query staking validator`](staking.md#chtd-query-staking-validator)\`\`

### chtd query staking delegation <a id="chtd-query-staking-delegation"></a>

Query a delegation based on delegator address and validator address.

```text
chtd query staking delegation [delegator-addr] [validator-addr]
```

#### Query a delegation <a id="query-a-delegation"></a>

The following will return delegations for a delegator to a particular validator address `<chronicvaloper...>` :

```text
chtd query staking delegation <chronic...> <chronicvaloper...>
```

Returns something similar to:

```text
balance:
  amount: "9159423104"
  denom: ucht
delegation:
  delegator_address: chronic1ludczrvlw36fkur9vy49lx4vjqhppn30h42ufg
  shares: "9439626961.941610808328957187"
  validator_address: chronicvaloper1ludczrvlw36fkur9vy49lx4vjqhppn30ggunj3
```

### chtd query staking delegations <a id="chtd-query-staking-delegations"></a>

Query all delegations delegated from one delegator.

```text
chtd query staking delegations [delegator-address] [flags]
```

#### Query all delegations of a delegator <a id="query-all-delegations-of-a-delegator"></a>

The following command will return all delegations from a delegators address `<chronic...>`:

```text
chtd query staking delegations <chronic...>
```

Will return something similar to:

```text
delegation_responses:
- balance:
    amount: "1100000"
    denom: ucht
  delegation:
    delegator_address: chronic1ludczrvlw36fkur9vy49lx4vjqhppn30h42ufg
    shares: "1100000.000000000000000000"
    validator_address: chronicvaloper1ms8tvfkerhyf6mca2qc79t7mr3eh9dsr79mjf2
- balance:
    amount: "9166092794"
    denom: ucht
  delegation:
    delegator_address: chronic1ludczrvlw36fkur9vy49lx4vjqhppn30h42ufg
    shares: "9446500690.213833508382324426"
    validator_address: chronicvaloper1ludczrvlw36fkur9vy49lx4vjqhppn30ggunj3
pagination:
  next_key: null
  total: "0"
```

### chtd query staking delegations-to <a id="chtd-query-staking-delegations-to"></a>

Query all delegations to one validator.

```text
chtd query staking delegations-to [validator-address] [flags]
```

#### Query all delegations to one validator <a id="query-all-delegations-to-one-validator"></a>

The following command will return all delegations to a validator address `<chronicvaloper...>`:

```text
chtd query staking delegations-to <chronicvaloper...>
```

Will return something similar to:

```text
delegation_responses:
- balance:
    amount: "990000675"
    denom: ucht
  delegation:
    delegator_address: chronic1qnshaxp9w7aecthj2sn4c0uct07urg3tsd2rqs
    shares: "1020286644.656983874857994825"
    validator_address: chronicvaloper1ludczrvlw36fkur9vy49lx4vjqhppn30ggunj3
- balance:
    amount: "180180122"
    denom: ucht
  delegation:
    delegator_address: chronic1na45quuuzuv5xtzl5qqp9zep9rkluqykwtcgd3
    shares: "185692169.327571065224155062"
    validator_address: chronicvaloper1ludczrvlw36fkur9vy49lx4vjqhppn30ggunj3
```

### chtd query staking unbonding-delegation <a id="chtd-query-staking-unbonding-delegation"></a>

Query an unbonding-delegation record based on delegator and validator address.

```text
chtd query staking unbonding-delegation [delegator-addr] [validator-addr] [flags]
```

#### Query an unbonding delegation record <a id="query-an-unbonding-delegation-record"></a>

```text
chtd query staking unbonding-delegation <chronic...> <chronicvaloper...>
```

### chtd query staking unbonding-delegations <a id="chtd-query-staking-unbonding-delegations"></a>

#### Query all unbonding delegations records of a delegator <a id="query-all-unbonding-delegations-records-of-a-delegator"></a>

```text
chtd query staking unbonding-delegations <chronic...>
```

### chtd query staking unbonding-delegations-from <a id="chtd-query-staking-unbonding-delegations-from"></a>

#### Query all unbonding delegations from a validator <a id="query-all-unbonding-delegations-from-a-validator"></a>

```text
chtd query staking unbonding-delegations-from <chronicvaloper...>
```

### chtd query staking redelegations-from <a id="chtd-query-staking-redelegations-from"></a>

Query all outgoing redelegations of a validator

```text
chtd query staking redelegations-from [validator-address] [flags]
```

#### Query all outgoing redelegatations of a validator <a id="query-all-outgoing-redelegatations-of-a-validator"></a>

```text
chtd query staking redelegations-from <chronicvaloper...>
```

### chtd query staking redelegation <a id="chtd-query-staking-redelegation"></a>

Query a redelegation record based on delegator and source validator address and destination validator address.

```text
chtd query staking redelegation [delegator-addr] [src-validator-addr] [dst-validator-addr] [flags]
```

#### Query a redelegation record <a id="query-a-redelegation-record"></a>

```text
chtd query staking redelegation <chronic...> <chronicvaloper...> <chronicvaloper...>
```

### chtd query staking redelegations <a id="chtd-query-staking-redelegations"></a>

#### Query all redelegations records of a delegator <a id="query-all-redelegations-records-of-a-delegator"></a>

```text
chtd query staking redelegations <chronic...>
```

### chtd query staking pool <a id="chtd-query-staking-pool"></a>

#### Query the current staking pool values <a id="query-the-current-staking-pool-values"></a>

```text
chtd query staking pool
```

Returns something similar to:

```text
bonded_tokens: "1547447152807"
not_bonded_tokens: "67232814293"
```

### chtd query staking params <a id="chtd-query-staking-params"></a>

#### Query the current staking parameters information <a id="query-the-current-staking-parameters-information"></a>

```text
chtd query staking params
```

Returns something similar to:

```text
bond_denom: ucht
historical_entries: 10000
max_entries: 7
max_validators: 68
unbonding_time: 1814400s
```

### chtd query staking historical-info <a id="chtd-query-staking-historical-info"></a>

#### Query historical info at given height <a id="query-historical-info-at-given-height"></a>

```text
chtd query staking historical-info <height>
```

### chtd tx staking create-validator <a id="chtd-tx-staking-create-validator"></a>

Send a transaction to apply to be a validator and delegate a certain amount of `cht` to it.

```text
chtd tx staking create-validator [flags]
```

**Flags:**

| Name, shorthand | type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| --amount | string | Yes |  | Amount of coins to bond |
| --commission-rate | float | Yes | 0.0 | The initial commission rate percentage |
| --commission-max-rate | float |  | 0.0 | The maximum commission rate percentage |
| --commission-max-change-rate | float |  | 0.0 | The maximum commission change rate percentage \(per day\) |
| --min-self-delegation | string |  |  | The minimum self delegation required on the validator |
| --details | string |  |  | Optional details |
| --genesis-format | bool |  | false | Export the transaction in gen-tx format; it implies --generate-only |
| --identity | string |  |  | Optional identity signature \(ex. UPort or Keybase\) |
| --ip | string |  |  | Node's public IP. It takes effect only when used in combination with |
| --node-id | string |  |  | The node's ID |
| --moniker | string | Yes |  | Validator name |
| --pubkey | string | Yes |  | Go-Amino encoded hex PubKey of the validator. For Ed25519 the go-amino prepend hex is 1624de6220 |
| --website | string |  |  | Optional website |
| --security-contact | string |  |  | The validator's \(optional\) security contact email |

#### Create a validator <a id="create-a-validator"></a>

```text
chtd tx staking create-validator --chain-id=morocco-1 --from=<key-name> --fees=0.025ucgas --pubkey=<validator-pubKey> --commission-rate=0.1 --amount=100000000ucgas --moniker=<validator-name>
```

{% hint style="info" %}
**TIP**

Refer to [mainnet](../../validators/mainnet.md) instructions for detailed information.
{% endhint %}

### chtd tx staking edit-validator <a id="chtd-tx-staking-edit-validator"></a>

Edit an existing validator's settings, such as commission rate, name, etc.

```text
chtd tx staking edit-validator [flags]
```

**Flags:**

| Name, shorthand | type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| --commission-rate | float |  | 0.0 | Commission rate percentage |
| --moniker | string |  |  | Validator name |
| --identity | string |  |  | Optional identity signature \(ex. UPort or Keybase\) |
| --website | string |  |  | Optional website |
| --details | string |  |  | Optional details |
| --security-contact | string |  |  | The validator's \(optional\) security contact email |
| --min-self-delegation | string |  |  | The minimum self delegation required on the validator |

#### Edit validator information <a id="edit-validator-information"></a>

```text
chtd tx staking edit-validator --from=<key-name> --chain-id=morocco-1 --fees=1cgas --commission-rate=0.10 --moniker=<validator-name>
```

### chtd tx staking delegate <a id="chtd-tx-staking-delegate"></a>

Delegate tokens to a validator.

```text
chtd tx staking delegate [validator-addr] [amount] [flags]
```

```text
chtd tx staking delegate <chronicvaloper...> <amount> --chain-id=morocco-1 --from=<key-name>
```

### chtd tx staking unbond <a id="chtd-tx-staking-unbond"></a>

Unbond tokens from a validator.

```text
chtd tx staking unbond [validator-addr] [amount] [flags]
```

#### Unbond some tokens from a validator <a id="unbond-some-tokens-from-a-validator"></a>

```text
chtd tx staking unbond <chronicvaloper...> 1000000ucht --from=<key-name> --chain-id=morocco-1 
```

### chtd tx staking redelegate <a id="chtd-tx-staking-redelegate"></a>

Transfer delegation from one validator to another.

{% hint style="info" %}
TIP

There is no `unbonding time` during the redelegation, so you will not miss the rewards. But you can only redelegate once per validator, until a period \(= `unbonding time`\) exceed.
{% endhint %}

```text
chtd tx staking redelegate [src-validator-addr] [dst-validator-addr] [amount] [flags]
```

#### Redelegate some tokens to another validator <a id="redelegate-some-tokens-to-another-validator"></a>

```text
chtd tx staking redelegate <chronicvaloper...> <chronicvaloper...> 1000000ucht --chain-id=morocco-1 --from=<key-name>
```
