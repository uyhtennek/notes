[Unite Austin 2017 - Testing for Sanity: Using Unity's Integrated TestRunner](https://www.youtube.com/watch?v=MWS4aSO7HAo)
___

# Test Driven Development

we should keep testing
* green, red, refractor
	* tests make us refractor right away
* the tests themselves work like a document

we sometimes write down `TODO` in our codes
* place them in tests

however, test drive development doesn't promote iteration in design
* which is bad
___

# Data Driven Development

we can do analytics
* because we have data

we do tests with data
* so we can test with people's actaull play data
	* if someone filed a bug report, he sends us data so we can figure things out
* not only our dummy data

the game can iterate and evolve easier
___

# Good Test

a good test is
* automated
* isolated
	* we shouldn't have to have three tests run in order to get an output that we wanted
* pass / fail
* easy to read
* fast
	* we shouldn't have to wait for 30 seconds to a minute
* not superfluous
	* we don't need to test every single line of code
	* if you already know something is alwasy true
___

# Live Demo Notes

The difference between Unity's TestRunner and striaght up end unit, is that
`UnityTest` allows coroutine

We released our app, and we discover that a float is not quite right
* maybe the player move speed is a little bit too slow
* now we have to rebuild, build XCode, release the app again
* except that we actually don't, we can use Unity's Remote Settings
	* also we can write down a commit message for our changes
	* it'll save a history of our changes any way

we can throw our data in Unity Analytics
* we can see it in Analytics > Data Explorer
* it can output CSV, and then we can do more analytics
___
