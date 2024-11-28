
#### 1、卡券列表接口 o2o-common-webAndroid/android/selectMyCouponList.do

{"token": "b7260fef-dfc5-4f6f-be8e-69d9e3d4e9f8", "type": 0, "pageSize": 100, "pageNo": 1, "versionNum": 1, "version": "2"}



  需增加两个字段：
  
       () month: '2024-11' --账单月份
        type 
        
        condition: '需缴清2024年11月之前的物业费和车位费'  --使用条件文字描述


       



SELECT e.*, f.preconditions, f.type 
FROM 
  (SELECT 
        a.`id` ticketid,
        a.`prize_name` CouponName,
        FROM_UNIXTIME(a.got_time,'%Y/%m/%d %H:%i:%S') EnableTime,
        IF(a.expire_time IS NULL ,'',FROM_UNIXTIME(a.expire_time,'%Y/%m/%d %H:%i:%S'))  OverdueTime,
        0 Deductible,
        a.prize_value ReduceMoney,
        0 DiscountAmount,
        0 InsteadMoney,
        0 InsteadTime,
        '' ExchangeInfo,
        '' PromotionInfo,
        a.prize_type couponType,
        IFNULL(a.prize_functions,0) prizeFunctions ,
        a.status STATUS,
        a.`record_id`,
        1 sourceType,
        d.activity_id activityId
    FROM award_prize_record a 
    LEFT JOIN award_prize_function f ON a.`prize_functions`=f.`id`
    LEFT JOIN sys_member s ON a.`user_id`=s.`id`
    LEFT JOIN award_record d ON  d.id =a.record_id ) 
    AS e  
LEFT JOIN award_activity f ON e.activityId= f.id
WHERE e.record_id = '636'




SELECT e.*, f.preconditions, f.type 
FROM (
    SELECT 
        a.id AS ticketid,
        a.prize_name AS CouponName,
        FROM_UNIXTIME(a.got_time, '%Y/%m/%d %H:%i:%S') AS got_time,
        IF(a.expire_time IS NULL, '', FROM_UNIXTIME(a.expire_time, '%Y/%m/%d %H:%i:%S')) AS expire_time,
        0 AS Deductible,
        a.prize_value AS ReduceMoney,
        0 AS DiscountAmount,
        0 AS InsteadMoney,
        0 AS InsteadTime,
        '' AS ExchangeInfo,
        '' AS PromotionInfo,
        a.prize_type AS couponType,
        COALESCE(a.prize_functions, 0) AS prize_functions,
        a.status AS Status,
        a.record_id,
		s.phone ,
e.prize_type,
        1 AS sourceType,
        d.activity_id AS activityId
    FROM award_prize_record a 
    LEFT JOIN award_prize_function f ON a.prize_functions = f.id
    LEFT JOIN sys_member s ON a.user_id = s.id
    LEFT JOIN award_record d ON d.id = a.record_id
) AS e 
LEFT JOIN award_activity f ON e.activityId = f.id

    WHERE 1=1
	<if test="userid != null ">
		AND e.`user_id` = #{userid}
	</if>
	<if test="phone != null and phone != '' ">
	  AND e.`phone` = #{phone}
	</if>
	<!-- type不为空，查询可使用的卡券-->
	<if test="type != null and type != 0">
		AND TIMESTAMPDIFF(SECOND,NOW(),if(a.expire_time is null ,'',FROM_UNIXTIME(e.expire_time,'%Y-%m-%d %H:%i:%S'))) >= 0
		AND TIMESTAMPDIFF(SECOND,NOW(),FROM_UNIXTIME(e.got_time,'%Y-%m-%d %H:%i:%S')) &lt; 0
		AND e.Status in (0)
		AND e.prize_type NOT IN(3,4)
		AND (e.prize_functions IS NULL  OR e.prize_functions IN (1,2))
	</if>
    and e.prize_from = 0
	order by e.Status ,e.got_time DESC
  </select>

