# Working with Regular Keys

Assign regular keys to your funded account to enable you to sign most transactions with regular keys, while keeping your master keys offline. A master key [compromise](concept-issuing-and-operational-addresses.html) can be difficult to recover from, while removing or changing compromised regular keys is a simpler task.

For an overview of master and regular keys, see [Understanding Master and Regular Keys](reference-transaction-format.html#understanding-master-and-regular-keys).

This tutorial covers:

* [Assigning Regular Keys](#assigning-regular-keys)
* [Removing or Changing Regular Keys](#removing-or-changing-regular-keys)


## Assigning Regular Keys

This tutorial walks you through the steps required to assign regular keys to an account:

1. [Generate a master account and its master keys](#1-generate-a-master-account-and-its-master-keys)
2. [Fund the master account](#2-fund-the-master-account)
3. [Generate a regular account and its regular keys](#3-generate-a-regular-account-and-its-regular-keys)
4. [Assign the regular keys to the master account](#4-assign-the-regular-keys-to-the-master-account)
5. [Submit a test payment using regular keys](#5-submit-a-test-payment-using-regular-keys)
6. [Explore next steps](#6-explore-next-steps)



### 1. Generate an `account_id` and Keys

If you already have a funded account, skip this step.

Use the [`wallet_propose`](reference-rippled.html#wallet-propose) method to generate an `account_id`, along with keys.

Here's an example `wallet_propose` request and response.


#### WebSocket Request

```
{
  "method": "wallet_propose"
}
```

#### WebSocket Response

```
{
  "result": {
    "account_id": "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
    "key_type": "secp256k1",
    "master_key": "MAS TER PRIV ATE KEY RFC 1751 XXX XXXX XXX XXXX XXXX",
    "master_seed": "MasterPrivateKeyXXXXXXXXXXXXX",
    "master_seed_hex": "MasterPrivateKeyHexXXXXXXXXXXXXX",
    "public_key": "aBQEK3r6hb8isnCWmortkNdv3MSMKP74o7gj3kHZQF9vDkbXo4Qs",
    "public_key_hex": "032D911AE9887278110A8FE25169546D4E95A82AF5EFA25525FC3FDFFDC5A8B604"
  },
  "status": "success",
  "type": "response"
}
```


### 2. Create and Fund an Account

If you already have a funded account, skip this step.

An `account_id`, such as the one generated in step 1, is not an actual account until it is funded. The act of sending a [`Payment`](reference-transaction-format.html#payment) transaction to the `account_id` creates the account in the XRP Ledger and funds it. After the account is created and funded, you can assign regular keys to it. ***TODO: Still working on this...***

In the `Payment` request, send the funder's `account_id` as the `Account` value, the funder's `master_seed` (private key) as the `secret` value, and the master account's `account_id` as the `Destination` value. ***TODO: Does not necessarily need to be the master_seed that signs. Could be whatever key the funding account uses to sign transactions.***

Here's an example `Payment` request and response.


#### WebSocket Request

```
{
   "command" : "submit",
   "tx_json" : {
      "TransactionType" : "Payment",
      "Account" : "rPZXdWPxSXCBfnSXGnnBhXjhJ8V5K7oB5s",
      "Destination" : "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "Amount" : "100000000"
   },
   "secret" : "FunderPrivateKeyXXXXXXXXXXXXX"
}
```


#### WebSocket Response

```
{
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "12000022800000002400000001614000000005F5E10068400000000000000A7321029FAB5CE43E10B0743EC6045616D674B0953DF1730EB53DB377D2164EB68A2CC17446304402207AD76AB81C15F44AF77757D796BB6A51DA851C2EF62C24FEF3A7DFC56D8BBB80022040C11F79C52BF15B7D3C5630F894B0047740C30932117D8B971A461FD2682E798114F779A56C00EF101471C8ADED9343532063E73AFE8314E40BE141DFE3544627F98879C9A600974C4C37B3",
    "tx_json": {
      "Account": "rPZXdWPxSXCBfnSXGnnBhXjhJ8V5K7oB5s",
      "Amount": "100000000",
      "Destination": "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 1,
      "SigningPubKey": "029FAB5CE43E10B0743EC6045616D674B0953DF1730EB53DB377D2164EB68A2CC1",
      "TransactionType": "Payment",
      "TxnSignature": "304402207AD76AB81C15F44AF77757D796BB6A51DA851C2EF62C24FEF3A7DFC56D8BBB80022040C11F79C52BF15B7D3C5630F894B0047740C30932117D8B971A461FD2682E79",
      "hash": "D3EDA40C31BC4679C1C429EACF81953065C2D9AD0879F36EA1B6AD6BDFBDD054"
    }
  },
  "status": "success",
  "type": "response"
}
```


### 3. Generate a Regular Account and Its Regular Keys

Use the [`wallet_propose`](reference-rippled.html#wallet-propose) method to generate a second account, along with its key pair to be used as regular keys for the master account.

Here's an example `wallet_propose` request and response.


#### WebSocket Request

```
{
  "method": "wallet_propose"
}
```

#### WebSocket Response

```
{
 "result": {
   "account_id": "rf9vUoqa83foreMr6CYFCEPTZUFagZv83c",
   "key_type": "secp256k1",
   "master_key": "REG ULAR PRIV ATE KEY RFC 1751 XXX XXXX XXX XXXX XXX",
   "master_seed": "RegularPrivateKeyXXXXXXXXXXXX",
   "master_seed_hex": "RegularPrivateKeyHexXXXXXXXXXXXX",
   "public_key": "aBR9dmoHfrfU24q1cbjdehNsFiuV9k5PrrQTLiH42qxhWr5ybc1K",
   "public_key_hex": "03A45E8E4BFECA7473F932839B982C33CD2FB7B788932A21DCDD8536AE63A1614C"
 },
 "status": "success",
 "type": "response"
}
```


### 4. Assign the Regular Keys to the Master Account

Use the [`SetRegularKey`](reference-transaction-format.html#setregularkey) method to assign the regular keys to your master account.

In the request, send the master account's `account_ID` as the `account` value, the master account's `master_seed` (private key) as the `secret` value, and the regular account's `account_id` (generated in step 3) as the `RegularKey` value.

Here's an example `SetRegularKey` request and response.


#### WebSocket Request:

```
{
   "command" : "submit",
   "tx_json" : {
      "TransactionType" : "SetRegularKey",
      "Account" : "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "RegularKey" : "rf9vUoqa83foreMr6CYFCEPTZUFagZv83c"
   },
   "secret" : "MasterPrivateKeyXXXXXXXXXXXXX"
}
```


#### WebSocket Response:

```
{
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200052280000000240000000168400000000000000A7321032D911AE9887278110A8FE25169546D4E95A82AF5EFA25525FC3FDFFDC5A8B604744730450221008EBFA6ABE01BB31A772816FF8560DA78273FA4DC982D0C85A6A516A9188AD70A02205CE1E9E3A685D4D29C62C9DA334BC8BB16468AAA11B572320AD2E98AE1B551BB8114E40BE141DFE3544627F98879C9A600974C4C37B3881443807E97EA8BCB97F4032DEFB4AC9208A359493E",
    "tx_json": {
      "Account": "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "Fee": "10",
      "Flags": 2147483648,
      "RegularKey": "rf9vUoqa83foreMr6CYFCEPTZUFagZv83c",
      "Sequence": 1,
      "SigningPubKey": "032D911AE9887278110A8FE25169546D4E95A82AF5EFA25525FC3FDFFDC5A8B604",
      "TransactionType": "SetRegularKey",
      "TxnSignature": "30450221008EBFA6ABE01BB31A772816FF8560DA78273FA4DC982D0C85A6A516A9188AD70A02205CE1E9E3A685D4D29C62C9DA334BC8BB16468AAA11B572320AD2E98AE1B551BB",
      "hash": "A9EE659DC08E6589F3454186C1D846763E55DEEF5525A8AE1F3D87B53EF5B86A"
    }
  },
  "status": "success",
  "type": "response"
}
```


### 5. Submit a Test Payment Using Regular Keys

To verify that the regular keys are set correctly on the master account, submit a test payment using the [`Payment`](reference-transaction-format.html#payment) transaction type using the regular keys.

In the request, send the regular account's `master_seed` (regular private key) (generated in step 3) as the `secret` value and the master account's `account_id` as the `Account` value.

Here's an example `Payment` request and response.


#### WebSocket Request:

```
{
   "method":"submit",
   "secret":"RegularPrivateKeyXXXXXXXXXXXX",
   "tx_json": {
         "Account":"rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
         "Fee":"10",
         "Amount":"10000000",
         "Destination":"rPZXdWPxSXCBfnSXGnnBhXjhJ8V5K7oB5s",
         "TransactionType":"Payment"
      }
}
```


#### WebSocket Response:

***TODO: Per earlier comment on a concepts doc about signatures, I think it would be useful to be able to link to such a doc to call out the SigningPubKey and/or TxnSignature. I think it's relevant in that the keys topic is surfaced via a discussion of transaction authorization via signing.***

```
{
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200002280000000240000000361400000000098968068400000000000000A732103A45E8E4BFECA7473F932839B982C33CD2FB7B788932A21DCDD8536AE63A1614C74463044022037DC86BEE1D0E1F394273B80E4072A35B2E221880879CC2C47FE0AD152B3E2120220511A6555B8A524321E962164EC830A83267AF97E9FDF24CCEC7B38944E2CC95C8114E40BE141DFE3544627F98879C9A600974C4C37B38314F779A56C00EF101471C8ADED9343532063E73AFE",
    "tx_json": {
      "Account": "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "Amount": "10000000",
      "Destination": "rPZXdWPxSXCBfnSXGnnBhXjhJ8V5K7oB5s",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 3,
      "SigningPubKey": "03A45E8E4BFECA7473F932839B982C33CD2FB7B788932A21DCDD8536AE63A1614C",
      "TransactionType": "Payment",
      "TxnSignature": "3044022037DC86BEE1D0E1F394273B80E4072A35B2E221880879CC2C47FE0AD152B3E2120220511A6555B8A524321E962164EC830A83267AF97E9FDF24CCEC7B38944E2CC95C",
      "hash": "A8C3D091245794A664A5C1F0D275D1DB9A9349F1F873AABAD7AD1E43E8A117FB"
    }
  },
  "status": "success",
  "type": "response"
}
```

### 6. Explore Next Steps

Now that you're familiar with the benefits of assigning regular keys to an account, consider taking a look at these related topics and tutorials:

* [How to Multi-Sign](tutorial-multisign.html)
* [Issuing and Operational Addresses](concept-issuing-and-operational-addresses.html)
* [Listing XRP as an Exchange](tutorial-listing-xrp.html)


## Removing or Changing Regular keys

If your regular keys are compromised, or if you just want to periodically change regular keys as a security measure, use the [`SetRegularKey`](reference-transaction-format.html#setregularkey) method to remove or change the regular keys for the master account. ***TODO: I believe the first if is valid. How about the second if? Is this something that someone might want to do?***

To change your regular keys, see steps 3 - 5 of [Assigning Regular Keys](#assigning-regular-keys).

If you want to remove compromised regular keys and assign new regular keys later, use the [`SetRegularKey`](reference-transaction-format.html#setregularkey) method, omitting the `RegularKey` value. Here's an example `SetRegularKey` request and response that removes any existing regular keys.

***TODO: Is it valid to treat these are two separate flows? Change vs Remove? If the keys are compromised, you may want to just quickly remove the key and generate and assign new keys later, or not at all. If you are just doing security maintenance, it may be more convenient to change them using a single method call.***

#### WebSocket Request:

```
{
   "command" : "submit",
   "tx_json" : {
      "TransactionType" : "SetRegularKey",
      "Account" : "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt"
   },
   "secret" : "MasterPrivateKeyXXXXXXXXXXXXX"
}
```


#### WebSocket Response:

```
{
  "result": {
    "engine_result": "tesSUCCESS",
    "engine_result_code": 0,
    "engine_result_message": "The transaction was applied. Only final in a validated ledger.",
    "tx_blob": "1200052280000000240000000468400000000000000A7321032D911AE9887278110A8FE25169546D4E95A82AF5EFA25525FC3FDFFDC5A8B60474463044022035B3076FED158D1CBFE2898521BDB068C6A2BE5C1BEA111D7F8611B1A35A10C302204CD5F547A7538C70404A5A157F5C1D4607B1F739C6C70D33895012E5B83E301E8114E40BE141DFE3544627F98879C9A600974C4C37B3",
    "tx_json": {
      "Account": "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "Fee": "10",
      "Flags": 2147483648,
      "Sequence": 4,
      "SigningPubKey": "032D911AE9887278110A8FE25169546D4E95A82AF5EFA25525FC3FDFFDC5A8B604",
      "TransactionType": "SetRegularKey",
      "TxnSignature": "3044022035B3076FED158D1CBFE2898521BDB068C6A2BE5C1BEA111D7F8611B1A35A10C302204CD5F547A7538C70404A5A157F5C1D4607B1F739C6C70D33895012E5B83E301E",
      "hash": "81F8D1D892D2A18814337470B02DD7AFA123CDDF85150DCE333674435254D046"
    }
  },
  "status": "success",
  "type": "response"
}
```

Here's an example response for a `Payment` transaction signed using an invalid regular key.

#### WebSocket Response:

```
{
  "error": "badSecret",
  "error_code": 41,
  "error_message": "Secret does not match account.",
  "request": {
    "method": "submit",
    "secret": "RegularPrivateKeyXXXXXXXXXXXX",
    "tx_json": {
      "Account": "rM8oJwkKtgEU2HZkCHsJScLHqegWpgBRxt",
      "Amount": "10000000",
      "Destination": "rPZXdWPxSXCBfnSXGnnBhXjhJ8V5K7oB5s",
      "Fee": "10",
      "TransactionType": "Payment"
    }
  },
  "status": "error",
  "type": "response"
}
```

In some cases, you can even send a [key reset transaction](concept-transaction-cost.html#key-reset-transaction) without paying the [transaction cost](reference-transaction-format.html#transaction-cost).

***TODO: I don't understand how this key reset transaction mentioned above works. Is it different from sending a SetRegularKey transaction that omits or changes the RegularKey value assigned to an account? Is the difference that the transaction cost can be 0? Does this mean that I can send a SetRegularKey transaction that omits or changes the RegularKey value assigned to an account and includes `Fee : 0` - as long as the `lsfPasswordSpent` flag is disabled? When I send such a transaction for a newly funded account that has had no activity on it other than one payment to fund it and one SetRegularKeys transaction for a `Fee : 10`, I get a response that says: `Fee insufficient`.***

***TODO: I understand that the account's `lsfPasswordSpent` flag must be disabled and that it starts out disabled. We say: "As a special case, an account can send a SetRegularKey transaction with a transaction cost of 0, as long as the account's lsfPasswordSpent flag is disabled. This transaction must be signed by the account's master key pair. Sending this transaction enables the lsfPasswordSpent flag. The lsfPasswordSpent flag starts out disabled. It gets enabled when you send a SetRegularKey transaction signed by the master key pair. It gets disabled again when the account receives a Payment of XRP." When we say that sending the transaction enables the flag - this means that it started off as disabled, fulfilled the `Fee : 0` key reset, and then enabled the flag to deny any further SetRegularKey with `Fee : 0` transaction requests? And it will continue to deny these requests until the account receives a payment? This means that if you are afraid that your account balance is close to 0, you should use your one SetRegularKey with `Fee : 0` to change your RegularKey value and not just remove it. If you remove it, then you won't be able to pay for the transaction to set a new value, correct?***

***TODO: This piece of info is important -- but until I understand the key reset transaction, it is difficult to explain when this prioritization will happen: "When the FeeEscalation amendment is enabled, rippled prioritizes key reset transactions above other transactions even though the nominal transaction cost of a key reset transaction is zero.***