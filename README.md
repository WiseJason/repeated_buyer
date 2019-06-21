# repeated_buyer


删除重复数据
delete from user_info_format1 where pid  not in (select dt.minno from (select min(pid) as minno from user_info_format1 GROUP BY user_id) dt )
  
  
  
 sql:
 
 1.
 create table user_feature8
SELECT
	b.user_id,
	count( b.user_id ) as total_num,
	count( CASE b.action_type WHEN 0 THEN 1 ELSE NULL END ) AS click_num,
	count( CASE b.action_type WHEN 1 THEN 1 ELSE NULL END ) AS add_carts_num,
	count( CASE b.action_type WHEN 2 THEN 1 ELSE NULL END ) AS purchase_num,
	count( CASE b.action_type WHEN 3 THEN 1 ELSE NULL END ) AS collection_num,
	count(DISTINCT b.merchant_id) as merchant_num,
	count(DISTINCT b.item_id) as item_num,
	count(DISTINCT b.cat_id) as cat_num,
	count(DISTINCT b.brand_id) as brand_num,
	(select count(right(a.time_stamp,2)) from user_log_format1  as a where a.user_id=b.user_id)/(select count(distinct(right(a.time_stamp,2))) from user_log_format1  as a where a.user_id=b.user_id) as day_num,
	(select count(left(a.time_stamp,2)) from user_log_format1  as a where a.user_id=b.user_id)/(select count(distinct(left(a.time_stamp,2))) from user_log_format1  as a where a.user_id=b.user_id) as month_num
FROM
	user_log_format1 as b
GROUP BY
	user_id 
  
  2.
  
create table merchant_feature1
SELECT
	d.merchant_id,
	count( d.merchant_id ) AS merchant_total_num,
	( SELECT count( a.merchant_id ) FROM user_log_format1 AS a WHERE d.merchant_id = a.merchant_id AND a.action_type = 0 ) AS click_merchant_num,
	( SELECT count( a.merchant_id ) FROM user_log_format1 AS a WHERE d.merchant_id = a.merchant_id AND a.action_type = 1 ) AS collection_merchant_num,
	( SELECT count( a.merchant_id ) FROM user_log_format1 AS a WHERE d.merchant_id = a.merchant_id AND a.action_type = 2 ) AS purchase_merchant_num,
	( SELECT count( a.merchant_id ) FROM user_log_format1 AS a WHERE d.merchant_id = a.merchant_id AND a.action_type = 3 ) AS add_carts_merchant_num,
	count( DISTINCT ( d.user_id ) ) AS total_user_num,
	count( DISTINCT ( d.item_id ) ) AS item_merchant_num,
	count( DISTINCT ( d.cat_id ) ) AS cat_merchant_num,
	count( DISTINCT ( d.brand_id ) ) AS brand_merchant_num,
	(
	SELECT
		count( e.merchant_id ) / count( DISTINCT ( RIGHT ( e.time_stamp, 2 ) ) ) 
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id 
	) AS day_merchant_num,
	(
	SELECT
		count( e.merchant_id ) / count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ) ) 
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id 
	) AS month_merchant_num
	
FROM
	user_log_format1 AS d 
GROUP BY
	d.merchant_id
  
  3.
  

create table merchant_feature2
SELECT
	a.merchant_id,
	count( CASE b.gender WHEN 0 THEN 1 ELSE NULL END )  as female_merchant_num,
	count( CASE b.gender WHEN 1 THEN 1 ELSE NULL END )  as male_merchant_num,
	count( CASE b.gender WHEN NULL THEN 1 ELSE NULL END )  as no_gender_merchant_num
FROM
	user_log_format1 AS a
	LEFT JOIN user_info_format1 AS b ON a.user_id = b.user_id 
GROUP BY
	a.merchant_id
4.
CREATE TABLE merchant_feature3
SELECT
a.merchant_id,
count( CASE a.age_range WHEN 0 THEN 1 ELSE NULL END ) AS no_age_merchant_num,
count( CASE a.age_range WHEN 1 THEN 1 ELSE NULL END ) AS 18_merchant_num,
count( CASE a.age_range WHEN 2 THEN 1 ELSE NULL END ) AS 24_merchant_num,
count( CASE a.age_range WHEN 3 THEN 1 ELSE NULL END ) AS 29_merchant_num,
count( CASE a.age_range WHEN 4 THEN 1 ELSE NULL END ) AS 34_merchant_num,
count( CASE a.age_range WHEN 5 THEN 1 ELSE NULL END ) AS 39_merchant_num,
count( CASE a.age_range WHEN 6 THEN 1 ELSE NULL END ) AS 49_merchant_num,
count( CASE a.age_range WHEN 7 THEN 1 ELSE NULL END ) AS 50_merchant_num 
FROM
	user_log_format1 AS a 
