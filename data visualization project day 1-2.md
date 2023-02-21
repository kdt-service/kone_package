# :chart_with_upwards_trend:정경원의 데이터 분석 패키지:books:

## 데이터 시각화 개인 프로젝트 

- 주제: 나스닥 가격과 비트코인 가격 간의 관계 데이터 시각화
- URL: 나스닥 지수 데이터 https://finance.yahoo.com/quote/%5EIXIC/history?p=%5EIXIC 칼럼 Close와 Date 에만 해당되는 부분 크롤링(기간이 바뀌었을 때 url이 어떻게 변하는지 확인 필요)
비트코인 가격 데이터 https://coinmarketcap.com/ko/currencies/bitcoin/historical-data/ 칼럼 날짜, 종가에 해당되는 부분 크롤링(비동기 방식, 날짜를 변경해보면서 network 탭에서 새 데이터 받아온 걸 확인하고 크롤링)

### 2/20/mon 1일차-비트코인 가격 데이터 수집

* 비동기방식으로 데이터를 불러오기때문에 일반적인 requests.get으로는 데이터 불러올 수 없음 
* 더 로드하기 했을 때 불러오는 request url을 나열해봄 (Network -> Fetch/XHR) 

* Request URL: https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical-on-this-day?id=1&convertId=2781 처음에 이걸로 시작함
https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical?id=1&convertId=2798&timeStart=1671494400&timeEnd=1676851200 start- 2022년 12월 20일 화요일 end-2023년 2월 20일 월요일
https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical?id=1&convertId=2798&timeStart=1668902400&timeEnd=1671408000 start- 2022년 11월 20일 일요일 end-2022년 12월 19일 월요일 
https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical?id=1&convertId=2798&timeStart=1666224000&timeEnd=1668816000
https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical?id=1&convertId=2798&timeStart=1663632000&timeEnd=1666137600 

* https://www.epochconverter.com/ 유닉스 시간을 이용한 파라미터를 사용하고 있었음

* 따라서 url= 'https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical?' 
* params = {
	'id' = 1,
	'convertld' = 2798,
	'timeStart'=1671494400, 
	'timeEnd'=1676851200  
}    

:question: 데이터를 가져오는 코드 짜기!   
:question: timeStart, timeEnd의 유닉스 시간 파라미터를 한달 주기로 반복하는 코드를 만든다   
:question: 한달 주기가 아닌 첫페이지는 따로 코드를 만든다?  

### 2/21/tue 2일차-비트코인 가격 크롤링 & 나스닥 가격 크롤링 
* 비트코인 가격 크롤링 코드  
***
1. 안되는 코드

왜 안되는지는 모르겠지만... 

    import requests 
    from bs4 import BeautifulSoup 

    URL= 'https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical'
    params = {
	'id' : 1,
	'convertld' : 2798,
	'timeStart': 1671494400, 
	'timeEnd': 1676851200 }   
    response = requests.get(URL, params=params)   
    #지정된 url과 params 딕셔너리를 조합하여 http get 요청을 보내고 해당 페이지의 html 코드를 반환   

    import json   
    json_result = json.loads(response.text)  

    bitcoin_list= soup.select('tbody > tr') # 열 하나씩 가져오기 

    bitcoin_close_price_by_date=[]
    for datenprice in bitcoin_list:
        date = datenprice.select_one('tbody > tr > td:nth-child(0)').text
        price = datenprice.select_one('tbody > tr > td:nth-child(4)').text
    bitcoin_list.append({
        'date': date, 
        'price': price}) 

***
2. 어째서인지 되는 코드 

의문인 코드.. 

    import requests  
    from bs4 import BeautifulSoup  

    URL= 'https://api.coinmarketcap.com/data-api/v3/cryptocurrency/historical'   
    params = {
	'id' : 1,
	'convertld' : 2798,
	'timeStart': 1671494400, 
	'timeEnd': 1676851200  
    }    

    response = requests.get(URL, params=params) 

    json_result = response.json()

    bitcoin_list = json_result['data'] # 리스트 안에 들어가는게 뭐임?????? 도대체 ㅠㅠ   

    bitcoin_close_price_by_date = []
    for datenprice in bitcoin_list:
        date = datenprice['timestamp']
        price = datenprice['close']
        bitcoin_close_price_by_date.append({
            'date': date, 
            'price': price}) 

    print(bitcoin_close_price_by_date)

*** 
* 나스닥 가격 크롤링 코드 

-- time period: 2022년 2월 21일 기준 ~ 2018년 2월 21일

https://finance.yahoo.com/quote/%5EIXIC/history?period1=1519171200&period2=1676937600&interval=1d&filter=history&frequency=1d&includeAdjustedClose=tru 

BdT Bdc($seperatorColor) Ta(end) Fz(s) Whs(nw) 이게 full 태그 
Py(10px) Ta(start) Pend(10px) date 태그 
tbody tr td:nth-child(0)
tbody tr td:nth-child(4)

    import requests 
    from bs4 import BeautifulSoup 

    URL = 'https://finance.yahoo.com/quote/%5EIXIC/history?period1=1519171200&period2=1676937600&interval=1d&filter=history&frequency=1d&includeAdjustedClose=tru' 

    headers = {'User-Agent': 'Mozilla/5.0'}
    response = requests.get(URL, headers=headers) #404 나와서 headers 추가함

    soup = BeautifulSoup(response.text, 'html.parser') 

    results = soup.select('tbody tr')
    #리스트 형태로 저장됨 

    nasdaq_list = []

    for result in results:
	date = result.select_one('td:nth-child(1)').text
	price = result.select_one('td:nth-child(5)').text

	nasdaq_list.append({
		'date': date,
		'price': price
	})

    print(nasdaq_list)
