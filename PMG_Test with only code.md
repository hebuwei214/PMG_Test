*  Question #1

select sum(clicks) as total_clicks

From marketing_data


*  Question #2

select store_location, sum(revenue) as revenue

from store_revenue 

group by store_location


*  Question #3

with cte as(

select *,right(store_location,2) as bridge

from store_revenue)

select geo,cte.date, avg(impressions) as impressions, avg(clicks) as clicks, sum(revenue) as total_revenue

from cte left join marketing_data md on cte.bridge=md.geo and cte.date=md.date

where cte.date is not null and geo is not null

group by geo,cte.date

* Question #4


Since there is no background description, I assume we are analyzing marketing data for E-commerce. In addition, there is no cost of impressions or clicks, so I assume the cost per click and cost per impression is the same for different states, or we are marketing on social media like Instagram at no expense. 

With these assumptions, I can use revenue per click and revenue per impression as metrics for calculating the efficiency of each store. I get revenue per impression for the store in CA is 10.42, which is much higher than in NY(2.59) and TX(0.57). And revenue per click for the store in CA is 758, which is also much higher than in NY(214) and TX(40). In conclusion, the store in CA is the most efficient.


Following is the SQL Query for Question #4


with cte as(

select *,right(store_location,2) as bridge

from store_revenue),

cte2 as(

select geo,cte.date, avg(impressions) as impressions, avg(clicks) as clicks, sum(revenue) as total_revenue

from cte left join marketing_data md on cte.bridge=md.geo and cte.date=md.date

where cte.date is not null and geo is not null

group by geo,cte.date

)

select geo,sum(total_revenue)/sum(impressions) as revenue_per_impression, sum(total_revenue)/sum(clicks) as revenue_per_clicks, sum(total_revenue) as total_revenue

from cte2

group by geo



* Question #5 (Challenge)

with cte as(

select store_location, sum(revenue) as total_revenue

from store_revenue

group by store_location 

order by total_revenue desc

limit 10

)

select right(store_location,2) as Top10_revenue_producing_states

from cte
​
