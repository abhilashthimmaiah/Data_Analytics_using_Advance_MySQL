
# Final Project: Data-Driven Story to seek the Next Round of Fundings

Maven Fuzzy Factory Pvt. Co – is seeking the next round of funding from various Venture Capital (VC) Funds after three
successful years of growth & existence. Subsequently, the CEO needs her analyst to craft a compelling Data-Driven Story 
of Maven Fuzzy Factory’s Growth to the VC’s.


## (1) Volume Growth Analysis:

 
(a) Quarterly Trended Analysis of Session to Order Volume Growth Analysis

```bash
Select
Year(website_sessions.Created_at) as Years,
Quarter(website_sessions.Created_at) as Quarters,
Count(Distinct website_sessions.website_session_id) as Total_Sessions,
Count(Distinct Orders.Order_id) as Total_Orders
from website_sessions 
Left Join Orders On Orders.website_session_id = website_sessions.website_session_id
where website_sessions.created_at >= 2015-03-20
group by 1, 2;
```

## (2) Showcasing Consistent Efficiency Improvements:

 
(a) Trended analysis of Session-To-Order Coverstion Rate 

(b) Revenue Per Order 

(c) Revenue Per Session

```bash
Select
Year(website_sessions.Created_at) as Years,
Quarter(website_sessions.Created_at) as Quarters,
Concat(Round(Count(Distinct Orders.Order_id) 
/ Count(Distinct website_sessions.website_session_id) * 100, 2), '%') as Sessions_To_Orders_Rate,
Round(Sum(Orders.Price_usd) / 
Count(Distinct Orders.Order_id), 2) as Revenue_Per_Order,
Round(Sum(Orders.Price_usd) / 
Count(Distinct website_sessions.website_session_id), 2) as Revenue_Per_Session
from website_sessions 
Left Join Orders On Orders.website_session_id = website_sessions.website_session_id
where website_sessions.created_at >= 2015-03-20
group by 1, 2;
```
## (3) Showcasing Various Marketing Channels Growth Rates in terms of Order Volume Growth Rates:

 
(a) Gsearch_Non_Brand Orders

(b) Bsearch_Non_Brand Orders 

(c) Brand Orders

(d) Organic Orders

(e) Direct Type In Orders

```bash
select
Year(website_sessions.created_at) as Years,
Quarter(website_sessions.Created_at) as Quarters, 
Count(Distinct Case When utm_source = 'gsearch' and utm_campaign = 'nonbrand' then orders.order_id else null end) 
as Gsearch_Non_Brand,
Count(Distinct Case When utm_source = 'bsearch' and utm_campaign = 'nonbrand' then orders.order_id else null end) 
as Bsearch_Non_Brand,
Count(Distinct Case When utm_campaign = 'brand' then orders.order_id else null end) Brand,
Count(Distinct Case When utm_source is null and http_referer in ('https://www.gsearch.com', 'https://www.bsearch.com') then orders.order_id else null end) 
Organic,
Count(Distinct Case When utm_source is null and http_referer is null then orders.order_id else null end) Direct_Type_In
from website_sessions
Left Join Orders On Orders.website_session_id = website_sessions.website_session_id
where website_sessions.created_at >= 2015-03-20
Group by 1, 2;
```
## (4) Showcasing Session-to-Order CVR for Specific Marketing Channels:

(a) Gsearch_Non_Brand CVR

(b) Bsearch_Non_Brand CVR

(c) Brand CVR

(d) Organic CVR

(e) Direct Type In CVR

