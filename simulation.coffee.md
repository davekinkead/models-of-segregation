# Models of Segregation

## Simulation Code

First, let's declare any global variables we might need to access. Depth being the neighbourhood radious and step being the average movement distance of an agent.


		depth = 50
		step  = 50


We start with our agents.  Our agents exist in a 2D space, are of a certain race, and hold a xenophobic disposition.  For now, we will randomly assign them their race and geographic location. Their xenophobia will also be a function of their race.


		class Agent
			constructor: (@space) ->
				@race = switch Math.floor Math.random() * 2
									when 0 then "blue"
									when 1 then "red"
									#when 2 then "green"
				@xenophobia = if @race is "blue" then 0.5 else 0.5
				@x = Math.floor Math.random() * 600 
				@y = Math.floor Math.random() * 600


The happiness of agents is determined by their neighbourhood composition.  If the homogeniality is greater than their xenophobia, then they are happy.  High levels of xenophobia should correlate with greater levels of unhappiness, all other things being equal.


			isHappy: () ->
				homogeneity = @space.homogeneity(this)
				if homogeneity >= @xenophobia then true else false


Next, we difine a 2D space representing the problem domain. Our space contains a geographical distribution of agents stored in a list.


		class Space
			constructor: () ->
				@agents = []


In spacial arranements, everybody is next to somebody - their neighbour.  The relative composition of that neighbourhood - 10% homogeniality or 50% homogeniality - is determined by the race of ones neighbours and how deep the conception of neighbourhood extends.

			neighbourhood: (x, y) ->
				neighbours = []
				for other in @agents
					if x - depth < other.x < x + depth and y - depth < other.y < y + depth
						neighbours.push other
				neighbours


The homogeniality of a neighbourhood relative to an agent is therefore calculated as the proportion of like neighbours to total neighbours.


			homogeneity: (agent) ->
				same = 0
				others = 0
				neighbours = this.neighbourhood agent.x, agent.y
				for neighbour in neighbours
					same += 1 if agent.race is neighbour.race
					others += 1 if agent.race isnt neighbour.race
				same / (same + others)


Now that we have defined our model, we need some functions to initiate and control behaviour.  We will instantiate the simulation by invoking the `agents` function.  This will create a space and populate it with agents.


		agents = () ->
			space = new Space()
			for n in [1..2000]
				space.agents.push new Agent space 
			space.agents


We then need some logic for moving our agents around the space.  Movement could be intentionally directed or (as in this case), agents move to a random spot near by, only moving if they are unhappy with their neighbourhood.


		move = (agent) ->
			unless agent.isHappy()
				agent.x += (Math.random() * step) - step/2
				agent.x = 25 if agent.x < 0
				agent.x = 575 if agent.x > 600
				agent.y += (Math.random() * step) - step/2
				agent.y = 25 if agent.y < 0
				agent.y = 575 if agent.y > 600


Finally, we declare our public API so that other modules can access it.


		module.exports = {agents: agents, move: move}