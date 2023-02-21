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
