---
description: The distribution module allows you to manage your staking rewards
---

# distribution

## Available Commands

| Name | Description |
| :--- | :--- |
| [commission](distribution.md#chtd-query-distribution-commission) | Query distribution validator commission |
| [community-pool](distribution.md#chtd-query-distribution-community-pool) | Query the amount of coins in the community pool |
| [params](distribution.md#chtd-query-distribution-rewards) | Query distribution params |
| [rewards](distribution.md#chtd-query-distribution-rewards) | Query all distribution delegator rewards or rewards from a particular validator |
| [slashes](distribution.md#chtd-query-distribution-slashes) | Query distribution validator slashes. |
| [validator-outstanding-rewards](distribution.md#chtd-query-distribution-validator-outstanding-rewards) | Query distribution outstanding \(un-withdrawn\) rewards for a validator and all their delegations |
| [fund-community-pool](distribution.md#chtd-tx-distribution-fund-community-pool) | Funds the community pool with the specified amount |
| [set-withdraw-addr](distribution.md#chtd-tx-distribution-set-withdraw-addr) | Set the withdraw address for rewards associated with a delegator address |
| [withdraw-all-rewards](distribution.md#chtd-tx-distribution-withdraw-all-rewards) | Withdraw all rewards for a single delegator |
| [withdraw-rewards](distribution.md#chtd-tx-distribution-withdraw-rewards) | Withdraw rewards from a given delegation address, and optionally withdraw validator commission if the delegation address given is a validator operator |

### chtd query distribution commission

Query validator commission rewards from delegators to that validator.

```text
chtd query distribution commission [validator] [flags]
```

### chtd query distribution community-pool <a id="chtd-query-distribution-community-pool"></a>

Query all coins in the community pool which is under Governance control.

```text
chtd query distribution community-pool [flags]
```

### chtd query distribution params <a id="chtd-query-distribution-params"></a>

Query distribution params.

```text
 chtd query distribution params [flags]
```

### chtd query distribution rewards <a id="chtd-query-distribution-rewards"></a>

Query all rewards earned by a delegator, optionally restrict to rewards from a single validator.

```text
chtd query distribution rewards [delegator-addr] [validator-addr] [flags]
```

### chtd query distribution slashes <a id="chtd-query-distribution-slashes"></a>

Query all slashes of a validator for a given block range.

```text
chtd query distribution slashes [validator] [start-height] [end-height] [flags]
```

### chtd query distribution validator-outstanding-rewards <a id="chtd-query-distribution-validator-outstanding-rewards"></a>

Query distribution outstanding \(un-withdrawn\) rewards for a validator and all their delegations.

```text
chtd query distribution validator-outstanding-rewards [validator] [flags]
```

### chtd tx distribution fund-community-pool <a id="chtd-tx-distribution-fund-community-pool"></a>

Funds the community pool with the specified amount.

```text
chtd tx distribution fund-community-pool [amount] [flags]
```

### chtd tx distribution set-withdraw-addr <a id="chtd-tx-distribution-set-withdraw-addr"></a>

Set the withdraw address for rewards associated with a delegator address.

```text
chtd tx distribution set-withdraw-addr [withdraw-addr] [flags]
```

### chtd tx distribution withdraw-all-rewards <a id="chtd-tx-distribution-withdraw-all-rewards"></a>

Withdraw all rewards for a single delegator.

```text
chtd tx distribution withdraw-all-rewards [flags]
```

### chtd tx distribution withdraw-rewards <a id="chtd-tx-distribution-withdraw-rewards"></a>

Withdraw rewards from a given delegation address, and optionally withdraw validator commission if the delegation address given is a validator operator.

```text
chtd tx distribution withdraw-rewards [validator-addr] [flags]
```
