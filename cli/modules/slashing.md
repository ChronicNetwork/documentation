---
description: The slashing module can unjail your validator
---

# slashing

## Available Commands

| Name | Description |
| :--- | :--- |
| [unjail](slashing.md#chtd-tx-slashing-unjail) | Unjail validator previously jailed for downtime or double-sign. |

### chtd tx slashing unjail

Unjail validator previously jailed for downtime.

```text
chtd tx slashing unjail [flags]
```

#### Unjail a validator

The following example will unjail a validator using its validator operator \(owner\) key :

```text
chtd tx slashing unjail --from chronic1ludczrvlw36fkur9vy49lx4vjqhppn30h42ufg --chain-id morocco-1
```

{% hint style="info" %}
**TIP**

The validator operator key must be stored in the local keystore. 
{% endhint %}
