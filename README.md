<h1>Analysis of Forbes Billionaires Dataset</h1>
<h2> 
♦️Analysis made in SQL code in PostgreSQL RDBMS. <br>
♦️Imported from .csv file <br>
♦️Years analyzed are 2021-2023 <br>  
♦️Dataset downloaded from Kaggle <br> (https://www.kaggle.com/datasets/guillemservera/forbes-billionaires-1997-2023) <br> </h2> 

<h2> ☑️Creating table <br> </h2>

CREATE TABLE IF NOT EXISTS public.billionaires <br>
( <br>
    year integer, <br>
    month integer,<br>
    rank integer,<br>
    net_worth text COLLATE pg_catalog."default",<br>
    last_name text COLLATE pg_catalog."default",<br>
    first_name text COLLATE pg_catalog."default",<br>
    full_name text COLLATE pg_catalog."default",<br>
    birth_date date,<br>
    age integer,<br>
    gender text COLLATE pg_catalog."default",<br>
    country_of_citizenship text COLLATE pg_catalog."default",<br>
    country_of_residence text COLLATE pg_catalog."default",<br>
    city_of_residence text COLLATE pg_catalog."default",<br>
    business_category text COLLATE pg_catalog."default",<br>
    business_industries text COLLATE pg_catalog."default",<br>
    organization_name text COLLATE pg_catalog."default",<br>
    position_in_organization text COLLATE pg_catalog."default",<br>
    self_made boolean,
    wealth_status text COLLATE pg_catalog."default"<br>
) <br>
![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/9ab73a5b-ecbd-4ec4-a1e6-434e2181cb15)



<h2> ☑️What is the biggest net worth of the dataset? <br> </h2>

select year,full_name, age, net_worth,replace(substring(net_worth,1,3),'.','')::integer as net_worth_int from billionaires<br>
order by net_worth_int desc<br>
limit 1<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/8df25b13-727b-46c1-a3e0-ac096e9788b6)

 
<h2> ☑️Which business category has most billionaires?<br> </h2>

select business_category, count(business_category) as business_count from billionaires<br>
group by business_category<br>
order by business_count desc<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/eaf9c95a-e94a-413d-8ba6-b5bd9229e108)


<h2> ☑️Which year had the most billionaires?<br> </h2>

select distinct year, count(full_name) from billionaires<br>
group by year<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/0f9f8c6f-88a5-4278-970f-e850f426f771)


<h2> ☑️Which countries have the most billionaires?<br></h2>

select country_of_citizenship, count(country_of_citizenship) as count_citizenship, count(country_of_residence) as count_residence from billionaires <br>
group by country_of_citizenship<br>
order by count_citizenship desc, count_residence desc<br>
limit 10<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/ccad65a1-fcd9-4050-ad65-7ada4206fe71)


<h2> ☑️Which cities have the most billionaires resided?<br></h2>

select city_of_residence,count(city_of_residence) as billionaires_count from billionaires<br>
group by city_of_residence <br>
order by billionaires_count desc<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/87081ce3-bfa7-45d3-9ca0-27ac64d27df5)


<h2> ☑️How many of all billionaires self-made?<br></h2>

select count(*) as count, 'true (self-made)' as type from billionaires<br>
where self_made = true<br>
union all<br>
select count(full_name) as count, 'false (not self-made)' as type from billionaires<br>
where self_made = false<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/cb89b7c7-9f56-4423-bb2f-8508b7a11f48)


<h2> ☑️Analysis of the ages group (18-26, 27-35, 36-44, 45-53, 53+)<br></h2>

select '18-26 years' as age, avg(cast(replace(left(net_worth,3),'.','') as int))::integer as average_net_worth_in_billions from billionaires<br>
where age between 18 and 26<br>
    union<br>
select '27-35 years', avg(cast(replace(left(net_worth,3),'.','') as int))::integer from billionaires<br>
where age between 27 and 35<br>
    union<br>
select '36-44 years', avg(cast(replace(left(net_worth,3),'.','') as int))::integer from billionaires<br>
where age between 36 and 44<br>
    union<br>
select '45-53 years', avg(cast(replace(left(net_worth,3),'.','') as int))::integer from billionaires<br>
where age between 45 and 53<br>
    union<br>
select '53+ years', avg(cast(replace(left(net_worth,3),'.','') as int))::integer from billionaires<br>
where age > 53<br>
order by age<br>

![image](https://github.com/hubost/SQL_Forbes-Billionaires/assets/103451733/fc8c227b-2363-4a0b-93a6-58ea1936feb9)

