### 1. Introduction/Business Problem
Business problem: if you are looking to open café, shop, or rent apartment, you may be interested where in the city there are places similar to those that you know. For example, you love atmosphere, people and venues of some district and want to rent an apartment here, but supply of apartments is too narrow and you should explore other places. You can explore the city on your own, but maybe you would like to have some recommendations. 
There are two ways, how we can make such recommendations:
-	clearly ask the customer about his/her preferences: what venues are important to be in the proximity, density of this venues, and so on
-	make recommendations based on what we know he/she like

The first way is not simple, because we should ask many questions people can find difficult to answer. 
The second way is obviously less confusing. We just say, “Hey, you like the district A, why not look at D and M?“ 
To pursue the second way, we need to somehow cluster districts, based on data we know about them. 
I like to solve this problem for my home city, Moscow, Russia.  

### 2. Data
To compare and cluster the Moscow districts, we will leverage Foursquare data. Foursquare is a popular service, allows people to explore, rate and recommend venues (café, restaurants, bars, shops, theaters, attractions, and so on), and stores vast amount of up-to-date information. For any geographical point, Foursquare API allows you to get list of N nearest venues, with information about these venues (name, category, address, coordinates, rating). We will automatically obtain information about venues in each district of Moscow from Foursquare, and automatically find groups (clusters) of similar districts based on this information.

For this task, we also need to know geographical coordinates of Moscow districts. List of districts available [here]( https://ru.wikipedia.org/wiki/%D0%A1%D0%BF%D0%B8%D1%81%D0%BE%D0%BA_%D1%80%D0%B0%D0%B9%D0%BE%D0%BD%D0%BE%D0%B2_%D0%B8_%D0%BF%D0%BE%D1%81%D0%B5%D0%BB%D0%B5%D0%BD%D0%B8%D0%B9_%D0%9C%D0%BE%D1%81%D0%BA%D0%B2%D1%8B). To obtain coordinates, we will use Nominatim service.

### 3. Methodology

1.	For each district in the Moscow city, obtain coordinates of centers of the districts (longitude, latitude) by Nominatim service

2.	For each district, get 100 most popular venues located within 1000 meters from the center of the district by Foursquare API. 
Parameters of the API call:

URL: https://api.foursquare.com/v2/venues/explore?client_id={}&client_secret={}&ll={},{}&v={}&radius={}&limit={}, where 

client_id, client_secret - access keys to the API, 

ll – longitude and latitude of the center of the district, 

radius = 1000

limit = 100

3.	Save info about venues into dataframe. The first 3 rows of the dataframe:
 ![alt text](https://raw.githubusercontent.com/fedormalyshev/Moscow_Neighborhoods/master/t1.png)

4.	Exclude districts with less than 20 venues (less than 20 venues aren’t sufficient for further clustering). 36 out of 120 districts left.

5.	Add to the dataframe dummy variables, representing each venue category

6.	Calculate frequency of occurrence of each venue category for each district

7.	 Create new dataframe that represents the top 10 venue categories for each district. The first 3 rows of the dataframe:
 ![alt text](https://raw.githubusercontent.com/fedormalyshev/Moscow_Neighborhoods/master/t2.png)

8.	Based on the top 10 most common venue categories, cluster districts (K-means clustering algorithm, K=3). The number of clusters K=3 is chosen experimentally and giving the most representative distribution of districts by clusters for this dataset. As a result, each district is assigned to one of 3 clusters.

### 4. Results

Let's show districts labels on the map of Moscow. Label color defines the district's belonging to a particular cluster:
![alt text](https://raw.githubusercontent.com/fedormalyshev/Moscow_Neighborhoods/master/m2.PNG)

Looking at the map, people who know Moscow city can clearly identify the following qualitative categories:
-	the first cluster (purple) – central prestigious districts (with 2 exceptions) 
-	the second cluster (orange) – non-central high quality districts
-	the third cluster (red) – dormitory areas 

This confirms that our clustering algorithm makes sense.

### 5. Discussion

There are some data issues, that limit effectiveness of the algorithm:
- insufficient data on Foursquare. From practice, to obtain reliable results of clustering, we should consider only districts with more than 20 venues. For Moscow, 84 out of 120 districts have 20 or less venues on Foursquare, and we should exclude these districs from consideration
- redundant categorization of venues on Foursquare. For example, there are venues with categories Restaurant, Middle Eastern Restaurant, Caucasian Restaurant, Asian Restaurant, Varenyky restaurant, and so on. For an algorithm, these are separate categories, although by the matter of fact they represent one category - restaurant. To overcome this issue, we can combine Foursquare's categorieds into more broad groups before clustering.

### Conclusion

The algorithm allow us to cluster city districts. To obtain reliable results, it's important to check that data is sufficient and well prepared, and carefully select paramenters of the algorithm (radius of districts, number of clusters, minimum number of venues to consider the district).
