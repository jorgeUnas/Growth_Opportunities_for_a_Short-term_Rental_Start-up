# Growth Opportunities for a Short-term Rental Start-up


<p align= "center">
<img src="https://static01.nyt.com/images/2019/05/29/realestate/00skyline-south4/88ce0191bfc249b6aae1b472158cccc4-superJumbo.jpg" height="200"> 
</p>

New York City is one of the preferred destinations for around 5 million tourists every month. That's why it is a place where listing companies, such as Pillow Palooza can flourish by catering to the high demand for temporary lodging among travelers. In order to maximize revenue and occupancy rates for our company, I conducted an analysis of publicly available data from [Airbnb](https://www.airbnb.com/), a well-known player in the rental industry. Utilizing **`Python`** for data wrangling and cleaning, **`SQL`** for data exploration and gathering and, presenting the results through a **`Tableau`** dashboard, I got valuable insights and actionable recommendations to make our company succeed in the competitive rental market and attract potential investors.

<a> [Video Presentation](https://drive.google.com/file/d/1tnJMGncje1ocSYc1dwRsE46Xb4D2ll2d/view?usp=drive_link) </a>

<a> [Tableau Dashboard](https://public.tableau.com/app/profile/jorge.humberto.unas.daza3556/viz/NYCShort-TermRentalStart-Up/Dashboard1?publish=yes) </a>

## Context

In order to identify growth opportunities for Pillow Palooza I was provided with 3 different datasets (See figure 1) in three different formats ( .csv, .tsv, and .xlsx). Basically, the company is focused in uncover trends in popular neighbourhoods, rental prices, property types, length of stay and demand over time. That’s why our team have come up with some strategic questions that I had to resolve: 

1.	What are the most popular neighbourhoods for short-term rentals in New York City?
2.	What is the average rental price for short-term rentals in New York City, and which neighbourhoods and property types have the higher average rental price? 
3.	What are the most commonly rented property types on Airbnb in New York City, and how does this vary by neighbourhood? 
4.	What is the average number of days short-term rentals in New York City are booked in a year, and how does this vary by neighbourhood and property type? 
5.	How has the demand for short-term rentals in New York City changed over time, and are there any monthly/seasonal trends that could impact business decisions?

Based on the insights generated from those questions I have to prepare some recommendations to help the company identify which neighbourhoods to invest in, which property types to focus on, and how to price their rentals to remain competitive in the market.

## Results

#### Data Wrangling and Cleaning

As the data came from different sources, it was necessary to ensure the information is accurate and reliable. Then, I used Python to import, join and clean the data. Some cleaning steps were performed in order to:
-	Evaluate data constraints such as removing units along with listing prices (see step 1 in the annexed jupyter notebook)
-	Convert inconsistent data types to the right type (e.g, in step 1 the strings were converted to numbers because are listing prices)
-	Find data range constraints (In step 3 I removed listing prices equal to 0, which wrongly indicates that those are free listings) 
-	Remove categorical data inconsistencies (see step 5 where the room type column was converted to lowercase to get just 3 unique categorical values)
-	Delete duplicates and missing values generated by merging the tables in step 7. 

As part of this data wrangling, I decide to create 2 new columns to help with the posterior data analysis: borough and monthly price. The first one was obtained from the preexistent nbhood_full column by splitting the categorical values (step 8). In this way, we can group the data either by neighbourhood or by borough. On the other hand, the monthly price column allows us to directly compare the Airbnb listing prices with the price of the private rental market.

#### Data Exploration with SQL

Once consistent and reliable datasets were obtained, a couple of requirements were returned to the data engineering team. The first one was to include the two new columns for boroughs and monthly listing prices. The second request was to add further information about the listings such as listing availability, booking timestamps, review dates, and number of listings per host.

The data engineer team kindly return me with the schema of Figure 1 where they included extra columns to enhance the analysis. 

<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/ERD.jpg" height="400"> 
</p>
Figure 1. Schema for Airbnb created by the data engineering team of Pillow Palooza

As expected, the booking timestamps were not available for us because our company is a competitor using public information from Airbnb. This makes it not possible to know how the demand for short-term rentals has changed over time. However, we still have some information about the reviews that can give us an idea of which month/season we can expect to have more reviews to boost our visibility.

Although it is straightforward to answer our initial questions using SQL1, it is better to interpret the result using visualizations. For that reason, in the next section, I will proceed to use a Tableau dashboard to discuss these answers and other remarkable insights. 

To proceed with the use of Tableau the three tables were joined using SQL as follows: 

``` sql
SELECT p.listing_id,
       p.price,
       p.borough,
       p.neighbourhood,
       p.price_per_month,
       p.latitude,
       p.longitude,
       rt.description,
       rt.room_type,
       r.host_name,
       r.last_review,
       r.minimum_nights,
       r.number_of_reviews,
       r.reviews_per_month,
       r.calculated_host_listings_count,
       r.availability_365,
       r.booked_days_365
FROM prices p
FULL JOIN reviews r
ON p.listing_id = r.listing_id
FULL JOIN room_types rt
ON r.listing_id = rt.listing_id;
```

## Data Analysis in Tableau

### Dashboard description:

Three of the main metrics under study are always displayed on the top of the dashboard: Average listing price, average listing price per month and average number of booked days. Take into account that these metrics are related to the overall number of listings in NYC that use Airbnb and change depending on the filters from each plot. The stacked bar plot represents the average metric by borough and room type. There is a dropdown next to the title to select a metric to be visualized in the plot. By clicking any of the borough(s) and/or room type the information of the entire dashboard is filtered. 
The second bar plot organizes the neighbourhoods from the higher to the lowest average listing price and displays just the top-N of them. There is also a dropdown to select another metric to sort the neighbourhoods. The options are the number of listings, the average of booked days/year, the average price per month and the total revenue. Notice that, this bar plot determines the neighbourhoods and the metric that are displayed in the treemap and in the map. The treemap allows us to be more granular with information about the top-N neighbourhoods. For example, Murray Hill is the place with the most expensive shared rooms among these neighbourhoods.
Additionally, observe that a tooltip pops in when you hover the cursor over each bar of the plot. This tooltip contains a pie chart that gives an idea of the proportion of each room type for that specific neighbourhood. 

### Business questions and insights:

1.	What are the most popular neighbourhoods for short-term rentals in New York City?

The 10 most popular neighbourhoods according to the largest number of listings are shown in Figure 2. As expected, these neighbourhoods are located in the most tourist areas of Manhattan and Brooklyn, such as Times Square, Chinatown, DUMBO, Prospect Park, Bushwick, and Greenpoint. The most popular room type in these neighbourhoods is entire homes/apartments. 


