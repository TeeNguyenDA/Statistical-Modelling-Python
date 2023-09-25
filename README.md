# Statistical-Modelling-with-Python-Project

## Project/Goals

Citybikes is an API that provides bike sharing data for apps, research and projects.
CityBikes supports more than 400 cities and the Citybikes API is an interesting dataset for building bike-sharing transportation projects. We use Citybikes to extract the coordinates of bike stations in the Bixi Montreal network.

Places POI is an API from Foursquare to help better business decisions with a rich POI data representing 120M+ venues across the world. The POIs in the scope of this project are bars that are 1Km radius from our Bixi Montreal bike stations.

Yelp Fusion API allows users to get the best local content and user reviews from millions of businesses around the world. Emphasizing on the world's best local content, there are dozens of attributes per business, and new reviews and photos are added by active Yelp users. We use mainly Business Search to Search for our "Bars" POI by by keyword, category.

The high-level goals of this Statistical Modelling project with Python are to:

1. Connect to CityBikes API, query and retrieve all bike stations available in Montreal.
2. Connect to Foursquare and Yelp APIs, query and retrieve information for bars as a POI. At this step, we compare the quality of the Yelp and Foursquare API for which API provides the most complete information/better coverage for the POI.
3. Join associated data from CityBikes,Foursquare and Yelp APIs, perform exploratory, statistical data analysis and data visualization to explore the data and select features for modelling.
4. Build a regression model using Python’s `statsmodels` module to demonstrate the relationship between the number of bikes in a particular bike station in Montreal and the characteristics of our bar POIs in that location.

## Process

### Part 1: Connect to [CityBikes API](/notebooks/city_bikes.ipynb)
Output mapping: The [**city_bikes.ipynb**](/notebooks/city_bikes.ipynb) notebook

1. Select a bike network to study: We chose Montreal with the Bixi bike sharing network. Being the third largest metropolitan in Canada, Montreal had an enormous number of bike stations (798 stations) and a big number of nearby bars within the 1 kilometer.

2. Gather information for every bike station within the Montreal Bixi bike network, including details such as station id, station name, latitude, longitude, total number of bikes at that station. We still requested the Citybikes API for the number e-bikes, the count of vacant slots etc. but will not include them for further analysis due to overcomplication.

### Part 2: Connect to [Foursquare API](/notebooks/foursquare_EDA.ipynb) and [Yelp API](/notebooks/yelp_EDA.ipynb)
Output mapping: The [**foursquare_EDA.ipynb**](/notebooks/foursquare_EDA.ipynb) and [**yelp_EDA.ipynb**](/notebooks/yelp_EDA.ipynb) notebooks

1. Retrieve data from both the Foursquare API and the Yelp API by using the latitude and longitude coordinates of Montreal Bixi bike stations. Relevant parameters in relation to the POIs to request includes: bar names, distance from the bike station, business categories, ratings, and review counts.

2. Put Foursquare and Yelp information into different Pandas DataFrame, compare their relative API quality for the same station coordinate from Citybikes.

### Part 3: [Joining Data](/notebooks/joining_data.ipynb)
Output mapping: The [**joining_data.ipynb**](/notebooks/joining_data.ipynb) notebook

1. Create a consolidated merged dataframe with data from Part 1 and Part 2. Create a SQLite database and store the data collected on the POIs.

2. Data cleaning and wrangling: Handle missing values, null values.

3. Visualize the data to detect possible patterns, trends between numerical and categorical variables. We also conduct statistical analysis and hypothesis testing to validate the quality of our data.

### Part 4: [Building a Model](/notebooks/model_building.ipynb)
Output mapping: The [**model_building.ipynb**](/notebooks/model_building.ipynb) notebook

1. Develop a Multivariate Linear Regression model to assess the relationships among the variables of significance. It was assumed that the target dependen variable is the number of bikes available at a station. Our feature selection in part 3 recommended the independent variables to predict the target are the review count of the bar POIs and their distance from per bike station, due to slightly higher correlations with the target variable.

2. Interpret the Output of our Multivariate Linear Regression by using model evaluation measures and assumption testing.

3. Recommend on how to turn the above regression problem into a classification one by using transform the output of the target variable 'cb_bike_num' into discrete distribution.


## Results

### About the comparative quality of API coverage in Montreal for bar POIs:
Output mapping: The **yelp_EDA.ipynb** notebook

1. Foursquare: 5907 POIs coverage with 10 columns. Relatively basic information about each place, their category taxonomy for bars is more comprehensive and organized.

2. Yelp: Delivered a more extensive dataset with 9081 POIs coverage and with columns. Yelp's dataset boasts impressive indicators, including ratings, review counts, and price information, which surpasses the information provided by Foursquare. Have less of a good category classification for bar businesses.

Both APIs provide a relatively accurate distance measurements from a bike location. We observed that some of the distance values in both Yelp and Foursquare response exceeded 1000 meters, although we clearly defined in our API endpoints.

### About the results of the Multivariate Linear Regression model:

In summary, our Multivariate Linear Regression model exhibits weak performance and does not align well with our dataset. As previously observed in part 3, none of our variables demonstrate a normal distribution. A possible direct improvement is to repeat the EDA and data transforming processes (such as normalize, encoding other categorical variables to include in the model) to potentially itirate and increase the fit of our Multivariate Linear Regression model. Other statistical methods or models that work well with non-normal data can be considered as well (for example, logistic regression models). Or we can seek other API options to acquire better bar features that have a better relationship with the dynamic number of bikes per station.

Business context wise, a precise prediction of station inventory (specifically the number of bikes available at the station) might, or might not have impacts in the local points of interests nearby, specifically bars is the problem our model is trying to solve in this project. The business conclusion from the model output indicates a weak linear relationship between the dynamic number of bikes available at a station; and the characteristics of bar POIs close by; such as distance from stations to bars, and its available review counts. Using those mentioned bar characteristics doesn't explain well the connection.

## Challenges 

1. My biggest challenge during this project centers around the need to leverage a comprehensive knowledge and applicability of using of Python (data structures like lists, dictionaries, series, dataframes and JSON), API knowledge, SQL in Python to follow through a full cycle project from connecting to API extended to builing a prediction model from the raw API data. It includes decomposing the big tasks per notebook into smaller, manageable processes with time management and iterative cycles to achieve cohesion and making sense of data; especially with a large amount of data (of over 14,988 entries combined from Foursquare and Yelp) to prepprocess. 

2. Also, the constraints imposed by limited API calls from Yelp (Yelp's API rate limit is 5000 API calls per day) caused a major delay in the first step of data collection. Yelp API will most likely response with an error message when hitting the limit. This challenge pushed me to seek assistance from more experience mentors and cohort students to form a creative strategy to stagger my requests into small sets and using more than one API keys to stay proactive in timing.

3. Finally, the collected data displays a lack of normal distribution, indicating a potential need for adjustment strategies in the API data collection methodology, cleaning and exploring new features we might have overlooked.

## Future Goals

1. To repeat the data prepprocessing stage to uncover any valuable POI parameters that we have missed in the scope of this repository.
2. To further explore the relationships between the categorical variables of the bike stations and bar characteristics to better understand the data.
3. To trial more machine learning models that work with non-normal distribution to improve the output of our prediction models.
