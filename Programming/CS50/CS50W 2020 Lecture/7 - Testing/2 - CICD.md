# CI/CD
*Continuous Integration and Continuous Delivery*

it refers to two best practices in the software development world
* how the code should be written
	* especially by groups or teams of people
* how the code should be delivered and deployed to users
___

## CI
Conatinuous Integration

* Frequent merges to main branch
* Automated unit testing

In the past, you might imagine that if multiple people are working on the same project, each developing different features or different parts of the project, then after everyone's done, so the project is ready for shipping for a new version, then people need to figure out how to combine them together and deliver the program to users.

Surely it has a tendency to cause problems, especially if people have been working on different big changes, all simultaneously. Because they might not all be compatible with one another. There might be conflicts between the different changes.

And you can imagine how much of a pain it is to merge several big changes together, if we wait until everyone is done working on their different features.

So CI is a solution that people came up with. The idea is that,
1. merge, to the main branch, frequently
	* so people are making smaller changes
		* so that it is less likely to cause problems
		* and if there were problems, it should be fairly easy to point out
	* so it's less likely to develop into two really divergent paths that the program has gone under, making it really hard to merge back together
2. we should do automated unit testing
	* remember unit testing refers to this idea of running a big series of tests that verity each little part of our program is indeed working correctly
	* although unit tests generally refer to testing small components, there are also bigger scale tests, like integration tests, to make sure the entire pathway is working as well
		* eg. a certain pipeline which involves some to many user requests and responses
	* the big idea is that any time someone made new changes and then they want to merge the changes to the main branch, then these tests should be run
		* so nobody ever make changes that break some other part of the program

> [!tip] None programmer is perfect.
> In a large enough code base, it's impossible for any one person to know exactly what the effect of one particular change is going to be on every other part of the program.
> 
> There are going to be unforeseen consequences.

Therefore, we rely on this CI practice,
* so if something doesn't pass a test, we'll know about that immediately
* as opposed to, waiting until everything is done, then merging everything together, then running the tests, then failing everything, and the worst part, we don't even know where to begin
	* we don't know where the bug is, or which change cause the bug
___

## CD
Continuous Delivery

this one is related to how the software is released to users
* how the application actually get deployed

you might imagine that the release cycle might be quite long
* like people spend months working on various different features
* and after they're happy with all the new changes, they release some new version of the web application

actually, this is not something we would like
* instead, we prefer having short release schedules
* so release cycles actually tends to be ever day or week
* so basically, changes made to the main branch, should be released perhaps some hours or days later, as opposed to weeks or months later

it comes with some benefits
1. if things went south, we know when and where
2. new features get out to users much more quickly
	* which is quite essential in a competitive market

it also sometimes represents Continuous Deployment
* although Continuous Delivery and Continuous Deployment is a bit different
* Continuous Deployment means the deployments happen automatically
* so we human have one less thing to think about
	* and that Continuous Delivery can happen even more quickly
___

# GitHub Actions

so now the question becomes what tools to use for CI or CD?
* one of them being GitHub Actions, for CI
* the idea is that, any time someone pushes changes to a GitHub repository, we would like for certain steps to take place
	* eg. checking to make sure that the code is styled well
	* eg. run testings
* then GitHub might send us email saying, "someone pushes some changes, we run a test for it and it failed"

it uses this language called YAML
* it's a configuration language
	* it's often described for configuration of various different tools and software
* here is how a YAML file is structured
```yml
key1: value1
key2: value2
key3:
	- item1
	- item2
	- item3
```
* again, just key-value pairs
	* kinda like a JSON object or a Python dictionary
	* here doesn't show it but YAML also can have nested key-value pairs
* `.yml` or `.yaml` are the conventional file extensions for a YAML file

here is an example, `.github/workflows/ci.yml`
* which is located inside of our Django project's root
```yml
name: Testing
on: push

jobs:
  test_project:
  runs-on: ubuntu-latest
  steps:
  - uses: actions/checkout@v2
  - name: Run Django unit tests
    run:
      pip3 install --user django
      python3 manage.py test
```

this now is the configuration for how this workflow ought to behave
* its name is `Testing`
	* because this workflow tests our web application
