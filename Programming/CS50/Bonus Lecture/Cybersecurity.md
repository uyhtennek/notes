# Is your phone secure?
Not just phone. Could be any computer and device.

To unlock our phone, we need to enter our passwords or passcodes, right?
Okay so how secure is this password thing?
* Do password keep things secure?
* If so, why you think it's secure?
* What makes a password secure?
	* perhaps randomness. it is or supposed to be hard to guess.
	* although that also makes a password hard to remember

But actually passwords are not as good as they seem to be, as a defense again adversaries ( 攻擊 )
___

# Common passwords

Here are some of the most common passwords that security researchers found on big exploited / compromised databases
1. 123456
2. 123456789
3. picture1
4. password
5. 12345678
6. 111111
7. 123123
8. 12345
9. 1234567890
10. senha

Why those are bad passwords?
* obvious. they are easy to guess.

We want the passwords more random than these
but somehow **balance** randomness with memorability, so we won't keep forgetting our passwords.

If we have a password that's so random that we always forgot it,
then we ourselves always can't unlock our phones, then what's the point of having a password.
___

# So how secure is secure?


**4-digit passcode**

* How long might it take to crack a 4-digit passcode?
* if we use *brute-force*
	* in the world of programming it means **try on possibilities**
	* maximum number of tries = $10 \times 10 \times 10 \times 10$ = 10,000

* we can also write code or software for cracking
	* then just use something like a USB or lightning cable to connect to the phone
	* then we can send our cracking program to the phone and execute it

```python
from string import digits
from itertools import product

for passcode in product(digits, repeat = 4)
	print(*passcode)
```

* this program takes less than 1 second to run
* so what's more secure than a 4-digit passcode?
	* a 6-digit passcode, or 7-digit, or 8-digit, or 9 or 10
	* more possibilities, so harder to guree and harder to brute-force
	* it doesn't stop the adversary, but it is going to slow down
		* **cost** of adversary increases

> [!Theme of cyberscurity]
> Cyberscurity is all about **raising the cost** of the adversary, financially or time-wise or the like.
> 
> Like in the physically world, we lock our doors at night, maybe even invest on a better deadbolt. Because we want to make sure it would take too much time, too much effort, to much risk, to the adversary to get into our homes.
> So that the adversary will look for another target.
> 
> To say "my phone is secure" is sort of non-sensical.
> To say "your phone is more secure than someone else's", that's is a reasonable, fair statement.


**4-letter passcode**
let's use alphabet letters ( a - z and A - Z) instead of numbers

* how many possible combinations?
	* $52\times52\times52\times52$ = 7,311,616
	* recall the number for 4-digit passcode was 10,000
		* so this is already way better

* let's rewrite our cracking program
```python
from string import ascii_letters
from itertools import product

for passcode in product(ascii_letters, repeat = 4)
	print(*passcode)
```

* this program could easily take a minute
	* recall the previous one takes less than a second


**8-character passcode**

* to futher improve, let's mix letters and numbers and also use special characters `@ # % & * ...`
* again, what's the total number of possible combinations?
	* 10 digits + 52 letters + 32 punctuation symbols
	* $94\times94\times94\times94\times94\times94\times94\times94$ = 6,095,689,385,410,816 = 6 quadrillion
* let's assume it takes 1 second to try 1 possibility, by the time it gets to the end, 193,000 years have already passed

* by this point, we are probably save from any human finger
* but still, Macs and PCs are pretty darn fast

* one last time, let's rewrite the program
```python
from string import ascii_letters, digits, punctuation
from itertools import product

for passcode in product(ascii_letters + digits + punctuation, repeat = 8)
	print(*passcode)
```
___

# Trade-off

Clearly we should use a 8-character passcode, to maximize security.
Is it?

Well, yes we could use a very big random passcode
but no, not if we would forget the password and end up writing it on a monitor on a post-it note...

and this is not necessarily our fault
this is a symptom perhaps of bad IT policy
* maybe the system is not very usable or forgiving, but it asks for a unneccessarily random passcode
* maybe it shouldn't require the human to have a very random passcode
___

# Mechanisms to bring balance


1. **Timer**
![[iPhone-locked.png]]

although it's annoying that it locks our phones, why it's actually a compelling feature?
* it really slows down the adversary
	* it's not gonna be 1 second for each try, it's 60 seconds for each try
	* unless the adversary is after you specifically, odds are they're rather take someone else's phone at this point

btw on Android it's even more annoying since it doesn't tell us when to try again
![[android-locked.png]]


2. **Self-destruct**
there is a feature in both iOS and Android phone we can enable, that if we enter a wrong passcode 10 times, the phone would delete itself
* Why 10 times?
	* Google and Apple decide that if the passcode is wrong for 10 times streak, you are probably not the person who own the phone

