# Title: Understanding urban growth over time - Comparing Neighborhoods of The Hague



[TOC]





# Part 1

### Introduction 



The Hague, in Netherlands is one of the oldest cities in Netherlands and the government of Netherlands and its parliaments works from this city. Most parts of the current Hague was under sea few centuries ago and land was slowly reclaimed from the sea. Hague forms one of the three major cities: Amsterdam, Rotterdam, Hague which form the Randstad region of Netherlands. Hague is on the west coast of Netherlands separated from the UK by the North Sea. Hague despite being an old city, is also very modern and multi-cultural with many international institutions and multi-national companies (see images). This mix of old and new urban developments give the city a good contrast amongst its neighborhoods . 







<u>Picture showing contrast in old and new architecture  in the city of Hague.</u>

![The Hague: Bringing sustainability to the heart of events](https://www.sustaineurope.com/images/binnehof%20with%20swan.jpg?crc=212812994)

​			Image 1

![Contact Us | The Hague Convention Bureau](https://conventionbureau.thehague.com/sites/thcb_corp/files/styles/content_image_small_500x/public/2020-01/skyline%20buildings%20from%20above_0.png?itok=IhYKj6oP)

​									image 2

### Problem Statement & Hypothesis



In this project, we will study and compare neighborhoods of Hague, The Netherlands. We will investigate if urban spread or sprawl over time, affects the composition of activities and texture of the urban fabric. For example, urban developments plans 30 years ago are not the same as now and will not be same 30 years later. The density of buildings, the landuse (mixed or segregated), composition of landuses depends on needs of the time. 



To understand more clearly, we wish to investigate if urban venues and their type, and their density differs over space and time.  Our hypothesis is yes, urban density, development  and land use depends on time, therefore the density of venues and type of venues we will be collecting from Foursquare should show this differences.



If our hypothesis is true, the clusters should show circular patterns as can be seen in the image below, which is an actual ground reality of 2021. This picture shows the color of buildings based on their age. The color range goes from hot-to-cold or red-to-blue. Red color shows the oldest part of the city while the blue color shows the new developments. Orange colors in between, are middle aged buildings. 



![](image-20210304204518465.png)

Image 3





# Part 2

### Data Acquired

In this section, we described the data used  and preparation of the dataset used in this project. 

- **Neighborhood data**: A neighborhood in Netherlands is known as a wijk, which roughly translates to a district. We collected this data from the City office website on this [link](https://denhaag.dataplatform.nl/#/home) . These are polygons and we use QGIS to calculate the centroids of each districts. This data has no additional information than just holding the latitude and longitude values. We have discarded all other information as it is not relevant for our project. 
  - Data Source: https://denhaag.dataplatform.nl/#/home
  - Extra Tools used: https://www.qgis.org/en/site/
  - The data is in Dutch language and therefore is not presented here. We have simplified the data and kept only three fields: City Name, Neighborhood Name, Latitude and Longitude
  - The Hague has 46 districts, the centroids of which are shown in the image below.
  - This data is stored in a csv

![image-20210304204641870](image-20210304204641870.png)

Image 4



- **Buildings data**:  This data shows the age of each building in the Hague, we could not get the raw data, but the visualization of the data can be see on this [link](https://parallel.co.uk/netherlands/#12.34/52.07984/4.28223). We will be using this data to validate our results to show if age of a neighborhood affects the activities and venues in the neighborhood. see Image below. 
  - Data Source:  https://parallel.co.uk/netherlands/#12.34/52.07984/4.28223



![image-20210304204518465](image-20210304204518465.png)

Image 5



- **Four Square data**: As prescribed by the project instructions, we will be using Four Square API to collect information on activity venues in all the districts of Hague.  Please see this [link](https://developer.foursquare.com/). 

  - We requested this API to provide locational information on 100 venues within a given radius of 1000m.

- This data is pulled via a python request and is not stored locally.

  

![image-20210304204828183](image-20210304204828183.png)

Image 6





# Part 3



## Methodology 

We will be following the methodology as listed in these steps

- collect and prepare data from **different sources**
  - collect neighborhood data from the local government website
  - collect venue information from the **Foursquare API**
  - collect existing landuse data and construction history of the city from opensource data 
- all the data has a common field, neighborhood/district name, so we can **combine them** into a big data base for our analysis
- we will explore this data with some **descriptive statistics** like frequency, tally counts along with maps
- we will then prepare the data for cluster analysis using **one-hot encoding** method. 
- among different clustering algorithms, we will use **K-means clustering algorithm**
- we will then calculate the appropriate number of clusters according to the **elbow plot**
- and finally make observations and **conclusions**



## Exploratory Data Analysis



In this section, we will explore the data collected from Foursquare. A Foursquare call was sent with the following parameters:

- LIMIT: 100
- radius: 1000m
- The Foursquare API call for our 46 neighborhoods (districts) returned 2125 Venues. A sample top 5 rows are shown here:

###  Cleaning received data

The data received from the API had some special characters and duplication especially with the restaurant category. A small function is used to clean this category and to aggregate venues of same category.



###  Unique & frequent venue categories

After the cleaning, we tabulate the venue category to find unique and frequent categories. We notice that there are 189 unique categories in our data from Foursquare, the most popular and frequent among them are restaurants. 

![img](img1.png)

​										Image 7

###  Most omnipresent venue category

Here we try to see how each venue category is spread in each neighborhood (district) of Hague. For example, as we can see from the graph below, restaurants and supermarkets are available in almost 40 of the 46 neighborhoods. Or we can say, that people have access to supermarkets within a Kilometer of their residence. 

![img](img2.png)

​										Image 8

### Top Neighborhoods (districts) with most venues

From this data we can also extract which neighborhood has most venues in them or within 1 Km. We see from the graph below that the top 5 neighborhoods (districts) have 100 venues. We must be aware that we had set our limit to 100 in our query, so these 5 districts have reached their maximum query limit. 



![img](img3.png)

​										Image 9





We have mapped these 5 districts and we notice that they are right in the center of the city and 1 of them on the beach which is a very popular beach in entire Netherlands. So the results appear to show reality.

![image-20210305200310962](image-20210305200310962.png)

​										Image 10





## Cluster Analysis



### Feature Selection

The purpose of the clustering is to group neighborhoods based on similarity among venue categories. Two most important features therefore are “District” and “Venue Category”. However we have to convert these to numeric values because the algorithm works with numerical features.




### Preparing for one-hot encoding

In order to prepare for the cluster analysis, we have to encode each of the 189 category into a binary value combination such that each combination falls into one category. This would also require us to regroup the data based on neighborhood (district).  We show here a sample of the resulting one-hot enconding operation.



![image-20210305200807772](image-20210305200807772.png)

​										Image 11



### Suitable Number of Clusters

One of the pitfalls of clustering algorithms are that with increase in number of clusters, the error reduces. However, large number of clusters is a kind of overfitting and should be avoided. The correct number of clusters for an analysis can be chosen using the elbow plot. To generate this plot, we run Kmeans algorithm with a given range of clusters and select a number where the rate of change of reduction in error becomes less sharp. The elbow plot for this analysis is shown in the figure below.

![img](img7.png)

​										Image 12



We analysed our data with a range of 1 to 10 clusters and the elbow chart above shows that around 5 clusters are a good fit for our data. In the following steps, we will analyse our data with 5 clusters





### Discussion and Conclusion



#### Ground Reality

We analyze the clusters in terms of an urban planning paradigm. That is if the clusters really capture the essence of the city and it growth. We refer the reader to this image 13 and a good understanding of it. This image forms the verification of our model.  A larger image is shown in the beginning of this report. This image shows how old different regions of the city are based on the age of construction of the buildings. 



The older buildings are dark-red, and the middle aged are in orange while the modern buildings and developments are blue in color. We can see from the image that the central core is red to dark red, surrounding this core is orange color and surrounding these orange areas are the blue areas which are modern developments. 



<img src="image-20210304204518465.png" style="zoom:33%;" />

​										Image 13

#### Clusters from Analysis



- Observations: 
  - These clusters are mapped in the map in image 14 below. Referring to the table 1 and the map, we can say , city is growing in a circular fashion. As discussed in ground reality.
  - The oldest part is the city center and the beach, this is captured correctly with the brown cluster 0. These locations in reality have smaller houses  and shops.
- Within each cluster, we have tabulated top 5 venue categories and their quantity in each cluster.  These are shown in Table 2. 
- 	Cluster 1 is modern development and we see presence of Shopping mall which are not in other clusters. 
- Cluster 2,3 are outer clusters and we can see the presence of Tram stations and Light rail stations. These transportation objects are among the most frequent venues in Foursquare API indicating that there are no other prominent venues. As these clusters are far from the city, they need such mass transportation modes to reach the city. 
- Within the  most dense clusters, cluster 0 we see a huge number of restaurants, and this agrees to the reality, because most of Europe has this city center where all the squares have restaurants and shops around a church. 







Table 1

| Color      | Cluster | No. of districts | Position in city         | time of development        |
| ---------- | ------- | ---------------- | ------------------------ | -------------------------- |
| brown      | 0       | 18               | Inner core               | Oldest (1800's)            |
| red        | 4       | 14               | first ring to inner core | second oldest (early 1900) |
| orange     | 3       | 6                | second ring              | 1925-1950                  |
| dark blue  | 1       | 5                | mixed/periphery          | 2000's                     |
| light blue | 2       | 3                | mixed/periphery          | 2000's                     |





![image-20210305203746441](image-20210305203746441.png)

​										Image 14





Table 2

![image-20210305210836387](image33-20210305210836387.png)

## Conclusion

In this project, the neighborhoods of Hague were clustered into multiple groups based on the categories (types) of the venues in these neighborhoods. The results showed that the clusters indeed reflect the existing reality in terms of development and landuse. If a deeper analysis—taking more aspects into account—is performed, it might result in discovering more intricate details that can help urban planners and designers understand the growth of the city.