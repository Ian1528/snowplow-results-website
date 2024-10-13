<!-- Navigation Bar
<nav>
  <a href="results"> Results</a>
</nav> -->

#### Ian Lam

## 1. Overview

In my hometown of Middleton, Wisconsin, winter storms present significant challenges to city operations. The efficient clearance of roads is of crucial importance, ensuring safe travel and efficient management of city resources such as salt and labor costs. While existing research primarily focuses on generalized route optimization problems such as the Traveling Salesman Problem (TSP) and Capacitated Arc Routing Problem (CARP), I present a solution that meets the specific constraints of snowplowing, including turn restrictions, road priorities, and salt capacity limits. Although private companies offer commercial-grade solutions for snowplow routing, the high costs of these services have prevented Middleton from devoting resources to them. Currently, the city devises its snowplow routes by hand, assigning each plow driver a predetermined section of the road network and relegating the specific order in which to plow those roads to the driver themself. Thus, to optimize these sectors, I provide an open source computational algorithm implemented in Python, consisting of an initial route construction heuristic followed by a hybrid local-search genetic algorithm. Moreover, in order to consider turn directions in the cost of a route, I transformed the edge-based road network graph sourced from OpenStreetMap into a node-based representation. Preliminary test results show an improvement for the average plow time in comparison to current routes ranging from 10-25%, depending on the sector of the network. In future work, I would like to explore alternative representations of routes using NumPy, which I anticipate would significantly improve the runtime of the algorithm.

This project has been a multi-year journey as I’ve delved into the scientific literature surrounding route optimization problems, learned about graph traversal and heuristic algorithms, and navigated the challenges of implementing my ideas in Python through previously unfamiliar libraries. I’m grateful to the mentorship of Carnegie Mellon University Professor Dr. Arthur Sugden for guiding me throughout this process, providing advice and ideas to help me realize my goal to serve my local community.

All of my code can be found in the following GitHub repository: [https://github.com/Ian1528/Snowplow-Routing-Middleton](https://github.com/Ian1528/Snowplow-Routing-Middleton)

## 2. Modeling Assumptions and Parameters

In this section, I describe the assumptions made regarding parameters in the setup of the model, based on information provided by the city.

1. Each road needs to be plowed multiple times for a single storm

    - 2 passes for most roads
    - 3 passes for streets wider than 36 feet
    - 4 passes for boulevards and major highways.

2. Every snowplow has a salt capacity of 10 tons, and salt is applied at a rate of 250 lbs per lane mile.

3. Since plows travel slower than posted speed limits, I use the following speeds to calculate travel times:

    - 11 mph on residential roads
    - 20 mph on major roads

4. The Municipal Operations Center serves as the depot for all plows

## 3. Description of Algorithm

There are four core stages of this algorithm that I'd like to highlight.

1. A transformation from an edge-based graph to a node-based graph
2. An initial, greedy route construction algorithm
3. A local-search algorithm to refine routes generated in step (2)
4. A broad genetic algorithm that combines routes generated in step (3) to diversify the solution space

### 3.1 Transformation

The initial graph I obtained from OpenStreetMap represented each road as an edge and each intersection as a node. While this allows factors such as travel time, road priorities, and street width to be directly incorporated into the weight of each edge, it is insufficient to encode information about turn directions which vary depending on the orientation of the edge immediately preceding it in a route. A node-based representation, where edges represent intersections between two streets, allows for this information to be stored directly in the weights of the edges.

### 3.2 Route Construction Algorithm

After transforming the graph, a greedy algorithm generates an initial set of routes, choosing to plow the closest street that has the highest priority, least cumbersome turn, and largest number of streets leading off of it. Each route is evaluated based on the following customizable cost function and turn penalties:

#### 3.2.1 Cost Function

The cost of a route depends on three primary components: total plow time, prioritization of high traffic streets, and time spent deadheading (driving over roads without plowing them). Currently, total plow time is weighted four times as heavily as the other two components. These weights can be adjusted depending on the preferences of the city.

#### 3.2.2 Penalizing Turns

Making U-turns and sharp left turns are inherently cumbersome for large plow vehicles. As such, I assign a 3-minute penalty to any U-turn and a 30-second penalty to left turns.

### 3.3 Local Search Improvement

After obtaining an initial route, a local-search algorithm makes small adjustments to improve upon the solution. For each road segment of the route, the algorithm considers swapping or inserting the three closest streets in its place. If an improvement to the cost function results, then the change is accepted. The algorithm terminates once all roads in the route have been considered.

### 3.4 Final Step: Genetic Algorithm

While the local search adeptly improves upon the original routes, it is unable to make significant changes to the overall structure of the routes. Thus, I generate an initial diverse population of ten (an arbitrary parameter that can be varied) independent routes by running the route construction and local search algorithms multiple times. Then, the genetic aalgorithm randomly combines two of these routes by replacing one section of the first route with a section of the another, before running the local search improvement on the route that results. These large-scale swaps allow for the algorithm to escape local minima in the cost function and explore radically different solutions.

After a specified number of iterations (currently set to 25), consisting of merging two independent routes into a new solution, the algorithm terminates and returns the best route it finds with respect to the cost function.

## 4. Results

Detailed, animated maps may be found in the [results](https://ian1528.github.io/snowplow-results-website/results/) section of the website.

### 4.1 Route 1

The first snowplow is responsible for the eastern half of the city. Estimated time: 6.1 hours.
![Plow 1 Routes](/assets/img/blue_routes.png)

### 4.2 Route 2

The second snowplow is responsible for the mid-northern section of the city. Estimated time: 7.7 hours.

![Plow 2 Routes](/assets/img/orange_routes.png)

### 4.3 Route 3

The third snowplow is responsible for the western section of the city. Estimated time: 6.7 hours.
![Plow 3 Routes](/assets/img/red_routes.png)

### 4.4 Route 4

The fourth snowplow is responsible for the mid-southern section of the city. Estimated time: 7.0 hours
![Plow 4 Routes](/assets/img/green_routes.png)

## 5 Discussion

The results I've presented offer a significant reduction from the 8.5 hours it usually takes for the city to clear the roads after a snowstorm. However, there are still numerous improvements and considerations that I plan to explore in the future. For instance, it is possible that switching to a NumPy-based representation of the routes, as opposed to Python objects, would significantly improve computation time, enabling on-the-fly adjustments and new routes to be easily generated depending on the conditions of each snowstorm. Moreover, in addition to the four major sectors of the city, I'd love to generate a set of routes for the city's specialized snowplow, responsible only for cul-de-sacs that are cumbersome for larger plow vehicles to clear. Lastly, I anticipate the need for further collaboration and discussion with the Streets Division to determine the hyperparameter values that best reflect real-world constraints. This could involve changing the cost function, certain road priorities, and/or turn penalties to generate not just optiaml, but also practical routes.

## 6. Credits

I'd like to thank the following individuals for their guidance and support which has made this project possible:

- Dr. Arthur Sugden (Adjunt Professor at CMU)
- Dr. Ganesh Mani (Distinguished Service Professor at CMU)
- Casey Ford (UW Madison Graduate Student)
