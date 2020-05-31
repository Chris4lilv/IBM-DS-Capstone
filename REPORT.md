# Making location recommendations for restaurants based on neighborhoods preferences in Los Angeles

## Problem
It's trivial that businesses like restaurants or barbershops are heavily relied on the amount of customers visit. Therefore, a good location plays an important role in booosting sales and profit. By investigating the neighborhood in Los Angeles, we're able to make recommandations of where to open up the business given a type of restaurants. In this project, we'll find optimal location for somebody who wants to open a mexican restaurant.

## Data
For this problem, we get postal code data of LA from <a href='https://en.wikipedia.org/wiki/List_of_districts_and_neighborhoods_of_Los_Angeles'>Wikipedia</a>
Combined with the usage of <a href="https://geocoder.readthedocs.io/">geocoder</a>, we are able to find the geo location(longitude and latitude) given the neighborhood name. With there data, it's easy to retrieve relevant venue information from Foursquare API.

## Methodology
* #### Data scraping pre-precessing
  As mentioned in the Data section, we build a dataframe using pandas to store neighborhood names as well as their geo location. As we see in the following table, there're two neighborhoods with NaN values. We impute the location of Angelus Vista manually and drop Kinney Heights since its location is not available online.
  ![alt-text](/missing_value.png)

* #### Exploratory data analysis
  By showing neighborhood on the map, we find from a larger scope that most neighborhoods are gathered around downtown LA while the rest of them reside on northwest LA. 
![alt-text](/nbh_dist.png)
Since we're not sure what radius to set to include enough venues around each neighborhood, it's better to show them all first.
![alt-text](/detailed.png)
From the map we can see that venues around most neighborhood are fairly close to them, but for certain areas like Beverly Glen(yellow), nearly all of its venues are on a distanced street instead of surrounding the neighborhood.
![alt-text](/zoom_in_bg.png)
This is why we need to take radius into consideration at first. However, after carefully tested several radius candidate, we decide to just include all 50 venues the API recommended because if we limit the radius to a certain value, number of venues in some neighborhoods drop significantly which is not good for further modeling.
![alt-text](/radius_200.png)

* #### Modeling
  Our goal is to cluster the neighborhood so a clustering model like **KMeans** would of good use.
  *Note that KMeans cluser the neighborhood based on their average value of each category rather than their geo location.
  So it might be a little confusing that the neighborhoods that far away belongs to the same cluster.*

## Results
From the clustering results, it's clear to see the clusters geographically. 
![alt-text](/clustered.png)
However, to explore their unique characteristics, we'll need to rank categories in each cluster to find out which one appears the most.
![alt-text](/top_ranking.png)
An example of opening a new mexican restaurant is used to illustrate how to utilize this project to make recommendation. As shown in the table below, we find that mexican restaurant ranks the highest in cluster one.
![alt-text](/mexican_ranking.png)
*From easier query, data will be stored in sqlite.*

## Discussion
![alt-text](/mexican_recom.png)
It's trivial from the map that people around Hollywood Hills like mexican food the most compare to other clusters. We also notice on the mpa that mexican food is also comparatively popular in Mandeville Canyon, but we choose to ignore it since the larger the customer base, the more likely the restaurant will be successful).
To make further improvement, it might worth trying to apply other clustering algorithms like Agglomerative or DBSCAN. Another potential improvement is to collect data from other sources and perspectives.

## Conclusion
From the results and discussion parts, we conclude that opening a new mexican restaurant in Hollywood Hills area might be a good option.
