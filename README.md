# BigData Programming Term Project (2023-2)

### Project Overview
: MZ를 위한 카테고리별 카페 분류 프로그램

[노션 프로젝트 페이지](https://heathered-felidae-571.notion.site/59b728d8b08c45e6a88a9bb8caa69714?v=3c8d18a732144636a699e861f70889fc&pvs=4)

<img width="1098" alt="스크린샷 2023-12-12 오후 2 50 55" src="https://github.com/cabojago/cabojago/assets/69382168/bef8c657-d071-4b3c-b2e0-2055a4d2ac41">


## Data Collection
[ 데이터 수집 방법 ]
- 크롤링(selenium, bs4)   
   - selenium: 웹 브라우저를 이용, 웹 사이트 제어를 위해 사용
   - bs4: html에서 데이터 추출, html 및 xml 문서를 parsing 하기 위해 사용

[ 개인별 데이터 수집 ]
- 이지은: 동네별 카페 리스트(카페 이름, 카페 타입) 크롤링
- 양채연: 카페의 리뷰 수, 주소, 고유 ID 크롤링
- 김도연: 카페 방문자 리뷰 크롤링
- 박하나: 카페 블로그 리뷰 크롤링

[ 데이터 전처리 ]
- 파일명: 정해진 규칙에 따른 영문 파일명으로 변경
- 카페 상세 정보: 주소, ID에 따른 이상치 처리
- 분석 데이터: konlpy를 이용하여 카페 리뷰 데이터의 명사, 형용사 추출

[ 데이터 분석 기법 ]
- spark를 이용한 wordcount
- 전체 카페 word 빈도수를 통해 카테고리별 키워드 선정
- pig를 이용한 테이블 조인
- 카페 카테고리별 분류 카운트 수 확인 및 가중치를 다르게 부여
- 최종적으로 가중치를 부여한 카테고리당 카페 분류


## Project Progress

#### 11/24
- 개인별 데이터 수집 시작

#### 11/29
- 박하나: 크롤링한 동네별 카페 리스트를 사용하여 블로그 리뷰 수집
- 양채연: 크롤링한 동네별 카페 리스트에 카페의 추가 정보(카페 고유 ID, 방문자 리뷰 수, 블로그 리뷰 수, 주소) 추가
  ( `cafelist_동네명.csv` -> `output_cafelist_동네명.csv` )

#### 12/4 
- 팀 hdfs 생성
- 각 폴더명은 본인이름
- 카페 종류 선정
   - [열중열중! 카공 카페, 달다구리 디저트 카페, 귀염뽀짝 애견동반 카페, 인생은 혼자.. 혼자갔을 때 눈치 안보이는 카페, 코히러버들을 위한 커피맛집 카페, 꽁냥꽁냥 데이트하기 좋은 카페, 힐링하고싶어? 뷰맛집 카페, 환경은 소중해! 친환경 카페, 인스타그래머블 카페]
- 카페 종류별 키워드 선정(상세내용은 노션페이지)

#### 12/5
- 수집한 데이터 hdfs에 업로드

#### 12/7
- 카페 정보 이상데이터 전처리 (cafe_info_list_seongsu.csv ...)
- 리뷰 파일 한글 타이틀 => 동네축약어+카페 고유 ID+리뷰형태 형식으로 변경 `ex) 망원동 퍼스널커피_blog_content.csv -> mw_11489338_blog.csv`
  - 변경 예외처리 코드도 작성
- local에서 konlpy를 이용해 텍스트 형태소 분석 & 명사, 형용사 추출
- spark wordcount 짜기

#### 12/8    
- 전처리한 카페 정보 리스트을 기준으로 다시 방문자 리뷰를 크롤링해 dafaFrame에 카페 고유 ID 컬럼추가

#### 12/9
- konlpy 결과를 이용해 블로그 리뷰 spark 워드 카운트 진행 (동네별, 한 카페당 워드카운트)   
- 카페별 리뷰 단어의 워드 카운트 결과를 다시 필터링   

#### 12/10     
- 리뷰 단어 카운트 결과를 키워드로 받아 카페 카테고리별로 매핑

#### 12/14
- pig를 이용한 테이블 조인
- 전체 카페 최종 분류

- 예시)
   - 한남 [💻 열중 열중! 카공하기 좋은 카페]
   <img width="825" alt="result" src="https://github.com/cabojago/cabojago/assets/69382168/11aec487-f8f1-493d-8620-cb03480613e9">
   - 한남 [🙏 내가 바로 트렌디세터, 인스타그래머블 카페]
  


