# Models of Segregation

A reproduction of Thomas Schelling's seminal 1969 work using agent based modelling with coffeescript, D3.js, and HTML. 

Schelling's paper is about how segregation is not necessarily the result of discriminatory individual choices. (p488)

His "conjecture is that the interplay of individual choices, where unorganised segregation is concerned, is a complex system with collective results that bear no close relation to individual intent." (p488)

The only requirement of the model is that the distinctions between agents be twofold, exhaustive and recognisable. (p488)  Certain logical constraints apply to all systems however (p489)

- both groups can't be numerically superior
- only complete segregation works if both groups want to be locally superior
- local majorities are determined by how far the concept of neighbour extends

## The Model

Rather than model a system as Schelling did, we are going to model agents acting independently and observe any emergent system level behaviours.  This means we are going to need agents with:

- satisfaction based on how far one's neighbourhood extends
- happiness depends on tolerance ratios being satisfied
- agents move if unhappy

Read through the code for a more detailed explanation of the various parts. The final result can be [viewed here](http://dave.kinkead.com.au/models-of-segregation)

## Usage

Fork this repo and then just open `index.html` in any browser and the simulation will automatically run.

Alternatively, you can checkout the [demonstration here](http://dave.kinkead.com.au/models-of-segregation)


## Customisation

Customising the simulation is easy.  Code of note is organised thusly:

		/
		- assets
			- styles.css
			- browser.js
			- d3.min.js
		- simulation.coffee.md
		- browser.coffee.md
		- index.html

Simulation logic is found in `simulation.coffee.md` while presentation code and D3.js hacks are in `browser.coffee.md`.  To alter the simulation, edit thse files and don't go touch'n `browser.js`.

The coffeescript files need to be built and compiled into `browser.js`.  To build, type the following:

		browserify -t coffeeify browser.coffee.md > assets/browser.js

You'll need to install browserify and coffeeify if you haven't already.

Licensed under a CC-BY-SA license.