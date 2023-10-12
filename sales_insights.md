/\* Style for SQL code \*/ .sql-code { font-family: 'Courier New', monospace; font-size: 16px; background-color: #f4f4f4; padding: 10px; border: 1px solid #ccc; border-radius: 5px; margin-bottom: 10px; overflow-x: auto; } /\* Style for SQL results table \*/ .sql-results { font-family: Arial, sans-serif; border-collapse: collapse; width: 100%; font-size: 12px; } .sql-results th, .sql-results td { border: 1px solid #ddd; padding: 8px; text-align: left; text-align: center; } .sql-results tr:nth-child(even) { background-color: #f2f2f2; } .sql-results th { background-color: #bdbeca; color: white; } /\* Style for SQL keywords \*/ .sql-keyword { color: #007acc; /\* Color for SQL keywords \*/ font-weight: bold; /\* Make keywords bold \*/ } .medium{ background-color: silver; } .col\_blue{ color: #007acc; } /\*Button to go back at top of page\*/ #myBtn { display: none; /\* Hidden by default \*/ position: fixed; /\* Fixed/sticky position \*/ bottom: 30px; /\* Place the button at the bottom of the page \*/ right: 50px; /\* Place the button 30px from the right \*/ z-index: 99; /\* Make sure it does not overlap \*/ border: none; /\* Remove borders \*/ outline: none; /\* Remove outline \*/ background-color: #007acc; /\* Set a background color \*/ color: white; /\* Text color \*/ cursor: pointer; /\* Add a mouse pointer on hover \*/ padding: 7px; /\* Some padding \*/ border-radius: 10px; /\* Rounded corners \*/ font-size: 18px; /\* Increase font size \*/ } #myBtn:hover { background-color: #555; /\* Add a dark-grey background on hover \*/ } Back top // Get the button: let mybutton = document.getElementById("myBtn"); // When the user scrolls down 20px from the top of the document, show the button window.onscroll = function() {scrollFunction()}; function scrollFunction() { if (document.body.scrollTop > 20 || document.documentElement.scrollTop > 20) { mybutton.style.display = "block"; } else { mybutton.style.display = "none"; } } // When the user clicks on the button, scroll to the top of the document function topFunction() { document.body.scrollTop = 0; // For Safari document.documentElement.scrollTop = 0; // For Chrome, Firefox, IE and Opera }

Sales Insights
==============

### A sales dashboard displaying several years of a company performance

This project is a sales insight on a mock data set from a hardware company. In this project, we try to analyze the sales revenue and profit evolution in various regions of India and for different customers over several years.

Summary
=======

*   [Overview of the Data and Challenges](#C1)

*   [Dataset](#dataset)
*   [Business Needs](#business_needs)
*   [Exploratory Data Analysis and Cleaning](#eda)

*   [Customers](#customers)
*   [Date](#date)
*   [Markets](#markets)
*   [Products](#products)
*   [Transactions](#transactions)

*   [Analysis](#analysis)

*   [Key Performance Indicator](#kpi)
*   [Dashboard](#dashboard)
*   [Business Insights](#BI)
*   [Final Remarks](#final)

  

Overview of the Data and challenges
===================================

  

### Dataset

We have a dataset from Haardaveyar, a copmany which supplies computer hardware and peripherals to many clients in India. The dataset contains 5 tables: **customers**, **date**, **markets**, **products** and **transactions**  
Here is a small description of each of them:

*   **customers**: it contains the identifier of the customers, their names, and a categorical value being either 'Brick & Mortar' or 'E-Commerce'
*   **date**: it contains several declinations of the date. The whole date and its 3 granularity (date, month, year), and a cy date column representing the 1st of each month for business purposes.
*   **markets**: it contains the unique market identifiers with their associated name and a zone column, the location in the country
*   **products**: it contains the identifiers for each products. For each of them there is a category 'Own Brand' or 'Distribution' associated
*   **transactions**: 10 columns that recap the whole information of each transaction, from the customer, market and product IDs, to the sales amount, profit and cost

### Business needs

Our client is a head of the sales team. He would like to get a dashboard to have an overview of the current state of the company sales. He wants to know in which market they should be invest on. Which products sell the most. Which products profits the company the most. Which customers should get our attention.

  

### Exploratory Data Analysis and Cleaning

First, let's take a closer look at our tables.

##### Customers

SELECT \* 
FROM customers 

customer\_code

custmer\_name

customer\_type

Cus001

Surge Stores

Brick & Mortar

Cus002

Nomad Stores

Brick & Mortar

...

Cus037

Propel

E-Commerce

Cus038

Leader

E-Commerce

  

SELECT c.customer\_type AS Customer\_type,
	count(c.customer\_type) AS Nb\_customer
FROM customers c
GROUP BY c.customer\_type 

Customer\_type

Nb\_customer

Brick & Mortar

19

E-commerce

19

There are 38 customers, out of which half sell through internet and the other hald in local stores.

##### Date

SELECT \*
FROM date 

date

cy\_date

year

month\_name

date\_yy\_mmm

2017-06-01

2017-06-01

2017

June

17-Jun

2017-06-02

2017-06-01

2017

June

17-Jun

...

2020-06-30

2020-06-01

2020

June

20-Jun

  

SELECT MIN(d.date) AS  First\_date,
	MAX(d.date) AS  Last\_date
FROM date d

First\_date

Last\_date

2017-06-01

2020-06-30

Our dataset contains 3 years of transactions, made between June 2017 and June 2020.

##### Markets

SELECT \*
FROM markets 

markets\_code

markets\_name

zone

Mark001

Chennai

South

Mark002

Mumbai

Central

Mark015

Bhubaneshwar

South

Mark097

New York

Mark999

Paris

The last 2 rows of this table don't show any _zone_. Our dataset is supposed to showcase sales which occured in India, so there should be neither Paris, nor New York. We can check if there are any transactions that involved one of these cities.

SELECT \*
FROM transactions t
WHERE t.market\_code = 'Mark097' or t.market\_code = 'Mark999'

product\_code

customer\_code

market\_code

order\_date

sales\_qty

sales\_amount

currency

There are no transactions involving any of these 2 cities. Therefore, we can safely remove them from our dataset.

SELECT m.zone AS Zone,
	count(m.zone) AS Zone\_count,
FROM markets m 
FROM Zone\_count  DESC

Zone

Zone\_count

North

6

South

5

Central

4

There are 15 market regions devided in 3 zones, with markets located mostly in North, then South then in the central area of India.

##### Products

SELECT \*
FROM products 

product\_code

product\_type

Prod001

Own Brand

...

...

Prod279

Distribution

  

SELECT p.product\_type AS Product\_type,
	count(p.product\_type) AS Count\_product\_type
FROM products p
GROUP BY Product\_type 

Product\_type

Count\_product\_type

Own Brand

191

Distribution

88

There are 279 products, out of which about 2/3 are of Own Brand type, and the remaing 1/3 are Distribution products.

##### Transactions

SELECT \*
FROM transactions 

product\_code

customer\_code

market\_code

order\_date

sales\_qty

sales\_amount

currency

Prod001

Cus001

Mark001

2017-10-10

100

41241

INR

Prod001

Cus002

Mark002

2018-05-08

3

\-1

INR

...

Prod018

Cus022

Mark002

2019-02-18

3

6125

INR

Prod018

Cus025

Mark002

2019-02-18

8

17495

INR

  

SELECT count(\*) AS Nb\_transactions
FROM transactions 

Nb\_transactions

150283

Here we have more than 150000 transactions. As we can see in the transactions table, there's a row that contain a -1. This should not happen as one should not be able to buy a produce of negative price.

SELECT distinct(t.sales\_amount) AS Distinct\_sales\_amount
FROM transactions t
ORDER BY Distinct\_sales\_amount ASC

Distinct\_sales\_amount

\-1

0

5

9

14

...

As we can see, there are amounts of 0 and -1. This should not happen, I will remove these transactions

SELECT distinct(t.sales\_qty) AS Distinct\_sales\_qty
FROM transactions t
ORDER BY Distinct\_sales\_qty ASC

Distinct\_sales\_amount

1

2

3

...

The issue does not arise for sales quantity.

SELECT \* 
FROM transactions t
WHERE t.sales\_amount = 0 OR t.sales\_amount = -1
ORDER BY t.sales\_amount ASC

product\_code

customer\_code

market\_code

order\_date

sales\_qty

sales\_amount

currency

Prod001

Cus002

Mark002

2018-05-08

3

\-1

INR

Prod001

Cus002

Mark002

2018-05-08

3

\-1

INR

Prod010

Cus015

Mark006

2018-05-26

1

0

INR

...

Prod265

Cus029

Mark011

2018-12-11

1

0

INR

There are 2 transactions of value -1. It is actually a duplicate of the same transaction. The remaining transactions with value 0 could either be some kind of mistake from an employee, a gift, or any kind of error in registering the transaction. As we don't have more information on this, I will remove them all.

SELECT count(\*) AS Nb\_outliers
FROM transactions t
WHERE t.sales\_amount = 0 OR t.sales\_amount = -1

Nb\_outliers

1611

SELECT t.currency AS Currency,
	count(t.currency) AS Count\_currency
FROM transactions t
GROUP BY Currency 

Currency

Count\_currency

INR

148393

USD

2

  

SELECT \*
FROM transactions t
WHERE t.currency = 'USD'

product\_code

customer\_code

market\_code

order\_date

sales\_qty

sales\_amount

currency

profit\_margin\_percentage

profit\_margin

cost\_price

Prod003

Cus005

Mark004

2017-11-20

59

500

USD

0.31

11625

25875

Prod003

Cus005

Mark004

2017-11-22

36

250

USD

0.17

3187.5

15562.5

In this case, since there are only 2 transactions that have been done in USD, we can either treat it as an outlier, compared to the huge amount of transactions made in Indian Rupees, and remove it. Or we can just change the currency at the transaction date's exchange rate and keep the record. In this case, I kept the record.

Analysis
========

  

### Key Performance Indicator

To satisfy our customer needs, it would be great to have measures that efficiently express the evolution of the different markets and products sales. To do so, we can create 3 performance indicators: the Revenue, the Profit Margin Percentage, and the Percentage Profit Margin.

##### Revenue

The Revenue consists of the raw total income, which is the sum of the _sales\_amount_. With this variable, we can observe the size and importance of different markets, customers or products over different time frames.

Here is the DAX formula to create the variable in Power BI:

Revenue =  
SUM ( 'sales transactions'\[sales\_amount\] )

##### Profit Margin %

The Profit Margin % is the percentage of profit among the total revenue.

Here is the DAX formula to create the variable in Power BI:

Profit Margin % =  
DIVIDE (  
    SUM ( 'sales transactions'\[profit\_margin\] ),  
    SUM ( 'sales transactions'\[sales\_amount\] ),  
    0  
)

##### Profit Margin Contribution %

The Profit Margin Contribution % is the relative profit of a given market, producer or customer, accross all the others.

Here is the DAX formula to create the variable in Power BI:

Profit Margin Contribution % =  
DIVIDE (  
    \[Total Profit Margin\],  
    CALCULATE (  
        SUM ( 'sales transactions'\[profit\_margin\] ),  
        ALL ( 'sales products' ),  
        ALL ( 'sales customers' ),  
        ALL ( 'sales markets' )  
    )  
)

### Dashboard

Using Power BI, I was able to create the following dashboard. It displays our 3 KPIs in various plots. We can change the granularity at many different levels: year, sales type or the market contributor. This first dashboard shows the overall trend and accomplishment over the 4 years of focus.

![](/img/posts/sales/dashboard/sales_dashboard_overall_markets.jpg)

Figure 1 - Overall dashboard with focus on Markets

From this first plot, we can see some overall statistics: the total revenue, profit and number of sales on the first line. The middle part of the dashboard present the revenue trend over the 4 years, with a pie chart presenting the amount of e-commerce vs local brick & mortar. Finally, the bar charts at the bottom show the contribution of each market in the dataset in revenue and profits. The latter can be changed to observe the contribution of the different customers and products as well. This can be very informative and suggestive in the markets, customers or products the company should be focusing on.

![](/img/posts/sales/dashboard/sales_dashboard_2019_customer_page-0001.jpg)

Figure 2 - Overall dashboard with focus on Customers

This second figure shows the relative importance of different customers. We can see that Electricalsara Stores is one of the most important customer given the revenue and profit we generate from them. Looking at the top customers in each bar charts allows us to drive deeper insights.

![](/img/posts/sales/dashboard/electricalsara_stores.jpg)![](/img/posts/sales/dashboard/top5_revenue_wo_number1.jpg)![](/img/posts/sales/dashboard/top7_profit_perc.jpg)

Figure 3/4/5 - Focus on Electricalsara Stores / Focus on top 6 Revenue without Electricalsara / Focus on top 7 Profit Margin %

Looking at various variation of the dashboard, we can get really interesting insights.  
On the first figure, we can see the importance of the Electricalsara Stores as the main source of Revenue and Profit of our company. However, we can see through the third bar chart that the average profit % is quite low, meaning that the hardware company generates overall a good amount of money over a huge number of transactions with low profits.  
The second figure shows that the next 5 highest revenue contributors' customers have various proportion of contribution towards global profit. We can see that Nixon and Electricalslytical are 2 very great customers with which our company makes a significant amount of transactions with fairly high profit.  
The last of the 3 figures show which markets make the most profit out of each transactions. Since this is a relative number, top customers in this category present various impacts in revenue. Again, Nixon is present in all top parts of the charts. Leader and Electricalsocity are also customers that present high numbers in revenue and profit.

### Business Insights

Many other insights can be made by looking at other variations of this dashboard. Since I cannot have it interactive here, I have printed some records over [here](https://github.com/ReneDCDSL/_PERSONAL-PROJECTS)  
Here are some high-level insights I would give my client with the appropriate dashboard illustrations: Note: to showcase the performance, I will write in braces the KPI: **\[% Revenue contribution/% Profit contribution/% Avg Profit per transaction\]**

###### Overall

  

###### Main customer: Electricalsara Stores

*   From 2017 to 2019, **Electricalsara Stores** have been the main contribution to the overall profit of the company, with more than a 3rd of the overall profit contribution each year. This is your main client with which the company is the most profitable and generates the most revenue. A small dip in mid 2020 is to be investigated.
  

###### Markets

*   **New Delhi, Mumbai, Ahmedabad, Bhopal and Nagpur** have always been the same top 5 markets in both revenue and profit contribution (5%-50%). These are the strongest markets.  
    **Kochi, Chennai, Patna and Surat** are the main follow-up with relative profit contribution (0.5%-3%) and relatively high average profit per transaction (1.7%-4.9%). These markets have been showing great performance in early 2020, except for Surat, with very high profits both in contribution and transaction. This is to be encouraged.
  

###### E-commerce status

*   Apart from Electricalsara Stores which are full Brick & Mortar, customers relying on E-commerce have been giving our company the most profitable transactions. These highly profitable transactions gives the company leverage to be taken advantage of (investing in more ads, promotions..)

  

###### 2019: previous year performance

  

###### Customers

*   ###### Leader, Control, Logic Stores
    
    **Leader \[1.9/6.7/11.1\], Control \[3/4.5/4.7\] and Logic Stores \[1.7/3.4/6.1\]** all have high average profit per transaction (> 4.5% each) in 2019. Each of these stores managed to have a transaction average profit of over 4.5%. These customers performed well last year, this performance should be encouraged.

  

###### 2020: current trend

  

###### Tier A products: 083, 070, 206, 308

*   In the beginning of 2020, products **083 \[0.6/9.8/25.7\], 070 \[0.5/7.6/22.7\], 206 \[0.5/7.7/21\] and 308 \[1.5/14.8/14.2\]** have been the star products. With a combined profit contribution of 39.9% despite their 3.1% cumulated revenue, and individual average profit of > 14% per transactions, these products have been selling very well.
  

###### Tier B products: 324, 159, 313, 332, 201, 318

*   These products have had a higher share of the revenue (0.7%-1.2%) in the beginning of 2020, but with a lesser profit contribution (4.9%-6.4%) and profit per transaction (6%-12.7%), except for product **324 \[3.5/7.9/3.3\]** which has high profit and revenue contribution rates but with a lower profit per transaction of 3.3%. These products have had very great performance in this first half of the year, I put them in this second, yet very good, tier as their relatively lower profit rates gives us less leverage to promote them.
  

###### Market: Hyderabad

*   The **Hyderabad** market has had a rough start in 2017 **\[1/-1/-3.1\]**, keeping low profile in 2018 **\[0.6/0.4/1.5\]**, before deeping down again in 2019 **\[0.8/-0.4/-1.5\]** but have rosen back up in early 2020 **\[0.8/3.9/6.7\]**, with a record 0.55M in revenue in March. A focus on this case is advised.

### Final remarks

This dashboard is a simple yet informative version of the company sales state. It displays revenue and profit metrics, sales trend and the top markets, customers or products at any given time. There are still many other metrics, combinations of tables or charts that could be added.