```bash
select
Year(website_sessions.created_at) as Years,
Quarter(website_sessions.Created_at) as Quarters, 
Round(Count(Distinct Case When utm_source = 'gsearch' and utm_campaign = 'nonbrand' then orders.order_id else null end) / 
Count(Distinct Case When utm_source = 'gsearch' and utm_campaign = 'nonbrand' then website_sessions.website_session_id else null end) * 100, 2) as Gsearch_Non_Brand_CVR,
Round(Count(Distinct Case When utm_source = 'bsearch' and utm_campaign = 'nonbrand' then orders.order_id else null end) / 
Count(Distinct Case When utm_source = 'bsearch' and utm_campaign = 'nonbrand' then Website_sessions.website_session_id else null end) * 100, 2) as Bsearch_Non_Brand_CVR,
Round(Count(Distinct Case When utm_campaign = 'brand' then orders.order_id else null end) /
Count(Distinct Case When utm_campaign = 'brand' then website_sessions.website_session_id else null end) * 100, 2) as Brand_CVR,
Round(Count(Distinct Case When utm_source is null and http_referer in ('https://www.gsearch.com', 'https://www.bsearch.com') then orders.order_id else null end) /
Count(Distinct Case When utm_source is null and http_referer in ('https://www.gsearch.com', 'https://www.bsearch.com') then Website_sessions.website_session_id else null end) * 100, 2) as Organic_CVR,
Round(Count(Distinct Case When utm_source is null and http_referer is null then orders.order_id else null end) /
Count(Distinct Case When utm_source is null and http_referer is null then website_sessions.website_session_id else null end) * 100, 2) as Direct_Type_In_CVR
from website_sessions
Left Join Orders On Orders.website_session_id = website_sessions.website_session_id
where website_sessions.created_at >= 2015-03-20
Group by 1, 2;
```

## (5) Showcasing Product Level Revenue & Margin Ratio On Monthly Trend Basis along with Total Revenue & Margin Ratio:

 
(a) Pdt 1 

(b) Pdt 2 

(c) Pdt 3 

(d) Pdt 4

```bash
select 
Year(created_at) as Yr,
Month(Created_at) as Mt,
Sum(Case When primary_product_id = 1 then price_usd else null end) as Pdt1_Revenue,
Concat(Round((Sum(Case When primary_product_id = 1 then price_usd else null end) - Sum(Case When primary_product_id = 1 then cogs_usd else null end)) 
/ Sum(Case When primary_product_id = 1 then price_usd else null end) * 100, 2), '%') as Pdt1_Margin,
Sum(Case When primary_product_id = 2 then price_usd else null end) as Pdt_2_Revenue,
Concat(Round((Sum(Case When primary_product_id = 2 then price_usd else null end) - Sum(Case When primary_product_id = 2 then cogs_usd else null end)) 
/ Sum(Case When primary_product_id = 2 then price_usd else null end) * 100, 2), '%') as Pdt_2_Margin,
Sum(Case When primary_product_id = 3 then price_usd else null end) as Pdt3_Revenue,
Concat(Round((Sum(Case When primary_product_id = 3 then price_usd else null end) - Sum(Case When primary_product_id = 3 then cogs_usd else null end)) 
/ Sum(Case When primary_product_id = 3 then price_usd else null end) * 100, 2), '%') as Pdt3_Margin,
Sum(Case When primary_product_id = 4 then price_usd else null end) as Pdt4_Revenue,
Concat(Round((Sum(Case When primary_product_id = 4 then price_usd else null end) - Sum(Case When primary_product_id = 4 then cogs_usd else null end)) 
/ Sum(Case When primary_product_id = 4 then price_usd else null end) * 100, 2), '%') as Pdt4_Margin,
Sum(Price_usd) as Total_Revenue,
Concat(Round(Sum(Price_usd - cogs_usd) / Sum(Price_usd) * 100, 2), '%') as Total_Margin
from orders where created_at >= 2015-03-20
Group By 1, 2;
```

## (6) Multistep Query Based Deep Dive into Impact of Introduction of Newer Products on Order_Converstion_Rates & Subsequent Impact on Revenue:

 
(a) /Product Page Click Trough Rates

(b) /Product Page to Order Conversion Rates

(c) Impact on Renevue

```bash
Create Temporary Table Charlie
Select 
website_session_id,
website_pageview_id,
created_at as Pdt_Page_Seen
from website_pageviews where Website_Pageviews.created_at >= 2015-03-20 and pageview_url = '/products';

Select
Year(Pdt_Page_Seen) as Yr,
Month(Pdt_Page_Seen) as Mt,
Count(Distinct charlie.website_session_id) as Total_Sessions_Pdt,
Count(Distinct website_pageviews.website_session_id) as Sessions_Clicked_Next,
Round(Count(Distinct website_pageviews.website_session_id) / Count(Distinct charlie.website_session_id) * 100, 2) as Click_Rate,
COUNT(DISTINCT orders.order_id) AS Total_Orders,
Round(COUNT(DISTINCT orders.order_id) / Count(Distinct charlie.website_session_id) * 100, 2) as Odr_CVR,
Sum(orders.price_usd) AS Total_Revenue
From Charlie
Left Join website_pageviews on website_pageviews.website_session_id = charlie.website_session_id
	and website_pageviews.website_pageview_id > charlie.website_pageview_id
Left Join Orders on Orders.website_session_id = charlie.website_session_id
Group by 1, 2;
```

