# An efficient, open-source implementation of the snowplow routing problem

## Ian Lam

In my hometown of Middleton, Wisconsin, winter storms present significant challenges to city operations. The efficient clearance of roads is of crucial importance, ensuring safe travel and efficient management of city resources such as salt and labor costs. While existing research primarily focuses on generalized route optimization problems such as the Traveling Salesman Problem (TSP) and Capacitated Arc Routing Problem (CARP), I present a solution that meets the specific constraints of snowplowing, including turn restrictions, road priorities, and salt capacity limits. Although private companies offer commercial-grade solutions for snowplow routing, the high costs of these services have prevented Middleton from devoting resources to them. Currently, the city devises its snowplow routes by hand, assigning each plow driver a predetermined section of the road network and relegating the specific order in which to plow those roads to the driver themself. Thus, to optimize these sectors, I provide an open source computational algorithm implemented in Python, consisting of an initial route construction heuristic followed by a hybrid local-search genetic algorithm. Moreover, in order to consider turn directions in the cost of a route, I transformed the edge-based road network graph sourced from OpenStreetMap into a node-based representation. Preliminary test results show an improvement for the average plow time in comparison to current routes ranging from 6-18%, depending on the sector of the network. In future work, I would like to explore alternative representations of routes using NumPy, which I anticipate would significantly improve the runtime of the algorithm.

This project has been a multi-year journey as I’ve delved into the scientific literature surrounding route optimization problems, learned about graph traversal and heuristic algorithms, and navigated the challenges of implementing my ideas in Python through previously unfamiliar libraries. I’m grateful to the mentorship of Carnegie Mellon University Professor Dr. Arthur Sugden for guiding me throughout this process, providing advice and ideas to help me realize my goal to serve my local community.


### Route 1
The first snowplow is responsible for the eastern half of the city. Estimated time: 8.3 hours.
![Plow 1 Routes](/assets/img/blue_routes.png)

### Route 2
The second snowplow is responsible for the mid-northern section of the city. Estimated time: 8.3 hours.

![Plow 2 Routes](/assets/img/orange_routes.png)

### Route 3
The third snowplow is responsible for the western section of the city. Estimated time: 6.7 hours.
![Plow 3 Routes](/assets/img/red_routes.png)

### Route 4
The fourth snowplow is responsible for the mid-southern section of the city. Estimated time: 8.0 hours
![Plow 4 Routes](/assets/img/green_routes.png)