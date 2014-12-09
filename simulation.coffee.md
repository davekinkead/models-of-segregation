# Models of Segregation

## Simulation Code


We start with our agents.  Our agents exist in a 2D space, are of a certain race, and hold a xenophobic disposition.  For now, we will randomly assign them their race and geographic location. Their xenophobia will also be a function of their race.

In this case, we will create 3 races of agents and give them a xenophobia level of just 40% (ie agent will only be happy if more than 40% of thier neighbours are the same).


		class Agent
			constructor: (@space) ->
				[@race, @xenophobia] = switch Math.floor Math.random() * 2
									when 0 then ["blue", 0.6]
									when 1 then ["red", 0.6]
				@x = Math.floor Math.random() * @space.width
				@y = Math.floor Math.random() * @space.height
				@step 	= 50 #@space.width * @space.height / 7200
				@depth 	= 100 #@space.width * @space.height / 7200


The happiness of agents is determined by their neighbourhood composition.  If the homogeniality is greater than their xenophobia, then they are happy.  High levels of xenophobia should correlate with greater levels of unhappiness, all other things being equal.


			isHappy: () ->
				homogeneity = @space.homogeneity(this)
				if homogeneity >= @xenophobia then true else false


Next, we difine a 2D space representing the problem domain. Our space contains a geographical distribution of agents stored in a list.  We also give a space a width and height to manage the relation between the simulation and the client.


		class Space
			constructor: (@height, @width) ->
				@agents = []


In spacial arranements, everybody is next to somebody - their neighbour.  The relative composition of that neighbourhood - 10% homogeniality or 50% homogeniality - is determined by the race of ones neighbours and how deep the conception of neighbourhood extends.


			neighbourhood: (x, y) ->
				neighbours = []
				for other in @agents
					if x - other.depth < other.x < x + other.depth and y - other.depth < other.y < y + other.depth
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


		agents = (height, width) ->
			space = new Space(height, width)
			for n in [1..2000]
				space.agents.push new Agent space 
			space.agents


We then need some logic for moving our agents around the space.  Movement could be intentionally directed or (as in this case), agents move to a random spot near by, only moving if they are unhappy with their neighbourhood.


		move = (agent) ->
			unless agent.isHappy()
				xRand = Math.random() * agent.step
				yRand = Math.random() * agent.step
				agent.x += xRand - agent.step / 2
				agent.x = xRand / 2 if agent.x < 0
				agent.x = agent.space.width - xRand /2 if agent.x > agent.space.width
				agent.y += yRand - agent.step / 2
				agent.y = yRand /2 if agent.y < 0
				agent.y = agent.space.height - yRand / 2 if agent.y > agent.space.height


Finally, we declare our public API so that other modules can access it.


		module.exports = {agents: agents, move: move}