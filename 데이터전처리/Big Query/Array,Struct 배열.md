
# ARRAY, STRUCT
### unnest 사용해서 array 구조 데이터 전처리 하기


연습문제 1) array_exercises 테이블에서 각 영화(title)별로 장르(genres)를 unnest해서 보여주세요

```
Select 
  title,
  -- genres, # 기존에 array_exercises에 저장되어 있던 컬럼 
  genre 
FROM advanced.array_exercises as ae
CROSS JOIN UNNEST(genres) as genre
;
```

array: 같은 타입의 여러 데이터를 저장하고 싶을 때 

array를 Flatten(평면화) => unnest

unnest를 할 때는 cross join + unnest (array_column)

unnest (array_column) as 새로운 이름

select 절에서 새로운 이름으로 사용한다. 기존의 array_column은 사용하지 않는다


BigQuery(SQL) 활용편 [https://inf.run/QVgPf] 강의 내용 인용