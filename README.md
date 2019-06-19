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
