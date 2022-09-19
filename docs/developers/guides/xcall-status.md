---
sidebar_position: 6
---

# Tracking an xcall

Every `xcall` is associated with a unique `transferId` that can be used to track its lifecycle through a cross-chain transaction.

## Querying Subgraphs

You can query the hosted subgraphs on each chain to check the transaction status.

| Chain | Subgraph |
| --- | --- |
| Goerli | [v0-Goerli](https://thegraph.com/hosted-service/subgraph/connext/nxtp-amarok-runtime-v0-goerli) |
| Optimism-Goerli | [v0-Opt-Goerli](https://thegraph.com/hosted-service/subgraph/connext/amarok-runtime-v0-opt-goerli) |

1.  Make note of the transaction hash that interacted with the Connext contract

2.  Navigate to the hosted subgraph for the origin domain
    - Query by your transaction hash
        
        ```graphql
        {
          originTransfers(
            where: {
              transactionHash: "<xcall_transaction_hash>"
            }
          ) {
            # Meta Data
            chainId
            transferId
            nonce
            to
            callData
            originDomain
            destinationDomain
            forceSlow
            receiveLocal
            recovery
            callback
            callbackFee
            # Asset Data
            transactingAsset
            transactingAmount
            status
          }
        }
        ```
        
3.  Navigate to the hosted subgraph for the destination domain
    - Query by the `transferId` obtained from the origin domain subgraph
        
        ```graphql
        {
          destinationTransfers(
            where: {
              transferId: "<xcall_transfer_id>"
            }
          ) {
            # Meta Data
            chainId
            transferId
            nonce
            to
            callData
            originDomain
            destinationDomain
            forceSlow
            receiveLocal
            recovery
            callback
            callbackFee
            # Asset Data
            localAsset
            localAmount
            transactingAsset
            transactingAmount
            sponsorVaultRelayerFee
            # Executed event Data
            status
            routers {
              id
            }
            originSender
            # Executed Transaction
            executedCaller
            executedTransactionHash
            executedTimestamp
            executedGasPrice
            executedGasLimit
            executedBlockNumber
            # Reconciled Transaction
            reconciledCaller
            reconciledTransactionHash
            reconciledTimestamp
            reconciledGasPrice
            reconciledGasLimit
            reconciledBlockNumber
          }
        }
        ```

## Connextscan

Another option is using [Connextscan](https://testnet.amarok.connextscan.io/) which can monitor the status of any `transferId`.