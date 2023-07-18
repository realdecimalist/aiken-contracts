# aiken-contracts

**Modified Vesting Contract**

This code represents a simple version of a smart contract for a vesting mechanism, which is commonly used in financial and investment situations.

In essence, the contract locks up a certain amount of a cryptocurrency (in this case, ADA from the Cardano blockchain) and only allows it to be retrieved under certain conditions.

The smart contract is defined in two parts: on-chain code and off-chain code.

**On-chain code:** This is the code that gets deployed to the blockchain as a smart contract. The contract defines the rules for how the locked funds can be accessed. The rules are defined in the Datum type and the vesting validator function.

The Datum type specifies three pieces of information:

lock_until: A timestamp that represents the time when the funds can be accessed by the beneficiary.
return_time: A timestamp that represents the time when the funds can be returned back to the sender if not retrieved by the beneficiary.
owner: The public key hash of the owner who can retrieve the funds after return_time.
beneficiary: The public key hash of the beneficiary who can retrieve the funds after lock_until time and before return_time.
The vesting validator function then uses these pieces of information to define the conditions under which the funds can be retrieved from the contract:

The funds can be retrieved by the owner at any time.
The funds can be retrieved by the beneficiary after the lock_until time has passed.
The funds can be retrieved by the owner if the return_time has passed and the beneficiary has not yet retrieved the funds.
Off-chain code: This is the code that interacts with the on-chain contract from outside the blockchain. It is responsible for creating and signing transactions, as well as submitting them to the blockchain.

**The off-chain code does the following:**

It locks a certain amount of ADA in the contract with the specified Datum.
It waits for the lock_until time to pass.
It then retrieves the funds from the contract if it is the beneficiary and the current time is after lock_until and before return_time.
If the return_time has passed and the beneficiary has not yet retrieved the funds, the owner can retrieve the funds.


The example vesting contract from the Aiken documentation (found here: https://aiken-lang.org/example--vesting) and this modified contract are both examples of a vesting contract, which locks funds until a certain time has passed. However, there are two key differences between them:

1. **Ability for the beneficiary to retrieve funds at any time:** In the example contract, the beneficiary can only retrieve the funds after the lock_until time has passed. In the modified contract, the beneficiary can retrieve the funds at any time after the contract has been deployed. This change makes the vesting contract more flexible by allowing the beneficiary to access the funds earlier if necessary.

2. **Returning locked funds to the sender:** In the example contract, if the beneficiary does not retrieve the funds after the lock_until time has passed, the funds remain locked in the contract. In the modified contract, if the beneficiary does not retrieve the funds within 2 days after the lock_until time has passed, the funds are returned to the sender. This change ensures that the funds are not locked indefinitely if the beneficiary does not retrieve them.

These modifications were achieved by adding a new field return_time to the Datum type, and by updating the vesting validator function to check the new conditions.
