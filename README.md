# public_data
공공데이터 중 활용이 가능한 데이터셋을 모아놓는 곳 입니다.

1. 무더위 쉼터 정보
  - 공공데이터포털에서 가져왔으나, 링크 정보 불분명함.
  - 위도/경도 정보가 없어 지도에 표시하기 불충분
  - 주요정보: 상세주소(내용부실), 연도, 시설유형, 입력시간, 사용여부, 쉼터명칭, 지역코드
  - 파일: 무더위쉼터 현황(API).json
  
1. 전국 도서관 표준 데이터
  - url: [도서관 표준 데이터](https://www.data.go.kr/subMain.jsp?param=REFUQUdSSURAMTUwMTMxMDk=#/L2NvbW0vY29tbW9uU2VhcmNoL2RhdGFzZXREZXRhaWwkQF4wMTJtMSRAXnB1YmxpY0RhdGFQaz0xNTAxMzEwOSRAXmJybUNkPU9DMDAwMSRAXm9yZ0luZGV4PURBVEFTRVQ=)
  - 주요정보: 도서관명, 시도명, 시군구명, 도서관유형, 휴관일, 운영시작/종료시각, 열람좌석수, 자료수, 홈페이지 주소, 위도, 경도 
  - 파일: 전국도서관표준데이터.csv
  - csv, excel 등에서 따옴표가 겹치는 문제가 발생, 
  - xml은 테그 정보 오류, (, )등을 쓰는 문제가 있음.
  -> 정제 작업 필요!!!
  
1. 제주특별자치도 제주시 무더위쉼터현황
  - url : [제주시 무더위쉼터현황](https://www.data.go.kr/subMain.jsp?param=REFUQUAxNTAxMDgxOA==#/L2NvbW0vY29tbW9uU2VhcmNoL29yZ2luRGF0YVNldCRAXjAxMm0xJEBecHVibGljRGF0YVBrPTE1MDEwODE4JEBeYnJtQ2Q9T0MwMDA2JEBeZXhjZWxDb3VudD0wJEBeZG93bmxvYWRDb3VudD0zMiRAXm9yZ0luZGV4PURBVEEkQF5tYXhSb3dzPTEwMDAkQF5za2lwUm93cz0w)
  - 주요정보: 위치(주소 text), 쉼터명칭, 쉼터유형, 이용가능인원
  - 파일: 제주특별자치도+제주시_무더위쉼터현황_20150931.csv, 제주특별자치도+제주시_무더위쉼터현황_20150931.xml

* 추가 사항은 별도 작업/관리 필요
