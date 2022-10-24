# Distances between bars in Wien

Inspired by the [Seidl-rallye](https://www.seidlrallye.at/) - an event in Vienna, during which you have to visit one pub/bar/cafe in each district in Vienna and do it in one day.

It's basically finding the shortest route in graph or optimizing travelling salesman problem (or [traveling politician problem](https://en.wikipedia.org/wiki/Travelling_salesman_problem#Related_problems)).

First step is to gather the data. The easiest way was to get it from OSM - through https://overpass-turbo.eu/ with amenity set to `pub`, `bar` or `cafe` and export is as Raw OSM data which is a XML file (files [wien_bar.osm](wien_bar.osm), [wien_pub.osm](wien_pub.osm), [wien_cafe.osm](wien_pub.osm))

Public transportation is used in the original ralley - to gather public transport distances we used google map api [distance_matrix](https://developers.google.com/maps/documentation/distance-matrix/distance-matrix) - the simple example is in [Wien_pubs_public_transports.ipynb](Wien_pubs_public_transports.ipynb) - processing of data, getting the distances and also some result of the problem using [networkx](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.shortest_paths.generic.shortest_path.html). Here we chose the start in 1st Bezirk and visit one bar in each district by order. The limitation of getting public transport distances from the Google's API is, that to correctly reflect real-world, it would be needed to ask for the distances in specific time of day which is shifted during the day (in our case we got the distances on Staurday 10am). Another limitation is the cost of this - Google charges 0.005usd/distance (but galso ives 200usd free credit). Resulting file with distances between the bars is in [wien_bar_distances.json](wien_bar_distances.json) - contains 410 nodes and 8172 edges.

Best found solution for this problem (using just pub and bar amenities) was 3:04h.

To make the problem more general and to be able to search for optimal route between all the pubs in Wien respecitng the rule "only one pub in each district" one needs much bigger graph of distances. Getting the public transport distances would cost too much and still not reflect the real/life scenario (traveling time during the day differs). For the sake of just solving this problem we use [osmnx](https://osmnx.readthedocs.io/en/stable/) to get whortes walking disances. See [multiprocess walk osmnx distances Wien bar-pub-cafe.ipynb](multiprocess walk osmnx distances Wien bar-pub-cafe.ipynb) and resulting file [wien_bar-pub-cafe_walk_distances.json.gz](wien_bar-pub-cafe_walk_distances.json.gz) - contains 987 nodes and 486591 edges.

One of the approaches how to choose the optimal pubs to get the shortest route is to use genetic algorithms - one simple try is shown in [GA.ipynb](GA.ipynb)

Best found solution so far was 38396m

TODO: Write a map with walking distances showing real route (with streets) not just straight lines.
