# 파이썬 데이터 전처리


## 1. 여러개의 구글스프레드시트에서 데이터 가져와서 1개의 Data Frame으로 통합하기


### [1] URL 변수 지정

`서울_url = "구글스프레드시트 주소"`

`부평_ur l=  "구글스프레드시트 주소"`

`수원_url = " "구글스프레드시트 주소"`



`branch_urls = {
    '서울': [서울_url],
    '부평': [부평_url],
    '수원': [수원_url]
}`

from credentials import authorize_gspread

from fetch_data import fetch_sheet_data

from process_data import process_branch_data

import pandas as pd


### [3] Google Sheets 인증
`gc = authorize_gspread('API KEY.json')`

### [2] 컬럼명 지정
`columns = ['수납일', '고객명', '차트번호', ]`

### [4] 지점 데이터 처리
`서울_pay_df = process_branch_data(fetch_sheet_data, gc, '서울', branch_urls['서울'], columns)`

`부평_pay_df = process_branch_data(fetch_sheet_data, gc, '부평', branch_urls['부평'], columns)`

`수원_pay_df = process_branch_data(fetch_sheet_data, gc, '수원', branch_urls['수원'], columns)`

### [5] 지점별 데이터 통합
`pay_df_raw = pd.concat([서울_pay_df,부평_pay_df,수원_pay_df], ignore_index=True)`