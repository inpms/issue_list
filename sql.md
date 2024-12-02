
#### 查询活动次数的逻辑

SELECT *
FROM award_record
WHERE activity_id = 494
AND user_id = 21774
and house_id = 

####  查询活动信息

SELECT 
NOW(),
FROM_UNIXTIME(a.OverdueTime,'%Y-%m-%d %H:%i:%S'),
ticketid, CouponName, EnableTime, OverdueTime, 0 Deductible, ReduceMoney, 0 DiscountAmount, 0 InsteadMoney, 0 InsteadTime, '' ExchangeInfo, '' PromotionInfo, couponType, prizeFunctions , STATUS, 
sourceType, a.type, a.preconditions, 
a.activityId, useTime FROM v_award_prize_function a 
WHERE 1=1 AND a.`user_id` = 21702 
AND TIMESTAMPDIFF(SECOND,NOW(),IF(a.OverdueTime IS NULL ,'',a.OverdueTime)) >= 0 
AND TIMESTAMPDIFF(SECOND,NOW(),a.EnableTime) < 0 AND STATUS IN (0) 
AND a.couponType NOT IN(3,4) 

AND (a.prizeFunctions IS NULL OR a.prizeFunctions IN (1,2)) 


AND a.prize_from = 0 
ORDER BY STATUS ,a.EnableTime DESC
