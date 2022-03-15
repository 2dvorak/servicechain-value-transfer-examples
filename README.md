# Requirement
1. `bridge_info.json` should be properly set before running tests. See [configuration example](https://ko.docs.klaytn.com/node/service-chain/getting-started/value-transfer)
2. Set `RPC_API="klay,subbridge"` in your `kscnd.conf`

# Install dependencies
`$ npm install`

# How to run
All the script files, ```*.sh``` sequantilly run contract deployment, one-step (`requestValueTransfer()`), and two-step (`approve()` and `requestXXXTransfer()`) transfers.
The `kscn` binary, ofifcially provided by Klaytn Team is currently not aware of one-step transfer handling from KIP7 and KIP17 token trasnfer reqeust and they are intentionally omitted.
### ERC-20
```
cd erc20
./erc20-deploy-and-test-transfer.sh
```

### ERC-721
```
cd erc721
./erc721-deploy-and-test-transfer.sh
```

### KIP-7
```
cd kip7
./kip7-deploy-and-test-transfer.sh
```

### KIP-17
```
cd kip17
./kip17-deploy-and-test-transfer.sh
```

### KLAY
```
cd klay
```

### All
```
./run_all.sh
```

### Reference
- [Getting Started](https://ko.docs.klaytn.com/node/service-chain/getting-started)
- [Value Transfer Reference](https://ko.docs.klaytn.com/node/service-chain/references/value-transfer)

## How the KIP token can be transferred using ERC-dedicated bridge contract function and vice versa?
The function `requestValueTransfer()` from both KIP7 and KIP17 contracts are currently not supported. However, both KIP7 and KIP17 are compatiable with ERC20 and ERC721, respectively,
which means the token transfer would be possible thorugh compatible interface contract. Our current bridge contract has built-in token transfer functions between chains.
Currently, `requestERC20Trasnfer()` and `requestERC721Transfer()` are supported and they are exploited to transfer the KIP7 and KIP17 standard token.
The functions consist of two steps: (1) Trasnfer the token first and (2) emit `RequestValueTransfer` event, which handled by the counterpart chain.
The standard implementation of token trasnfer is same between KIP and ERC. Of course, modification can be made in those implementation by specific purpose, but the principal is not changed unless the developer have does satisfied the standard implementation.
Thus, token trasnfer would be done successfully. Second, the logic right after trasnfer the token contains burning the token, returning fee, and emit the corresponding event. They are currently not the modifiable context from third-party users.
Thus, the behavior is fixed. This is the techinical background how the KIP token can be transferred via ERC-dedicated bridge contract transfer function and vice versa.

## TODO
Both KIP7 and KIP17 bridge contracts would be supported corresponding `kscn` binary soon. If you want to experience it early, go to [klaytn-not-merged-yet](https://github.com/hyunsooda/klaytn/commits/SC-support-KIP7-KIP17) and compile it in your local environment.
