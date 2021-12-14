# Opensource_class_Finalexam


#자료는 https://drive.google.com/drive/folders/106h5of1TEoCyigCLlohVC5XkX7bent_P?usp=sharing


#첫번째 코드파일

'''
코로나 확진자수를 연도별로 통계낸 코드 (2021년 12월 12일 기준)
자료출처:질병 관리청 코로나바이러스감염증-19 사이트 (코로나바이러스감염증-19(COVID-19) (mohw.go.kr))를 변형하여 만들었다.
'''

import matplotlib.pyplot as plt
import pandas as pd



## 코로나 확진자 데이터 불러오고 정리과정

data = pd.read_csv('C:/Users/Songgyuseung/Desktop/colona.csv', encoding='CP949',thousands = ',' )# 데이터 불러오기 encoding을 통해 오류를 없애고 thousands를 통해 천단위의 ,를 없앰 


# 데이터의 컬럼명 변경 
data.columns = ['date','daytotal','Domestic','overseas','die']

# 데이터의 헤더정보 보기(내림차순)
data.head()  

## 연도별로 데이터 나누고 총확진자를 출력하는 과정

# date의 형식을 datetime으로 변경
data['date'] = pd.to_datetime(data['date'],format='%Y-%m-%d')

# DataFram의 query 를 사용하여 년도 조회
target_year2020 = 2020
data_2020 = data.query('date.dt.year == @target_year2020')


target_year2021 = 2021
data_2021 = data.query('date.dt.year == @target_year2021')


## 총확진자 수치로 표현하고 연도별 그래프 그리는 과정

print("코로나 총확진자(2021년 12월 12일 기준):",data['daytotal'].sum())
print("2020년 총확진자:",data_2020['daytotal'].sum())
print("2021년 총확진자(12월 12일 기준):",data_2021['daytotal'].sum())


graph_2020 = pd.DataFrame({'daytotal':data_2020['daytotal'],'date':data_2020['date']})
graph_2020.plot(kind='line', x='date', y='daytotal', color='blue')
graph_2021 = pd.DataFrame({'daytotal':data_2021['daytotal'],'date':data_2021['date']})
graph_2021.plot(kind='line', x='date', y='daytotal', color='red')



#두번째 코드파일

'''
백신 접종자수를 통계 내기위한 코드 (2021년 12월 12일 기준)
자료출처:질병 관리청 코로나19 예방접종 사이트(코로나19 예방접종 > 예방접종 현황 > 백신별 일일 접종현황 (kdca.go.kr))를 변형하여 만들었다.
'''

import matplotlib.pyplot as plt
import pandas as pd

## 백신 데이터 불러오고 정리과정

back_data = pd.read_csv('C:/Users/Songgyuseung/Desktop/백신.csv', encoding='CP949',thousands = ',' )# 데이터 불러오기 encoding을 통해 오류를 없애고 thousands를 통해 천단위의 ,를 없앰

back_data.head()  

back_data.columns = ['date','First','Second','Third']

# date의 형식을 datetime으로 변경
back_data['date'] = pd.to_datetime(back_data['date'],format='%Y-%m-%d')

## 차수별 백신 접종 수치로 표현하고 연도별 그래프 그리는 과정(누적 데이터라 막대그래프로 표현하고싶었지만 실패)

# 누적 데이터의 접종자수는 최대가 총 접종자수이므로 max를 써서 표현
print("1차 접종자수(2021년 12월 12일기준):",back_data['First'].max())
print("2차 접종자수(2021년 12월 12일기준):",back_data['Second'].max())
print("3차 접종자수(2021년 12월 12일기준):",back_data['Third'].max())

# 그래프 그리기
ax = plt.gca()
graph_back = pd.DataFrame({'date':back_data['date'],'First':back_data['First'],'Second':back_data['Second'],'Third':back_data['Third']})
graph_back.plot(kind='line', x='date', y='First', color='blue',ax=ax)
graph_back.plot(kind='line', x='date', y='Second', color='red',ax=ax)
graph_back.plot(kind='line', x='date', y='Third', color='green',ax=ax)


# 세번째 코드 파일
'''
백신 접종 전후의 코로나 확진자 증가율을 비교하기 위한 코드이다.(2021년 12월 12일 기준)
'''



import matplotlib.pyplot as plt
import pandas as pd


## 코로나 확진자 데이터 불러오고 정리과정

data = pd.read_csv('C:/Users/Songgyuseung/Desktop/colona.csv', encoding='CP949',thousands = ',' )# 데이터 불러오기 encoding을 통해 오류를 없애고 thousands를 통해 천단위의 ,를 없앰 


# 데이터의 컬럼명 변경 
data.columns = ['date','daytotal','Domestic','overseas','die']

# 데이터의 헤더정보 보기(내림차순)
data.head()  

## 연도별로 데이터 나누고 총확진자를 출력하는 과정

# date의 형식을 datetime으로 변경
data['date'] = pd.to_datetime(data['date'],format='%Y-%m-%d')

# DataFram의 query 를 사용하여 년도 조회(확진자가 증가하는 시점 찾기)

target_year2020 = 2020
data_2020year = data.query('date.dt.year == @target_year2020')

target_month02 = 2
data_02=data_2020year.query('date.dt.month == @target_month02')

target_day19 = 18
target_day29 = 30
data_1=data_02.query('date.dt.day > @target_day19 and date.dt.day < @target_day29')

target_month08 = 8
data_08=data_2020year.query('date.dt.month == @target_month08')

target_day13 = 12
target_day28 = 29
data_2=data_08.query('date.dt.day > @target_day13 and date.dt.day < @target_day28')

target_month12=12
data_20_12=data_2020year.query('date.dt.month == @target_month12')



target_year2021 = 2021
data_2021year = data.query('date.dt.year == @target_year2021')

target_month07 = 7
data_07=data_2021year.query('date.dt.month == @target_month07')

target_day06 = 5
target_day22 = 23
data_3=data_07.query('date.dt.day > @target_day06 and date.dt.day < @target_day22')



data_21_12=data_2021year.query('date.dt.month == @target_month12')
target_day10= 11
data_4=data_21_12.query('date.dt.day < @target_day10')

#백신 맞기전
print(data_1)
print(data_2)
#백신 접종시작 5개월 후
print(data_3)

#백신 접종시작 10개월 후
print(data_4)


graph_1 = pd.DataFrame({'daytotal':data_1['daytotal'],'date':data_1['date'],'number':[0,1,2,3,4,5,6,7,8,9,10]})
graph_2 = pd.DataFrame({'daytotal':data_2['daytotal'],'date':data_2['date'],'number':[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]})
graph_3 = pd.DataFrame({'daytotal':data_3['daytotal'],'date':data_3['date'],'number':[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16]})
graph_4 = pd.DataFrame({'daytotal':data_4['daytotal'],'date':data_4['date'],'number':[0,1,2,3,4,5,6,7,8,9]})


graph_1.plot(kind='bar', x='number', y='daytotal', color='blue')
graph_2.plot(kind='bar', x='number', y='daytotal', color='red')
graph_3.plot(kind='bar', x='number', y='daytotal', color='green')
graph_4.plot(kind='bar', x='number', y='daytotal', color='purple')


