# Contributors Guide

- What We Value
- How We Work
- Issues and Pull Requests
	- Issue Contributions
	- Code Contributions
- Technical How To
	- Code Conventions
	- Code Reviews
	- Testing
- The Top of Our To Do List

##What We Value
We aren't like many Open Source projects. Our work affects not only our direct users, but their users as well. To the extent that it's deployed, our code impacts users' privacy, security, business continuity, revenue, bottom line, liability, and investments in development. Awareness of that potential affects the choices we make about software development. As contributors, we need to take responsibility to ensure our impact is positive. 

When there are clear advantages to rewriting existing dependencies or rolling our own rather than using what's readily available, we do not hesitate to do so. We value informed judgment over arbitrary rules and, when appropriate, gladly violate conventional wisdom in pursuit of our goals.

###Striving for accessibilty
We're willing go to great pains to ensure the accessibility of our code to users and other contributors. It's not enough that it works. A healthy codebase is easy to contribute to, because it's easy to understand. It lends itself to debugging, invites reading, and simplifies auditing.

##How We Work
Anvil Connect is a highly collaborative project. We welcome all questions and input, regardless of your area of interest. Here's our guidelines:

   - Readability
    - consistency of conventions
    - meaningful naming; 
    - names should be concise; 
    - tell the story of how it works
    - using whitespace
    - keep function calls clean
      - pass objects and destructure inside the fn
      - destructure before calling args
      		- fn(req.body) is not ideal, but ok
     		- fn(req.body.foo, req.query.bar, etc.other.thing, fn) is not ok
     			- instead: fn(foo, bar, thing, fn)

   - Explicitness over cleverness
       - start with concrete implementations, discover sensible, coherent abstractions, and refactor

- Engineering/quality
       - First identify and test failure/error cases, only then implement the successful behavior

- Interoperability
       - play well in the sandbox
       - with self
       - with other providers
       - with other compliant software
       - complete, strict, and pedantic interpretation of specs


- Adaptability
       - consistency is helpful
       - foolish consistency is bad, m'kay
       - actively look for ways to improve our conventions over time


      
We are in Gitter every day, come join us:

[![Gitter](https://badges.gitter.im/anvilresearch/connect.svg)](https://gitter.im/anvilresearch/connect)

##Issues and Pull Requests

### Issue contributions
We welcome you to open issues relating directly to the development of Anvil Connect.

Discussion of docs and philosophical chat should be posted to [Anvil Research Connect Docs] (https://github.com/anvilresearch/connect-docs).

####For main project contributions: 

- please look for open issues if you're going to work on something and let us know 
- when you're working on something, if its not a substantial addition, file an issue and let us know

###Code Contributions
We welcome code contributions and participation in bug fixes and issues.
 
- for new features
       - be prepared for a code review hangout
       - pair with a core team member
           - avoid duplicate effort
           - talk through naming

- for all contributions
       - small commits and pull requests
       - separate features and improvements
       - verify dependencies are listed in manifest
       - features must include test coverage and code review to be accepted
       - for adding new features,

###Submission process is as follows:

####Fork
- Fork the project [on Github](https://github.com/anvilresearch) and check out your copy locally.
	1. For new features and bug fixes, use the master branch.
	2.  (anything else we want to say here)

####Branch
- Create a branch and hack away!
	
####Commit
- When you commit make sure you include your name and address.

				$ git config --global user.name "I. CanCode"
				$ git config --global user.email "icancode@example.com"
- Be sure and include a commit log, including a title and relevant changes.

####Vet
- We use [Javascript Standard Style] (https://github.com/feross/standard) to keep the code standardized

####Push
- Go to [] and select your feature branch. Click the 'Pull Request' button and fill out the form.

- Pull requests are usually reviewed right away. 
	- If there are comments to address, apply your changes in a separate commit and push that to your branch. 
	- Post a comment in the pull request afterwards; GitHub does not send out notifications when you add commits.