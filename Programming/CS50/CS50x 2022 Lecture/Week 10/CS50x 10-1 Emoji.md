# Unicode
recall from [[CS50x 0-1 Information in computer explained|week 0]] we talked about how data are represented in binary

we talked about not just binary, but ASCII, then Unicode
and Unicode allows us to represent not just letters of the English alphabet, but really letters of any human alphabet, and even alphabets that are continuing to develop

😷 this emoji in Mac or PC or Android phone or iPhone is represented by
a pattern of bits like `11110000 10011111 10011000 10110111`

and over time, there are more different patterns for new emojis that yet to come
indeed, most any time we update our Mac, PC, phone these days, at least on a semi-annual basis, we're getting some new and improved emojis

Ultimately the goal of Unicode is to represent all human languages.
___

# Unicode Consortium
the organization who control Unicode

* a non-profit based in Mountain View, California
* 12 full voting members ( laste 2015 )
	* Oracle, IBM, Microsoft, Adobe, Google, Apple, Facebook, Yahoo
	* SAP, a German software company
	* Huawei, a Chinese company
	* the government of Oman
* they paid $18,000 a year to have this full voting power

But as an individual, we can instead pay $75
* no voting power
* but have other abilities
	* sign up for the email list
	* show up at the meetings
___

# Emojination
[Emojination](http://www.emojination.org/)
> [!motto]
> Emoji by the people, for the people.

* organized by [Jennifer 8. Lee](https://www.jennifer8lee.com/), and her friend [Yiying Lu](https://www.yiyinglu.com/)
	* Jennifer later became a vise chair of the Unicode Emoji Subcommittee

* they notice there is no dumpling emoji despite it's very globally popular food
* they brought up a campaign asking for [Dumpling Emoji](http://www.dumplingemoji.org/), #dumplingemoji
___

# Proposing an Emoji
Anyone can propose an emoji

1. You have an idea
2. You write a proposal
3. Hand it to the Emoji Subcommittee
	* via a Google Form, which is opened between April and August each year
1. Revise the idea and proposal if it's not approved
2. Once they're happy with it, they kick it to the full Unicode Technical Committee
3. the full Unicode Technical Committee approved it
4. the proposal gets voted on, once a year
5. passed emoji are added for the next year list
6. the list gets sent to all the companies like Apple, Google, Adobe, Facebook
7. and eventually add to all our devices

> [!tips]
> It takes about 18-24 months.
> Look up to Unicode register or Emojination for any new emojis.


**Can your idea be an emoji?**

* Popular demand, frequently request
	* if we search for it on Google, does it have 500 millions plus results?
	* which is what 'elephant' gets, it's like a median
		* right at the middle of popular emoji and not popular emoji

* Multiple usages/meanings
	* eg. 🦥 sloth emoji

* Visually distinctive enough, can be recognized
	* since emoji is small in size

* Filling gap, improve emoji's "completeness"
	* eg. for long time there are ❤️💛💚💙💜, but not 🧡
	* a gay designer from Adobe was very heartbroken by that, and proposed that

* Existing vendor compatibility
	* is the emoji exists in some third-parties or companies
	* eg. Whatsapp one day randomly added 🏳️‍⚧️


**Bad emoji**

* too specific, narrow
	* eg. poutine for Canadians

* Redundant
	* eg. Butterball proposed a roasted turkey emoji, but there is already an unroasted live emoji of a turkey

* Not visually discernible
	* eg. kimchi (泡菜), cave

* No logos, brands, deities, celebrities

* No more flags
	* they regret to add flags
	* lots of politics
___

# Why Unicode controls emoji?

emoji is originally popularized in Japan
* Docomo, 1999

the problem is all the Japanese vendors at the time have their own set of glyphs
* they have different representation for the same pattern of bits or bytes
that makes users can only text to people who are on the same platform
* Docomo people can only text to Docomo people
* SoftBank people can only text to SoftBank people

Later when Apple and Google started introducing smartphones into Japan
* since it's expected when we did something in smartphone, it also shows up in other devices and maybe other people's devices
* most Japan devices can't do that at the time

In 2007, Japanese vendors went to Unicode and asked them to unify the emoji set
eventually it took 3 years to introduce the first Unicode emoji set, Unicode 6.0 in 2010


**Why Unicode?**

> [!Unicode's mission]
> Enable everybody, speaking every language on the Earth, to be able to use their language on computer and smartphones.

So it's more like a human right thing to Unicode. Because at a certain point, if a language cannot be captured digitally, it's going to disappear.
* they spent a lot of time doing Chinese, Arabic, Cyrillic in the very early days
* In 2001, they had a proposal for Klingon although it's not approved

They have 3 main projects
1. Encoding characters, including emoji
2. Localization Resources
	* Common Locale Data Repository, "CLDR"
		* fun fact, a confused girlfriend of one of the engineers heard it as "seal deer" and made one "seal deer" puppet
	* eg. in this country the date time format is `DD/MM/YYYY`, in another country it's `MM/DD/YYYY`
3. Programming Libraries
	* International Components for Unicode, "ICU"

Unicode doesn't want to be encoding emoji
but somehow it's part of their business

extra.
Back when 2010, emojis are kinda just lay there in our devices, seldomly used
In 2011, Apple makes it much easier to access on our phones
___

# How emoji works?
* emojis are Unicode characters

> [!quote]
> A Unicode code point is a unique number assigned to each Unicode character.

eg. 😂
= `U+1F602`
= `128514`
= `11111011000000010`

the thing is, back in the days, there are limited emojis for women
male emojis have many occupations, but female emojis have only 4
* princess, bridge, dance, playboy bunny

later people introduced **ZWJ**, *Zero Width Joiner*, to emojis
which can glue emojis together with a "ZWJ" character in between to form another emoji

🏳️‍🌈 = 🌈 + ZWJ + 🏳
👩‍🌾 = 👩 + ZWJ + 🚜
👨‍🍳 = 👨 + ZWJ + 🍳

because of the gender parity stuff
we now also have 🤵‍♀️👰‍♂️🧔‍♀️👨‍🍼

Skin tones emojis are not using ZWJ though, they used an older technology
they are a generic yellow character, plus a color box like 🟨🟧🟫 at the end
* called skin tone modifiers

fact: Emojination and Tinder once work together for the "interracial couples" emojis 🧑‍🤝‍🧑💏💑
* apparently Tinder makes the rates of interracial marriage go up
* Skin tone emoji + ZWJ + Skin tone emoji


**How emoji can work?**

A Standford professor proposed creating hashes for emojis
* so we use hash to look up emoji instead of representing them as Unicode
* didn't get take off though

Using Wikipedia's or Wikimedia's wiki data QID numbers
* everything in Wikipedia has a number that allows it to match things between different languages
* eg. there is an Obama page and there are different language pages for Obama
___

# Emoji's Stories
The number of new emoji and proposals decreased per year so far

🧕 was proposed by a 15-year-old Saudi Arabian girl but lived in Germany, Rayouf Alhumedhi
* fun fact: who got an offer from Harvard but chose Standford
* who also is the subject of a documentary The Emoji Story

🧉 was proposed by a group of Argentinians

🩸 was made possible by a non-profit for girls advocacy, Plan International
* originally they proposed a menstruation emoji, which has a "bloody underpant" image
* which is terrible, so later re-designed the blood emoji together with Emojination

Skin tones in emoji were proposed by a mom from Houston, who also is an entrepreneur
* her daughter shares to her emojis
* she actually had worked in procurement with NASA so she understood forum for proposals

🥿🩱 were proposed by a mother
* who was very offended at that all the shoe emojis for women had high heels

🤨 was from a guy in Germany

🧖🧖‍♂️🧖‍♀️ were originally proposed by the Finnish government
* originally they proposed a "sauna" emoji and later re-designed with Emojination's aid
* funny enough the final look is from Unicode instead of the submitted design

Here are some of the emojis that Emojination made
🧶🧵🥿🥾🧤🧢🧕🥽🦞🦒⛅🥦🥟🥠🥮🥪🥫🥨🥯🥣🥡🥢🥤🧁🦠🧫🧽🧹🧻🧸🧨🧱🧧🧼🧩🧬🧣🧺👁️🧘🥬🦚..........

Most used emojis are
1. 😂 - 9.9%
2. ❤️  - 6.6% (red heart)
3. 😍 - 4.2%
4. 🤣 - 3.2%
5. 😊 - 2.0%
6. 🙏 - 1.9%
All fall off pretty quickly
___
