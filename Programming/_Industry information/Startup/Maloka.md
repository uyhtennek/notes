[Building a traditional tech startup vs a gaming company | Unite 2022 - YouTube](https://www.youtube.com/watch?v=e_2DM0CebY4)
___

# The Startup Curve

![FNqlJVwXoAc8IBp (2048×1396) (twimg.com)](https://pbs.twimg.com/media/FNqlJVwXoAc8IBp?format=jpg&name=large)

* Trough of Sorrow
	* experimenting
	* trying to find market fit
____

# 6 weeks cycle
Goal - Minimize **tech** and **design** risk

1. warm up
2. sprint
3. sprint
4. sprint
5. sprint
6. cool down

**warm up**
* scope the feature
	* build requirements
* know what's feasible

**sprint**
* focus on producing shippable work

**cool down**
* focus on stability
* QA
___

## scoping the feature

awesome things tend to create large feature branches
* will stay outstanding (undone)
* difficult to maintain

instead, scoping something achievable in a two week sprint
* we don't need to ship a full feature that go to the end user
* just ship something that we can merge

2 weeks later we'll have something that can help advance the game
* another great thing is that, since we can already get it into the game, we can get feedback from all the stakeholders
* less work, then get some valuable opinion
___

## merging

better to maintain a strong culture of merging quickly
* keep develop up to date, with everybody else's changes

sometimes the changes can interfere with other features and development
* just put a feature flag in front of it

also, merges can be a real pain if we haven't merge for a long while
* frequent merges save a lot of headaches
___

# Cross functional teams

everyone has valuable feedback
* don't do something like artists pass the assets to UX, then QA
* do bringing QA in to help plan
	* this make sure the artist has approval after the feature is developed as well
___

# Modular data

* design the data model early
	* how we're going to store the data
	* what questions we want the data to answer
* early on in building the feature
	* maybe before building UI/UX, implement to Unity

also, if we do automated testing
* it's a lot easier if we have modularized, abstracted data

clear data model can
* make UI/UX obvious
* track user's data, do analytics
	* lead to better gameplay or design

abstract our data correctly helps testing as well
* easy to jump between game states
	* eg. a brand new user versus a power user who played the game for a year already
* traditional startups have "seed data file"
	* then we can run automated tests off the seed data, or put the application state into various places to easily develop features that are relevant to those places
___

# Analytics

Find a **north star metric**
* how do I know if we're being successful?
* are we getting closer to that product market fit?
* do we have more happy gamers or users?

**Cohort** user
* to clarify ourselves what we're doing is actually working
___

# Data-driven development
the benefits of data-driven

1. it's common to have large global objects that store all of the data
* but it's a bad thing
* focus on data would kinda force us to stick to the SOLID design principles
	* or maybe more function designs

* like **separation of concerns**
	* each scene is its own thing, so less dependencies
	* easier to build on new functionality
* eg. more network requests is a better design than just one giant state

2. work better in cross platform functionality
* we had an initial tendency to separate repos for the mobile app and for the VR game
* but if we can get most of the stuffs like metadata to the server side, it's easier to migrate data or put out like a new version of an existing feature

3. likely to have faster iteration during the development

4. lastly, nice to do analytics
___

# How to do quality QA

quality QA is a very time consuming activity
* but super important
* if you want above 4 star reviews
* no bug allowed

1. inform QA of all changes
* every time a merge is created, let QA know about that
* give them a change log / release notes
	* as well as good commit messages
2. better to have each merge tested as well

3. Embed QA, rather than treating them as a downstream function
* oftentimes QA is the segment that knows your game the best
* they spent the most time in game, likely more than everyone else, and keep testing things out
	* gonna give a lot of value if bringing them in the planning process as part of the teams

4. just need one final QA pass of the overall game prior to the release
* because each individual thing has already been tested
* super valuable, as it enables us to release more frequently and regularly
___

## Logging and observability
When QA finds a bug, how do we reproduce that bug?

don't spend days trying to reproduce and track down, adapt other solutions
* send all clients side logs to the server
	* so we can unify those. we have full context of what happens. what the game state is.
* fire off some custom events, that help us understand how everything is functioning, from an overall game state

In SAS companies, users are relying on the software for their day-to-day function
* if there is a bug, or a feature request, people are loud and clear about that

In games though, people can notify us if there is an issue
* but it's likely that there is not a lot of context around that
* even not an email address or anything to track down the user to get any information
	* also it's better if we don't need to rely on users to communicate what the issue is to us
___

# Building community
reach out to users

* do interviews and surveys
* do Facebook group, Discord, etc.
	* put free content there as well to lure users
	* great place to do surveys as well

* can get supre valuable feedback
* sometimes nice things happen too

* find a community manager

tell the user you're a mission-driven company
* rather than just a game company trying to optimize revenue
___

> [!summary]
> * design the data model early
> * think about analytics to gather up front when designing the feature
> * shrink scope until a feature is shippable (mergeable) in a single cycle
> * make testing easy
> * add logging wherever possible
> * engage with your users

we can heard a lot of something like
* getting things into users' hands
* providing value as fast as possible

these are the big ideas
* but we need to tweak our processes to find out how

___