GROUP BY
	a.merchant_id

5.
SELECT
merchant_id,
count( CASE  WHEN age_range=0 and gender=NULL THEN 1 ELSE NULL END ) AS no_age_no_gender_merchant_num,

count( CASE  WHEN age_range=0 and gender=0 THEN 1 ELSE NULL END ) AS no_age_female_merchant_num,

count( CASE  WHEN age_range=0 and gender=1 THEN 1 ELSE NULL END ) AS no_age_male_merchant_num,

count( CASE  WHEN age_range=1 and gender=NULL THEN 1 ELSE NULL END ) AS 18_no_gender_merchant_num,

count( CASE  WHEN age_range=1 and gender=0 THEN 1 ELSE NULL END ) AS 18_female_merchant_num,

count( CASE  WHEN age_range=1 and gender=1 THEN 1 ELSE NULL END ) AS 18_male_merchant_num,

count( CASE  WHEN age_range=2 and gender=NULL THEN 1 ELSE NULL END ) AS 24_no_gender_merchant_num,

count( CASE  WHEN age_range=2 and gender=0 THEN 1 ELSE NULL END ) AS 24_female_merchant_num,

count( CASE  WHEN age_range=2 and gender=1 THEN 1 ELSE NULL END ) AS 24_male_merchant_num,

count( CASE  WHEN age_range=3 and gender=NULL THEN 1 ELSE NULL END ) AS 29_no_gender_merchant_num,

count( CASE  WHEN age_range=3 and gender=0 THEN 1 ELSE NULL END ) AS 29_female_merchant_num,

count( CASE  WHEN age_range=3 and gender=1 THEN 1 ELSE NULL END ) AS 29_male_merchant_num,

count( CASE  WHEN age_range=4 and gender=NULL THEN 1 ELSE NULL END ) AS 34_no_gender_merchant_num,

count( CASE  WHEN age_range=4 and gender=0 THEN 1 ELSE NULL END ) AS 34_female_merchant_num,

count( CASE  WHEN age_range=4 and gender=1 THEN 1 ELSE NULL END ) AS 34_male_merchant_num,

count( CASE  WHEN age_range=5 and gender=NULL THEN 1 ELSE NULL END ) AS 39_no_gender_merchant_num,

count( CASE  WHEN age_range=5 and gender=0 THEN 1 ELSE NULL END ) AS 39_female_merchant_num,

count( CASE  WHEN age_range=5 and gender=1 THEN 1 ELSE NULL END ) AS 39_male_merchant_num,

count( CASE  WHEN age_range=6 and gender=NULL THEN 1 ELSE NULL END ) AS 49_no_gender_merchant_num,

count( CASE  WHEN age_range=6 and gender=0 THEN 1 ELSE NULL END ) AS 49_female_merchant_num,

count( CASE  WHEN age_range=6 and gender=1 THEN 1 ELSE NULL END ) AS 49_male_merchant_num,

count( CASE  WHEN age_range=7 and gender=NULL THEN 1 ELSE NULL END ) AS 50_no_gender_merchant_num,

count( CASE  WHEN age_range=7 and gender=0 THEN 1 ELSE NULL END ) AS 50_female_merchant_num,

count( CASE  WHEN age_range=7 and gender=1 THEN 1 ELSE NULL END ) AS 50_male_merchant_num
FROM
	temp
GROUP BY
	merchant_id
	
	
6
CREATE TABLE merchant_feature5
SELECT
a.merchant_id,
count( CASE a.age_range WHEN 0 THEN 1 ELSE NULL END ) AS no_age_merchantpurchase_num,
count( CASE a.age_range WHEN 1 THEN 1 ELSE NULL END ) AS 18_merchantpurchase_num,
count( CASE a.age_range WHEN 2 THEN 1 ELSE NULL END ) AS 24_merchantpurchase_num,
count( CASE a.age_range WHEN 3 THEN 1 ELSE NULL END ) AS 29_merchantpurchase_num,
count( CASE a.age_range WHEN 4 THEN 1 ELSE NULL END ) AS 34_merchantpurchase_num,
count( CASE a.age_range WHEN 5 THEN 1 ELSE NULL END ) AS 39_merchantpurchase_num,
count( CASE a.age_range WHEN 6 THEN 1 ELSE NULL END ) AS 49_merchantpurchase_num,
count( CASE a.age_range WHEN 7 THEN 1 ELSE NULL END ) AS 50_merchantpurchase_num 
FROM
	user_log_format1 AS a where a.action_type=2
GROUP BY
	a.merchant_id
	
7