* when should this workflow run? on push.
	* meaning any time someone pushes their code to GitHub, then run this workflow
* every workflow consists of some jobs
	* what should happen, or what it means, to run this workflow?

* in this case we have one job only, `test_project`
	* this is a job's name, can be any name that we want
* what machine should this job run on?
	* GitHub has its own virtual machines (known as VMs), there are virtual machines for various different operating systems
	* here we want it to be run on `ubuntu-latest`
		* the latest version of Ubuntu, which is the latest version of Linux
* what steps are involved in the job? or what actions should happen we run this job?

* `actions/checkout@v2` is an action that GitHub provided
	* it will checkout the code in the Git repository
	* so `uses: actions/checkout@v2` makes GitHub now look at the branch that just get pushed
		* so the job is performed on this branch
* then, what do we want it to do? we want it to run Django unit tests
	* `name: Run Django unit tests` is just a name, again, it can be any name
* then under `run:`, are the things that we would like it to run
	* `pip3 install --user django`, makes the VM installs Django
		* if there are other requirements, we need to install those first
		* but here the only requirement is installing Django
	* `python3 manage.py test`
		* run the test
___

## more GitHub features

we will first see what happen if it failed the test
* so we introduced some bug intentionally

now we push the changes
```bash
git add .
git commit -m "Use wrong valid flight function"
git push
```

now we go to our GitHub repository
and while we're at it, let's introduce some of the repository tabs
* Issues
	* a place where people can report that something is not quite right
	* or there is a feature request
	* so the Issues might maintain a list of all pending action items
		* ie. things that we still need to deal with
	* once an issue is dealt with, it can be closed
* Pull requests
	* for people that are trying to merge from one branch into another
	* in a larger project, the master branch is probably protected
		* so people can't directly merge into the master
		* instead they need to propose a pull request
	* when a pull request is approved, the merge actually takes place
	* and that allows for some other features
		* like someone could offer a code review
			* that someone can review the code, write comments, and propose suggestions for what changes should be made
			* before allowing the changes to be merged into the master branch
		* code review actually is a common practice
* Actions
	* represent GitHub Actions
	* we can see all the performed workflows here
	* it will clearly show us whether the workflow has been performed successfully
		* and we can see what step in what job in the workflow causes it to fail, if it failed
		* we can open that up, and see the results of the unit tests
	* and actually if we go back to the Code tab, we can see the commit is labelled with a ❌
		* meaning something went wrong in this commit, when running the workflow
		* if it's 🟡, it means the tests are pending, the workflow is in progress
			* since GitHub needs to take some time to start up the VM, initialize the job, check out our code, and run the test, ....
	* and surely we can add rules to say that, "I don't want to merge code into the branch if the tests don't pass.", to guarantee that any code that does get merged has passed the tests
___

# Docker

when we're deploying applications to a web server, there often times are some troubles
* like, the program was running fine on our computer, but it just doesn't work on a web server
* maybe due to some configuration problems
	* after all, your computer and the computer on the cloud, are different
	* eg. maybe the operating systems are different, maybe the Python versions are different, maybe we have installed some certain packages but they don't, ....

likewise, if you're working in a team, there are more computers
* there are more configurations to think about

so how do we make all sort of different environments, work the same?
* again, there are many solutions
* one of the most popular is Docker

It's some sort of containerization software
* it means, when we're running an application, instead of just running it on our computer, we run it inside of a container on our computer
* and each container is going to have its own configuration
	* eg. installed some certain packages, installed some certain versions of software, ...
* so different computers with the same container, can be considered as configured in exactly the same way
	* actually, we don't get to just use the same container, but we have instructions for how to set up a Docker container
* if a package is installed in our container, it's installed in our colleague's container as well
	* and it doesn't have to be our colleague's container, it can be a container in a server
	* so this idea benefits CD

one thing to be aware of, container and VM are in fact different
* VM is like a virtual computer, that has its own operating system and libraries and application
	* all inside of a "physical" computer
	* so it usually ends up taking up a lot of memory
* Docker containers are a bit lighter-weight
	* because they don't have their own operating system
	* they're running on top of the host operating system, but there is this Docker layer on top of the OS, that keeps track of all of these different containers
	* and keeps track of for each container, what libraries and binaries and what application are installed
