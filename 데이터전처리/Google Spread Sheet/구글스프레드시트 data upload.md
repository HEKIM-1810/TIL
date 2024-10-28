# 구글 스프레드 시트 데이터 업로드

### index
1. 데이터프레임
2. 구글 계정 API 
3. 업로드
   1. 업로드할 특정 시트 선택
   2. 폴더 및 시트 생성
--- 



### 1. 스프레드시트 열기
```
pay_spreadsheet_key = '스프레드시트 Key'  
pay_worksheet_name = '시트1'  # 워크시트 이름 변경 가능
pay_sh = gc.open_by_key(pay_spreadsheet_key)
pay_worksheet = pay_sh.worksheet(pay_worksheet_name)
```
### 2. 구글스프레드시트 전체 데이터 delete

`pay_worksheet.clear()`

### 3. 데이터프레임 업로드
```
set_with_dataframe(pay_worksheet, smart_pay_df)

print("수납조회 전처리 완료된 데이터가 구글 스프레드시트에 업로드되었습니다.")
```