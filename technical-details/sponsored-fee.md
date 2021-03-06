# Sponsored Fee Transactions

### Use Cases

Users can set a transaction fee nominated in an asset. However, node owners need to explicitly allow transaction fees in the asset by manually editing node configuration file. Otherwise, node won't be able to mine a block with these transactions.

To avoid an extra configuration step, sponsorship could be set for an asset. In this case miner will receive fee in Waves for processing of transactions if their fee is nominated in sponsored asset.

Only the issuer of an asset can set up sponsorship. The sponsorship is set by giving the rate at which fee in an asset is converted to Waves.

### Feature activation

* SponsorFeeTransaction is invalid unless "Fee Sponsorship" feature isn't activated.
* After "Fee Sponsorship" feature activation SponsorFeeTransaction is available to process, but it starts work only after 10000 blocks after activation. Before that, the Asset Fee calculation remains unchanged.
* After 10000 blocks sponsor pays 1/minSponsoredAssetFee for each token used fo fee.

### Implementation

#### Sponsored Fee Transaction representations

Binary format of a SponsorFee transaction is as follows:

| \# | Field name | Type | Position | Length |
| --- | ---: | --- | --- | --- |
| 1 | Transaction type (0x0e) | Byte | 0 | 1 |
| 2 | Version (0x01) |  Byte | 1 | 1 | 
| 3 | Sender's public key | Bytes | 2 | 32 |
| 4 | Asset ID | Bytes | 34 | 32 |
| 5 | Minimal fee in assets\* | Long | 66 | 8 | 
| 6 | Fee | Long | 74 | 8 |
| 7 | Timestamp | Long | 82 | 8 |
| 8 | Proofs\*\* | Bytes | 90 | 64 | 

\* Zero value assume canceling sponsorship.
\*\* Currently only signature is supported, signature have Length = 64

JSON representation example:

```js
{
  "type" : 14,
  "id" : "CwHecsEjYemKR7wqRkgkZxGrb5UEfD8yvZpFF5wXm2Su",
  "sender" : "3FjTpAg1VbmxSH39YWnfFukAUhxMqmKqTEZ",
  "senderPublicKey" : "5AzfA9UfpWVYiwFwvdr77k6LWupSTGLb14b24oVdEpMM",
  "minSponsoredAssetFee": 100000, // null assume canceling sponsorship, number - minimum amount assets require to fee.
  "fee" : 100000000,
  "timestamp" : 1520945679531,
  "proofs" : [ "4huvVwtbALH9W2RQSF5h1XG6PFYLA6nvcAEgv79nVLW7myCysWST6t4wsCqhLCSGoc5zeLxG6MEHpcnB6DPy3XWr" ],
  "version" : 1,
  "height" : 303
}
```

### Fees

#### Fee for Sponsored Fee Transaction
A fee for a sponsor is payable in WAVES only. The fee for this transaction is fixed and equal to 1.0 WAVES.

#### Fee for miner in WAVES
The total miner's fee in WAVES for transactions with a fee in sponsored (after sponsorship activation) can be compute by this formula:
```
    feeInWaves = assetFee * feeUnit / sponsorship
```
where: 
* `assetFee` - a fee in asset from transaction
* `feeUnit` - for sponsorship is equal to 100000
* `sponsorship` - the `minSponsoredAssetFee` value from Sponsored Fee Transaction for this asset 

### API

`POST /assets/sponsor` signs and sends a start/update sponsorship transaction. This endpoint requires API key. Sample input is as follows:

```js
{
  "version": 1,
  "sender": "3FjTpAg1VbmxSH39YWnfFukAUhxMqmKqTEZ",
  "assetId":"AP5dp4LsmdU7dKHDcgm6kcWmeaqzWi2pXyemrn4yTzfo",
  "minSponsoredAssetFee": 100000,
  "fee": 100000000
}
```

`POST /asset/sponsor` signs and sends a canceling sponsorship transaction. This endpoint requires API key. Sample input is as follows:

```js
{
  "version": 1,
  "sender": "3FjTpAg1VbmxSH39YWnfFukAUhxMqmKqTEZ",
  "assetId":"AP5dp4LsmdU7dKHDcgm6kcWmeaqzWi2pXyemrn4yTzfo",
  "minSponsoredAssetFee": null,
  "fee": 100000000
}
```

Sponsorship information for the asset present in [asset description](/development-and-api/waves-node-rest-api/asset-transactions/public-functions.md#get-assetsdetailsassetid) and [asset balance](/development-and-api/waves-node-rest-api/asset-transactions/public-functions.md#get-assetsbalanceaddress).

### Constraints

Only issuer may sponsor asset.

### Related Changes

Minimal fee was moved to consensus.

