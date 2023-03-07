# 5년간 비트코인과 나스닥의 가격 변화 (Bitcoin and Nasdaq Price Change Over 5 years)
* * *
## Summary
* * *    
* 나스닥 지수와 비트코인 가격 간의 관계를 파악하기 위해 2018년 2월부터 2023년 2월까지 5년간의 데이터를 수집하여, 각각의 가격 변화를 하나의 그래프로 시각화하고자 함.    
* To illustrate the relationship between the Nasdaq index and Bitcoin price, I collected data for five years from February 2018 to February 2023 and aim to represent the price changes for each on a single graph.
## Tech Stack   
* * *
<p align="center"><img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white"></p>
 

분석 결과: 나스닥 지수와 비트코인 그래프 모양이 비슷하고 상승과 하락의 시기가 비슷한 것을 보아 나스닥 지수와 비트코인 가격이 상관관계가 있다고 추측해볼 수 있다. 

인사이트: ?????

분석 프로세스 
1. 크롤링 
수집한 사이트: yahoo finance
nasdaq composite(나스닥 지수) historical data - https://finance.yahoo.com/quote/%5EIXIC/history?p=%5EIXIC 
Bitcoin USD historical data - https://finance.yahoo.com/quote/BTC-USD/history?p=BTC-USD 

2018년 2월 21일부터 2023년 3월 1일까지 5년간의 나스닥 지수와 비트코인 가격을 크롤링함

2. 전처리
-특별히 처리한 것
비트코인은 주말을 포함하여 24시간 거래 가능하지만 주식은 휴장일이 있어서 결측치(missing value)로 인해 그래프가 그려지지 않았음. 
따라서 그 전날의 price 값을 이용해 missing value 를 채워주기로 하였습니다. 
비트코인의 date 값을 이용하여, price가 빈 dictionary를 만들어 나스닥 딕셔너리를 반복문을 돌려 price가 빈 딕셔너리에 그 전날의 나스닥 price를 넣어줌.  
비트코인의 date는 빈날짜 없으니까 date는 비트코인의 date를 이용을 하고 이중 for 루프를 통해서 비트코인  date데이터와 나스닥 date가 일치하면 나스닥 주가 가격이 있으면 그 해당 가격을 출력하고, 만약에 비트코인 date 데이터에 일치되는 나스닥 date 데이터가 없으면 그 전날의 나스닥 주가 가격을 사용하는 이런 코드를 만들었습니다.

3. 시각화
-사용한 라이브러리, 시각화 내용 
matplotlib를 사용해서 plot multiple lines on same graph 
