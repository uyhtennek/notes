["Not So Random Randomness" in Game Design and Programming](https://www.gamedev.net/tutorials/_/creative/game-design/not-so-random-randomness-in-game-design-and-programming-r3423/)
* see also [Lucky Breaks](https://www.gamedeveloper.com/design/lucky-breaks)
___

we want random outcomes but within some reasonable parameters

eg. rolling dice for calculating "loot" in a "chest"
* the chance of finding a golden ticket, alongside the usual sell-loot, is 1:10,000
* if we use true randomness, players can
	* find a ticket in three consecutive chests
	* find no ticket in three months of play

solution: increase the chance per chest opened
* eg. reduce the number 10,000 by 100 with every chest opened
	* eventually the ticket will be granted
* store this value in a `howLikelyIsPlayerToFindTicket` variable
* then use this variable to calculate the random chance instead

additionally, we can make a player never find another ticket again, too shortly after
* we can use another variable for this, `absolutelyNoTicketFindable`
	* set it to, say, 10
	* decrease the number by 1 with every chest looted
* when it reaches 0, we go back to the 1:10,000 chance

> [!info] The variables
> Base Chance - 10,000
> Increase Chance - 100
> Blocker - 10
> Current Chance - X, 0-10,000
> Current Blocks - Y, 0-10

Then we can do all kind of stuffs around these variables
* luck buffs, power-ups, in-game events, .....
* we can make "rich" and "poor" area, that have rare or less rare items
* apply this not-so-random-randomness to critical hits, enemy encounters, monster stats, ....
* ....
___