## (7) Products Cross Sales Analysis & Cross Sales Data:

 
(a) Product wise Total Orders

(b) Product wise Total Cross Sales Orders

(c) Product Wise Cross Sales Rate Analysis

```bash
Create Temporary Table Primary_Product_Table
Select
orders.Order_id,
orders.Primary_Product_Id,
orders.Created_at as Time_Stamp
from Orders
where orders.created_at > '2014-12-05 00:00:00';

Select
Primary_Product_Id,
Count(Distinct Order_id) as Total_Orders,
COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 1 THEN order_id ELSE NULL END) AS Cross_Sold_Pdt1,
Concat(Round(COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 1 THEN order_id ELSE NULL END) / Count(Distinct Order_id) * 100, 1),'%') as Pdt1_Rt,
COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 2 THEN order_id ELSE NULL END) AS Cross_Sold_Pdt2,
Concat(Round(COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 2 THEN order_id ELSE NULL END) / Count(Distinct Order_id) * 100, 1),'%') as Pdt2_Rt,
COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 3 THEN order_id ELSE NULL END) AS Cross_Sold_Pdt3,
Concat(Round(COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 3 THEN order_id ELSE NULL END) / Count(Distinct Order_id) * 100, 1),'%') as Pdt3_Rt,
COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 4 THEN order_id ELSE NULL END) AS Cross_Sold_Pdt4,
Concat(Round(COUNT(DISTINCT CASE WHEN Cross_Sold_Product_ID = 4 THEN order_id ELSE NULL END) / Count(Distinct Order_id) * 100, 1),'%') as Pdt4_Rt
from
(
Select
Primary_Product_Table.*,
Order_items.product_id as Cross_Sold_Product_ID
from Primary_Product_Table
Left Join Order_items On Order_Items.Order_id = Primary_Product_Table.Order_id
and order_items.is_primary_item = 0
)
As Cross_Sell_Analysis Group By 1;
```

## (8) Final Key Points to Investors:

 
(1) Growth in Revenue (Q-o-Q)

(2) Growth in Operating Profits (Q-o-Q)

(3) Consistent High Profit Margins (Q-o-Q)

(4) Customer Centric Culture: Striving to Keep Customer Returns Rate Below 10% Threshold in all Quarters

(5) Growth in Brand Value among Customers - Increased Direct Type-In's

(6) Product Proportion in Revenue Portfolio

```bash
Select
Years,
Quarters,
Orders,
Revenue,
Operating_Profit,
Concat(Round(Operating_Profit / Revenue * 100, 2), '%') as Profit_Margins,
Concat(Round(Customer_Returns / Orders * 100, 2), '%') as Returns_Rate
from
(
Select
year(orders.created_at) as Years,
quarter(orders.created_at) as Quarters,
Count(Distinct orders.Order_id) as Orders,
Sum(orders.Price_usd) as Revenue,
Sum(orders.cogs_usd) as Cost_Of_Goods_Sold,
Sum(orders.Price_usd) - Sum(orders.cogs_usd) as Operating_Profit,
Count(order_item_refunds.order_id) as Customer_Returns
from orders
Left Join Order_item_refunds on order_item_refunds.order_id = orders.order_id
where orders.created_at >= 2014-12-31
group by 1, 2
)
as Final_Analysis
Group By Years, Quarters;

Select
Year(website_sessions. created_at) as Years,
Quarter(website_sessions. created_at) as Quarters,
Count(Distinct website_sessions.website_session_id) as Direct_Type_In_Sessions,
Count(Distinct orders.website_session_id) as Direct_Type_In_Orders,
Concat(Round(Count(Distinct orders.website_session_id) / Count(Distinct website_sessions.website_session_id) * 100, 2), '%') as Direct_Type_In_Sessions_Order_Rt
from website_sessions
Left Join orders on orders.website_session_id = website_sessions.website_session_id
where website_sessions.created_at >= 2015-03-20
and utm_source is null
and utm_campaign is null
and http_referer is null
Group By 1, 2;
```

![Logo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/2ox0w9na5bgla6z9wga2.png)

![Logo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/gn4k6ymjxblpn5tkledb.png)


## Acknowledgements

 - [Maven Analytics: Project Platform](https://mavenanalytics.io/)

