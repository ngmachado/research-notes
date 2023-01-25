
#erc20 #ida #agreement #superfluid

### What does this mean?

Use ERC20 balanceOf to know how much units or distribution weight receiver of IDA should get.

### **Roles in IDA**

IDA **Publisher**: administrative role like setting units, and paying for distributions.

IDA **Subscriber**: Only approve/revoke subscription from publisher.

Publisher is **sender** and Subscriber is **receiver**

From Superfluid Notion docs:

### IDA:

**publisher** (sender) API:

`function createIndex(token, indexId)`

`function setSubscription(token, indexId, subscriber, units)`

`function setSubscriptions(token, indexId, subscribers[], units[])`

`function distribute(token, indexId, amount)`

**subscriber** (receiver) API:

`function approveSubscription(token, publisher, indexId)`

`function revokeSubscription(token, publisher, indexId)`

### IDA:

**publisher** (sender) API:

### Sender perspective

### CreateIndex

`function createIndex(token, indexId)`

In new version, `createIndex` would be equivalent of deploying a new ERC20 index Token with support of ERC165

`token` is SuperToken address

`indexId` is ERC20 index Token address

Detail: deployer can be owner or not of ERC20 token itself.

### Set Subscription

`function setSubscription(token, indexId, subscriber, units)`

Operations do give subscriptions are now simple ERC20 operations like transfer and burn.

Detail: deployer can decide what operations to support on ERC20 contract

Batch operations for transfer/burn operations

`function setSubscriptions(token, indexId, subscribers[], units[])`

Batch operations for transfer/burn operations

### Distribute

`function distribute(token, indexId, amount)`

Function to move token balances from sender to receivers. Takes into consideration receiver weight (units to distribute)

_**note: i think this can be included on ERC20 transfer itself. self transfer = Transfer(address(Index)) to kick distribution logic.**_

### Receiver perspective

**subscriber** (receiver) API:

### approveSubscription

`function approveSubscription(token, publisher, indexId)`

TODO: open discussion. Open Slots, Restricted Slots, etc.

`function revokeSubscription(token, publisher, indexId)`

If weight is zero, subscription is revoke automatic.

Detail: This is implying that ERC20 operations will work SuperToken integration Hooks.


### Cool things

**Receiver can transfer units to multi accounts.** This open a massive integration with DeFi.

-   Uniswap get/sell some units, LP on Aave get aToken. aToken is distributed to a new IDA index ;)
    
-   Depending on ERC20 index, receiver can sell future revenues by simply transfer tokens. No complicated process.
    
-   Nothing blocks ERC20 Index being wrapped as SuperToken and streamed out.
    
-   Each index is ERC20 contract that can include custom logic. We can support by default a set of use cases like non-minting, not transferable, etc. But by extending base contract “Publisher” can support more fine tune use cases.
    

**IDA Units = Blue Elephant**