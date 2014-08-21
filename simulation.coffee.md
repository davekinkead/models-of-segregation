# Models of Segregation

So we start with a space representing our problem domain.  Our space contains a geographical distribution of agents.

		depth = 100

		class Space
			constructor: () ->
				@agents = []

			homogeneity: (agent) ->
				same = 0
				others = 0
				neighbours = this.neighbourhood agent.x, agent.y
				for neighbour in neighbours
					same += 1 if agent.race is neighbour.race
					others += 1 if agent.race isnt neighbour.race
				same / (same + others)

			neighbourhood: (x, y) ->
				neighbours = []
				for other in @agents
					if x - depth < other.x < x + depth and y - depth < other.y < y + depth
						neighbours.push other
				neighbours


Then we define agents.  Initially, we will randomly assign attributes.

		class Agent
			constructor: (@space) ->
				@race = if Math.floor(Math.random() * 2) is 0 then "blue" else "red"
				@xenophobia = if @race is "blue" then 0.3 else 0.7
				@x = Math.floor Math.random() * 600 
				@y = Math.floor Math.random() * 600


Agents only move if they are unhappy.  To determine agent happiness, we look at the composition of there neighbourhood.  If the neighbourhood homogeniality is less than 50%, agents are unhappy.

			isHappy: () ->
				homogeneity = @space.homogeneity(this)
				if homogeneity >= @xenophobia then true else false


We instantiate the simulation by invoking the `agents` function.  This will create a space and populate it with agents.

		agents = () ->
			space = new Space()
			for n in [1..1000]
				space.agents.push new Agent space 
			space.agents

Now we need logic for moving our agents.  Agents move to a random spot near by and only move if they are unhappy.

		move = (agent) ->
			unless agent.isHappy()
				agent.x += (Math.random() * 20) - 10
				agent.x = 0 if agent.x < 0
				agent.x = 600 if agent.x > 600
				agent.y += (Math.random() * 20) - 10
				agent.y = 0 if agent.y < 0
				agent.y = 600 if agent.y > 600


Finally, we declare our public API.

		module.exports = {agents: agents, move: move}