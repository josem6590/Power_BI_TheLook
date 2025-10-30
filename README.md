# PROJECT 2

## THELOOK Power BI Report Example

### Introduction
I decided to host a second project and thought it a good idea to create, document and explain my thought process behind the below Power BI Dashboard.

### Google BigQuery
I used the fictitious ecommerce data called "TheLook" hosted by Google BigQuery and imported this directly into Power BI. Once imported i took a look at the data as there was no descriptions to be found of the tables and coliumns,
Therefore, for this example I decided to make some assumptions of the data, something i wouldn't do in a real world example. Exploring the data i came to the conclusion that t was an online clothing store and the company was an American company. The data for the most part was clean but i made the below changes in the "Transform" section of Power BI:


![F1 SQL](CloudSQL.png) 

Added Table Descriptions for clarification.
Added a stand alone calendar table, good practice to help with time intelligence.
Renamed some columns for clarity.
Changed some timestamps to date
Added a custom column to the "website" table to show if a user was an existing customer or not.
Fixed Spain & Germany showing twice with different names.

### What do I want to show?
I then decided to take some time out to sit down and brainstorm the next steps. I put myself in the CEO chair of this business and thought about what i would want to see, what were the KPI's that we needed to track? what were the possible overall business goals of the business as this would help me to develop a plan in building this dashboard and provided the insights needed to myself, the CEO.

I decided initially on the below but also added some extras as certain things came to mind during the build, which we will come onto later:

Report Pages:
1) YOY Revenue
Having decided that Revenue was a key focus, I wanted to have a look at how the business is performing YOY, based on Sales Completed
2) Sales Completed All Years
I wanted the ability to check all years and add some further KPIs, profit, orders, items ordered etc with the ability to see what product was being sold and by location.
3) Items Returned/Cancelled
We would have as a business an acceptable measure for items returned and cancelled, hence why i wanted to track this.
4) Website
Myself and my marketing team need to understand the traffic, by country, city and have an understandimg of the conversion rate

### Power BI Semantic Model
Once I had a general idea of what i wanted to build I looked at the tables within Power Bi and separated out the Data Tables(below) from the Lookup Tables(above).
Then i started to create the relationships whilst creating tables and testing the relationships to make sure they are correct and working.

### Example DAX measures used

m_YTD_Revenue = TOTALYTD([m_sale_price],'Calendar'[Date])
Provides total revenue Year to Date

m_rev_LY_sameperiod = CALCULATE([m_YTD_Revenue], SAMEPERIODLASTYEAR('Calendar'[Date]))
Provide Revenue Year to date but from the sames period last year

m_gross_profit = SUMX(ordered_items, ordered_items[sale_price] - RELATED(products[cost]))
I needed the row context in this example before the SUM calculation hence why i used SUMX. This gives us the profit by subtracting the cost from the sale price and providing that as a sum

m_avg_items_ordered = CALCULATE([m_avg_items_per_order], ordered_items)
This provide the average items ordered but because in the semantic model the orders tables is downstream from the ordered_items tables, i have forced the relationship uphill using the above DAX.

%_items_returned = DIVIDE([m_returned_items], [m_count_items_ordered])
Returns the % of items returned by dividing the number of returned items from the count if items ordered.

m_distinct_session_count = DISTINCTCOUNT('website events'[session_id])
Using DISTINCTCOUNT here will provide the unique count of sessions

### Visualisations
I have tried to keep each reporting tab visually consistant with the following:
Clear Title for the page, top left
Filters always top right
High Level Overview Cards
Then a mix of detail with tables and relevant at a mix of graphs for easy view

### YOY Revenue
We are 60% up YOY with significant growth in all markets apart from Poland, Austria and Colombia. We have out performed in all quarters in comparison to last year and sales consistently trend up thorugh all quarters. We Can also see if the Revenue by Month Full Year, we see how we have grown Year over Year significantly. In regards to products its the larger more costly items that are producing the most revenue.

### Sales Completed
Overall sales completed shows that China, United States and Brasil account for 71% of the revenue we receive, these are key business countries for us. We can further see by cliucking through to see that such places as Shanghai and Beijing are really driving the business. This will help the marketing team when we try to drive our marketing efforts. We can also see this from the map view especially if we filter for specific countries and zoom in, to see what areas are driving business.
This page also has more of a focus on the products being sold and with the filters we have we can really drill down to see what are they key brands and products.
I am able to see the number of ordered and the number of items ordered.

### Items Cancelled/Returned
The key metric on this pages is the % of items returned because we need to check for any anomalies by product or even country. Also we need to understand what good looks like because 10% of items being returned seems to high and raises more questions in regards to how we can go about to reduce that. In the same vein further in vestigation needs to be done on my so many items are being cancelled.

### Website
We need to keep growing our web page views and sessions so we need to keep our eye on this. When you filter trough the years you can see our existing xcustomer base grow significantly YOY. Email and Adwords together account for 75% of the traffic coming to our site.

### Next steps and suggestions
In this example the main focus has been on building a Power BI report with descriptive analytics helping the business track key performance indicators and providing a good user experience. Our next steps would be to perform some exploratory analysis of the data to uncover any significant insights which may thus lead to changes to the reporting structure also.
