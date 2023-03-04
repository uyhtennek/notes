[Gameplay programmer interview guide – Mint Banjo](https://mintbanjo.com/articles/gameplay-programmer-interview-guide/)
___

# "gameplay programmer"

* mostly working on code and logic for gameplay mechanics / gameplay loops
* or general code to tie other game systems together
* or general programmer duties, probably in small studios

Common things for a gameplay programmer
* input handling
* game rules / game mode / game states handling
* player interactions with game objects
* player inventories
* missions / quests
* vehicles / mounts
* weapons / spells / skills / damage handling
* character movements
* camera
* etc.

Things that is NOT a gameplay programmer's job
* UI, physics, AI, animation
* core engine code, core network code, tools programming

* a gameplay programmer still need to understand the technology though
	* just probably won't do much editing of theme
	* probably depends on studios as well
___

# Prerequisite

This guide focuses on gameplay programming and on the interview process
* you should already built a portfolio / CV / cover letter
* and found and applied for a suitable role

Some concepts may be overkill for junior level roles
* but it'll be expected you learn the concepts eventually on the job any way
___

# Hiring process structure
generally

1. Screening Interview with HR / hiring team
* discuss general job requirements
	* eg. location, seniority, rough salary range
* touch on experience and reasons for job search and some more details about the role

2. First Interview
* a fairly informal chat, about your experience, education, games you've worked on
* more information about the role
* perhaps a few easy technical questions

3. Second Interview / Technical Assessment
* usually invovles solving problems, writing code, answering questions about some techs

4. Third Interview
* might involve meeting and getting to know team members, even directors / studio heads as well
* could be a cultural fit check or an on-site studio visit

5. Offer
* salary, benefits, contractual negotiations
* typically happens via email


## Shortened process

some studios might condense it into fewer rounds
* always had at least the first and second interviews though

so be patient
* it might take weeks before you even get a response
* then it could be a week or so in between interview rounds
* it can last 1-2 months, or less than 2 weeks, depending on studio
* so, plan ahead and factor this as well

Although from your perspective it's annoying and long, be polite and keep your temper
* they are likely processing and interviewing many other candidates
* they are trying to make games at the same time
___

# First Interview

likely be a chat with 1-3 employees of the studio
* probably including a tech director, a lead programmer, or a studio head
* or any member of the production team

format is usually a ~1 hour discussion / panel interview
* probably be online, via MS teams / skype / zoom / etc.

General advice is, just to be yourself
* be personable, enthusiastic, and friendly
* always try to phrase things in a positive manner
	* never bad-mouth your current company or previous companies, or your co-workers
	* don't be overly negative or arrogant
* be relaxed, as these are usually quite informal

* read the job description throughly
* then try to sneak in demonstrating that you satisfy the requirements


## Common questions to be asked
you should prepared an answer for these questions

"Talk through your experience in the industry so far."
* should be easy enough
* maybe touch on how and why you got into the industry in the first place as well
* try to explan how you like playing / making games, working in a collaborative environment, etc.
* talk about the projects in detail
	* and the tools you used

"What are some of your strengths / weaknesses?"
* this seems to be becoming less common though
* for the weaknesses, be honest
	* but pick something that isn't devastating to your chances
	* pick one that can be easily be worked on
		* mention how you've improved it lately

"Why are you looking to leave your current company?"
or "Why do you want to work here?"
* always asked.
* phrase your answer positively, and reflect well on yourself
	* don't say anything negative about your current company
	* instead, talk about what you're looking for in the new company
* great to reference things about the role or studio as well
	* eg. "I was impressed by game XYZ which your studio made."
	* eg. "I want to work with X engine / genre more."
* sometimes honest logistical reasons are also fine
	* eg. "I want to relocate."
	* eg. "I want to work remotely."
	* mention the job description accomodates that
		* probably not the salary though, be caution

"Talk about your side projects."
"What are some gameplay features you worked on in your last role."
"What's the code feature you're most proud of in your role on game X."
* so refresh your memory about some jobs you've done
* probably will follow up with some details
	* eg. how you structured the code
	* eg. how you solved X problem
* can have a video prepared from a YouTube "let's play" or trailer or something
	* for games or features you've worked on
	* you can offer to show a mechanic via sharing your screen as well
		* which helps with the explanation
		* and breaks up the boring talking
			* better experience for them

"Talk about an instance where you overcame some technical difficulties."
"What kind of games do you like to play?"
* always seems to come up towards the end
* any answer is fine
	* you may want to focus the answer towards games similar to the one you'll potentially be working on, if you know it
	* you can mention about a certain game mechanic you enjoyed as well
	* maybe even discuss how you think it was implemented

"Do you enjoy mentoring?"
"Have you done any mentoring?"
* typically asked for non-junior roles
	* as mid-senior upwards are expected to help with the progression of juniors on the team
* they're trying to guage whether you work well with other people
	* and whether you're happy to help your colleagues
* they might ask for instances of when you helped a colleague fixing some problems

"Talk about an instance where you worked collarobatively with designers / artists / etc. to improve a feature."
* another check on whether you work well with others
* try to have some examples prepared

"Talk about a time you've fixed a particularly hard bug / optimised a certain piece of code."
* because debugging and optimisation are some big parts too

Technical questions
* generally will be from a high level
* probably won't grill you in too much detail
	* because that's reserved for the second interview
* eg. your experience with a certain engine
	* how you enjoyed working with it overall
	* general questions about its structure
* eg. your knowledge of a certain language
	* your thoughts about feature X of that language
	* rarely they'll throw in a few detailed quiz-style questions about a language
* If the role involves a lot of multithreading or networking, they might ask high level questions about those paradigms
	* they might ask examples of where you've used them previously
* your experience with source control and other tools
* your experience with other specialised code areas like UI, AI, animations, etc.


## Questions to ask
you get to throw questions to them, usually it happens towards the end

think of some interesting questions you want to know about the role
* because this part is also about you interviewing them
* if a candidate couldn't think of anything to ask here, it would reflect poorly on them
	* showing a lack of curiousity and interest in the role

ask stuff like the size of the team or structure of the team
* do they follow best practices in terms of code reviews / documentation / etc.
* how do programmers work with designers?
	* are there design docs?
	* is it more iterative?
* what kind of features you'll get to work on?
* the production processes?
* etc.

You can ask specifics about the project
* basically you can ask anything about it
* they might not want to share too many details though, but that's fine
	* especially if the game is unannounced
	* some studios might make you sign an NDA pre-interview, so they can talk freely
___

# Tech assessment
read up the book *Cracking the coding interview*

note that you may not be expected to get every question 100% correct
* you're measured against how other candidates handle the material

usually inovlve one or more senior or lead programmers
* last for 1-2 hours
* online or on-site
* the style of questioning varies a lot
	* Leetcode style questions
	* Algorithmic / data structure questions
	* Fix or review this buggy program
	* Implement this game feature
	* Verbal questions
___

## Preparation / revision


**Data structures**
practice by implementing your own simple versions of the common data structures
* Dynamic array
* Binary tree
* Linked list
* Stack
* Queue
* Graph
* Hashmap

Ideally templated / generic, with common interface functions and memory management
Be aware of the pros and cons of each structure and when it's best to use them


**Algorithms**
practice implementing a few common algorithms, identifying big O time complexity, and optimising
* finding prime numbers
* sorting algorithms
	* like bubble sort, selection sort, merge sort
* Binary search
* finding all permutations of a string
* reversing a linked list, swapping a pair in a linked list
* in-place rotation of an NxN matrix
	* [Rotate Image - LeetCode](https://leetcode.com/problems/rotate-image/)
* Container with most water
	* [Container With Most Water: LeetCode #11 Short & Simple Solution](https://www.code-recipe.com/post/container-with-most-water)
* Longest substring without repeating characters
	* [Official Solution - Longest Substring Without Repeating Characters - LeetCode](https://leetcode.com/problems/longest-substring-without-repeating-characters/solutions/127839/longest-substring-without-repeating-characters/)


**Mathematics**
read the book *3D math primer for graphics and game development*
make sure you know the following:
* Basic vector maths
	* the difference between a vector and a scalar
* Dot product
	* [Dot Product (falstad.com)](https://www.falstad.com/dotproduct/)
* Cross product
* Coordinate spaces
* Rotations
	* euler angles, matrics, quaternions
	* how they are represented in code
	* their trade-offs, eg. gimbal lock, space complexity, etc.
* Interpolation
* Calculus
* Gemoetry
* Intersection tests
	* line-line intersection, line-sphere intersection, box-box intersection, etc.


**Language**
You'll be expected to be an expert in the language used in the project, which may be C++ or C# in the industry
* Keywords
* Inheritance
	* polymorphism, abstract base classes / interfaces
	* common pitfalls, eg. virtual destructors, diamond problem
* Pointers and references
* Smart pointers & dynamic memory
* Casting
* Constructors
* Move semantics
* Const correctness
* Operator overloading
* Templates / generic programming


**Others**
* Multithreading
* Networking / replication
* Cache efficiency
___