CREATE TABLE merchant_feature5
SELECT
a.merchant_id,
count( CASE a.age_range WHEN 0 THEN 1 ELSE NULL END ) AS no_age_merchant_purchase_num,
count( CASE a.age_range WHEN 1 THEN 1 ELSE NULL END ) AS 18_merchant_purchase_num,
count( CASE a.age_range WHEN 2 THEN 1 ELSE NULL END ) AS 24_merchant_purchase_num,
count( CASE a.age_range WHEN 3 THEN 1 ELSE NULL END ) AS 29_merchant_purchase_num,
count( CASE a.age_range WHEN 4 THEN 1 ELSE NULL END ) AS 34_merchant_purchase_num,
count( CASE a.age_range WHEN 5 THEN 1 ELSE NULL END ) AS 39_merchant_purchase_num,
count( CASE a.age_range WHEN 6 THEN 1 ELSE NULL END ) AS 49_merchant_purchase_num,
count( CASE a.age_range WHEN 7 THEN 1 ELSE NULL END ) AS 50_merchant_purchase_num 
FROM
	user_log_format1 AS a where a.action_type=2
GROUP BY
	a.merchant_id
	
	
	8.
	create table merchant_feature6
SELECT
	a.merchant_id,
	count( CASE a.gender WHEN 0 THEN 1 ELSE NULL END )  as female_merchant_purchase_num,
	count( CASE a.gender WHEN 1 THEN 1 ELSE NULL END )  as male_merchant_purchase_num,
	count( CASE a.gender WHEN NULL THEN 1 ELSE NULL END )  as no_gender_merchant_purchase_num
FROM
	user_log_format1 AS a
	where a.action_type=2
GROUP BY
	a.merchant_id
	
	9.
	CREATE TABLE merchant_feature7
	SELECT
merchant_id,
count( CASE  WHEN age_range=0 and gender=NULL THEN 1 ELSE NULL END ) AS no_age_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=0 and gender=0 THEN 1 ELSE NULL END ) AS no_age_female_merchant_purchase_num,

count( CASE  WHEN age_range=0 and gender=1 THEN 1 ELSE NULL END ) AS no_age_male_merchant_purchase_num,

count( CASE  WHEN age_range=1 and gender=NULL THEN 1 ELSE NULL END ) AS 18_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=1 and gender=0 THEN 1 ELSE NULL END ) AS 18_female_merchant_purchase_num,

count( CASE  WHEN age_range=1 and gender=1 THEN 1 ELSE NULL END ) AS 18_male_merchant_purchase_num,

count( CASE  WHEN age_range=2 and gender=NULL THEN 1 ELSE NULL END ) AS 24_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=2 and gender=0 THEN 1 ELSE NULL END ) AS 24_female_merchant_purchase_num,

count( CASE  WHEN age_range=2 and gender=1 THEN 1 ELSE NULL END ) AS 24_male_merchant_purchase_num,

count( CASE  WHEN age_range=3 and gender=NULL THEN 1 ELSE NULL END ) AS 29_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=3 and gender=0 THEN 1 ELSE NULL END ) AS 29_female_merchant_purchase_num,

count( CASE  WHEN age_range=3 and gender=1 THEN 1 ELSE NULL END ) AS 29_male_merchant_purchase_num,

count( CASE  WHEN age_range=4 and gender=NULL THEN 1 ELSE NULL END ) AS 34_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=4 and gender=0 THEN 1 ELSE NULL END ) AS 34_female_merchant_purchase_num,

count( CASE  WHEN age_range=4 and gender=1 THEN 1 ELSE NULL END ) AS 34_male_merchant_purchase_num,

count( CASE  WHEN age_range=5 and gender=NULL THEN 1 ELSE NULL END ) AS 39_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=5 and gender=0 THEN 1 ELSE NULL END ) AS 39_female_merchant_purchase_num,

count( CASE  WHEN age_range=5 and gender=1 THEN 1 ELSE NULL END ) AS 39_male_merchant_purchase_num,

count( CASE  WHEN age_range=6 and gender=NULL THEN 1 ELSE NULL END ) AS 49_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=6 and gender=0 THEN 1 ELSE NULL END ) AS 49_female_merchant_purchase_num,

count( CASE  WHEN age_range=6 and gender=1 THEN 1 ELSE NULL END ) AS 49_male_merchant_purchase_num,

count( CASE  WHEN age_range=7 and gender=NULL THEN 1 ELSE NULL END ) AS 50_no_gender_merchant_purchase_num,

count( CASE  WHEN age_range=7 and gender=0 THEN 1 ELSE NULL END ) AS 50_female_merchant_purchase_num,

count( CASE  WHEN age_range=7 and gender=1 THEN 1 ELSE NULL END ) AS 50_male_merchant_purchase_num
FROM
	user_log_format1 where action_type=2
GROUP BY
	merchant_id
10

