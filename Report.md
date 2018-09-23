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
