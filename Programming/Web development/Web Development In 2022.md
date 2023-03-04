[Web Development In 2022 - A Practical Guide](https://www.youtube.com/watch?v=EqzUcMzfV1w)
___

# Intro

The line between Frontend and Backend is becoming more blurred
* every body is Fullstack now

Programming Tools
* git & GitHub
* markdown, useful in GitHub
* npm
	* stands for *node package manager*, and it's the official JavaScript package manager
		* so it requires Node.js to be installed
	* an alternative is yarn, which is quite popular as well
		* it shares the same repository of packages with npm
* be familiar with your browser's dev tools
* there are many Visual Studio Code extensions
	* Emmet, for writing HTML and CSS
		* available with VS Code by default
	* Live Server, Prettier, ESLint, GitHub Copilot, etc.
___

# Frontend Tech

Frontend devs and freelancers are very likely to need to know design, for that
* Figma, Adobe XD, Sketch
* Figma probably is the best for beginners

HTML & CSS
* learn HTML semantic tags
* learn CSS flexbox & grid
* lean media queries
* learn animations / transitions
* Sass is a good tool as well

CSS Frameworks
* Bootstrap is the most popular
	* include some JavaScript components as well
* Tailwind CSS
	* unlike Bootstrap, which is high-level, Tailwind CSS is low-level
	* basically it means one class does only one thing
		* meaning we have to write way more classes
		* but we gain more control over the elements
* others, eg. Materialize, Bulma, Foundation

> [!tip] UI Design Principles
> make sure you know some basic design principles if you're a frontend developer
> - [?] Color & Contrast
> - [?] Whitespace
> - [?] Scale
> - [?] Visual Hierarchy
> - [?] Typography

JavaScript
* basics & DOM
* Ajax & Promises
* fetch API & HTTP
	* so JSON and XML are important as well
* Array methods

Platforms for Deploying Frontend Projects
* eg. netlify, Vercel
	* create a GitHub repository, then select the repository in these platforms to deploy
	* both have a very generous free tier
* there are multiple ways to deploy to a hosting platform
	* eg. git, ssh, ftp/sftp
	* ftp is the old school way of doing things so it is pretty slow

Domain Name Registrars
* 10 to 15 dollars per year
	* unless you're buying a specific domain name off someone, then it can get really expensive
* eg. namecheap
* some companies offer shared hosting, vps, dedicated servers
	* eg. bluehost, hostgator

Frontend Frameworks
* we build our user interfaces with *components*
	* it's encapsulated piece of code that usually include the output or HTML as well as the JavaScript logic
* React.js is the most popular framework
	* it's created and maintained by Facebook
	* need to learn React Router, JSX, Hooks, ContextAPI as well
		* Redux is less popular recently but still widely used
* Vue.js is the second most popular framework
	* it doesn't have a backing from a company
	* easier than React and Angular
	* Vue CLI, Vue Router, Composition API, Vuex
* Angular.js is popular among enterprises but we won't see it in a startup
	* created and maintained by Google
	* have a steep learning curve, but offer more functionality by default
	* Angular CLI, Services, Observables
	* it uses TypeScript by default
* Svelte
	* it's not a framework but a JavaScripte compiler
	* so it's very lightweight
	* so it's much easier to use compared to others

also, it's better to learn TypeScript as well
* it's JavaScript with additional features, including
	* static-type checking
	* since it's statically typed, our code is more robust and definitive
		* less prone to errors

UI Kits
* look into styled-components as well
* React
	* Material UI is the most popular
	* Chakra UI is easier to use
	* Onsen UI targets more on mobile apps
	* React Bootstrap
* Vue
	* Vuetify is the most popular
	* Vue Material, Buefy, Bootstrap Vue
* Angular
	* Angular Material
	* Ng-Zorro, Ng-Bootstrap, MD Bootstrap
* Svelte
	* Svelte Material UI
	* Smelte, Materialify, Sveltestrap

From now on, you can further develop to different routes
* web design, learn how to make pretty web page, UI/UX
* frontend framework
* backend
* advanced JavaScript
	* not a very good route though in terms of money
___

# Frontend Superstar

Testing
* many developers don't know testing well, but it's something good to know
* Unit Testing, Integration Testing, E2E Testing
* JS testing tools include Jest, Cypress, Puppeteer

Server-Side Rendering
* helps us get away from single page application
	* since single page application is managed through JavaScript, search engine can't really crawl the site
	* so server-side rendering solved this, in addition performance is better, routing is easier
* Next.js
	* it's for React
	* solved limitations from React
* Nuxt.js
	* it's pretty much the same as Next.js
	* but it's for Vue
* Remix
	* it's for React
	* offers some different features to Next
* SvelteKit
	* obviously it's for Svelte
* Angular Universal
	* for Angular

Static Site Generators
* allow us to generate web pages or content using data of various format
	* eg. markdown
* it's very performance as well because things are static and pre-loaded
* Gatsby
	* uses React
	* we can use GraphQL as well
* Next.js
* Gridsome
	* uses Vue
	* ready for progressive web app
* Jekyll
	* Ruby based
	* quite old actually
	* uses the Liquid Template Engine
* there are others for php, Go

Headless CMS
* CMS = Content Management System
* allow us or our clients to have some kind of admin area
* it has nothing to do with the frontend, so we're free to use whatever frontend stuffs
	* it just spits out a JSON API
* they take away a lot of works for us when it comes to backend and server
* Strapi
	* open source and self hosted
* Sanity.io
	* has free tier
	* great for collaboration
	* offers Sanity Studio Toolkit
* ContentFul
	* great for teams, so it's popular in the large businesses world
* GraphCMS
	* allow us to use GraphQI

[Jamstack](https://jamstack.wtf/)
* Jam = JavaScript + APIs + Markup
	* but they got rid of that acronym already
* it's an architectural approach for building websites

> [!important] What a Frontend developer should be able to do?
> - [?] build user interfaces with a frontend framework
> - [?] understand and work with local and global state
> - [?] work with REST APIs & HTTP
> - [ ] use TypeScripte
> - [ ] maybe use GraphQL as well
> - [ ] work with Server-Side Rendering and Jamstack technologies
> - [ ] maybe Static Site Generator and Headless CMS as well
> - [ ] write unit, integration, e2e tests

___

# Backend Tech

Language
* Node.js
	* where it falls short is CPU heavy tasks
	* Deno is another server-side tech that uses JavaScript
		* not sure can it keep being relevant though
* Python
* PHP
	* it used to be bad, but it's all good now
	* it's still widely used
	* great for freelancing and small business
* C#
	* .NET, ASP.NET are popular
* GoLang
	* used on large scale servers and big distributed systems
* Ruby
* Java
	* widely used in the enterprise world
* Kotlin
	* relatively new
	* designed for the JVM, *Java Virtual Machine*
		* so it's replacing Java
	* widely used in Android apps
* Rust
	* can be great for things like microservices
* Scala
* R
	* strong in Python stuffs
		* eg. statistical computing, data analytics, scientific research, etc.
* Swift
	* for developing native iOS apps, along with Objective-C

Web Framework
* Node.js
	* Express is the most popular
	* Fastify
		* as its name implies, it's super fast
	* Koa is very similar to Express
	* Nest.js
		* Angular developer might like it
	* Adonis
* Python
	* Django
		* probably is the best framework out of any language
		* it's very specific though, we have to build things in Django way
			* so it's pretty high-level
	* Flask
		* unlike Django, it's very low-level and minimalistic, much like Express
	* FastAPI
		* as its name implies, it builds fast APIs
* PHP
	* PHP is a bit different to other languages because it basically is a templating language itself
	* Laravel
		* easily the top five web framework out of any language
		* PHP itself can be messy, but Laravel is really elegant
	* Symfony
		* Laravel is actually built on Symfony
	* Slim
		* it's Express for PHP
* C#
	* ASP.NET and ASP.NET MVC
	* C# is not so frequently used in web dev, but used in Windows app development
* Go
	* it doesn't even need an additional framework, but it does have some
	* Gin probably is the most popular
	* Beego is used for rapid development of enterprise applications
* Ruby
	* Ruby on Rails
		* it used to be really really popular
		* it's not that popular now but it's still great
	* Sinatra
		* sometimes referred to as a micro framework
* Java
	* Spring
		* Sprint Boot is one part of it
			* which is good for building standalone applications
		* Spring Cloud is for building cloud native apps
	* Struts
* Kotlin
	* Spring
		* yes Kotlin can use Spring as well
	* Vert.x
* Scala
	* Play
		* Play framework can be used with Java as well
	* Lift
* Rust
	* Rocket
	* Actix Web

Databases
* it actually has two kinds of database: relational databases, NoSQL databases
	* relational databases store data in tables
	* NoSQL databases store data in collections
		* which are usually formatted like JSON Objects
* PostgreSQL
	* it's a object relational database
		* as opposed to MySQL which is purely relational
	* so it has features like table inheritance, function overloading
* MySQL
	* actually we won't see much of a difference between PostgreSQL and MySQL
		* especially in small applications
	* it's popular in the PHP world
* Microsoft SQL Server
* SQLite
	* it's file based database, so it's very rarely used in production
	* but it's great for development since we don't have to go through some crazy install process
* MongoDB
	* it's a NoSQL, but it's a bit different even in NoSQL
	* it's known as a document database
		* it holds collections of documents
		* each document is basically like a JSON Object
	* so it's popular in the Node.js world
	* MERN = MongoDB + Express + React + Node
		* others including MEAN, MEVIN

* Firebase
	* it's more than just a NoSQL database
	* it's an entire hosted platform by Google
	* it's a great all-in-one platform so it's great for small to medium sized projects
	* the downside is that we don't have full control over the database
		* our project is controlled by Google
* Supabase
	* much like Firebase, but this one uses PostgreSQL
	* it's open sourced
	* since it uses PostgreSQL, we can back up the database and move it to a new server

* Redis
	* it's different from everything else
	* because it's an in-memory data structure store
		* so it's often used for caching

And there are actually many tools that act as an abstraction layer on the databases
so we actually don't have to write SQL statements
* it's called ORM
* Some frameworks like Laravel and Django have built-in ORMs

ORMs
* Sequelize
	* for MongoDB
* TypeORM
* Mongoose
	* for MongoDB with Node.js
* Prisma
	* powerful when working with server-side rendering framework like Next.js or Remix
* SQLAlchemy
	* for Python
* Doctine
	* for PHP

REST APIs
* REST = Representational State Transfer
* very important to understand

GraphQL
* people though it could be the next for REST APIs, but it still isn't
* we need to setup a GraphQL server in the backend
* we have more control over what we get back from an API
* easier to maintain than REST
* Apollo is a popular GraphQL client

How to do Authentication?
* JSON Web Tokens
	* it's a popular way to do this with single page applications
* Cookies & Sessions
* OAuth
	* it means using Google, Twitter, GitHub, etc. to login
	* so we delegate the authentication
* Authentication libraries
	* eg. Passport.js for Node.js, Grant, etc.
* Password hashing
	* there are libraries that help this, eg. Bcrypt
* and we need to protect endpoints and routes as we're creating APIs

Wordpress
* still widely used despite having some criticisms
	* people who need to build a product within 2 weeks might still use Wordpress happily
* great for freelancing or small businesses
	* small business clients like it too
* it can be used as a headless CMS

Hosting Platforms
* aws = Amazon Web Services
	* really really popular
	* especially for really big in-depth applications
	* can be a bit overkill for some projects though
		* interface is very complicated
* heroku
	* setup is easy, we simply push our code using git
	* it runs on Deno
* DigitalOcean
	* offers cloud servers called droplets
		* which are basically Linux servers that we can install whatever we want
	* a similar alternative would be Lenode

Web Server
* APACHE
* NGINX is easier to work with

Containers & Virtualization
* DevOps must learn these things
* docker
	* a really popular container software
	* uses operating system level virtualization to deliver software packages called containers
	* makes everything run the same on different environments
		* very helpful for teams
* kubernetes
	* frequently used along with docker

Image & Video Upload
* Cloudinary
	* stores and optimizes images
	* gives us an API with different image sizes and so on as well
* Amazon S3
	* it's actually a part of aws
	* provides object storage through a web service interface
	* it's very scalable as well

> [!important] What a Backend developer should be able to do?
> - [?] Comfortable with server-side programming language
> - [?] Know how to setup and manage a database
> - [?] Know how to create REST APIs
> - [?] Comfortable with the terminal and Unix commands
> - [?] Know how to deploy projects and work with servers

___

# Mobile Tech

There are many ways to build mobile apps with different technologies. eg.
* build native iOS app with Objective-C or Swift
* build native Android with Kotlin or Java
* build cross-platform apps with web-friendly tools. eg.
	* React Native
		* uses React to build apps for iOS and Android
		* have native capabilities
			* it uses React Native Bridge to invoke the native APIs
	* Flutter
		* created and maintained by Google
		* uses the Dart programming language
			* which is pretty similar to JavaScript
		* it's extremely fast
			* faster than React Native because React Native leverages JavaScript to connect to native components via the bridge
			* while Flutter doesn't need all these
	* Ionic
		* great for progressive web apps or web apps optimized for mobile devices
		* slower than React Native and Flutter
		* but we're not bound to any framework
	* Xamarin
		* used along with .NET & C# to build cross-platform apps
		* high performance
___

# Web3
clearly there are hypes going on around Web3

> [!info] What's the matter of Web3?
> Web 1.0 was basically the information web. We went to websites and it was mostly just read only content.
> 
> Web 2.0 is the interactive web where the public creates content. However, everything is centralized. All the data and the control is all centralized by the developer such as Twitter, Amazon, Google, etc.
> 
> Web 3.0 is about decentralized apps, *Dapps*, that run on blockchain. Therefore it offers benefits like transparency, security, anonymity and more that Web 2.0 doesn't.

> [!info] more about blockchain
> Bitcoin is the first major use of blockchain. At its core, it's a digital asset. But blockchain allows it to work like how currency works, so we name it cryptocurrency.
> 
> Then Ethereum came out and it's extremely popular, because it's more than just a digital asset. It maintains a decentralized payment network. How it works is that it uses smart contracts to deal with the transactions, which is a fancy of saying code for doing decentralized transactions.
> 
> Then how smart contracts are created? It's written in Solidity.

Solidity
* a statically typed programming language
* designed to develop smart contracts
* smart contracts run on the Ethereum Virtual Machine, *EVM*
* it's pretty high-level
	* if you know JavaScript or Python, you should be able to pick it up pretty quickly

NFTs
* Non-Fungible Tokens
* works much like Bitcoin, except it can be things other than currency

> [!warning] Why some people hate NFTs?
> NFT isn't the problem. The problem is there are a lot of scamming going on in the crypto and NFT world. There's all kind of pump and dump schemes.

> [!info] Is Web 3.0 good?
> We can expect Web3 will get huge at some point. But it's a little like the wild west right now.
> But yes, it's benefitial to learn these stuffs.

___

# Web Assembly
or WASM for short

as its name implies, it's a low-level assembly-like language for the web
* so it can create really high performance applications
* we write programs in Rust, C, C++, then compile to WASM
	* so its performance is much better than JavaScript
* but if we want to use JavaScript, there is a language called AssemblyScript
	* which is a TypeScript variant
	* then we can compile JavaScript codes to web assembly
___
