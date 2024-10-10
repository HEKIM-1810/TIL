

# 파이썬 데이터 전처리


## 1. 여러개의 구글스프레드시트에서 데이터 가져와서 1개의 Data Frame으로 통합하기

### [1] 구글스프레드시트 URL 변수 지정

```
2409_서울_url = "구글스프레드시트 주소1"
2410_서울_url = "구글스프레드시트 주소1"

2409_부평_url=  "구글스프레드시트 주소"
2410_부평_url=  "구글스프레드시트 주소"

2409_수원_url = " "구글스프레드시트 주소"
2410_수원_url = " "구글스프레드시트 주소"


branch_urls = {'서울': [2409_서울_url, 2410_서울_url], 
                '부평': [2409_부평_url, 2410_부평_url], 
                '수원': [2409_수원_url, 2410_수원_url], }
```

### [2] Google Sheets 인증

```
import pandas as pd
import gspread
from gspread_dataframe import get_as_dataframe
from oauth2client.service_account import ServiceAccountCredentials
import time 
import re

# Google Sheets 인증
scope = ['https://spreadsheets.google.com/feeds', 'https://www.googleapis.com/auth/drive']
creds = ServiceAccountCredentials.from_json_keyfile_name('API KEY.json', scope)
gc = gspread.authorize(creds)

# Google Sheet 데이터를 받아오는 함수 정의
def fetch_sheet_data(url, sheet_name="탭이름"):
    sheet = gc.open_by_url(url).worksheet(sheet_name)
    df = get_as_dataframe(sheet)
    return df

# 공통으로 사용할 컬럼 리스트
columns = ['수납일', '고객명', '차트번호','매출액']

# 지점 데이터를 통합하는 함수
def process_branch_data(branch_name, urls):
    df_list = []
    for url in urls:
        df = fetch_sheet_data(url)
        time.sleep(3)
        df_selected = df[columns]
        df_list.append(df_selected)

    branch_df = pd.concat(df_list, ignore_index=True)
    branch_df.dropna(subset=['수납일'], inplace=True)
    branch_df['수납일'] = pd.to_datetime(branch_df['수납일'])
    branch_df = branch_df.sort_values(by='수납일').reset_index(drop=True)
    branch_df['지점'] = branch_name
    return branch_df
```


### [3] 데이터 전처리 함수 모듈 사용
```
columns = ['수납일', '고객명', '차트번호']

서울_pay_df = process_branch_data('서울', branch_urls['서울'])
부평_pay_df = process_branch_data('부평', branch_urls['부평'])
수원_pay_df = process_branch_data('수원', branch_urls['수원'])

```
### [4] 지점별 데이터 통합
`pay_df_raw = pd.concat([서울_pay_df,부평_pay_df,수원_pay_df], ignore_index=True)`
