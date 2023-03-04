[Monetization strategy](https://docs.unity.com/ads/en/manual/MonetizationStrategy)
* tips for optimizing Unity Ads implementation
* using rewarded ads
___

# How ads generate revenue

1. Every time our game requests an ad, Unity Ads holds an open auction
* advertisers will then bid on our impression

2. The highest bidder provides an ad

3. an ad can have diverse source of revenue streams
* Ad start
	* the player triggers an ad
* Completed view
	* the player watches an ad to the end
* Click
	* the player clicks the ad
* Install
	* the player clicks out to the external product page and installs the product

> [!info] eCPM, *effective cost per impression*
> it means our overall revenue across the above streams

therefore the key to maximizing eCPM is designing our ads to consistently generate the highest quality impressions
___

# Rewarded ads
ads that provide in-game rewards to players in exchange for watching ads

* effectively turned watching ads, an negative player experience, to a positive experience
* rewarded ads monetize better
	* so it typically yields higher quality impressions

a byproduct being players are less likely to invest on in-app purchases
___

## Let the player choose
allow players to opt in to watching ads

![image (600×337) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy.png%3Fwidth%3D400px&w=640&q=100)

players get to choose to skip them or receiving a reward
* both options are rewarding for the player

when players voluntarily watch an ad, they are more likely to pay attention and show interest in the advertised product
___

## Identify the right incentive

rewarded ads need a compelling incentive
* eg. in-game currency, power-ups, additional lives, or consumable items
* so that it helps players recover and flourish in the game
___

## Identify the right placement

When considering your Ad Units, think about stress points in our game that might be a good opportunity for an injection of resources
* a place or a moment that the player can really need some resources


**Special shops**
a special shop type where the player can purchase unique items in exchange for watching an ad

![image (600×503) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_2.png%3Fwidth%3D400px&w=640&q=100)


**Augment retention mechanics**
boost rewards in exchange for watching an ad

![image (384×216) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_3.png&w=384&q=100)![image (384×215) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_4.png&w=384&q=100)


**Mini games**
player get to play a fun mini-game if they watch an ad

![image (600×337) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_5.png%3Fwidth%3D400px&w=640&q=100)


**Extra lives**
![image (600×337) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_6.png%3Fwidth%3D400px&w=640&q=100)


**Double up**
present a chance to receive 30 minutes of double points or earned currency in exchange for watching an ad

![image (600×336) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_7.png%3Fwidth%3D400px&w=640&q=100)


**Competitive edge**
offer a boost if the player is about to lose

![image (250×402) (unity.com)](https://docs.unity.com/ads/_next/image?url=%2Fads%2Fen%2Fimages%2FMonetization%20strategy_8.png&w=256&q=100)


**Get creative!**

* offer players some currency when they don't have enough to purchase an item at a shop
* place a character or shop right before a challenging boss fight
	* offer to heal or regenerate some resources
* offer a boost to help push players who continue to fail a mission
___

# Pair with in-app purchasing

rewarded ads often generate engagment through reward and investment
* eg. an ad place that grant small amounts of permium currency
	* it offset the cost of paid content
	* players with some premium currency might buy more currency to supplement a purchase
		* rather than let the currency go to waste
* [Just in time for the holidays - How to get the most out of currency sales | Unity Blog](https://blog.unity.com/games/just-in-time-for-the-holidays-how-to-get-the-most-out-of-currency-sales)
___

# Don't hurt the game's economy

> [!info] As a general rule
> Rewarded Ads provide small benefits, to drive engagement and retention.
> In-app purchases provide large benefits, that feel worth the financial investment.
> 
> so try to avoid offering purchasable items as ad rewards.

rewarded ads can impact game balance and diminish the value of other rewards
* so the rewards should be kept within a reasonable range of values

ng.
* a special sword sells for $4.99, but players can get it in exchange for watching an ad
* a reward for watching an ad is on par with the reward for winning an very challenging battle
* offering rewarded ads at any time
	* it's the same as offering free resources at any time
___

# Use ad filters

filter out ads that are irrelevant to the players' interests
* so that players are more likely to interact with the ads
___

# Preserve the user experience

ideally ads shouldn't break user experience
* so make it as little disruption as possible
* eg. place interstitial ads at natural pauses or breaks in gameplay, such as a load or level change

focus on well-designed ad placements and incentives rather than sheer volume
* at the end of the day, our ads monetization success depends on players engagement
* having more ads doesn't help with that, but a better designed game does
___
