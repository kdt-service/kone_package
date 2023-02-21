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