create table user_merchant_feature1
SELECT user_id,merchant_id,count(user_id) as user_merchant_num,
count( CASE action_type WHEN 0 THEN 1 ELSE NULL END ) AS user_merchant__click_num,
	count( CASE action_type WHEN 1 THEN 1 ELSE NULL END ) AS user_merchant_add_carts_num,
	count( CASE action_type WHEN 2 THEN 1 ELSE NULL END ) AS user_merchant_purchase_num,
	count( CASE action_type WHEN 3 THEN 1 ELSE NULL END ) AS user_merchant_collection_num
FROM
	temp
GROUP BY
	user_id,
	merchant_id
11
create table user_merchant_feature2
SELECT
	d.user_id,
	d.merchant_id,(
	SELECT
		count( e.merchant_id ) / count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ) ) 
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id  and e.user_id=d.user_id 
	) AS month_user_merchant_num ,(
	SELECT
		count( e.merchant_id ) /(case when count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ) ) =0 then NULL else count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ))    end )
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id  and e.user_id=d.user_id  and e.action_type=0
	) AS month_user_merchant_click_num,(
	SELECT
		count( e.merchant_id ) / (case when count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ) ) =0 then NULL else count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ))    end )
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id  and e.user_id=d.user_id and e.action_type=1
	) AS month_user_merchant_add_cart_num ,
	(
	SELECT
		count( e.merchant_id ) / (case when count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ) ) =0 then NULL else count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ))    end )
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id  and e.user_id=d.user_id and e.action_type=2
	) AS month_user_merchant_purchase_num ,
	(
	SELECT
		count( e.merchant_id ) / (case when count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ) ) =0 then NULL else count( DISTINCT ( LEFT ( e.time_stamp, 2 ) ))    end )
	FROM
		user_log_format1 AS e 
	WHERE
		e.merchant_id = d.merchant_id  and e.user_id=d.user_id and e.action_type=3 
	) AS month_user_merchant_add_favourite_num 
FROM
	user_log_format1 AS d  
GROUP BY
	d.user_id,d.merchant_id

12
create table user_merchant_feature3

SELECT
	a.user_id,
	a.merchant_id,
	DATEDIFF((
		SELECT
			max(
				date(
				concat( '2019', b.time_stamp ))) 
		FROM
			user_log_format1 AS b 
		WHERE
			a.user_id = b.user_id 
			AND a.merchant_id = b.merchant_id 
			),(
		SELECT
			min(
				date(
				concat( '2019', b.time_stamp ))) 
		FROM
			user_log_format1 AS b 
		WHERE
			a.user_id = b.user_id 
			AND a.merchant_id = b.merchant_id 
		)) AS first_last_time_diff,
	count(
		DISTINCT (
		LEFT ( time_stamp, 2 ))) AS month_user_merchant,
	count(
	DISTINCT ( right(time_stamp,2) )) AS day_user_merchant,
	count(
	DISTINCT ( item_id )) AS user_merchant_item_num,
	count(
	DISTINCT ( cat_id )) AS user_merchant_cat_num,
	count(
	DISTINCT ( brand_id )) AS user_merchant_brand_num
FROM
	user_log_format1 AS a 
GROUP BY
	a.user_id,
	a.merchant_id
13
SELECT
	user_id,
	(total_num /(
	SELECT
		( sum( total_num )+ 10 ) 
	FROM
			user_feature1 )) *100 as user_total_proportion,
		(purchase_num /(
	SELECT
		( sum( purchase_num )+ 10 ) 
	FROM
			user_feature1)) *100 as user_purchase_proportion
FROM
	user_feature1
	
14

SELECT
	merchant_id,
	(merchant_total_num /(
	SELECT
		( sum( merchant_total_num )+ 10 ) 
	FROM
			merchant_feature1 )) *100 as merchant_total_proportion,
		(purchase_merchant_num /(
	SELECT
		( sum( purchase_merchant_num )+ 10 ) 
	FROM
			merchant_feature1)) *100 as merchant_purchase_proportion
FROM
	merchant_feature1
15

create table user_merchant_proportion_feature1
SELECT
	user_id,
	merchant_id,
	10000 *(
		user_merchant_num /(
			( SELECT sum( user_merchant_num ) FROM user_merchant_feature1 )+ 10 
		) 
	) AS user_merchant_total_proporation,
	10000 *(
		user_merchant_purchase_num /(
			( SELECT sum( user_merchant_purchase_num) FROM user_merchant_feature1 )+ 10 
		) 
	) AS user_merchant_purchase_proporation
	
FROM
	user_merchant_feature1
	16
	
create table user_merchant_proportion_feature2
SELECT
	a.user_id,a.merchant_id,100*(a.user_merchant_num/(b.merchant_total_num+10)) as user_total_merchant_proportion,
	100*(a.user_merchant_purchase_num/(b.purchase_merchant_num+10)) as user_purchase_merchant_proportion
FROM
	user_merchant_feature1 as a 
	LEFT JOIN merchant_feature1 as b 
	on a.merchant_id=b.merchant_id 
