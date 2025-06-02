# Credit Card Transaction Insights Project using SQL

Domain : Banking 🏦 

📌 ℙ𝕣𝕠𝕛𝕖𝕔𝕥 𝕆𝕧𝕖𝕣𝕧𝕚𝕖𝕨

A SQL based Project to analyse Credit card spending habits in India by identifying consumer trends and interests by looking at the type of purchases people make based on their gender and city.


⚙️ 𝕋𝕠𝕠𝕝𝕤 𝕦𝕤𝕖𝕕 

Excel, MySQL Server & Canva

📌 𝔸𝕕𝕧𝕒𝕟𝕔𝕖𝕕 𝕊ℚ𝕃 𝕔𝕠𝕟𝕔𝕖𝕡𝕥𝕤

CTEs, window functions, Date & Time
functions, aggregation, filtering

📊 𝔻𝕒𝕥𝕒𝕤𝕖𝕥 𝕆𝕧𝕖𝕣𝕧𝕚𝕖𝕨  

Input dataset offers a comprehensive look at the spending habits of around 29k customers across the nation.

Transaction table contains field such as transaction _id (int) ,city  (string), transaction_date (string), card_type (string),exp_type (string), Gender (string) , Amount (int)


🎯 𝕂𝕖𝕪 𝔹𝕦𝕤𝕚𝕟𝕖𝕤𝕤 𝕊ℚ𝕃 𝕢𝕦𝕖𝕣𝕚𝕖𝕤 / 𝕃𝕖𝕧𝕖𝕝-𝔸𝕕𝕧𝕒𝕟𝕔𝕖𝕕  


🔶 𝗧𝗼𝗽 𝟱 𝗰𝗶𝘁𝗶𝗲𝘀 𝘄𝗶𝘁𝗵 𝗵𝗶𝗴𝗵𝗲𝘀𝘁 𝘀𝗽𝗲𝗻𝗱𝘀 𝗮𝗻𝗱 𝘁𝗵𝗲𝗶𝗿 𝗽𝗲𝗿𝗰𝗲𝗻𝘁𝗮𝗴𝗲 𝗰𝗼𝗻𝘁𝗿𝗶𝗯𝘂𝘁𝗶𝗼𝗻 𝗼𝗳 𝘁𝗼𝘁𝗮𝗹 𝗰𝗿𝗲𝗱𝗶𝘁 𝗰𝗮𝗿𝗱 𝘀𝗽𝗲𝗻𝗱𝘀

WITH citywise_spend as ( SELECT city,sum(amount) as total_amount 
FROM credit_card_transactions.data
GROUP BY city ),
total_spend as ( SELECT SUM(amount) as total_spend FROM credit_card_transactions.data )

SELECT c.*,ROUND ((total_amount/total_spend)*100,1) as percentage_contribution FROM citywise_spend c INNER JOIN total_spend t ON 1=1
ORDER BY total_amount desc
LIMIT 5 ;

