What's your favorite video game?
1. Dota
2. Call of Duty
3. Zelda
4. Minecraft
5. League of Legends
6. Grand Theft Auto (GTA)
7. Valorant
8. Player Unknown's Battlegrounds (PUBG)
9. FIFA
10. Assassin's Creed
___

# Old AI

* based on raw code
	* if, else-if, else, etc.
* a lot of randomness or pseudo-randomness

the computer or CPU might be playing against us
* by either being really good at choosing random numbers
* or maybe tracking our behavior in the game

btw
### David Malan favorite games

* original Donkey Kong ( Commodore 64)
* Montezuma's Revenge ( Apple II )
	* enemies (skulls) will move back and forth, up and down
* King's Quest
	* David Malan's favorite, in childhood.
	* by Sierra Online
* Super Mario Brothers 3
	* David Malan's ultra favorite

he later took the decade off from gaming thereafter, then
* The Legend of Zelda: Twilight Princess ( Wii )
* has motion tracking
___

# Machine Learning

**pattern recognition**
* cats & dogs

**learn to play game**
* eg. I don't know how to play chess, but I know the rules. That's enough for us to write a program that can play chess far better than me.
___

# Branching Factor
in each turn, how many moves are valid

1. Chess
* 20 or 30 moves

2. Go
* ~100

3. Dota 2
* thousands or ten thousands
* neural network can't be looking through all the possibilities
	* it's gonna be something a lot more like human intuition

AI can deal with imperfect information
___

# How to make AI

1. translate the game into data models
* the human interactions
* the game mechanics

2. set rewards
* encourage good behaviors, ultimately lead to winning the game

3. design the learning curve
* eg. train an AI to walk across a bridge
	* the AI will walk randomly, resulting keep falling off the bridge
	* although the reward is there, at the end of the bridge, the AI can never know about it
	* takes forever to train the AI
* shorten the bridge can lead to a better result
	* sometimes the AI does get across the bridge, and get the reward

* eg. Dota
	* takes out all the items, takes out all the heros, just left a few essential things
	* then gradually add the bits back in

4. scale up
* poor or buggy algorithms are easier to see
* AI simply needs to **self-play** more
	* like if we're trying to learn to play tennis, but we're playing against Serena Williams, we can't learn anything
	* balance is important in learning
* eg. 300,000 CPUs, 2,000 GPUs
	* 555 bots, play ~180 years worth of dota gameplay, every day

5. save the progress and load the AI in a newer version of the game
* since actions are just data models
* if the game has new feature, we can just insert those new actions to the AI model, and then continue training from there
___

# Limitation

AI learns by playing against itself
* it builds up strategies based on its own creativity or previous strategies
* if there is a kinda different strategy, it can surprise the AI

surely we can take that winning actions or strategies to have the bot figure out itself why it lost or win aginst that, and filling in its weakness
* but the thing is it wasn't an instantaneous process, it'd take about 12 hours
* that's why it's hard to implement AI to the real world
	* eg. self-driving cars need to know how to handle all the variations on the road

but the things is, we don't really need new methods
* just really scale up the calculations, by a ridiculous amount of self-play and a ridiculous amount of data, was shown good enough to crack this problem

but still, we can't apply the same method to the real world
* in the real world, we're not able to collect trillions of samples, play trillions of games of Dota 2
* we need to find a way to get sample efficient enough
___

# New AI tech

**common knowledge**
* human has sort of background knowledge, to any thing
* in science terms, *assumed structure* that people just commonly understand
	* we don't know everything, we have a background already

**robotic arms**
* Open AI apply this large-scale reinforcement learning technique to the robot fields
* can solve a rubik already
___
