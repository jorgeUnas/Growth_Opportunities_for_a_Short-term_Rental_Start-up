# Growth Opportunities for a Short-term Rental Start-up


<p align= "center">
<img src="https://static01.nyt.com/images/2019/05/29/realestate/00skyline-south4/88ce0191bfc249b6aae1b472158cccc4-superJumbo.jpg" height="200"> 
</p>

New York City is one of the preferred destinations for around 5 million tourists every month. That's why it is a place where a listing company, such as [Airbnb](https://www.airbnb.com/) can flourish by catering to the high demand for temporary lodging among travelers. Airbnb offers listings for stays ranging from a few nights to several months. In this notebook, I worked with datasets in a variety of formats( `.csv`, `.tsv`, and `.xlsx`) and cleaned the data to extract insights that can be used by the growth team of our Start-up. The goal is to identify opportunities for success in the short-term rental market. 


Three files containing data on 2019 Airbnb listings are available:

**data/airbnb_price.csv**
- **`listing_id`**: unique identifier of listing
- **`price`**: nightly listing price in USD
- **`nbhood_full`**: name of borough and neighborhood where listing is located

**data/airbnb_room_type.xlsx**
This is an Excel file containing data on Airbnb listing descriptions and room types.
- **`listing_id`**: unique identifier of listing
- **`description`**: listing description
- **`room_type`**: Airbnb has three types of rooms: shared rooms, private rooms, and entire homes/apartments

**data/airbnb_last_review.tsv**
This is a TSV file containing data on Airbnb host names and review dates.
- **`listing_id`**: unique identifier of listing
- **`host_name`**: name of listing host
- **`last_review`**: date when the listing was last reviewed

In this file you will find enough information to answer the folowing questions:

- What is the average price, per night, of an Airbnb listing in NYC?
- How does the average price of an Airbnb listing, per month, compare to the private rental market?
- How many adverts are for private rooms?
- How do Airbnb listing prices compare across the five NYC boroughs?
