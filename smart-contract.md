# 📔 Smart Contract

The Upgradeable contracts mean they are behind proxies which means they are mutable and their code can change over time. There are very good reasons why we have chosen this design:

* We want the project to be long term so we want to be able to add features and fix bugs without having to redeploy a new ERC-721 token every time since it would be a terrible hassle for the holders to migrate all the time there is a change in the contract

{% hint style="info" %}
When Ring and its forks were hacked they had to redeploy their node contract to fix it which forced them to send a Google Form to its holders to restore their nodes. A very cumbersome process for both the holders and the team and a lot of nodes are still missing, such as in Louverture to quote a few.
{% endhint %}

* Using a proxy for `PlanetsManagerUpgradeable` allows us to retain in memory the data like the holders, the planets they own, the tiers and rewards they have while still being able to fix bugs and add features as the roadmap goes on
* For `WalletObserverUpgradeable`, making it upgradeable allow us to keep in memory who bought how much, their limits as well as the malicious addresses that have been blocked in the past

We know proxies aren't perfect and we will be working toward setting them behind a Timelock + Multi-sig when we figure out how to do it. Crafting transactions to a proxy from a multi-sig to a timelock can become difficult and we need to extensively test it before it goes into production to avoid a lockout.

The main Universe contract which is a standard ERC-20 is however forever immutable. The `accountBurn` and `accountReward` functions were added to it to allow the `PlanetsManagerUpgradeable` to dynamically burn and mint rewards to the holders without having the need to approve the use of the token. The `liquidityReward` function was added to actually transfer the [fees collected](broken-reference) to the `LiquidityPoolManager`.

The main Universe contract which is a standard ERC-20 is however forever immutable. The `accountBurn` and `accountReward` functions were added to it to allow the PlanetsManagerUpgradeable to dynamically burn and mint rewards to the holders without needing to approve the use of the token. The liquidityReward function was added to transfer the [fees collected](broken-reference) to the `LiquidityPoolManager`.

`LiquidityPoolManager` is also immutable but can be replaced by redeploying the contract and updating the `LiquidityPoolManager` implementation on all the other contracts as it doesn't contain data that need to be retained.