___

## how to Docker

we need to write a Docker file
* it describes the instructions for creating a Docker image
	* which represents all of the libraries and other things that we want to have and install inside of the container
* so this is how we create the same container in different computers

`Dockerfile`
```Dockerfile
FROM python:3
COPY . /usr/src/app
WORKDIR /usr/src/app
RUN pip install -r requirements.txt
CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]
```

`FROM python:3`
* it is actually another Docker image
	* it's going to install Python 3 and the related packages
* on which, we're going to base our instructions
	* so our instructions come in to play after Python 3 is installed

`COPY . /usr/src/app`
* we copy everything in our current directory, `.`, into the container
	* we can choose where the container is installed
	* in this case it's `/usr/src/app`
* basically, we copy our whole project to the container

`WORKDIR /usr/src/app`
* change the working directory

`RUN pip install -r requirements.txt`
* install the requirements
* assuming we put already all the requirements in that file

`CMD ["python3", "manage.py", "runserver", "0.0.0.0:8000"]`
* the command to run, when we start up our container
* the command is `python3 manage.py runserver 0.0.0.0:8000`

and the nice thing is,
it can run on Mac and Windows and Linux
___

## Docker Compose

thus far we have been using SQLite
but in most production environments, in most real web applications, people rarely use SQLite
* because it doesn't scale well
	* it can't handle many users that are trying to access it concurrently

so often times, we want our database hosted elsewhere, on some separate server
* so it handles its own incoming requests and connections
* eg. MySQL, PostgreSQL

but then, it seems that it's quite hard to test on our own
* because we now have one server for our web application, and one server for PostgreSQL
* well, Docker effectively allows us to run each of these processes in a different container

we can have one container, running our web application, using one Docker file
we can have another container, runing Postgres

there is a feature of Docker, known as Docker Compose
* it allows us to compose multiple services together
* we have one container for the web application and one container for Postgres, but we can have them talk to each other
* we need a YAML file for that

`docker-compose.yml`
```yml
version: '3'

services:
	db:
		image: postgres
	
	web:
		build: .
		volumes:
			- .:/usr/src/app
		ports:
			- "8000:8000"
```

it's a file that describes all of the various different services that I want to be part of our application
* each service is going to be its own container
* each could be based on a different Docker image

here we have two services, `db` and `web`
* `db` is gonna based on the `postgres` Docker image
	* it's an image that Postgres wrote, we don't have to worry about it
* `web` is going to be built based on the Docker file in my current directory, so `build: .`
	* which is the Docker file that we wrote
* we specify the current directory should be correspond to the `/usr/src/app` directory
* we also specify that when we're running this on our own computer, the port `8000` on the container is corresponded to the port `8000` on our own computer
	* so if we go port `8000` in our browser, we get into port `8000` of the container
	* so we can open up the web application, which is in the container, with our browser, which is in our computer

and then here is how to start up a Docker container
* we run a command in our terminal
```bash
docker-compose up
```
* meaning, "go ahead and start up the services"
* it will output things like
```bash
Starting airline_db_1  ... done
Starting airline_web_1 ... done
Attaching to airline1_db_1, airline_web_1
...
```

so now our application is started up
* on port `8000`
* and we can go to that address, `0.0.0.0:8000/flights/`, and see, in our browser
	* note that it's running inside of the Docker container
___

## get into a container

now say if we want to create a super user for the application
* we can't just do `python3 manage.py createsuperuser` in our directory
* because that would be creating a super user in the project in our directory
	* not the Docker container's

so how we can do that?
there are some Docker commands to use

`docker ps`
* show all of the Docker containers, that are currently running
* since we want to get to the container where our web app in, we need to copy its ID

then use, `docker exec -it <containter_id> bash -l`
* meaning, execute a command on the container `<container_id>`
* `-it` to make it interactive
* `bash -l` is the command, which will run a bash prompt
	* so that we can run commands inside of that container
* now you can see our directory becomes `root@<container_id>:/usr/src/app#`

then we can run `python3 manage.py createsuperuser`
* to create a super user
* same for other commands

finally, we can press `Ctrl + D`, or type `logout`, to log out the container
* and get back to our own computer
___
