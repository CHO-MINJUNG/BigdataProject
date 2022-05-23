# 코로나19 전, 후의 서울시 주요 서비스 업종 비교 분석 최종 보고서
## 프로젝트 주제
코로나19로 인해 많은 사람들의 일상이 크게 변화되었다. 사회 모습도 코로나19 이전과는 비교되는 모습을 가진다. 그 중에서도 서비스 업종에 큰 영향을 미쳤다는 것을 우리는 상권의 모습, 배달 앱의 큰 성장 등을 통해 확인할 수 있다. 그래서 정확한 데이터 수치를 이용하여 코로나19 이전에 어떤 업종이 주를 이뤘고 현재 어떻게 바뀌었는지, 어떤 업종이 주를 이루는지 등을 비교 분석하였다.
## 시스템 아키텍처
![image](https://user-images.githubusercontent.com/61917990/169749926-1a1493dc-f6f6-404f-9cde-436d5260ce1e.png)
## 데이터 수집
### 공공데이터
서울시 우리마을가게 상권분석서비스(상권-점포): 서울시 우리마을가게 상권분석 서비스(상권-점포)>데이터셋>공공데이터|서울열린데이터광장 
 http://data.seoul.go.kr/dataList/OA-15577/S/1/datasetView.do
서울시 코로나 19 확진자 현황: 서울시 코로나19 확진자 현황> 데이터셋> 공공데이터 | 서울열린데이터광장 (seoul.go.kr)
	http://data.seoul.go.kr/dataList/OA-20279/S/1/datasetView.do
### 네이버 블로그 내용
주요 상권의 직관적 수치 변화를 살펴보고자 공공데이터를 사용했다면, 실제 코로나 이전과 이후 사람들의 라이프 스타일이 어떻게 바뀌었는지 파악하고자 네이버 블로그 게시물을 크롤링하였다. 
-	크롤링 모듈
Python의 selenium을 사용하여 크롤러 모듈을 생성했다. 크롤러 모듈은 크게 세가지 부분으로 나뉜다. 첫 번째 게시글 작성 날짜를 크롤링할 때 포맷이 달라 발생하는 문제 해결을 위한 날짜 포맷 통일 함수, 두 번째 네이버 블로그 메인 페이지에서 키워드가 들어가며 기간 설정에 매칭되는 게시물 url을 가져오는 모듈, 마지막으로 가져온 url을 하나씩 방문하여 날짜 데이터와 게시글 내용을 가져오는 모듈로 구성된다. 
* 날짜 포맷 통일 함수

![image](https://user-images.githubusercontent.com/61917990/169750090-30ff69ab-843e-4d72-abf3-99617c6f16de.png)
* 게시물 url 가져오는 모듈

![image](https://user-images.githubusercontent.com/61917990/169750100-5640eb9e-d2c9-457e-b3a8-5cd417334e1c.png)
Text 변수에 검색할 키워드를, start와 end변수에는 수집할 게시글 들의 업로드 날짜를 설정하였다. 페이지 수를 573까지 한 이유는 네이버 블로그는 572페이지까지만 게시글들이 나오기 때문이다. 그 이상으로 넘어가면 한 게시물이 반복적으로 나오는 현상이 일어난다.

* 날짜 데이터와 게시글 내용을 가져오는 모듈

![image](https://user-images.githubusercontent.com/61917990/169750185-5fe5a8a9-80f9-44a8-a8ba-9a1986612553.png)
![image](https://user-images.githubusercontent.com/61917990/169750202-03ca421d-ea01-4e7b-952f-bf8f21ad0c75.png)

- 크롤링 결과

![image](https://user-images.githubusercontent.com/61917990/169750253-c80eca8f-596b-4b6d-b849-c9659acd2b95.png)

## 데이터 분석
### 공공데이터-확진자 누적 그래프
* 데이터 불러오기 & 전처리
![image](https://user-images.githubusercontent.com/61917990/169750346-ed9fc499-1516-4e7c-a7fd-d534804f435b.png)

분석을 원하는 년도는 20년도 이기 때문에 이 후 데이터는 지워주었으며 역순으로 데이터를 정렬하였다.
* 시각화
![image](https://user-images.githubusercontent.com/61917990/169750361-98d241bd-0964-4900-a1fb-83ef88accced.png)

위의 시각화 결과를 통해 3월, 8월, 11월 후반부터 12월까지 확진자가 급증하는 양상을 확인해볼 수 있다.

### 공공데이터-상권 분석
서울시에서 제공하는 서울시 우리마을가게 상권분석 서비스 엑셀 파일을 통하여 코로나 이전인 2019년도의 주요 상권과 코로나 이후인 2020년은 4분기로 나누어 각 분기별 주요 상권을 비교해 보았다. 데이터 시각화를 위하여 Zeppelin에서 spark를 사용하였다.
* 데이터 불러오기
![image](https://user-images.githubusercontent.com/61917990/169750456-1fdf4e32-b3ba-416e-8d42-6ba60855e744.png)

* 데이터 전처리
![image](https://user-images.githubusercontent.com/61917990/169750470-83a810a6-4c0f-4f48-84fb-3486f1d4d9e1.png)
![image](https://user-images.githubusercontent.com/61917990/169750484-440ed142-8108-4a2d-8992-d708cca49e76.png)

분석에 필요 없는 column들을 삭제한 후 사용의 편의성을 위하여 모든 컬럼들의 이름을 영어로 바꾸었다.
분석에 필요한 컬럼은 서비스 업종, 현재 점포 수, 개업 점포 수, 개업 율, 폐업 율, 폐업 점포 수 들이다. 2020년도 데이터는 분기별로 나누어서 분석해야 하기 때문에 기준 분기 코드 컬럼도 분석에 사용하였다.
* 데이터 분석과 결과
![image](https://user-images.githubusercontent.com/61917990/169750547-504199ee-f284-49c6-850c-d231dcbb19dc.png)

Spark sql을 통하여 2019년도 서울시 업종 데이터를 query하였다. 위의 sql문을 통해 도출된 결과는 2019년 서울시에 자리 잡은 업종 순위이다. 위와 같은 방식으로 2020년도 분기별로 서울시 자리 잡은 업종의 순위를 분석하였다.

![image](https://user-images.githubusercontent.com/61917990/169750736-487c14a4-64aa-4dc7-bafb-d6819a6f0666.png)

주요 업종과 더불어 개업한 업종의 순위를 알아보았다. 이도 똑같이 2020년을 분기별로 분석하였다.

![image](https://user-images.githubusercontent.com/61917990/169750766-27e56b41-a4cd-4fa5-ad67-c4a7ac1e491d.png)

다음으로는 평균 폐업 율 순위를 분석하였다.

* 분석 결과(주요 상권 순위)
![image](https://user-images.githubusercontent.com/61917990/169750881-6761af39-f7ab-481b-a3f8-1d29422a6980.png)
![image](https://user-images.githubusercontent.com/61917990/169750887-715923e5-a438-4201-8168-ae1a9e85f830.png)

왼쪽: 2019 주요 업종, 오른쪽: 2020 4분기 주요 업종
주요 업종 데이터에서는 분기별로 큰 차이가 없었기에 가장 차이가 명확한 19년도 데이터와 20년도 4분기 데이터를 비교했다. 주목할 부분은 코로나의 타격으로 인해 노래방 상권이 19년도에 비해 20년도에 굉장히 약해진 것을 확인할 수 있다. 뿐만 아니라 일반 교습학원은 20년도 순위권에서 사라진 것을 확인할 수 있다. 그에 반해 육류판매 업종은 20년도에 순위권에 올라온 것을 확인할 수 있다.  

* 분석 결과(개업한 업종 순위)
![image](https://user-images.githubusercontent.com/61917990/169750959-9a4bfe49-1cb2-4496-8fad-4c6b60c43a4c.png)
![image](https://user-images.githubusercontent.com/61917990/169750967-37b4b82d-3739-4548-b2d1-fcc851b86e53.png)

왼쪽: 2019 개업한 업종 상위, 오른쪽: 2020 4분기 개업한 업종 상위
큰 차이는 없었지만 코로나로 인해 호프, 간이주점의 개업 수가 줄어든 것을 확인할 수 있으며 스포츠 강습 업종은 순위 권 밖으로 밀려났다.

