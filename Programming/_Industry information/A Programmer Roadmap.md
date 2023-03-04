https://youtu.be/pEfrdAtAmqk
___

# Noob

* Scrach
	* a programming tool to learn programming without knowing programming

* Basic
	* a friendly programming language with basic commands and easy syntax
	* included in most personal computers, at least in the old days
___

# Mega Popular
**Dynamimc High-Level Languages**

* Python
	* have minimal syntax

* JavaScript
	* required for web development
	* every developer would have to touch it at some point
	* "Any application that can be written in JavaScript, will eventually be written in JavaScript."

This is all you need.
You can build any application from now on.
___

# Specialized
but still **Dynamic High-Level Languages**

* Bash / Powershell
	* [[CS50x 1-1 Programming Environment#Command Line Interface|Command Line Interface]]
	* instead of typing commands again and again, we can write a bash script to make it reproducible

* HTML / CSS
	* required for web development
	* define structure and style of a website
	* don't say HTML is not a programming language...

* Structured Query Language = SQL
	* used to work with database
	* not regular programming, but it can read and write data in a relational database

* php
	* easy to build server-side web apps

* Lua
	* easier and faster than Python
	* embedded into many engines
		* Roblox
		* World of Warcraft

* Ruby
	* an easy to learn object oriented language
	* common to build web apps with the *rails framework*

* R
	* used in data science. used for statistics and data visualization

* julia
	* used for scientific computing

All of these are *dynamic type system*.
___

# Static Type System
**Static High-Level Languages**

it allows a more rigid framework

* Java
	* it uses *java virtual machine (JVM)*
		* Java compiles to *bytecode* that runs on the JVM
		* allows developers to target any computer architecture from a single codebase
	* syntax is awful

* C#
	* can build games in Unity
	* can build web and desktop apps with .NET framework

* TypeScript
	* = JavaScript + a type system
	* easier to work with on large complex projects

* Kotlin
	* build mobile app for Android

* Swift
	* build mobile app for iOS

* Dart
	* build applications with the flutter framework

* GO
	* a high performance language from Google to build low-level systems
	* designed to replace C
		* *Ken Thompson*, one of the original creators of C, helps design it
	* has a garbage collector
		* unlike C, it has automatic memory management
___

# Functional Languages
this is where developers who start to hate object-oriented languages would go

* Haskell
	* no need class inheritance or design patterns
	* it only need the function
	* inspired by the *miranda language*, and named after a mathematician *Haskell Curry*
	* variables are immutable
	* functions have no side-effects
	* can build almost anything with these limitations
		* although most Haskell developers run into problems when trying to figure out what a *monad*[^1] is...
[^1]: A Monad is just a monoid in the category of endofunctors. <--- ???

* F#
	* a functional sister language to C#
	* unlike Haskell is purely functional, F# is *functional imperative object-oriented*
	* so it's more approachable

* Scala
	* like F#, supports both *OOP* and *functional* programming
	* but it runs on a Java Virtual Machine

* Clojure
	* both *functional* and *dynamic*
	* can get things done quickly, but worse type safety

* Ocaml
	* used extensively at Facebook

* Elixir
	* has a Ruby-like syntax
	* capable of building high-performance real-time web apps

* ELM
	* purely functional language that compiles to JavaScript
	* can build front-end UIs with zero runtime errors
___

# Systems Languages
low-level systems languages, that does manual memory management
used to build things like operating system kernels and compilers that make all the other soy-based languages
= mother of all languages

* C
	* legendary
	* build the Windows, Mac, Linux operating system kernels
	* surprisingly it's easy
		* has a smaller set of keywords to memorize
	* but to use it effectively, requires extensive knowledge of algorithms and computer architecture
		* eg. C doesn't have hash maps or dictionaries
		* if we want to use them, we need to code up that data structure on our own
	* only supported procedural programming

* C++
	* originally a super set of C
	* designed to extend C with object-oriented programming patterns
		* like classes and inheritance
	* unlike C, it's extremely hard
		* "C makes it easy to shoot yourself in the foot; C++ makes it harder, but when you do it blows your whole leg off."
		* it refers to *manual memory management* with *pointers*
	* extremely prolific
	* used to build highly optimized software
		* like game engines, compilers, etc.

* Rust
	* doesn't have a garbage collector
	* uses a technique called *borrow checking* instead of *pointers* for memory management
		* so easier to write memory-safe programs
	* consistently ranked 1 as the most loved language
___

# Modern Languages
you probably never heard of

* V
	* high performance systems language
	* feels similar to *go*, but doesn't have *garbage collector*, and doesn't have *borrow checker* either
	* but still can create memory safe applications, with its own *autofree* innovation
		* it means the compiler will cleans everything up

* ZIG
	* designed to simplify low level programming
		* eliminated features like *macros* and *metaprogramming*
		* have explicit pointers
	* can cross compile C and C++

* nim
	* another high performance languages
	* very expressive like python
	* statically typed
	* has a tunable garbage collector
		* can be turned off, and then use pointers again

* Carbon
	* from Google
	* designed to be a successor to C++
	* can fully interop with a legacy C / C++ code base

* Solidity
	* statically typed object-oriented language
	* designed for implementing *smart contracts*
		* especially on the *ethereum* blockchain

* Hack
	* from Facebook
	* designed to interop with php
	* use case: original website is built with php, but it needs a better performance + type system
		* to scale up to the modern monstrosity

* Crystal
* Hacks
* Pharo
___

# Historically Important Languages

* Fortran
	* 1st high-level programming language
	* the most popular one until C came around

* LISP
* Algorithm Language
* Cobol
	* used in most banking system
* APL
	* stands for *A Programming Language*
* Pascal
	* used to be #1 between 80s and C
* simula
* smalltalk
* Erlang
	* basically powered the entire telecom industry
* Ada
	* named after *Ada Lovelace*, who is generally considered the world's first computer programmer
	* still used today by the *department of defense*
* Prolog
* META
___

# Esoteric Languages
= funny and bizarre languages

* InterCal
	* one of the keywords is *please*
		* it does nothing but makes you a more polite programmer
* Brainf\*\*k
	* extremely minimal
* Malbolge
	* Design Goal: make programming harder
* Chef
* Shakespeare
* PIET
* LOLCODE
* Emojicode
* C--
* Holy C
___

# Absolute Lowest Level

* Assembly
	* correspond directly to the architecture on the CPU

* Machine Code = Binary
	* 1s and 0s
	* usually represented in hexadecimal format
	* at this level, you need to
		* have intimate knowledge of the computer's architecture
		* able to count in binary

* Transistors
	* a transistor represents 1 bit
		* if stored electricity, it's 1
		* if not, it's 0
	* need to be organized to *Logic Gates*
		* AND, OR, NOR, NOT, NAND, XOR

* Quantum Electrodynamics
	* to build *quantum computer*
___
