# Native Ice Phishing Bot

## Description

This bot monitors:

- Transactions where the receiver is either an externally-owned account (EOA) or a suspicious contract, and the input data is the hash of a known function signature.
- When a suspicious EOA receives native tokens from a number of different EOAs that exceeds a certain threshold.

## Supported Chains

- Ethereum
- Arbitrum
- Avalanche
- BNB Smart Chain
- Fantom
- Optimism
- Polygon

## Alerts

- NIP-1
  - Fired when the receiver of a transaction is an EOA, the value is non-zero and the input data is the hash of a known function signature.
  - Severity is always set to "Medium"
  - Type is always set to "Suspicious"
  - Metadata contains:
    - `attacker`: The receiver of the transaction
    - `victim`: The initiator of the transaction
    - `funcSig`: The function signature in the transaction input
    - `anomalyScore`: The anomaly score of the alert
  - Labels contain:
    - Label 1:
      - `entity`: The transaction's hash
      - `entityType`: The type of the entity, always set to "Transaction"
      - `label`: The type of the label, always set to "Attack"
      - `confidence`: The confidence level of the transaction being an attack (0-1), always set to 0.9
    - Label 2:
      - `entity`: The transaction initiator address
      - `entityType`: The type of the entity, always set to "Address"
      - `label`: The type of the label, always set to "Victim"
      - `confidence`^: The confidence level of the address being a victim (0-1), always set to 0.9
    - Label 3:
      - `entity`: The transaction receiver address
      - `entityType`: The type of the entity, always set to "Address"
      - `label`: The type of the label, always set to "Attacker"
      - `confidence`^: The confidence level of the receiver being an attacker (0-1), always set to 0.9
- NIP-2
  - Fired when the receiver of a transaction is an EOA, the value is 0 and the input data is the hash of a known function signature.
  - Severity is always set to "Info"
  - Type is always set to "Suspicious"
  - Metadata contains:
    - `attacker`: The receiver of the transaction
    - `victim`: The initiator of the transaction
    - `funcSig`: The function signature in the transaction input
    - `anomalyScore`: The anomaly score of the alert
  - Labels contain:
    - Label 1:
      - `entity`: The transaction's hash
      - `entityType`: The type of the entity, always set to "Transaction"
      - `label`: The type of the label, always set to "Attack"
      - `confidence`: The confidence level of the transaction being an attack (0-1), always set to 0.6
    - Label 2:
      - `entity`: The transaction initiator address
      - `entityType`: The type of the entity, always set to "Address"
      - `label`: The type of the label, always set to "Victim"
      - `confidence`^: The confidence level of the address being a victim (0-1), always set to 0.6
    - Label 3:
      - `entity`: The transaction receiver address
      - `entityType`: The type of the entity, always set to "Address"
      - `label`: The type of the label, always set to "Attacker"
      - `confidence`^: The confidence level of the receiver being an attacker (0-1), always set to 0.6
  - NIP-3
    - Fired when the receiver of a transaction is a contract, the value is non-zero, the function called is one that has previously created one NIP-1 alert, and the number of exposed contract functions is under a threshold.
    - Severity is always set to "Low"
    - Type is always set to "Suspicious"
    - Metadata contains:
      - `attacker`: The receiver of the transaction
      - `victim`: The initiator of the transaction
      - `funcSig`: The function signature in the transaction input
      - `anomalyScore`: The anomaly score of the alert
    - Labels contain:
      - Label 1:
        - `entity`: The transaction's hash
        - `entityType`: The type of the entity, always set to "Transaction"
        - `label`: The type of the label, always set to "Attack"
        - `confidence`: The confidence level of the transaction being an attack (0-1), always set to 0.6
      - Label 2:
        - `entity`: The transaction initiator address
        - `entityType`: The type of the entity, always set to "Address"
        - `label`: The type of the label, always set to "Victim"
        - `confidence`^: The confidence level of the address being a victim (0-1), always set to 0.6
      - Label 3:
        - `entity`: The transaction receiver address
        - `entityType`: The type of the entity, always set to "Address"
        - `label`: The type of the label, always set to "Attacker"
        - `confidence`^: The confidence level of the receiver being an attacker (0-1), always set to 0.6
  - NIP-4
    - Fired when a suspicious EOA receives funds from an over a threshold number of different EOAs.
    - Severity is always set to "High"
    - Type is always set to "Suspicious"
    - Metadata contains:
      - `attacker`: The receiver of the transaction
      - `victim`: The initiator of the transaction
      - `funcSig`: The function signature in the transaction input
      - `anomalyScore`: The anomaly score of the alert
    - Labels contain:
      - Label 1:
        - `entity`: The transaction receiver address
        - `entityType`: The type of the entity, always set to "Address"
        - `label`: The type of the label, always set to "Attacker"
        - `confidence`^: The confidence level of the receiver being an attacker (0-1), always set to 0.7
      - Label #:
        - `entity`: The victim address
        - `entityType`: The type of the entity, always set to "Address"
        - `label`: The type of the label, always set to "Victim"
        - `confidence`^: The confidence level of the address being a victim (0-1), always set to 0.7

## Test Data

The bot behaviour can be verified with the following transactions on Ethereum Mainnet:

- [0x28aec33f80d6d62965e524f9f97660cc0efff6aff1ebe4902c7849b06070f3cc](https://etherscan.io/tx/0x28aec33f80d6d62965e524f9f97660cc0efff6aff1ebe4902c7849b06070f3cc) (NIP-1 alert)
- [0x80bb173bed260691b72117849f198fbf467238e6001e6ff772412c3179d2b2c6](https://etherscan.io/tx/0x80bb173bed260691b72117849f198fbf467238e6001e6ff772412c3179d2b2c6) (NIP-2 alert)