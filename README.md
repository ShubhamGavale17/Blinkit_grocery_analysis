1. Create table & Import data into Sql.
   
CREATE TABLE blinkit (
  Item_Fat_Content Varchar(26),
  Item _Identifier Varchar2(26),
  Item_Type varchar(30)
Outlet_Establishment_Year year,
  Outlet Identifier varchar(20),
  Outlet_Location_Type varchar(30),
  Outlet_Size varchar(30),
  Outlet_Type varchar(30),
  Item_Visibility number(10,8),
  Item_Weight number(3,3),
  Rating number(5),
  Sales number(3,5)
);

2. Load into SQL and clean

Q1. Check for inconsistent fat content labels?
Ans - select distinct item_fat_content from blinkit;

Q2. Standardise: 'LF' and 'low fat' → 'Low Fat'?
Ans - update blinkit set item_fat_content = 'Low fat' where item_fat_content in ('LF','Low fat');

3. KPI queries.

Q1. Total Sales Revenue?

Ans- select round(sum(sales),2)as total_sales
from blinkit;

Q2. Average Sales per item?

Ans - select round(avg(sales),2) as avg_sales
from blinkit;

Q3. Average customer rating?

Ans- select round(avg(rating),2) as avg_rating
from blinkit;

Q4. Total number of items

Ans - Select count(*)as total_items from blinkit;


Q5. Sales by Fat Content — do customers prefer Low Fat?

Ans - select item_fat_content,
round(sum(sales),2) as total_sales,
round(avg(rating),2) as avg_rating
from blinkit
group by item_fat_content
order by total_sales desc;

Q6. Which item types generate the most revenue?

Ans- select item_type,
sum(sales) as total_sales,
count(*) as item_count
from blinkit
group by item_type
order by total_sales desc;

Q7. Sales performance by outlet size?

Ans- select outlet_size,
sum(sales) as total_sales,
count(*) as outlet_count,
avg(sales) as avg_sales
from blinkit
group by outlet_size
order by total_sales;


Q8. Which location tier performs best?

Ans - select outlet_location_type,
sum(sales) as total_sales,
avg(rating)as best_performance
from blinkit
group by outlet_location_type
order by best_performance desc;

Q9. Sales by outlet type?

Ans - select outlet_type,
sum(sales) as total_sales,
count(*)as item_count,
avg(sales) as avg_Sales
from blinkit
group by outlet_type
order by total_Sales desc;

Q10. Best performing outlet type + location combo?

Ans - select outlet_location_type,outlet_type from
(select outlet_location_type, outlet_type ,sum(sales) as total_sales
from blinkit
group by outlet_location_type,outlet_type
order by total_sales desc)
where rownum <=10;

Q11. find total sales per item_type?

Ans - select item_type,
sum(sales) as total_Sales,
count(*) as Item
from blinkit
group by item_type
order by total_sales desc;

