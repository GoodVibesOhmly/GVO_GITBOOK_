# Equations

## Staking

$$
deposit = withdrawal
$$

Swaps between INDEX and sINDEX during staking and unstaking are always honored 1:1. The amount of INDEX deposited into the staking contract will always result in the same amount of sINDEX. And the amount of sINDEX withdrawn from the staking contract will always result in the same amount of INDEX.

$$
rebase = 1 - ( INDEXDeposits / sINDEXOutstanding )
$$

The treasury deposits INDEX into the distributor. The distributor then deposits INDEX into the staking contract, creating an imbalance between INDEX and sINDEX. sINDEX is rebased to correct this imbalance between INDEX deposited and sINDEX outstanding. The rebase brings sINDEX outstanding back up to parity so that 1 sINDEX equals 1 staked INDEX.

## Minting

$$
mint Price = 1 + Premium
$$

INDEX has an intrinsic value of 1 MIM, which is roughly equivalent to $1. In order to make a profit from minting, Index DAO charges a premium for each bond.

$$
Premium = debt Ratio * BCV
$$

The premium is derived from the debt ratio of the system and a scaling variable called [BCV](https://docs.olympusdao.finance/references/glossary#bcv). BCV allows us to control the rate at which mint prices increase.

The premium determines profit due to the protocol and in turn, stakers. This is because new INDEX is minted from the profit and subsequently distributed among all stakers.

$$
debt Ratio = mintsOutstanding/INDEXSupply
$$

The debt ratio is the total of all INDEX promised to minters divided by the total supply of INDEX. This allows us to measure the debt of the system.

$$
mintPayout_{reserveBond} = marketValue_{asset}\ /\ mintPrice
$$

Mint payout determines the number of INDEX sold to a minter. For reserve bonds, the market value of the assets supplied by the minter is used to determine the bond payout. For example, if a user supplies 1000 MIM and the mint price is 250 MIM, the user will be entitled 4 INDEX.

$$
mintPayout_{lpBond} = marketValue_{lpToken}\ /\ mintPrice
$$

For liquidity bonds, the market value of the LP tokens supplied by the minter is used to determine the bond payout. For example, if a user supplies 0.001 INDEX-MIM LP token which is valued at 1000 MIM at the time of bonding, and the mint price is 250 MIM, the user will be entitled 4 INDEX.

## INDEX Supply

$$
INDEX_{supplyGrowth} = INDEX_{stakers} + INDEX_{minters} + INDEX_{DAO} + INDEX_{vestedTeamTokens}
$$

INDEX supply does not have a hard cap. Its supply increases when:

- INDEX is minted and distributed to the stakers.
- INDEX is minted for the minter. This happens whenever someone mints INDEX.
- INDEX is minted for the DAO. This happens whenever someone mints INDEX. The DAO gets the same number of INDEX as the minter.

$$
INDEX_{stakers} = INDEX_{totalSupply} * rewardRate
$$

At the end of each epoch, the treasury mints INDEX at a set reward rate. These INDEX will be distributed to all the stakers in the protocol.

$$
INDEX_{minters} = mintPayout
$$

Whenever someone mints INDEX, a set number of INDEX is minted. These INDEX will not be released to the minter all at once - they are vested to the minter linearly over time. The mint payout uses a different formula for different types of bonds.

$$
INDEX_{DAO} = INDEX_{minters}
$$

The DAO receives the same amount of INDEX as the minter. This represents the DAO profit.

## Backing per INDEX

$$
INDEX_{backing} = treasuryBalance_{stablecoin} + treasuryBalance_{otherAssets}
$$

Every INDEX in circulation is backed by the Index DAO treasury. The assets in the treasury can be divided into two categories: stablecoin and non-stablecoin.

$$
treasuryBalance_{stablecoin} = RFV_{reserveBond} + RFV_{lpBond}
$$

The stablecoin balance in the treasury grows when bonds are sold. RFV is calculated differently for different bond types.

$$
RFV_{reserveBond} = assetSupplied
$$

For reserve bonds such as the MIM bond, the RFV simply equals to the amount of the underlying asset supplied by the minter.

$$
RFV_{lpBond} = 2sqrt(constantProduct) * (\%\ ownership\ of\ the\ pool)
$$

For LP bonds such as INDEX-MIM bond, the RFV is calculated differently because the protocol needs to mark down its value. Why? The LP token pair consists of INDEX, and each INDEX in circulation will be backed by these LP tokens - there is a cyclical dependency. To safely guarantee all circulating INDEX are backed, the protocol marks down the value of these LP tokens, hence the name _risk-free_ value (RFV).
