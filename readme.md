
#### 1、卡券列表接口 o2o-common-webAndroid/android/selectMyCouponList.do

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

