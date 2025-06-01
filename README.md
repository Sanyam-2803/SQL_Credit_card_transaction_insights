# Credit Card Transaction Insights Project using SQL

📌 Project Overview

A SQL based Project to analyse Credit card spending habits in India by identifying consumer trends and interests by looking at the type of purchases people make based on their gender and city.


🧰 Tools used 

Excel, MySQL Server & Canva

📌 Advanced SQL concepts

CTEs, window functions, Date & Time
functions, aggregation, filtering

📊 Dataset Overview  

Input dataset offers a comprehensive look at the spending habits of around 29k customers across the nation.

Transaction table contains field such as transaction _id (int) ,city  (string), transaction_date (string), card_type (string),exp_type (string), Gender (string) , Amount (int)


🎯 Key Business SQL queries/Level-Advanced


𝗧𝗼𝗽 𝟱 𝗰𝗶𝘁𝗶𝗲𝘀 𝘄𝗶𝘁𝗵 𝗵𝗶𝗴𝗵𝗲𝘀𝘁 𝘀𝗽𝗲𝗻𝗱𝘀 𝗮𝗻𝗱 𝘁𝗵𝗲𝗶𝗿 𝗽𝗲𝗿𝗰𝗲𝗻𝘁𝗮𝗴𝗲 𝗰𝗼𝗻𝘁𝗿𝗶𝗯𝘂𝘁𝗶𝗼𝗻 𝗼𝗳 𝘁𝗼𝘁𝗮𝗹 𝗰𝗿𝗲𝗱𝗶𝘁 𝗰𝗮𝗿𝗱 𝘀𝗽𝗲𝗻𝗱𝘀

WITH citywise_spend as (SELECT city,sum(amount) as total_amount 
FROM credit_card_transactions.data
GROUP BY city),
total_spend as (SELECT SUM(amount) as total_spend FROM credit_card_transactions.data)

SELECT c.*,ROUND((total_amount/total_spend)*100,1)as percentage_contribution FROM citywise_spend c INNER JOIN total_spend t ON 1=1
ORDER BY total_amount desc
LIMIT 5 ;


𝗛𝗶𝗴𝗵𝗲𝘀𝘁 𝘀𝗽𝗲𝗻𝗱 𝗺𝗼𝗻𝘁𝗵 𝗮𝗻𝗱 𝗮𝗺𝗼𝘂𝗻𝘁 𝘀𝗽𝗲𝗻𝘁 𝗶𝗻 𝘁𝗵𝗮𝘁 𝗺𝗼𝗻𝘁𝗵 𝗳𝗼𝗿 𝗲𝗮𝗰𝗵 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲

WITH cte1 as (SELECT card_type,MONTH(str_to_date(transaction_date,"%d-%M-%y")) as month,YEAR(str_to_date(transaction_date,"%d-%M-%y"))as year,SUM(amount) as total_amount 
FROM credit_card_transactions.data
GROUP BY month,year,card_type
ORDER BY card_type,total_amount desc)

SELECT A.card_type, A.month,A.year,A.Total_amount 
FROM (SELECT cte1.*,dense_rank() OVER (PARTITION BY card_type ORDER BY total_amount desc) as rnk FROM cte1) A
WHERE rnk=1 ;

𝗧𝗿𝗮𝗻𝘀𝗮𝗰𝘁𝗶𝗼𝗻 𝗱𝗲𝘁𝗮𝗶𝗹𝘀(𝗮𝗹𝗹 𝗰𝗼𝗹𝘂𝗺𝗻𝘀 𝗳𝗿𝗼𝗺 𝘁𝗵𝗲 𝘁𝗮𝗯𝗹𝗲) 𝗳𝗼𝗿 𝗲𝗮𝗰𝗵 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲 𝘄𝗵𝗲𝗻 𝗶𝘁 𝗿𝗲𝗮𝗰𝗵𝗲𝘀 𝗮 𝗰𝘂𝗺𝘂𝗹𝗮𝘁𝗶𝘃𝗲 𝗼𝗳 𝟭𝟬,𝟬𝟬𝟬𝟬𝟬 𝘁𝗼𝘁𝗮𝗹 𝘀𝗽𝗲𝗻𝗱𝘀 (𝗪𝗲 𝘀𝗵𝗼𝘂𝗹𝗱 𝗵𝗮𝘃𝗲 𝟰 𝗿𝗼𝘄𝘀 𝗶𝗻 𝘁𝗵𝗲 𝗼𝘂𝘁𝗽𝘂𝘁 𝗼𝗻𝗲 𝗳𝗼𝗿 𝗲𝗮𝗰𝗵 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲 )

WITH cte1 as ( SELECT *,SUM(amount)
OVER( PARTITION BY card_type ORDER BY str_to_date(transaction_date,"%d-%M-%y"),transaction_id ) as running_spend 
FROM credit_card_transactions.data )

SELECT transaction_id,city,str_to_date(transaction_date,"%d-%M-%y") as transaction_date,card_type FROM 
( SELECT *,row_number() OVER  (PARTITION BY card_type ORDER BY running_spend) as rw
FROM cte1 
WHERE running_spend>=1000000 )A
WHERE rw=1 ;

𝗖𝗶𝘁𝘆 𝘄𝗵𝗶𝗰𝗵 𝗵𝗮𝗱 𝗹𝗼𝘄𝗲𝘀𝘁 𝗽𝗲𝗿𝗰𝗲𝗻𝘁𝗮𝗴𝗲 𝘀𝗽𝗲𝗻𝗱 𝗳𝗼𝗿 𝗴𝗼𝗹𝗱 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲


