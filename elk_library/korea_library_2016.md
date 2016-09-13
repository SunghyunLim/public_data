#목적
1. 공공데이터를 활용, 전국 도서관 위치 가시화
1. 공공데이터에 있는 오류를 수정, 활용할 수 있도록 조정

#작업 내용
[데이터 정제]
1. 다운로드
  - data.go.kr에서 전국 [도서관 표준 데이터](https://www.data.go.kr/subMain.jsp?param=REFUQUdSSURAMTUwMTMxMDk=#/L2NvbW0vY29tbW9uU2VhcmNoL2RhdGFzZXREZXRhaWwkQF4wMTJtMSRAXnB1YmxpY0RhdGFQaz0xNTAxMzEwOSRAXmJybUNkPU9DMDAwMSRAXm9yZ0luZGV4PURBVEFTRVQ=) 의 excel 파일 다운로드
2. 분석 결과 다음의 이슈 발생
  - 특정 데이터의 경우 컬럼 값이 오른쪽으로 넘어간 경우 
  ![1.PNG](img/1.PNG)
  - 숫자에 따옴표/ 쉼표등이 함께 포함되어 있는 경우
  ![2.PNG](img/2.PNG)
  - 위치 정보등 컬럼에 빈 값이 존재하는 경우
  - 특히 휴뮤일의 경우 따옴표 존재, 쉼표, 근무일과 휴뮤일이 혼재되어 있어 총 371개의 유형 존재
  ![3.PNG](img/3.PNG)
  - 데이터 일부가 쪼개져 있는 경우
  ![4.PNG](img/4.PNG)
  - 보이지는 않지만, 숫자 뒤에 SPACE가 존재하는 경우
3. 엑셀에서 일부 수정 -> 구글 시트로 업로드 -> csv로 다운로드 한 뒤 정리 진행
  - 한글 인코딩의 문제를 가장 손쉽게 해결하는 방안

[데이터 업로드]
1. ELK 다운로드/ 설치
  - ElasticSearch(엔진) - Logstash(데이터 수집기) - Kibana(가시화)
  - https://www.elastic.co/kr/ 사이트 참조
2. 서비스 가동 
  - localhost:5601 서비스 확인
3. ElasticSearch에 속성 업로드
  - 아래 내용을 참조해서 속성 등록
```json
curl -XPUT http://localhost:9200/korea-library-2016 -d '
   {
     "mappings" : {
       "korea-library" : {
       "properties" : {
       "도서관명" : {"type" : "string", "index" : "not_analyzed" },
       "시도명" : {"type" : "string", "index" : "not_analyzed" },
       "시군구명" : {"type" : "string", "index" : "not_analyzed" },
       "도서관유형" : {"type" : "string", "index" : "not_analyzed" },
       "휴관일" : {"type" : "string", "index" : "not_analyzed" },
       "열람좌석수" : {"type" : "integer" },
       "자료수(도서)" : {"type" : "integer"},
       "소재지도로명주소" : {"type" : "string", "index" : "not_analyzed" },
       "운영기관명" : {"type" : "string", "index" : "not_analyzed" },
       "도서관전화번호" : {"type" : "string", "index" : "not_analyzed" },
       "홈페이지주소" : {"type" : "string", "index" : "not_analyzed" },
       "library_geo" : { "type" : "geo_point" },
       "데이터기준일자" : {"type" : "date" }
         }
       }
     }
   }'
```
  - Kibana에서 인덱스 추가함(time pased event 체크 풀기)
  ![5.PNG](img/5.PNG)
4. Logstash conf 파일 만들기
  - lib_csv.conf 파일을 다음과 같이 만들어본다.  
```json
input {  
  file {
    path => "F:/zz.project/easy/ELK/logstash-2.4.0/data/korea_library_2016.csv"
    start_position => "beginning"    
  }
}
filter {  
   csv {
       separator => ","
       columns => ["도서관명","시도명","시군구명","도서관유형","휴관일","운영시작시각","운영종료시각","열람좌석수","자료수(도서)","자료수(연속간행물)","자료수(비도서)","대출가능권수","대출가능일수","소재지도로명주소","운영기관명","도서관전화번호","부지면적","건물면적","홈페이지주소","latitude","longitude","데이터기준일자"]
   }
   mutate {
      convert => { "latitude" => "float" }
      convert => { "longitude" => "float" }
   }

   mutate {
      rename => {
          "latitude" => "[library_geo][lat]"
          "longitude" => "[library_geo][lon]"
      }
   }
   mutate {
       remove_field => ["운영시작시각","운영종료시각", "자료수(연속간행물)", "자료수(비도서)", "대출가능권수", "대출가능일수", "부지면적","건물면적"]
   }
}
output {  
    elasticsearch {
        hosts => ["127.0.0.1"]
        index => "korea-library-2016"
        document_type => "korea-library"
    }
    stdout {
        codec => rubydebug
    }
}
```
5. 실행 및 결과
  - bin/logstash.bat -f conf/lib_csv.conf 등과 같이 실행한다.
  ![6.PNG](img/6.PNG)
6. 업로드 결과
  - 모든 내용 업로드 되었는지 확인 
  ![7.PNG](img/7.PNG)

[가시화]
1. 