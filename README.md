# PROJECT 2

## THELOOK Power BI Report Example

### Introduction
I decided to host a second project and thought it a good idea to create, document and explain my thought process behind the below Power BI Dashboard.

### Google BigQuery
I used the fictitious e-commerce data called "TheLook" hosted by Google BigQuery and imported this directly into Power BI. I did look to see if there was any explanation or description of the tables and coloumns but there was not.
![blah](Big%20Query.png)
![jajaja](PowerBi%20Import.png)
Therefore, once imported into Power BI, I took a good look at the data and decided to make some assumptions, something I obviously would not do in the real world. I concluded that it was an online clothing store, and the company was an American company. The data, for the most part, was clean, but I made the following changes in the "Transform" section of Power BI:

- Added Table Descriptions for clarification. <br>
- Added a standalone calendar table, good practice to help with time intelligence. <br>
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
#### YOY Revenue<br>
Having decided that Revenue was a key focus, I wanted to have a look at how the business is performing YOY, based on Sales Completed.<br>

#### Sales Completed All Years<br>
I wanted the ability and add some further KPIs, profit, orders, items ordered, etc, with the ability to see what product was being sold by location.<br>

#### Items Returned/Cancelled<br>
We would have as a business an acceptable measure for items returned and cancelled, hence why I wanted to track this.<br>

#### Website<br>
The Marketing team and I need to understand website traffic, by country, city and have an understanding of the conversion rate.<br>


### Power BI Semantic Model
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

%_items_returned = DIVIDE([m_returned_items], [m_count_items_ordered])
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
We are 60% up YOY with significant growth in all markets apart from Poland, Austria and Colombia. We have outperformed in all quarters in comparison to last year, and sales consistently trend up throughout all quarters. We can also see the Revenue by Month - Full Year, seeing how we have grown YOY significantly. In regard to products, it's the larger, more costly items that are producing the most revenue.
![aaa](page%201.png)

### Sales Completed
Overall sales completed show that China, the United States and Brazil account for 71% of the revenue we receive; these are key business countries for us. We can further see by clicking through that such places as Shanghai and Beijing are really driving the business. This will help the marketing team when we try to drive our marketing efforts. We can also see this from the map view, especially if we filter for specific countries and zoom in to see what areas are driving business. Providing insights for more localised strategic planning.
This page also has more of a focus on the products being sold, and with the filters we have, we can really drill down to see what the key brands and products are.
I can see the number of orders and the number of items ordered.
![ajaiwhg](PAGE%202.png)

### Items Cancelled/Returned
The key metric on this page is the % of items returned/cancelled because we need to check for any anomalies by product or even country. Also, we need to understand what good looks like because 10% of items being returned seems high and raises more questions in regards to how we can go about reducing that. In the same vein, further investigation needs to be done on my many items that are being cancelled.

### Website
We need to keep growing our web page views and sessions, so we need to keep our eye on this. When you filter through the years, you can see our existing customer base grow significantly YOY. Email and Adwords together account for 75% of the traffic coming to our site.

### Next steps and suggestions
In this example, the main focus has been on building a Power BI report with descriptive analytics, helping the business track key performance indicators and providing a good user experience. Our next steps would be to perform some exploratory analysis of the data to uncover any significant insights, which may thus lead to changes to the reporting structure.