![1](https://github.com/user-attachments/assets/3cca26df-1c45-4bc7-a0fc-bb82fde770cf)



🔶 𝗛𝗶𝗴𝗵𝗲𝘀𝘁 𝘀𝗽𝗲𝗻𝗱 𝗺𝗼𝗻𝘁𝗵 𝗮𝗻𝗱 𝗮𝗺𝗼𝘂𝗻𝘁 𝘀𝗽𝗲𝗻𝘁 𝗶𝗻 𝘁𝗵𝗮𝘁 𝗺𝗼𝗻𝘁𝗵 𝗳𝗼𝗿 𝗲𝗮𝗰𝗵 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲

WITH cte1 as ( SELECT card_type,MONTH(str_to_date (transaction_date,"%d-%M-%y")) as month,YEAR (str_to_date(transaction_date,"%d-%M-%y"))as year,SUM (amount) as total_amount 
FROM credit_card_transactions.data
GROUP BY month,year,card_type
ORDER BY card_type,total_amount DESC )

SELECT A.card_type, A.month,A.year,A.Total_amount 
FROM ( SELECT cte1.*,dense_rank() OVER ( PARTITION BY card_type ORDER BY total_amount desc) as rnk FROM cte1 ) A
WHERE rnk=1 ;


![2](https://github.com/user-attachments/assets/a085a315-b887-462d-ae61-3fb68079384d)



🔶 𝗧𝗿𝗮𝗻𝘀𝗮𝗰𝘁𝗶𝗼𝗻 𝗱𝗲𝘁𝗮𝗶𝗹𝘀 (𝗮𝗹𝗹 𝗰𝗼𝗹𝘂𝗺𝗻𝘀 𝗳𝗿𝗼𝗺 𝘁𝗵𝗲 𝘁𝗮𝗯𝗹𝗲 ) 𝗳𝗼𝗿 𝗲𝗮𝗰𝗵 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲 𝘄𝗵𝗲𝗻 𝗶𝘁 𝗿𝗲𝗮𝗰𝗵𝗲𝘀 𝗮 𝗰𝘂𝗺𝘂𝗹𝗮𝘁𝗶𝘃𝗲 𝗼𝗳 𝟭𝟬,𝟬𝟬𝟬𝟬𝟬 𝘁𝗼𝘁𝗮𝗹 𝘀𝗽𝗲𝗻𝗱𝘀 ( 𝗪𝗲 𝘀𝗵𝗼𝘂𝗹𝗱 𝗵𝗮𝘃𝗲 𝟰 𝗿𝗼𝘄𝘀 𝗶𝗻 𝘁𝗵𝗲 𝗼𝘂𝘁𝗽𝘂𝘁 𝗼𝗻𝗲 𝗳𝗼𝗿 𝗲𝗮𝗰𝗵 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲 )

WITH cte1 as ( SELECT *,SUM (amount)
OVER ( PARTITION BY card_type ORDER BY str_to_date (transaction_date,"%d-%M-%y") ,transaction_id ) as running_spend 
FROM credit_card_transactions.data )

SELECT transaction_id,city,str_to_date( transaction_date,"%d-%M-%y") as transaction_date,card_type FROM 
( SELECT *,row_number () OVER  ( PARTITION BY card_type ORDER BY running_spend ) as rw
FROM cte1 
WHERE running_spend>=1000000 )A
WHERE rw=1 ;


![3](https://github.com/user-attachments/assets/6716b9da-97a2-4df7-a1f1-50e3f439c92d)


🔶 𝗖𝗶𝘁𝘆 𝘄𝗵𝗶𝗰𝗵 𝗵𝗮𝗱 𝗹𝗼𝘄𝗲𝘀𝘁 𝗽𝗲𝗿𝗰𝗲𝗻𝘁𝗮𝗴𝗲 𝘀𝗽𝗲𝗻𝗱 𝗳𝗼𝗿 𝗴𝗼𝗹𝗱 𝗰𝗮𝗿𝗱 𝘁𝘆𝗽𝗲

WITH cte as (SELECT city,card_type,SUM(amount)as amt,SUM(CASE WHEN card_type ="Gold" THEN amount END)as gold_amt 
FROM credit_card_transactions.data
GROUP BY city,card_type)
,cte1 as (SELECT city,ROUND((SUM(gold_amt)/SUM(amt))*100,2)as percentage_goldamount FROM cte 
GROUP BY city)

SELECT * FROM cte1
WHERE percentage_goldamount IS NOT NULL
ORDER BY percentage_goldamount ASC
LIMIT 1;

![image](https://github.com/user-attachments/assets/c82c1aef-e79b-4bae-855f-d7fbc5ff3fc2)



🔶 𝐏𝐞𝐫𝐜𝐞𝐧𝐭𝐚𝐠𝐞 𝐜𝐨𝐧𝐭𝐫𝐢𝐛𝐮𝐭𝐢𝐨𝐧 𝐨𝐟 𝐬𝐩𝐞𝐧𝐝𝐬 𝐛𝐲 𝐟𝐞𝐦𝐚𝐥𝐞𝐬 𝐟𝐨𝐫 𝐞𝐚𝐜𝐡 𝐞𝐱𝐩𝐞𝐧𝐬𝐞 𝐭𝐲𝐩𝐞

SELECT exp_type,SUM (CASE WHEN gender ="F" THEN amount ELSE 0 END )/ SUM(amount) *100 as percentage_womenspend 
FROM credit_card_transactions.data
GROUP BY exp_type
ORDER BY percentage_womenpend DESC ;


![image](https://github.com/user-attachments/assets/c680c7a9-af27-4ee7-a777-8d1c852166de)


🔶 𝗤𝘂𝗲𝗿𝘆 𝘁𝗼 𝗳𝗶𝗻𝗱 3 𝗰𝗼𝗹𝘂𝗺𝗻𝘀 : 𝗰𝗶𝘁𝘆, 𝗵𝗶𝗴𝗵𝗲𝘀𝘁_𝗲𝘅𝗽𝗲𝗻𝘀𝗲_𝘁𝘆𝗽𝗲 , 𝗹𝗼𝘄𝗲𝘀𝘁_𝗲𝘅𝗽𝗲𝗻𝘀𝗲_𝘁𝘆𝗽𝗲 (𝗲𝘅𝗮𝗺𝗽𝗹𝗲 𝗳𝗼𝗿𝗺𝗮𝘁 : 𝗗𝗲𝗹𝗵𝗶 , 𝗯𝗶𝗹𝗹𝘀, 𝗙𝘂𝗲𝗹)  

WITH cte1 as (SELECT city,exp_type,SUM(amount)as total_amount FROM credit_card_transactions.data
GROUP BY  city,exp_type),
cte2 as (SELECT *,rank() OVER(PARTITION BY city ORDER BY total_amount desc) as desc_rk,rank() OVER(PARTITION BY city ORDER BY total_amount asc) as asc_rk FROM cte1)

SELECT city,MIN((CASE WHEN desc_rk=1 THEN exp_type END)) as highest_exp_type,MIN((CASE WHEN asc_rk=1 THEN exp_type END)) as lowest_exp_type 
FROM cte2
GROUP BY city;


![6](https://github.com/user-attachments/assets/50aa0a08-9a67-4da3-a442-4b4ec6d186e3)



🔶 𝗩𝗮𝗿𝗶𝗼𝘂𝘀 𝗰𝗮𝗿𝗱 𝗮𝗻𝗱 𝗲𝘅𝗽𝗲𝗻𝘀𝗲 𝘁𝘆𝗽𝗲 𝗰𝗼𝗺𝗯𝗶𝗻𝗮𝘁𝗶𝗼𝗻 𝗺𝗼𝗻𝘁𝗵 𝗼𝘃𝗲𝗿 𝗺𝗼𝗻𝘁𝗵 𝗴𝗿𝗼𝘄𝘁𝗵(%) 𝗶𝗻 𝗝𝗮𝗻-2014 

WITH cte as  (SELECT * FROM (SELECT card_type,exp_type,EXTRACT(YEAR_MONTH FROM str_to_date(transaction_date,"%d- %M- %y" ) ) as yr_mnth,SUM(amount) as total_amt 
FROM credit_card_transactions.data
GROUP BY card_type,exp_type,yr_mnth) A
WHERE yr_mnth = 201312 OR yr_mnth = 201401
), 
cte2 as ( SELECT *,LAG(total_amt,1) OVER (PARTITION BY card_type,exp_type ORDER BY yr_mnth asc ) as prev_mth_amt 
FROM cte )
 
SELECT card_type,exp_type,((total_amt-prev_mth_amt)/prev_mth_amt)*100 as mom_growth_perc FROM cte2
WHERE prev_mth_amt IS NOT NULL
ORDER BY mom_growth_perc DESC;

![10](https://github.com/user-attachments/assets/5efc2f40-0b91-4bd6-855d-df91f9548447)



🔶 𝗗𝘂𝗿𝗶𝗻𝗴 𝘄𝗲𝗲𝗸𝗲𝗻𝗱𝘀,𝘄𝗵𝗶𝗰𝗵 𝗰𝗶𝘁𝘆 𝗵𝗮𝘀 𝗵𝗶𝗴𝗵𝗲𝘀𝘁 𝘁𝗼𝘁𝗮𝗹 𝘀𝗽𝗲𝗻𝗱 𝘁𝗼 𝘁𝗼𝘁𝗮𝗹 𝗻𝗼 𝗼𝗳 𝘁𝗿𝗮𝗻𝘀𝗮𝗰𝘁𝗶𝗼𝗻𝘀 𝗿𝗮𝘁𝗶𝗼

SELECT city,(total_amount/transaction_count) as ratio FROM
(SELECT city,SUM(amount) as total_amount,COUNT(amount)as transaction_count 
FROM credit_card_transactions.data
WHERE DAYNAME(str_to_date(transaction_date,"%d-%M-%y"))IN ("Saturday","Sunday") 
GROUP BY city)A
ORDER BY ratio DESC
LIMIT 1;

![image](https://github.com/user-attachments/assets/f1b8db48-8389-4dac-8c1d-7447dbeec68d)



🔶 C𝐢𝐭𝐲 𝐭𝐨𝐨𝐤 𝐥𝐞𝐚𝐬𝐭 𝐧𝐮𝐦𝐛𝐞𝐫 𝐨𝐟 𝐝𝐚𝐲𝐬 𝐭𝐨 𝐫𝐞𝐚𝐜𝐡 𝐢𝐭𝐬 500 𝐭𝐡 𝐭𝐫𝐚𝐧𝐬𝐚𝐜𝐭𝐢𝐨𝐧 𝐚𝐟𝐭𝐞𝐫 𝐭𝐡𝐞 𝐟𝐢𝐫𝐬𝐭 𝐭𝐫𝐚𝐧𝐬𝐚𝐜𝐭𝐢𝐨𝐧 𝐢𝐧 𝐭𝐡𝐚𝐭 𝐜𝐢𝐭𝐲


WITH cte as (SELECT *,row_number() OVER(PARTITION BY city ORDER BY str_to_date(transaction_date,"%d-%M-%y")asc) as rw_no 
FROM credit_card_transactions.data)

SELECT city,datediff( 500th_order_date,first_order_date) as no_days FROM (SELECT city,count(*),MIN( str_to_date(transaction_date,"%d-%M-%y") ) as first_order_date,MAX( str_to_date(transaction_date,"%d-%M-%y") ) as 500th_order_date from cte 
WHERE rw_no IN (1,500)
GROUP BY city
HAVING COUNT(*) = 2 ) A
ORDER BY no_days asc
LIMIT 1; 


![20](https://github.com/user-attachments/assets/6752ed88-3ac5-43ad-a3c6-4aac99bec6e1)






📚 𝗞𝗲𝘆 𝗦𝘁𝗿𝗮𝘁𝗲𝗴𝗶𝗰 𝗜𝗻𝘀𝗶𝗴𝗵𝘁𝘀



✅️ 𝖬𝖾𝗍𝗋𝗈 𝖼𝗂𝗍𝗂𝖾𝗌 𝗅𝗂𝗄𝖾 𝖦𝗋𝖾𝖺𝗍𝖾𝗋 𝖬𝗎𝗆𝖻𝖺𝗂,𝖡𝖾𝗇𝗀𝖺𝗅𝗎𝗋𝗎,𝖠𝗁𝗆𝖾𝖽𝖺𝖻𝖺𝖽,𝖣𝖾𝗅𝗁𝗂 & 𝖪𝗈𝗅𝗄𝖺𝗍𝖺 𝖽𝗈𝗆𝗂𝗇𝖺𝗍𝖾 𝗂𝗇 𝖢𝗋𝖾𝖽𝗂𝗍 𝖼𝖺𝗋𝖽 𝗌𝗉𝖾𝗇𝖽𝗂𝗇𝗀 𝖺𝖼𝖼𝗈𝗎𝗇𝗍𝗂𝗇𝗀 𝖺𝗋𝗈𝗎𝗇𝖽 60 % 𝗈𝖿 𝗍𝗈𝗍𝖺𝗅 𝖼𝗋𝖾𝖽𝗂𝗍 𝖼𝖺𝗋𝖽 𝗌𝗉𝖾𝗇𝖽𝗂𝗇𝗀 𝗐𝗂𝗍𝗁 𝖦𝗋𝖾𝖺𝗍𝖾𝗋 𝖬𝗎𝗆𝖻𝖺𝗂 𝗍𝗈𝗉𝗌 𝗍𝗁𝖾 𝗅𝗂𝗌𝗍 𝗐𝗂𝗍𝗁  14.2% 𝖿𝗈𝗅𝗅𝗈𝗐𝖾𝖽 𝖻𝗒 𝖡𝖾𝗇𝗀𝖺𝗅𝗎𝗋𝗎. 

✅️ 𝖠𝗆𝗈𝗇𝗀 𝖺𝗅𝗅 𝖼𝖺𝗍𝖾𝗀𝗈𝗋𝗂𝖾𝗌 𝗈𝖿 𝖼𝗋𝖾𝖽𝗂𝗍 𝖼𝖺𝗋𝖽𝗌,𝗍𝗁𝖾 𝖦𝗈𝗅𝖽 𝖼𝖺𝗋𝖽 𝗐𝗂𝗍𝗇𝖾𝗌𝗌𝖾𝖽 𝗍𝗁𝖾 𝗁𝗂𝗀𝗁𝖾𝗌𝗍 𝗌𝗉𝖾𝗇𝖽𝗂𝗇𝗀 𝗂𝗇 𝗍𝗁𝖾 𝗆𝗈𝗇𝗍𝗁 𝗈𝖿 𝖩𝖺𝗇𝗎𝖺𝗋𝗒 (𝖸𝖾𝖺𝗋 2015),𝗍𝗈𝗍𝖺𝗅𝗂𝗇𝗀 𝖺𝗋𝗈𝗎𝗇𝖽 55 𝗆𝗂𝗅𝗅𝗂𝗈𝗇 𝗋𝗎𝗉𝖾𝖾𝗌.𝖳𝗁𝗂𝗌 𝗐𝖺𝗌 𝖿𝗈𝗅𝗅𝗈𝗐𝖾𝖽 𝖻𝗒 𝗍𝗁𝖾 𝖯𝗅𝖺𝗍𝗂𝗇𝗎𝗆 𝗐𝗁𝗂𝖼𝗁 𝗋𝖾𝖼𝗈𝗋𝖽𝖾𝖽 𝖺𝗋𝗈𝗎𝗇𝖽 57𝗆𝗂𝗅𝗅𝗂𝗈𝗇 𝗋𝗎𝗉𝖾𝖾𝗌 𝗂𝗇 𝗍𝗁𝖾 𝗆𝗈𝗇𝗍𝗁 𝗈𝖿 𝖲𝖾𝗉𝗍𝖾𝗆𝖻𝖾r(𝖸𝖾𝖺𝗋 2016).

✅️ 𝖦𝗈𝗅𝖽 c𝗋𝖾𝖽𝗂𝗍 c𝖺𝗋𝖽 𝗂𝗌 𝗍𝗁𝖾 𝖿𝗂𝗋𝗌𝗍 𝗍𝗈 𝗁𝗂𝗍 𝗆𝗂𝗅𝖾𝗌𝗍𝗈𝗇𝖾 𝗈𝖿 1,000,000 𝗂𝗇 𝗍𝗈𝗍𝖺𝗅 𝗌𝗉𝖾𝗇𝖽 !

✅️ 𝖦𝗈𝗅𝖽 𝖼𝗋𝖾𝖽𝗂𝗍 𝖼𝖺𝗋𝖽 𝗂𝗌 𝗍𝗁𝖾 𝗅𝖾𝖺𝗌𝗍 𝗉𝗋𝖾𝖿𝖾𝗋𝗋𝖾𝖽 𝖼𝗁𝗈𝗂𝖼𝖾 𝖺𝗆𝗈𝗇𝗀 𝖼𝗎𝗌𝗍𝗈𝗆𝖾𝗋𝗌 𝗂𝗇 𝖣𝗁𝖺𝗆𝗍𝖺𝗋𝗂 𝖼𝗂𝗍𝗒.

✅️ 𝖶𝗈𝗆𝖾𝗇 𝗉𝗋𝖾𝖿𝖾𝗋 𝗎𝗌𝗂𝗇𝗀 𝖼𝗋𝖾𝖽𝗂𝗍 𝖼𝖺𝗋𝖽𝗌 𝖿𝗈𝗋 𝗉𝖺𝗒𝗂𝗇𝗀 𝖻𝗂𝗅𝗅𝗌 𝖼𝗈𝗆𝗉𝖺𝗋𝖾𝖽 𝗍𝗈 𝗈𝗍𝗁𝖾𝗋 𝖾𝗑𝗉𝖾𝗇𝗌𝖾 𝖼𝖺𝗍𝖾𝗀𝗈𝗋𝗂𝖾𝗌.

✅️ 𝖠𝖼𝗁𝖺𝗅𝗉𝗎𝗋 𝖾𝗑𝗁𝗂𝖻𝗂𝗍 𝗍𝗁𝖾 𝗁𝗂𝗀𝗁𝖾𝗌𝗍 𝗌𝗉𝖾𝗇𝖽𝗂𝗇𝗀 𝗈𝗇 𝗀𝗋𝗈𝖼𝖾𝗋𝗂𝖾𝗌 𝖺𝗇𝖽 𝗅𝖾𝖺𝗌𝗍 𝗈𝗇 𝖾𝗇𝗍𝖾𝗋𝗍𝖺𝗂𝗇𝗆𝖾𝗇𝗍 𝖿𝗈𝗅𝗅𝗈𝗐𝖾𝖽 𝖻𝗒 𝖠𝖽𝗂𝗅𝖺𝖻𝖺𝖽.

✅️ 𝖦𝗈𝗅𝖽 𝖼𝗋𝖾𝖽𝗂𝗍 𝖼𝖺𝗋𝖽 𝗁𝗈𝗅𝖽𝖾𝗋𝗌 𝗐𝗂𝗍𝗇𝖾𝗌𝗌𝖾𝖽 𝗍𝗁𝖾 𝗁𝗂𝗀𝗁𝖾𝗌𝗍 𝗆𝗈𝗇𝗍𝗁 𝗈𝗏𝖾𝗋 𝗆𝗈𝗇𝗍𝗁 𝗀𝗋𝗈𝗐𝗍𝗁 𝗈𝖿 𝖺𝗋𝗈𝗎𝗇𝖽 88% 𝗂𝗇 𝗌𝗉𝖾𝗇𝖽𝗂𝗇𝗀 𝗈𝗇 𝗍𝗋𝖺𝗏𝖾𝗅 𝖼𝖺𝗍𝖾𝗀𝗈𝗋𝗒.

✅️ 𝖲𝗈𝗇𝖾𝗉𝗎𝗋 𝗋𝖾𝖼𝗈𝗋𝖽𝖾𝖽 𝗍𝗁𝖾 𝗁𝗂𝗀𝗁𝖾𝗌𝗍 𝖠𝗏𝖾𝗋𝖺𝗀𝖾 𝗍𝗋𝖺𝗇𝗌𝖺𝖼𝗍𝗂𝗈𝗇 𝗏𝖺𝗅𝗎𝖾 𝗈𝗇 𝗐𝖾𝖾𝗄𝖾𝗇𝖽𝗌.

✅️ 𝖨𝖳 𝗁𝗎𝖻 𝖡𝖺𝗇𝗀𝖺𝗅𝗈𝗋𝖾  𝗍𝗈𝗈𝗄  𝗈𝗇𝗅𝗒 81 𝗇𝗈 𝗈𝖿 𝖽𝖺𝗒𝗌  𝗍𝗈 𝗋𝖾𝖼𝗈𝗋𝖽 500𝗍𝗁 𝗍𝗋𝖺𝗇𝗌𝖺𝖼𝗍𝗂𝗈𝗇 𝖺𝖿𝗍𝖾𝗋 𝗍𝗁𝖾𝗂𝗋 𝖿𝗂𝗋𝗌𝗍 𝗍𝗋𝖺𝗇𝗌𝖺𝖼𝗍𝗂𝗈𝗇.

