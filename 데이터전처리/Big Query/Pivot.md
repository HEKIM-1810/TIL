데이터 피벗하기

select 
    order_date,

    Max = sum 
    SUM(IF(user_id =1, amount,0)) as user 1, 