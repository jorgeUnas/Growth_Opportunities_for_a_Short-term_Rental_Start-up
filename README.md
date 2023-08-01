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

<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/most%20popular%20neighbourhoods.png" height="400"> 
</p>
<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/Dashboard%201%20copy.png" height="200"> 
</p>

2.	What is the average rental price for short-term rentals in New York City, and which neighbourhoods and property types have the higher average rental price? 

The average Airbnb rental price in NYC in 2019 was $142 per listing, while the average monthly price was $4,314. However, it's important to consider that the average monthly price was calculated based on the assumption that the listings are rented for all 365 days of the year. This indicates that the actual average price is likely lower than this figure. Additionally, the monthly price offered by Airbnb tends to be higher than the average monthly rental price in the private rental market. This is because Airbnb has fewer regulations on the price of the listings than the private rental market, which makes this business model so appealing to hosts. 

Among the ten neighbourhoods (Figure 3) with the highest average rental prices, three stand out for providing a quieter experience to tourists compared to the bustling neighbourhoods located in Manhattan and Brooklyn: Sea Gate, Neposit, and Willowbrook. These neighbourhoods are exclusively dominated by Airbnb listings that are entire homes/apartments. Then, this presents an opportunity for Pillow Palooza to differentiate itself from Airbnb by promoting the renting of private and shared rooms in these areas. However, Neposit, being a luxury area frequently visited by famous people, may pose additional challenges compared to the other two neighbourhoods.

<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/average%20rental%20price.png" height="200"> 
</p>
<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/average%20rental%20price%202.png" height="200"> 
</p>

3. What are the most commonly rented property types on Airbnb in New York City, and how does this vary by neighbourhood? 

By selecting all 216 neighbourhoods in the dropdown, the treemap shows that the most common rented listings in NYC are entire homes/apartments, followed closely by private rooms (Figure 4). At this point, I consider it appropriate to mention that if Pillow Palooza wants to position itself as a competitor of Airbnb, we have to find a way to differentiate from this company and others of the same style. In that sense, my recommendation is to focus our attention on hosts offering private rooms and shared rooms. In the next question, I will elaborate on this idea.

<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/the%20most%20commonly%20rented%20property%20types.png" height="200"> 
</p>


4. What is the average number of days short-term rentals in New York City are booked in a year, and how does this vary by neighbourhood and property type?

Since I consider it is important that Pillow Palooza explore opportunities not yet covered by Airbnb, I recommend focusing our marketing strategies on places where Airbnb is strong but the market is not overly saturated as it is in the neighbourhoods belonging to Manhattan and Brooklyn. For that reason, I set apart the top neighbourhoods by the number of booked days per year that are located out of those areas (Figure 5). All of these neighbourhoods are located in State Island where Airbnb offers entire homes/apartments and private rooms but not shared rooms. Consequently, my recommendation to our company is to promote the rental of shared rooms in these neighbourhoods. This is an idea that some rental companies, such as Ollie and Common, have successfully explored in NYC. These companies would be our direct competitors, especially Common, which has achieved a significant annual revenue of $37.5 million. 

Shared rooms are a popular choice among students and individuals seeking affordable short-term rentals. Additionally, due to the growing popularity of platforms like Airbnb, many permanent residents in NYC also opt for shared rooms as long-term housing options become limited. This is because hosts often find short-term rentals to be more profitable, and Airbnb capitalizes on this by intentionally increasing prices.

<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/Average%20number%20of%20days%20are%20booked%20in%20a%20year.png" height="200"> 
</p>
<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/Average%20number%20of%20days%20are%20booked%20in%20a%20year%202.png" height="200"> 
</p>

5. How has the demand for short-term rentals in New York City changed over time, and are there any monthly/seasonal trends that could impact business decisions?

This question cannot be fully answered with the public data available because, as competitors, we do not have access to the booking timestamps of Airbnb. The only historical data we possess pertains to the date of the last review for each listing. The chat in Figure 6 is an attempt to get historical information based on the dates of the last reviews. However, it is important no misinterpret this plot. Without the dates of previous reviews, we cannot say there is a tendency for users to provide the majority of their reviews during the summer. I recommend that the data engineering team obtain the dates of previous reviews, which will allow us to understand in which months/seasons Pillow Palooza could expect more reviews. This way, we will know the best time to boost our visibility to the public.

<p align= "center">
<img src="https://github.com/jorgeUnas/Growth_Opportunities_for_a_Short-term_Rental_Start-up/blob/main/How%20did%20demand%20for%20short-term%20rentals%20changed%20over%20time.png" height="200"> 
</p>

## Recommendations

-	As Pillow Palooza is going to incursion as a new rental company in a place where this market is very dynamic, I highly recommend following the market experience of Airbnb but trying to differentiate itself from this competitor to get visibility. For example, I recommend focusing our marketing efforts on neighbourhoods where Airbnb has high revenues but offers a quiet tourist experience, unlike the neighbourhoods in Manhattan and Brooklyn. Examples of such neighbourhoods are Seat Gate, Neponsit and Willowbrook. As these neighbourhoods are calmer, we can incentivize tourists to rent private or shared rooms as a safe and cheaper alternative to entire homes/apartments. 
-	A similar approach that was applied to the listing prices can also be used for the number of booked days. We could focus our attention on neighbourhoods where the listings have the highest occupancy throughout the year and are located outside of oversaturated areas of NYC like Manhattan and Brooklyn. Examples of such neighbourhoods on Staten Island include Emerson Hill, Lighthouse Hill, Prince's Bay, New Springville, and Rossville, which are located on Staten Island. Upon examining the types of rooms available on Airbnb in these areas, we find that shared rooms are not being offered. This presents a great opportunity for Pilow Palooza to promote the rental of shared rooms in these neighbourhoods and quickly gain visibility not only among tourists but among students and local residents looking for cheaper alternatives to long-term rentals.
-	I highly recommend researching historical data on previous reviews. This will help us understand the months or seasons during which Pillow Palooza can anticipate a higher number of reviews. By doing so, we can determine the optimal time to enhance our visibility to the public.





