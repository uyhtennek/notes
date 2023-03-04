[How Authentication works](https://docs.unity.com/authentication/en/manual/HowAuthenticationWorks)
[Approaches to authentication](https://docs.unity.com/authentication/en/manual/AuthenticationApproaches)
[Recommended best practices](https://docs.unity.com/authentication/en/manual/BestPractices)
___

# Authentication

when a returning or new player logs into the app, Unity Authentication generate these things
* `PlayerID`, a random string of numbers
	* used to identify the player
* session token
	* used to re-authenticate the user
	* re-authenticate is required when the session expires
* authentication token
	* prevent anyone from overriding the `PlayerID`
___

## Anonymous authentication
a.k.a. guest sign-in

doesn't require the player to enter credentials or create a user profile
* [Use anonymous sign-in](https://docs.unity.com/authentication/en/manual/UsingAnonSignIn)
* doesn't collect or use the player's identifiable information

this method provides the lowest friction for players in a game
but players can't log-in from another device

> [!info] note
> players can't log in to their anonymous account if they uninstall and then reinstall the game

___

## Platform-specific authentication
a.k.a. third-party authentication or external authentication

uses external identity providers
* making it possible to authenticate the same player from multiple devices

typically, the process begins when a player signs in through an external identity provider with their email address, or their username and password
* when a player signs in, a token is sent to Unity Authentication for validation
* when it is validated successfully by the external identity provider, the token is then associated with the `PlayerID`
___

## Custom authentication

allows us to use our existing authentication system
* we need to integrate the authentication solution with Unity Authentication
___

# Best practices
tips to implement Unity Authentication

1. Use anonymous authentication to introduce players to the game
* provides a frictionless First Time User Experience
* players can go throught the first few levels using an anonymous account
* so game servers can still track their progress

2. Once a player becomes invested in the game, prompt the player to upgrade the anonymous account to sign in with a platform account
* eg. when the player reaches a certain level, or it they're trying to make their first purchase
* make the player's game progress recoverable

![image (1520×572) (unity.com)](https://docs.unity.com/authentication/_next/image?url=%2Fauthentication%2Fen%2Fimages%2FFirstTimeUserEx.png&w=1920&q=100)
* FTUE = First-time user experience

another approach is to request the player to link their platform accounts from the start
![image (1906×912) (unity.com)](https://docs.unity.com/authentication/_next/image?url=%2Fauthentication%2Fen%2Fimages%2FCopy%20of%20SignInIdentity.png&w=1920&q=100)
___
