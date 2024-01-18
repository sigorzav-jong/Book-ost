# deep.daiv_BookOST
deep.daiv 여름 추천시스템 Book-ost 프로젝트


--------------------------------------------------------------

#### 폴더 + 데이터 파일 설명

힙합 제거 반영 : audio feature 감정 추출 모델 학습시킬때부터 힙합 제거된 상태의 데이터셋 이용, 최종 모델링할때 기존 데이터셋과 힙합 제거된 것과 같이 처리해서 최종 결과에서 힙합 제거
=> 'audio feature 감정 추출 모델' 폴더는 힙합 제거 후인 상태


--------------------------------------------------------------

[데이터 수집/Spotify]
- af_전체.csv : 힙합 제거 전
- 노래_전체.csv : 힙합 제거 전
- af_전체_장르포함.csv : 힙합 제거 후
- 노래_전체_장르포함.csv : 힙합 제거 후

--------------------------------------------------------------

[audio feature 군집화]
- audio feature 데이터 : 데이터 수집/Spotify/노래_전체.csv
- 클러스러별 af 데이터 : k=6/cluster1_2 ~ cluster6_2

--------------------------------------------------------------

[노래 데이터 통합]
- 크롤링한 노래 가사 데이터 : lyrics1.xlsx ~ lyrics10.xlsx
- T5 가사 요약 후 가사 데이터 : lyrics_summary_final.xlsx
- 노래 가사 텍스트 전처리 결과 : song_keyword.xlsx

=> song_keyword_culst1.xlsx ~ song_keyword_culst6.xlsx
: 노래 클러스터마다 기본정보+요약된 노래가사+그걸 텍스트 전처리한 결과+클러스터 정보 다 통합한 상태


--------------------------------------------------------------

[text 감정 추출 모델]
- 원래 트위터 감정 데이터 : tweet_emotions.csv
- DA 후 트위터 감정 데이터 : tweet_data_agumentation.csv
* 노래 가사 감정 벡터 추출 (은서) : 노래 가사 text에 SVM 모델 적용
* 도서 text 감정 벡터 추출 (은서) : 도서 설명 text에 SVM 모델 적용
* EDA 진행 (해원) : 트위터 데이터에 EDA 진행하는 과정, 은서->정리

=> lyrics_sentiment.xlsx : 노래 DB 전체 가사 text 감정 벡터 추출 결과
가사 감정 추출할 때, 은서 언니 방식으로 텍스트 전처리한 상태에서 SVM 모델에 적용 (앞서 내가 진행했던 song_keyword_culst1.xlsx의 텍스트 전처리 결과를 사용하지 않았고)



--------------------------------------------------------------

여기서부터 힙합 제거 + 해원 index 수정

[audio feature 감정 추출 모델]
- song_clust.csv : 노래 DB 전체 + af 기반 mood labeling 결과 + 힙합제거
=> song_final_data.xlsx : 노래 DB 전체 af 기반 감정 벡터 추출 결과

- 18books_song_data : 도서-ost 데이터셋 만들기 위해 수집한 18권 책들의 ost 노래 정보
- plus_books_song_data : 18books_song_data에 spotify id 정보 추가한 책-노래 DB
- to_make_af : plus_books_song_data에서 'O'에 해당하는 행만 추출 (Spotify에 존재해서 af 값을 받아올 수 있는 행만 추출)
- traindata_df : to_make_af 노래들 af 정보까지 추가한 상태

- book_ost_CustomModel_sentiment : custom 모델로 돌린, 전체 노래 DB + book_ost의, af 기반 감정 확률 벡터 추출 결과
=> book_ost_song_final_data.xlsx
: song_clust.csv 내용 + 전체 노래 DB + book_ost, af 기반 감정 확률 벡터 추출 결과


* af 기반 감정 추출 모델 학습 (전체 노래 DB + book_ost, 은서) : book_ost 데이터 추가 후
* af 기반 감정 추출 모델 학습 (전체 노래 DB만 한 경우, 은서) : 처음 시도

=> 해원 수정 후
- book_ost_CustomModel_sentiment (해원)
- book_ost_song_final_data (해원).xlsx
- af_sentiment (해원).xlsx 
: song_clust.csv 내용 + 전체 노래 DB만 + 장르,  af 기반 감정 확률 벡터 추출 결과
* af 기반 감정 추출 모델 학습 (전체 노래 DB + book_ost, 해원)
 : book_ost 데이터 추가 후 + index 오류 수정



--------------------------------------------------------------

[전체 데이터 통합]
* 가사 기반 + af 기반 감정 벡터 통합 (은서)
- final_data : 가사/af 기반 감정 확률 벡터 통합한 상태
=> 해원 수정 후
* 가사 기반 + af 기반 감정 벡터 통합 (해원)
- final_data (해원)



--------------------------------------------------------------

[최종 모델링]
- 최종 파일 (은서) : 결과물로 제출할 때 사용한 최종 파일
*최종 유사도 계산 + 플리 제시 (은서)
- 최종 파일 (해원 수정 후) : custom 모델 수정 후 최종 파일
*최종 유사도 계산 + 플리 제시 (해원)


[최종 웹 구현]
- 위에 '최종 모델링' 폴더 내 2가지 최종 데이터 파일 중에서 원하는거 여기로 끌고와서 웹페이지 돌리기
- app.py : 기존 제출본, BeautifulSoup으로 크롤링
- app(해원).py : 에러 수정, Selenium으로 크롤링
- Selenium으로 크롤링한게 시간이 훨씬 오래 걸림






