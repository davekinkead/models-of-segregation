# Models of Segregation

A reproduction of Thomas Schelling's seminal 1969 work using agent based modelling with coffeescript and HTML. 

## Usage

For this repo and then nust open `index.html` in any browser and the simulation will automatically run.

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