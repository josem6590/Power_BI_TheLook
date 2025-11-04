# PROJECT 2

## TheLOOK Power BI Report Example

### Introduction
I decided to host a second project and thought it a good idea to create, document and explain my thought process behind the below Power BI Dashboard.

### Google BigQuery
I used the fictitious e-commerce data called "TheLook" hosted by Google BigQuery and imported this directly into Power BI. I did look to see if there was any explanation or description of the tables and columns, but there was not.
![blah](Big%20Query.png)
![jajaja](PowerBi%20Import.png)
Therefore, once imported into Power BI, I took a good look at the data and decided to make some assumptions, something I obviously would not do in the real world. I concluded that it was an online clothing store, and the company was an American company. The data, for the most part, was clean, but I made the following changes in the "Transform" section of Power BI:

- Added Table Descriptions for clarification. <br>
- Added a standalone calendar table, a good practice to help with time intelligence. <br>
- Renamed some columns for clarity. <br>
- Changed some timestamps to dates. <br>
- Added a custom column to the "website" table to show if a user was an existing customer or not. <br>
- Fixed Spain & Germany showing twice with different names. <br>

![jaja](Custom%20Column%20for%20Existing%20Customer.png)
![akjauiaa](Spain%20%26%20Germany.png)


### What do I want to show?
I then decided to take some time out to sit down and brainstorm the next steps. I put myself in the CEO chair of this business and thought about what I would want to see, what were the KPI's that we needed to track? What were the possible overall business goals of the business? This would help me to develop a plan in building this dashboard, and provide the insights needed to me, the CEO.

I decided initially on the below, but also added some extras as certain things came to mind during the build, which we will come onto later:

#### Report Pages:
##### YOY Revenue
Having decided that Revenue was a key focus, I wanted to have a look at how the business is performing YOY, based on Sales Completed.<br>

##### Sales Completed All Years
I wanted the ability and add some further KPIs, profit, orders, items ordered, etc, with the ability to see what product was being sold by location.<br>

##### Items Returned/Cancelled
We would have as a business an acceptable measure for items returned and cancelled, hence why I wanted to track this.<br>

##### Website
The Marketing team and I need to understand website traffic, by country, city and have an understanding of the conversion rate.<br>


#### Power BI Semantic Model
Once I had a general idea of what I wanted to see, I looked at the tables within Power BI and separated the Data Tables(below) from the Lookup Tables(above).
Then I started to create the relationships, whilst creating tables to test those relationships to make sure they are correct and working as intended.

![ajajau](Semantic%20Model.png)

### Example DAX measures used

m_YTD_Revenue = TOTALYTD([m_sale_price],'Calendar'[Date])
- Provides total revenue Year to Date

m_rev_LY_sameperiod = CALCULATE([m_YTD_Revenue], SAMEPERIODLASTYEAR('Calendar'[Date]))
- Provide Revenue Year to date, but from the same period last year

m_gross_profit = SUMX(ordered_items, ordered_items[sale_price] - RELATED(products[cost]))
- I needed the row context in this example before the SUM calculation, hence why I used SUMX. This gives us the profit by subtracting the cost from the sale price and providing that as a sum

m_avg_items_ordered = CALCULATE([m_avg_items_per_order], ordered_items)
- This provides the average items ordered, but because in the semantic model the orders table is downstream from the ordered_items tables, I have forced the relationship uphill using the above DAX.

m_%_items_returned = DIVIDE([m_returned_items], [m_count_items_ordered])
- Returns the % of items returned by dividing the number of returned items by the count of items ordered.

m_distinct_session_count = DISTINCTCOUNT('website events'[session_id])
- Using DISTINCTCOUNT here will provide the unique count of sessions

### Visualisations
I have tried to keep each reporting tab visually consistent with the following:
Clear Title for the page, top left.<br>
Filters always top right.<br>
High-Level Overview Cards.<br>
Then a mix of detail with tables and relevant graphs for easy viewing.<br>

### YOY Revenue
We are 60% up YOY with significant growth in all markets apart from Poland, Austria and Colombia. We have outperformed in all quarters in comparison to last year, and sales consistently trend up throughout all quarters. We can also see the Revenue by Month - Full Year, seeing how we have grown YOY significantly. In regard to products, it's the larger, more costly items that are producing the most revenue.<br>

![aaa](page%201.png)

### Sales Completed
Overall sales completed show that China, the United States and Brazil account for 71% of the revenue we receive; these are key business countries for us. We can further see by clicking through that such places as Shanghai and Beijing are really driving the business. This will help the marketing team when we try to drive our marketing efforts. We can also see this from the map view, especially if we filter for specific countries and zoom in to see what areas are driving business. Providing insights for more localised strategic planning.
This page also has more of a focus on the products being sold, and with the filters we have, we can really drill down to see what the key brands and products are.
I can see the number of orders and the number of items ordered.<br>

![ajaiwhg](PAGE%202.png)

### Items Cancelled/Returned
The key metric on this page is the % of items returned/cancelled because we need to check for any anomalies by product or even country. Also, we need to understand what good looks like because 10% of items being returned seems high and raises more questions in regards to how we can go about reducing that. In the same vein, further investigation needs to be done on my many items that are being cancelled.<br>

![atere](pAGE%203.png)

### Website
We need to keep growing our web page views and sessions, so we need to keep our eye on this. When you filter through the years, you can see our existing customer base grow significantly YOY. Email and Adwords together account for 75% of the traffic coming to our site.<br>

![tatat](PAGE%204.png)

### EDA
The next steps would have been to perform some exploratory analysis in the hope of answering some of the following questions and potentially creating further revenue:<br>
1) Can we expand into new territories?<br>
2) Although on small numbers, why have sales dropped in the following countries: Poland, Austria and Colombia?<br>
3) What percentage of the population has ordered items in the last three years?<br>
4) Do any territories stand out? smaller country but with a bigger percentage of population as customers?<br>
5) If so, what are we doing differently in those markets in comparison to some of our larger markets like China? because perhaps we can replicate and grow those countries by an even larger number, and quickly.<br>
6) I would look at online purchasing habits within each country, by clothing store and gender, to see if they reflect our gender analysis. If, for example, in some countries 80% of online clothes are purchased by women, it would raise the question that perhaps we should spend some extra marketing specifically for women in those countries.<br>
7) If 10% of our items are being returned, what would be the bottom-line return if we reduced this by 5%?<br>
8) Why are so many items being cancelled? And can we reduce that?<br>
9) We need to look time it takes from receiving an order to it being sent out of fulfilled, to make sure we don't have any issues. Perhaps there are some delays, and people are then cancelling.<br>
10) We need to look at the whole purchasing funnel to evaluate this better and provide more useful information to the user.

Asking these questions would lead me to make some changes to the dashboard, but iterations often happen during the exploratory stage.

### Conclusion
The main focus of this exercise has been on building a Power BI report using this fictitious data, explaining my thought process along the way, highlighting my knowledge of data modelling, DAX measures, visualisation and UX. Thus, I will not go into summarising the key insights that are visible on the Power BI file, so that I can be as brief as possible. 


