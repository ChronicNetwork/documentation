---
description: >-
  The governance module provides tools for querying, submitting, and voting on
  proposals
---

# gov

## Available Commands

| Name | Description |
| :--- | :--- |
| [proposal](gov.md#chtd-query-gov-proposal) | Query details of a single proposal |
| [proposals](gov.md#chtd-query-gov-proposals) | Query proposals with optional filter |
| [vote](gov.md#chtd-query-gov-vote) | Query details of a single vote |
| [votes](gov.md#chtd-query-gov-votes) | Query votes on a proposal |
| [deposit](gov.md#chtd-query-gov-deposit) | Query details of a deposit |
| [deposits](gov.md#chtd-query-gov-deposits) | Query deposits on a proposal |
| [tally](gov.md#chtd-query-gov-tally) | Get the tally of a proposal vote |
| [param](gov.md#chtd-query-gov-param) | Query the parameters \(voting |
| [params](gov.md#chtd-query-gov-params) | Query the parameters of the governance process |
| [proposer](gov.md#chtd-query-gov-proposer) | Query which address proposed a proposal with a given ID. |
| [submit-proposal](gov.md#chtd-tx-gov-submit-proposal) | Submit a proposal along with an initial deposit |
| [deposit](gov.md#chtd-tx-gov-deposit) | Deposit tokens for an active proposal |
| [vote](gov.md#chtd-tx-gov-vote) | Vote for an active proposal, options: yes/no/no\_with\_veto/abstain |

### chtd query gov proposal <a id="chtd-query-gov-proposal"></a>

Query details of a single governance proposal:

```text
chtd query gov proposal [proposal-id] [flags]
```

**Flags:**

| Name, shorthand | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| --depositor | Address |  |  | Filter proposals by depositor address |
| --limit | uint |  | 100 | Limit to the latest \[number\] of proposals. Default to all proposals |
| --status | string |  |  | Filter proposals by status |
| --voter | Address |  |  | Filter proposals by voter address |

### chtd query gov proposals <a id="chtd-query-gov-proposals"></a>

Query all proposals:

```text
chtd query gov proposals
```

Query proposals with conditions filters:

```text
chtd query gov proposals --limit=3 --status=Passed --depositor=<chronic...>
```

### chtd query gov vote <a id="chtd-query-gov-vote"></a>

Query details of a single vote.

```text
chtd query gov vote [proposal-id] [voter-addr] [flags]
```

### chtd query gov votes <a id="chtd-query-gov-votes"></a>

Query votes on a proposal.

```text
chtd query gov votes [proposal-id] [flags]
```

### chtd query gov deposit <a id="chtd-query-gov-deposit"></a>

Query details for a single proposal deposit on a proposal by its identifier.

```text
chtd query gov deposit [proposal-id] [depositer-addr] [flags]
```

### chtd query gov deposits <a id="chtd-query-gov-deposits"></a>

Query details for all deposits on a proposal.

```text
chtd query gov deposits [proposal-id] [flags]
```

### chtd query gov tally <a id="chtd-query-gov-tally"></a>

Query tally of votes on a proposal.

```text
chtd query gov tally [proposal-id] [flags]
```

### chtd query gov param <a id="chtd-query-gov-param"></a>

Query the parameters \(voting \| tallying \| deposit\) of the governance process.

```text
chtd query gov param [param-type] [flags]
```

Example:

```text
# query voting parameters
chtd query gov param voting

# query tallying parameters
chtd query gov param tallying

# query deposit parameters
chtd query gov param deposit
```

### chtd query gov params <a id="chtd-query-gov-params"></a>

Query the all the parameters for the governance process.

```text
chtd query gov params [flags]
```

### chtd query gov proposer <a id="chtd-query-gov-proposer"></a>

Query which address proposed a proposal with a given ID.

```text
chtd query gov proposer [proposal-id] [flags]
```

### chtd tx gov submit-proposal <a id="chtd-tx-gov-submit-proposal"></a>

Submit a proposal along with an initial deposit. Proposal title, description, type and deposit can be given directly or through a proposal JSON file. Available Commands:

| Name | Description |
| :--- | :--- |
| cancel-software-upgrade | Cancel the current software upgrade proposal |
| community-pool-spend | Submit a community pool spend proposal |
| param-change | Submit a parameter change proposal |
| software-upgrade | Submit a software upgrade proposal |

#### chtd tx gov submit-proposal community-pool-spend <a id="chtd-tx-gov-submit-proposal-community-pool-spend"></a>

Submit a community pool spend proposal along with an initial deposit. The proposal details must be supplied via a JSON file.

```text
chtd tx gov submit-proposal community-pool-spend <path/to/proposal.json> --from=<key_or_address>
```

Where proposal.json contains, for example:

```text
{
    "title": "Community Pool Spend",
    "description": "Send me tokens, to benefit the Chronic community",
    "recipient": "chronic1ludczrvlw36fkur9vy49lx4vjqhppn30h42ufg",
    "amount": "1000000ucht",
    "deposit": "1000ucht"
}
```

#### chtd tx gov submit-proposal param-change <a id="chtd-tx-gov-submit-proposal-param-change"></a>

Submit a parameter proposal along with an initial deposit. The proposal details must be supplied via a JSON file. For values that contains objects, only non-empty fields will be updated.

{% hint style="warning" %}
**IMPORTANT**

Currently parameter changes are evaluated but not validated, so it is very important that any "value" change is valid \(ie. correct type and within bounds\) for its respective parameter, eg. "MaxValidators" should be an integer and not a decimal.
{% endhint %}

Proper vetting of a parameter change proposal should prevent this from happening \(no deposits should occur during the governance process\), but it should be noted regardless.

```text
chtd tx gov submit-proposal param-change <path/to/proposal.json> --from=<key_or_address>
```

Where proposal.json contains, for example:

```text
{
    "title": "Staking Param Change",
    "description": "Update max validators",
    "changes": [
        {
        "subspace": "staking",
        "key": "MaxValidators",
        "value": 105
        }
    ],
    "deposit": "1000ucht"
}
```

#### chtd tx gov submit-proposal software-upgrade <a id="chtd-tx-gov-submit-proposal-software-upgrade"></a>

Submit a software upgrade along with an initial deposit. Please specify a unique name and height OR time for the upgrade to take effect.

```text
chtd tx gov submit-proposal software-upgrade [name] (--upgrade-height [height] | --upgrade-time [time]) (--upgrade-info [info]) [flags]
```

**Flags:**

| Name, shorthand | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| --deposit | Coin | Yes |  | Deposit of the proposal |
| --title | string | Yes |  | Title of proposal |
| --description | string | Yes |  | Description of proposal |
| --upgrade-height | int64 |  |  | The height at which the upgrade must happen \(not to be used together with --upgrade-time\) |
| --time | string |  |  | The time at which the upgrade must happen \(not to be used together with --upgrade-height\) |
| --info | string |  |  | Optional info for the planned upgrade such as commit hash, etc. |

{% hint style="info" %}
**TIP**

To enable nodes managed by [forbole/cosmovisor](https://github.com/forbole/cosmovisor) to undertake an automatic upgrade, where the operator has the required environment variable set.

Store an os/architecture -&gt; binary URI map in the upgrade plan `info` field as JSON under the `"binaries"` key, eg:

```text
{
  "binaries": {
    "linux/amd64":"https://example.com/cht.zip?checksum=sha256:"
  }
}
```
{% endhint %}

#### chtd tx gov submit-proposal cancel-software-upgrade <a id="chtd-tx-gov-submit-proposal-cancel-software-upgrade"></a>

Cancel a software upgrade along with an initial deposit.

```text
chtd tx gov submit-proposal cancel-software-upgrade [flags]
```

**Flags:**

| Name, shorthand | Type | Required | Default | Description |
| :--- | :--- | :--- | :--- | :--- |
| --deposit | Coin | Yes |  | Deposit of the proposal |
| --title | string | Yes |  | Title of proposal |
| --description | string | Yes |  | Description of proposal |

### chtd tx gov deposit <a id="chtd-tx-gov-deposit"></a>

Submit a deposit for an active proposal. You can find the `proposal-id` by running `chtd query gov proposals`.

```text
chtd tx gov deposit [proposal-id] [deposit] [flags]
```

### chtd tx gov vote <a id="chtd-tx-gov-vote"></a>

Submit a vote for an active proposal. You can find the `proposal-id` by running `chtd query gov proposals`. Vote for an active proposal, options: \(yes \| no \| no\_with\_veto \| abstain\).

```text
chtd tx gov vote [proposal-id] [option] [flags]
```

Example vote, voting `yes` on proposal number `1`:

```text
chtd tx gov vote 1 yes --from=<key_or_address> --fees=1cgas
```