* It's an extreme one though
	* maybe we just get up in the morning or really drunk, that our head is not very straight at the moment, 10 times maybe not enough to protect our own phones from us
* Unless we have backups


3. **Two-factor authentication**
The key point is to have a **second factor** other than just the passcode.

* needless to say it's not two-factor authentication if we just have 2 passwords
	* because both passwords could be forgotten by us or stolen by someone else if we had write them down on a post-it note or the like

> [!definition]
> it's about having a fundamentally different factor, that is available to us, so that the odds that someone get something we know, the password, and something we have, our phone, is much smaller, than the threat of just figuring out something we know, the password alone.

* it typically use one-time codes, a 6-digits code
	* meaning we can only use it once
	* and it's texted to or pushed to our device, so we and only we can use it
![[two-factor-authentication-google.png]]

* this already sounds very safe and balance, are we done?
* we still need to remember our big random passcode for security sake...


4. **Password Managers**
there are softwares can manage our passwords for us

* some are built into the operatings system
	* Windows - credential manager
	* Mac OS - keychain
* some are third-party software
	* 1password, LastPass
	* companies and universities often have site license so their members can use these softwares for free

* What can a password manager do?
	1. manage our passwords
	2. generate passwords
	3. automatically fill out forms for us on the web

* even a website's database is compromised, our passwords leaked out
	* since that password is only used on that particular website
		* the same password is not used any where else
	* that's only 1 website we need to worry about
* the password is not gonna be guessed via brute force
	* it's just too long
	* it would take many years trying to crack it

* what are the downsides?
* what if we are using someone else's computer?
	* we can't access our passwords
	* but we can actually share passwords across devices
		* although it usually is a paid feature
* we're putting all the eggs in the same basket
	* if someone can get into the password manager
	* then everything is compromised
		* except for the things that at least have two-factor authentication
		* unless the adversary also steal our phones

> [!warning]
> Actually if someone gets physical access to the device, then basically all bets are off.
> But at least a very long random passcode for the password manager would hopefully buy us many years of time.

* perhaps the risk that most likely to happen, is we forgot the password for the password manager
* so it's even okay to write that down
	* but putting it in like a safe deposit box
	* or keep it very very hidden
* if we do lost our password, we lose everything
___

# Misleading Security Mechanisms


eg. **Google's Confidential mode**
![[Gmail-Confidential-mode.png]]
this is a feature from Google's Gmail
> [!brief]
> Recipients won't have the option to forward, copy, print, or download this email.

* Seems pretty good huh?
* Great for lawyers, for business or any kind of private correspondence, it would seem.

* It's faulty because people can simply take a screenshoot or take a picture of the email
* this feature essentially just disable the Forward button, the Print button
* but really **once something is already digital, it's out there**
	* and there are other ways to get the information, like simply taking a photo


eg. **Incognito mode / Private mode**
![[Incognito-mode.png]]
What does it do?
It doesn't log **locally** what we're doing. ^8fef5b

* This time it said on the briefing page that information can still be visible to other
* So browsing history still in some where in the Internet
	* but at least not in the local machine
* IP address still visible by other
* People out there can still know who you are
	* so it's actually not as its name suggested, *incognito* ( 匿名 / 隱姓埋名 )
___

# Encryption
recall [[encryption]]

> [!definition]
> Encryption is in general about scrambling the appearance of some message in a way that only the recipient knows how to reserve that process to see the actual message but anyone intervening in between can't get the information.

eg. `https://`
* `s` in here stands for *secure*
* `https://` means you and the website are having an encrypted communication
	* when we enter our password, our credit card information
	* no one between us and the website, theoretically, knows the information

**end-to-end encryption**
* basically it means not even the service provider can know the information that we sent to someone else through its service
* for example, WhatsApp has that. the message we send to our friends Alice, can just be seen using either ours or Alice's devices.
	* not even Facebook, or MetaServer, the owner and service provider of WhatsApp, can see it
* iMessage for iPhone also does that automatically
	* so long the text messages are blue, and not green

* eg. **Zoom**
* they claimed they are using end-to-end encryption
* but in fact they're not, at least initially
	* probably it's marketing gone awry, not quite understanding what end-to-end encryption means
	* they do use encryption though
* zoom meetings do have encrypted connection, but to Zoom centrally
	* so Zoom can decrypt that information and see and listen to theoretically anything going on in that metting or that classroom
* up until this days, it still by default uses the *Enhanced encryption* option instead of the *End-to-end encryption* option
	* which is stupid because *Enhanced encryption* is just encryption, so *End-to-end encryption* actually is the better one

* But there too is trade-off here
	* several features will be disabled when using *End-to-end encryption* including *Cloud recording* and some phone stuff
	* so if it was a class, and if the class can't record the lesson, it feels already like a big loss
		* but it makes sense that Zoom can't do that for us though
___
