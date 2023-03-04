# HC Art Process

Phase 1 - Develop mechanics

Phase 2 - Testing with WIP arts
* we don't care about the assets

Phase 3 - Improve contrasts and colours, improve UI as well probably
* so it looks nice on YouTube

Then it's time to test the game for the first time

Phase 4 - Reach better KPIs

Launch the game
* Game Ops kick in
___

# Mindsets

* You should source your assets on Unity Asset Store & Sketchfab
* Avoid creating assets in 3D software unless you have enough time and optimization skills
	* you create your own assets only if you're launching the game and you have time for that
	* in a MVP, we don't make assets, we find them on Asset Store or Sketchfab
* Art shouldn't take more than 1 hour per day and focus only on contrasts
	* until you reach good KPIs
	* if it has good KPIs, we polish the game
* On a HC game, arts are about readability, not beauty
	* because it's 50% of the KPIs
___

# Arts Support Gameplay

**Reward players visually**
* every input is rewarding
* avoid keeping on a boring screen
* use visual rewards to enhance your screens
	* eg. confettis, fireworks, size effects, etc.
* make sure all the important elements have good feedbacks
	* eg. collectables, breakables, etc.

**Mass market theme**
* assets must be simple and without too much details
* avoid realistic assets
* avoid niche assets
	* eg. monsters, blood, politics, etc.
___

# Contrasts

1. early on in production, turn the game in black and white to check contrasts
* fix any issue
2. around the time for the first test, check if anyone can understand the game in one look
* fix the issues
3. after launch, polish the overall look

> [!tip] Checking Contrasts
> if the game looks clear in black and white, then the arts are delivering the message well
> also, protect the colors related to gameplay, from the colors related to environment

give a unique shape language to each gameplay element
* eg. roundish = positive, spiky = negative

avoid having the same saturation level everywhere
* 8% of the audience is colorblind
* make use of various saturation level to deliver the message

Generally, use various size, saturation, colors, ....
* to put highlights
* to differentiate different elements
* so that the game is clear
___

# Avoid Black

a common mistake is to make shadows too dark
* besides shadows, it could be lines as well
* it's too aggressive and eye-catching

> [!tip] soft contrasts typically lead to lower CPI

> [!tip] use cartoon shader to lighten the shadows

___

# Colors

Don't use Unity Standard material
Use cartoon style shaders
* many on Asset Store, eg. Cartoony
* it's simple and clean and optimized for quick and efficient use
* also Standard shaders lead to performance issues

Small or limited color palette with pastel colors is recommended
* pastel colors = not aggressive colors

Use at least 2 colors and up to 4-6 colors
* if there is not that much elements to highlight, fewer colors is preferred

> [!tip] mycolor.space
> A website in which we pick a color and it gives us a nice palette.

Don't make it personal
* use the colors that people like
* not the colors that you like
* don't limit your choice
___

# Avoid Textures

We don't need textures to understand an environment
* games that look good often times are just plain color with some gradient and that's it
* unless it serves the purposes of our game
___

# Use Water

water works very well in mobile gaming
* create a beautiful water, better to be discreet and simple
* something as simple as a blue plane to a cartoony shader style water

avoid low poly water
* it breaks the clarity of the scene
* it's not cartoony enough and not bright enough
___

# User Interfacts

**Heat map**
* players don't use the entire screen space for input
* position the UIs according to these habits

> [!tip] Voodoo's UI rules
> ![[Voodoo-UI-rules.png|400]]

**Buttons**
* should be simple and easy-to-understand
* have color rules
	* eg. Green = free, Yellow = paying, Blue = other
* we can use a bottom bar to keep the screen space clean

**Texts**
* use mass-market fonts
	* free of rights as well
	* eg. Arial Bold white, always work
	* fonts should look neutral
* always use Text Mesh Pro
* never use phrases, prefer simple words
	* eg. Play, Revive, Retry, ....
___

# Camera Types

**Side View**
* classic runner camera
	* typically runner avatar is moving left to right
* gameplays generally revolve around **height**
	* eg. Flappy Dunk, Helix Jump
* players have little room to see upcoming obstacles to the right

**30° Camera**
* a more modern runner camera
	* more immersive
	* allow 3D backgrounds
* fix the shortcomings of the side view
* we can also add a second lane
	* maybe making it a competitive runner game
* very versatile
	* eg. Cube Surfer, Scribble Rider, Spiral Roll, .....
* many room for players to grow visually

**Back View**
* typically used in left/right runner games
* allows for maximum visibility of the track
* make sure the leave enough room for player's input
	* usually the lower third of the screen is enough
	* and place avatar right above

**Top-Down**
* generally combined with joystick movement
* we can see all obstacles all around the player
* since it shows more information, so make sure the gameplay elements stay clear
* since it's can be associated with niched genres such as RPG, it may lead to higher CPIs

**FPS**
* the most immersive camera
* usually associated with aiming gameplays
	* eg. Knock Em All
* associated with hardcore gameplays, so it generally generate higher CPIs
* also movements can leave players feeling sick

**Still Cameras**
* they imply player's precision
	* maybe the gameplay needs players to be very precise
* work best for focusing on clarity, so good for puzzles
* we can use it to emphasize game feel in ASMR gameplay
___

# Camera Guidelines

1. Maximize Clarity
* the first thing we want to display, is the player avatar
	* it has to be very centered and well contrasted
	* so that viewers know instantly what they are controlling
* and we want to show clear objectives
	* eg. finish line, checkpoint, enemy, target, ....
* background is secondary
	* players' focus should never be the scenery or backdrop
	* so maybe limit the colors or objects you're using there

2. No Hidden Information
* we want all threats are visible to players
	* players don't enjoy getting caught off guard
	* they will feel the game is unfair to them
* then they will rage quit

3. Limit Camera Movements
* rapid changing scenary can feel sick to look at
	* so limit the camera movements
* we want to adjust fluidity
	* eg. screen shakes look cool, but if there is too many, it becomes hard to understand

4. Match Camera And Gameplay
* **Benchmark**: look into your favorite hits to find the best camera
* **Tweak**: adjust the camera
	* to a point that a single screenshot can convey the gameplay
* **Test**: test different setups on Facebook to see which one returns the highest CPI
___

# Camera Checklist

before testing the game
* the camera type should be optimal
* framing includes all important information
* correct visual hierarchy
* single-frame test
* enough room for player input
* limit camera movements
___
