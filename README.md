# Digital-Asset-Rules

1 **Introduction**

In the blockchain space in general, and on the Tezos blockchain in particular, there is a lot of interest in and momentum behind STOs, where a real-world asset is tokenized. The nature of these tokenized security offerings frequently requires smart contract functions that go beyond simple transfer functionality. For example, a designated owner address may be allowed to reissue tokens or whitelist a particular address before it may receive tokens.
Where banks and other financial institutions are involved, private-key handling is frequently outsourced to a third-party storage provider. The above-mentioned on-chain functionality is then accessed through APIs provided by the storage provider. Without proper standardization, this has the potential to lead to extensive fragmentation in terms of the provided APIs, and limits the possibility for an STO platform to support multiple storage providers.
Often, standardization is done on the smart contract level, for example via the well-known ERC specifications. From the above perspective, however, this has multiple drawbacks: First, there is ample reason for a smart contract standard to offer a larger feature set than might be warranted for the storage provider to support based on their customer demand. For example, even the relatively simple ERC-20 standard supports transfer on behalf of others which is often not needed. Also, the interests of a multitude of other stakeholders often results in very general and flexible APIs whose one-to-one implementation is prohibitive for storage providers to support from both a cost and security perspective.


Therefore, the goal of this document is to describe a minimal set of APIs as they are foreseen to be consumed by an STO platform. Without trying to anticipate the exact smart contract API, we believe this is the functionality that any well-received STO smart contract is bound to support. Note in particular that this includes the possibility that some of the functions described here end up being delegated to other smart contracts attached to the token smart contract.
This absolves interested integration parties from awaiting the results of lengthy standardization processes. We hope this document can help in driving discussions forward and in helping to ensure a vibrant STO landscape on the Tezos blockchain.

**2 Preliminary Remarks**

The next chapter will contain the description of the APIs. Here, we include some brief remarks that will help provide rationale and perspective for them and point out some limitations.

**2.1 FA2 as a token basis**

As mentioned in the introduction, the API standardization suggested here strives to stay independent of the underlying smart contract API. Nevertheless, we recognize that the FA2 standard has emerged as the clear leader in the Tezos token landscape. Where applicable,
therefore, we also briefly discuss how the proposed APIs map onto the versions provided by FA2.
One beneficial consequence of assuming an FA2-like token is the ability to assume multi-asset support. For the STO use-case, this can significantly simplify administration when multiple similar tokens can be subject to the same ownership and administration rules.
For reasons detailed in the introduction, we don’t foresee full FA2 support to be necessary. Indeed, this document only makes use of the “transfer” FA2 endpoint.

**2.2 Write operations only**

This document focuses on smart contract interactions that change the underlying contract state. Read-only functions may also be desired, but they generally pose fewer integration hurdles. Also, in the case of off-chain use, they can be obtained by querying the node instead.

**2.3 Token ownership**

Some administrative functions listed below require access control. For simplicity, we assume that this is provided by a fixed contract owner address, specified at the time of contract creation. Ownership is shared among all tokens within a particular multi-asset contract.

**2.4 Limitations**

This document is not a specification. Differing circumstances and constraints among different storage providers preclude identical implementations. Given that, we still believe it is helpful to strive for a common feature set and to drive alignment as far as circumstances permit.
A particularly hard to foresee tradeoff involves the level of batch-processing provided by APIs. For example, the FA2 transfer function allows for batch-processing on multiple levels to save transaction costs. However, for security and user experience reasons, it is often advantageous to restrict the level of batch-processing. For simplicity therefore, this document presents all APIs in their singular form, while recognizing that more nested versions thereof will result in the end.
