# ProfitSplitter
Profit splitter contract to Pay your Associate-level employees quickly and easily. Using solidity 

![contract](Screenshots/pic.png)

In this project my aim was to create a solidity contract that allowed me to evenly distribute payment to my associate level employees. Since there salary is only a base pay we can just send in the amount we need and let the contract evenly distribute payments. Below you can see first the contract deployed on my private ganache network and a payment of 40 ether split up between the 3 employees. After that we can see the contract deployed to the Ropsten network ready to be used. 

### Starting the project

Navigate to the [Remix IDE](https://remix.ethereum.org) and create a new contract called `AssociateProfitSplitter.sol` using the starter code for above.

While developing and testing your contract, use the [Ganache](https://www.trufflesuite.com/ganache) development chain, and point MetaMask to `localhost:8545`, or replace the port with what you have set in your workspace.

### Level One: The `AssociateProfitSplitter` Contract

At the top of your contract, you will need to define the following `public` variables:
13
* `employee_one` -- The `address` of the first employee. Make sure to set this to `payable`. Address used 0x5C66D90AD51DC68C883c436815c852CB0C936A21

* `employee_two` -- Another `address payable` that represents the second employee. Address used 0xd6b89833AAcA5a07Cf49dcB58a16A143eF05E4f5

* `employee_three` -- The third `address payable` that represents the third employee. Address used 0x57F16564b9D5da896821d5cD95d95503e02F8847

Create a constructor function that accepts:

* `address payable _one`

* `address payable _two`

* `address payable _three`

Within the constructor, set the employee addresses to equal the parameter values. This will allow you to avoid hardcoding the employee addresses.

Next, create the following functions:

* `balance` -- This function should be set to `public view returns(uint)`, and must return the contract's current balance. Since we should always be sending Ether to the beneficiaries, this function should always return `0`. If it does not, the `deposit` function is not handling the remainders properly and should be fixed. This will serve as a test function of sorts.

* `deposit` -- This function should set to `public payable` check, ensuring that only the owner can call the function.

  * In this function, perform the following steps:

    * Set a `uint amount` to equal `msg.value / 3;` in order to calculate the split value of the Ether.

    * Transfer the `amount` to `employee_one`.

    * Repeat the steps for `employee_two` and `employee_three`.

    * Since `uint` only contains positive whole numbers, and Solidity does not fully support float/decimals, we must deal with a potential remainder at the end of this function since `amount` will discard the remainder during division.

    * We may either have `1` or `2` wei leftover, so transfer the `msg.value - amount * 3` back to `msg.sender`. This will re-multiply the `amount` by 3, then subtract it from the `msg.value` to account for any leftover wei, and send it back to Human Resources.

* Create a fallback function using `function() external payable`, and call the `deposit` function from within it. This will ensure that the logic in `deposit` executes if Ether is sent directly to the contract. This is important to prevent Ether from being locked in the contract since we don't have a `withdraw` function in this use-case.

#### Test the contract

In the `Deploy` tab in Remix, deploy the contract to your local Ganache chain by connecting to `Injected Web3` and ensuring MetaMask is pointed to `localhost:8545`. Also ensure that you have 4 accounts ready to for the contract.
![contract](Screenshots/gnache_acc.png)

Once you click deploy you will be charged with meta mask and if you are able to pay a green check mark should appear on remix

![contract](Screenshots/succ.png)

You will need to fill in the constructor parameters with your designated `employee` addresses. Make sure the account that you deployed the contract with has enough ether to send to the other 3 accounts.

![contract](Screenshots/deploying_contract.png)

Test the `deposit` function by sending various values. Keep an eye on the `employee` balances as you send different amounts of Ether to the contract and ensure the logic is executing properly. Sending 40 ether to the 3 associate employee accounts. 

![contract](Screenshots/deposit.png)

As you can see below we have subtracted 40 ether from the original account and split it between the 3 designated accounts. 
![contract](Screenshots/gan_ether_split.png)


 As you can see you can also deploy the contract on the Ropsten network and as long as you have enough ether you can send some to the accounts designated at deployment using the same steps as above.

![ropsten](Screenshots/rop_test.png)

ropsten contractID 0x16c3335d6bce1e39ce6604aee63bdc2f0dfc66ab71f90262dc4d57503f435c1b
![ropsten](Screenshots/rop.png)
