# Checks

In the XRP Ledger (XRPL), a Check is similar to a personal paper check. Like traditional checks, XRPL Checks start with the sender of the funds creating a Check that specifies an amount and receiver. The receiver cashes the check to pull the funds from the sender's account into the receiver's account. No money moves until the receiver cashes the Check. Because funds are not put on hold when the Check is created, cashing a Check can fail if the sender doesn't have enough funds when the receiver tries to cash it, just like traditional checks. If there's a failure cashing the check, the sender can retry until the check expires.

Unlike traditional checks, XRPL checks have expiration dates after which they may no longer be cashed. This is to prevent an unusable Check object from cluttering the Ledger. If the receiver doesn't successfully cash the Check before it expires, the Check object remains in the XRP Ledger until someone cancels it. Anyone may cancel the Check after it expires. Only the sender and receiver can cancel the Check before it expires or is cashed. The Check object is removed from the Ledger when the sender cashes the check or someone cancels it.

Checks are similar to [Escrow](https://ripple.com/build/escrow/#escrow) and [Payment Channels](https://ripple.com/build/payment-channels-tutorial/), two other asynchronous transfer methods available on the XRP Ledger. There are two important differences between those methods and Checks:

* You can send issued currency with Checks. You can only use XRP with Payment Channels and Escrow.

* Checks do not tie up any funds. The XRP involved in Payment Channels and Escrow cannot be spent after until it is released by the sender (Payment Channels), an expiration, or crypto-condition (Escrow).


***Note:*** The [Checks](https://ripple.com/build/known-amendments/#checks) amendment changes the [OfferCreate transaction](https://ripple.com/build/transactions/#offercreate) to return `tecEXPIRED` when trying to create an Offer whose expiration time is in the past. Without this amendment, an OfferCreate whose expiration time is in the past returns `tesSUCCESS` but does not create or execute an Offer.

## Why Checks?

Traditional checks allow people to transfer balances without immediately exchanging physical currency. XRPL Checks allow people to exchange funds asynchronously using a process that is familiar to and accepted by the banking industry.

XRPL Checks also solve a problem that is unique to the XRP Ledger: they allow users to reject unwanted payments or accept only a portion of a payment. This is useful for institutions that need to be careful about accepting payments for compliance reasons.

Checks potentially enable many other use cases. Ripple encourages the community to find new and creative applications for Checks.


### Use Case: Payment Authorization

**Problem:** To comply with regulations like [BSA, KYC, AML, and CFT](https://ripple.com/build/gateway-guide/#gateway-compliance), financial institutions must provide documentation about the source of funds they receive. Such regulations seek to prevent the illicit transfer of funds by requiring institutions to disclose the source and destination of all payments processed by the institution. Because of the nature of the XRP Ledger, anyone could potentially send XRP (and, under the right circumstances, issued currencies) to an institution's account on the XRP Ledger. Dealing with such unwanted payments adds significant overhead cost to these institutions' compliance departments, including potential fines or penalties.

**Solution:** Institutions can enable [Deposit Authorization](https://ripple.com/build/deposit-authorization/#deposit-authorization) on their XRP Ledger accounts by [setting the `asfDepositAuth` flag in an `AccountSet` transaction](https://ripple.com/build/transactions/#accountset-flags). This makes the account unable to accept Payment transactions. The only way for accounts with Deposit Authorization set to receive funds is through Escrow, Payment Channels, or Checks. Checks are the most straightforward, familiar, and flexible way to transfer funds if Deposit Authorization is enabled.


## Usage

Checks typically have the lifecycle described below.

<!--{# Diagram sources: https://docs.google.com/drawings/d/1Ez8OZVB2TLH-b_kSFOAgfYqXlEQt4KaUBW6F3TJAv_Q/edit #}-->


[![Check flow diagram (successful cashing)](img/checks-happy_path.png)](img/checks-happy_path.png)

**Step 1:** To create a Check, the sender submits a [CheckCreate](https://ripple.com/build/transactions/#checkcreate) transaction and specifies the receiver (`Destination`), expiration date (`Expiration`), and maximum amount that may be debited from the sender's account (`SendMax`).


**Step 2:** After the CheckCreate transaction is processed, a [Check object](https://ripple.com/build/ledger-format/#check) (`tlCheck`) is created on the XRP Ledger. This object contains the properties of the Check as defined by the transaction that created it. The object can only be modified by the sender (by canceling it with a [CheckCancel](https://ripple.com/build/transactions/#checkcancel) transaction) or receiver (by canceling it or cashing it) before the expiration time is reached. After the expiration time, anyone may cancel the Check.

**Step 3:** To cash the check, the receiver submits a [CheckCash](https://ripple.com/build/transactions/#checkcash) transaction. The receiver has two options for cashing the check:

* `Amount` — The receiver can use this option to specify an exact amount to cash. This may be useful for cases where the sender has padded the check to cover possible [transfer fees](https://ripple.com/build/transfer-fees/) and the receiver can only accept the exact amount on an invoice or other contract.

* `DeliverMin` — The receiver can use this option to specify the minimum amount they are willing to receive from the check. If the receiver uses this option, `rippled` attempts to deliver as much as possible and will deliver at least this amount. The transaction fails if the sender doesn't have at least this amount in their account.

If the sender has enough funds to cover the Check and the expiration date has not passed, the funds are debited from the sender's account and credited to the receiver's account, and the Check object is is destroyed.



#### Expiration Case

In the case of expirations, Checks have the lifecycle described below.

<!--{# Diagram sources: https://docs.google.com/drawings/d/1JOgI3H5tpV1yasYe5WLrdxVXLhcQu0bhPfN0mzzS1YQ/edit #}-->


[![Check flow diagram (expiration)](img/checks-expiration.png)](img/checks-expiration.png)


All Checks start the same way, so **Steps 1 and 2** are the same.

**Step 3a:** If the Check expires before the receiver can cash it, the Check can no longer be cashed but remains in the Ledger.

**Step 4a:** After a Check expires, anyone may cancel it by submitting a [CheckCancel](https://ripple.com/build/transactions/#checkcancel) transaction. That transaction removes the Check from the Ledger.  



## Availability of Checks

Checks were enabled in `rippled` v0.90.0 with the  ["Checks" amendment](https://ripple.com/build/known-amendments/#checks).

You can check the status of the Checks amendment using the [`feature` command](https://ripple.com/build/rippled-apis/#feature).


## Further Reading

For more information about Checks in the XRP Ledger, see:

<!--{#TODO: add link to Checks tutorial#}-->

* [Transaction Reference](https://ripple.com/build/transactions/#transaction-types)
  * [Checks amendment](https://ripple.com/build/known-amendments/#checks)
  * [CheckCreate](https://ripple.com/build/transactions/#checkcreate)
  * [CheckCash](https://ripple.com/build/transactions/#checkcash)
  * [CheckCancel](https://ripple.com/build/transactions/#checkcancel)

For more information about related features, see:

* [Deposit Authorization](https://ripple.com/build/deposit-authorization/)
* [Escrow](https://ripple.com/build/escrow/)
* [Payment Channels Tutorial](https://ripple.com/build/payment-channels-tutorial/)