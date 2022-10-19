# ArgusNFT API Partner Integration Guide

This document contains instructions on how to integrate with ArgusNFT API.

ArgusNFT is a Machine Learning powered Cardano NFT Fake detection platform. 

Whether you're a NFT Creator, Collector or a Marketplace, ArgusNFT can provide you with tools to verify NFT
minting history and help you decide whether an NFT is a copy.

You can learn more about ArgusNFT on the platform [website](https://argusnft.com).

## ArgusNFT API

ArgusNFT api is available at `https://api.argusnft.com`.

In order to use the API, partners will require an `X_ARGUS_ID`. Reach out to the ArgusNFT team at [team@argusnft.com](mailto:team@argusnft.com)
if you're interested in testing or integrating the API on your platform.

Integrating with the ArgusNFT API is very straightforward and requires a simple "POST" on the following endpoint: `https://api.argusnft.com/native_assets` (see below for the payload).

In order to offer maximum flexibility, we've implemented three different ways of identifying Cardano's NFT:

* Via `policy_id` (hex encoded) and `asset_name` (hex encoded)
* Via `fingerprint`
* Via `asset_id` (hex encoded concatenation of policy_id and asset_name)

### Examples

Let's say that the original SpaceBudz `SpaceBud1576` is under investigation. Its `policy_id` is `d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc` and `asset_name` is `SpaceBud1576`.

It is possible to query for the SpaceBudz authenticity in the following ways:

Via `policy_id` and `asset_name`:

```bash
curl -H 'Content-Type: application/json' -H 'Accept: application' -H 'X_ARGUS_ID: xyz123' -X POST -d '{ "policy_id":"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc", "asset_name":"537061636542756431353736" }' https://api.argusnft.com/native_assets/verify
```

Via `fingerprint`:

```bash
curl -H 'Content-Type: application/json' -H 'Accept: application' -H 'X_ARGUS_ID: xyz123' -X POST -d '{ "fingerprint": "asset19yf2vds4svv9jnfw289edh4gwr3mr4wphy564z" }' https://api.argusnft.com/native_assets/verify
```

Via `asset_id`:

```bash
curl -H 'Content-Type: application/json' -H 'Accept: application' -H 'X_ARGUS_ID: xyz123' -X POST -d '{ "asset_id":"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc537061636542756431353736" }' https://api.argusnft.com/native_assets/verify
```

The response will look something like:

```json
{"native_asset":{"policy_id":"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc","asset_name":"537061636542756431353736","asset_id":"d5e6bf0500378d4f0da4e8dde6becec7621cd8cbf5cbb9b87013d4cc537061636542756431353736"},"authenticity":"original"}
```

The attribute `authenticity` can assume one of the following values:

1. `original` if the digital asset(s) attached to the NFT have been seen for the **first time** on the blockchain
2. `copy` if there is at least one NFT with the same or similar digital asset minted **before**
3. `unknown` in the rare event the system cannot process the current NFT

An example of a fake NFT is:

```bash
curl -H 'Content-Type: application/json' -H 'Accept: application' -H 'X_ARGUS_ID: xyz123' -X POST -d '{ "fingerprint": "asset1t7f8fsdpu5l0e8nu3fewyndt8emvwzezjrer2d" }' https://api.argusnft.com/native_assets/verify
```

And the response is:

```json
{"native_asset":{"policy_id":"08893e9bb60c8072b0692c36f4d11ff0a23c6ba2bbaa85729d951527","asset_name":"537061636542756c6c31353736","asset_id":"08893e9bb60c8072b0692c36f4d11ff0a23c6ba2bbaa85729d951527537061636542756c6c31353736"},"authenticity":"copy"
```

Please note that the response will contain NFT identifiers such as `policy_id`, `asset_name` and `asset_id`, where `asset_id`
is the hex encoded concatenation of `policy_id` and `asset_name`.

## Further information

If you have any questions please reach out to us via email ([team@argusnft.com](mailto:team@argusnft.com)) or on twitter @Argus_NFT
