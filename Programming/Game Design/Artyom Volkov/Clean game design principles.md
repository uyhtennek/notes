[Clean game design principles | by Artyom Volkov](https://medium.com/@tricky_fat_cat/clean-game-design-principles-8000ffdd48e1)
___

# "How to make a good game?"
or "How to make a good game design?"

the problem is that, there is no answer to these questions
* every designer has their own opinion because of their personal experience and approach
* and the answer depends on the genre as well

However, for a beginner, it's quite overwhelming
* not only they usualy don't know which game genre to develop
* but also they still can't make head nor tale of the game design basics

But, there're some general guidelines
* they are quite similar to programming principles as well
* they're not strict rules though, just guidelines that help making better games
___

# 1. KISS
Keep it simple, stupid

most systems work best if they are kept simple rather than made complicated
* simplicity should be a key goal in design
* unnecessary complexity should be avoided

the majority of beginner game developers are prone to design convoluted game systems which are arduous to develop, maintain, and update.
* because they have a false notion that the more sophisticated the game system is, the more fun it is for players, the more "professional" it looks

be careful though, not to overuse KISS too much to a degree that your game becomes bland and boring, as everything is overly simplified
___

# 2. YAGNI
You aren't gonna need it
* don't design a mechanic/system until deemed necessary

usually beginner game designers strive to add as many game mechanics as they can, because they
* think that the more mechanics game has, the better it is
* tend to copy mechanics and systems from their favourite games blindly
* try to cover all "what if" cases

In software development, it's hard for less experienced developers to appreciate how rarely architecting for future requirements/applications turns out net-positive.
In game design, that is also true. The less experience you have, the less you understand if you really need a mechanic.

To start with, you can write down a list of goals which you want to achieve by adding a certain mechanic or system.
* it helps you to understand why you need this mechanic
* it'll help to explain it to your team
* it can lead to a feature cut since often times you can come to a conclusion, that you don't need it

Remember, that abstract "fun" is not a goal at all
but what kind of fun do we want in this game, or in this mechanic, is the real question
___

# 3. SRP
Single-responsibility principle

every game system and mechanic should have responsibility over a single part of a game, and it should encapsulate that part
* creating a system that serves several purposes at the same time, is clever, but hard to maintain
* if you can't handle it, don't mess with it
___

# 4. LCP
Loose coupling principle

game mechanics/systems should be weakly associated and have minimal influence on each other
* so that it's easier to add and remove systems
___
