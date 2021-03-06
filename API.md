<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### Table of Contents

-   [Ed25519Keypair](#ed25519keypair)
-   [makeEd25519Condition](#makeed25519condition)
-   [makeSha256Condition](#makesha256condition)
-   [makeThresholdCondition](#makethresholdcondition)
-   [makeCreateTransaction](#makecreatetransaction)
-   [makeOutput](#makeoutput)
-   [makeTransferTransaction](#maketransfertransaction)
-   [serializeTransactionIntoCanonicalString](#serializetransactionintocanonicalstring)
-   [signTransaction](#signtransaction)
-   [ccJsonLoad](#ccjsonload)
-   [ccJsonify](#ccjsonify)
-   [getBlock](#getblock)
-   [getStatus](#getstatus)
-   [getTransaction](#gettransaction)
-   [listBlocks](#listblocks)
-   [listOutputs](#listoutputs)
-   [listTransactions](#listtransactions)
-   [listVotes](#listvotes)
-   [pollStatusAndFetchTransaction](#pollstatusandfetchtransaction)
-   [postTransaction](#posttransaction)

## Ed25519Keypair

**Parameters**

-   `secret` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)?** A seed that will be used as a key derivation function

**Properties**

-   `publicKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 
-   `privateKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** 

## makeEd25519Condition

**Parameters**

-   `publicKey` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** base58 encoded Ed25519 public key for the recipient of the Transaction
-   `json` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If true returns a json object otherwise a crypto-condition type (optional, default `true`)

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Ed25519 Condition (that will need to wrapped in an Output)

## makeSha256Condition

**Parameters**

-   `preimage` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Preimage to be hashed and wrapped in a crypto-condition
-   `json` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If true returns a json object otherwise a crypto-condition type (optional, default `true`)

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Preimage-Sha256 Condition (that will need to wrapped in an Output)

## makeThresholdCondition

**Parameters**

-   `threshold` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** 
-   `subconditions` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)**  (optional, default `[]`)
-   `json` **[boolean](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Boolean)** If true returns a json object otherwise a crypto-condition type (optional, default `true`)

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Sha256 Threshold Condition (that will need to wrapped in an Output)

## makeCreateTransaction

**Parameters**

-   `asset` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Created asset's data
-   `metadata` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Metadata for the Transaction
-   `outputs` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)>** Array of Output objects to add to the Transaction.
                              Think of these as the recipients of the asset after the transaction.
                              For `CREATE` Transactions, this should usually just be a list of
                              Outputs wrapping Ed25519 Conditions generated from the issuers' public
                              keys (so that the issuers are the recipients of the created asset).
-   `issuers` **...[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)>** Public key of one or more issuers to the asset being created by this
                                 Transaction.
                                 Note: Each of the private keys corresponding to the given public
                                 keys MUST be used later (and in the same order) when signing the
                                 Transaction (`signTransaction()`).

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Unsigned transaction -- make sure to call signTransaction() on it before
                  sending it off!

## makeOutput

**Parameters**

-   `condition` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Condition (e.g. a Ed25519 Condition from `makeEd25519Condition()`)
-   `amount` **[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** Amount of the output (optional, default `1`)

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** An Output usable in a Transaction

## makeTransferTransaction

**Parameters**

-   `unspentTransaction` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Previous Transaction you have control over (i.e. can fulfill
                                       its Output Condition)
-   `metadata` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Metadata for the Transaction
-   `outputs` **[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)&lt;[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)>** Array of Output objects to add to the Transaction.
                              Think of these as the recipients of the asset after the transaction.
                              For `TRANSFER` Transactions, this should usually just be a list of
                              Outputs wrapping Ed25519 Conditions generated from the public keys of
                              the recipients.
-   `fulfilledOutputs` **...[number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)** Indices of the Outputs in `unspentTransaction` that this
                                        Transaction fulfills.
                                        Note that the public keys listed in the fulfilled Outputs
                                        must be used (and in the same order) to sign the Transaction
                                        (`signTransaction()`).

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Unsigned transaction -- make sure to call signTransaction() on it before
                  sending it off!

## serializeTransactionIntoCanonicalString

**Parameters**

-   `transaction`  
-   `null` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** (transaction)

Returns **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** a canonically serialized Transaction

## signTransaction

**Parameters**

-   `transaction` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Transaction to sign. `transaction` is not modified.
-   `privateKeys` **...[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** Private keys associated with the issuers of the `transaction`.
                                   Looped through to iteratively sign any Input Fulfillments found in
                                   the `transaction`.

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** The signed version of `transaction`.

## ccJsonLoad

**Parameters**

-   `conditionJson` **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 

Returns **cc.Condition** Ed25519 Condition (that will need to wrapped in an Output)

## ccJsonify

**Parameters**

-   `fulfillment` **cc.Fulfillment** base58 encoded Ed25519 public key for the recipient of the Transaction

Returns **[object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** Ed25519 Condition (that will need to wrapped in an Output)

## getBlock

**Parameters**

-   `blockId`  

## getStatus

**Parameters**

-   `tx_id`  

## getTransaction

**Parameters**

-   `txId`  

## listBlocks

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `$0.tx_id`  
    -   `$0.status`  
-   `tx_id`  
-   `status`  

## listOutputs

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `$0.public_key`  
    -   `$0.unspent`  
-   `onlyJsonResponse`  
-   `public_key`  
-   `unspent`  

## listTransactions

**Parameters**

-   `$0` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `$0.asset_id`  
    -   `$0.operation`  
-   `asset_id`  
-   `operation`  

## listVotes

**Parameters**

-   `block_id`  

## pollStatusAndFetchTransaction

**Parameters**

-   `tx_id`  

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)** 

## postTransaction

**Parameters**

-   `transaction`  
