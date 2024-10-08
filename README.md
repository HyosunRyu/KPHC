# KPHC: Korean Political Hate Speech Classifier
한국어 정치적 혐오표현 분류 모델 및 데이터세트

## 1. 개요 (Overview)
본 프로젝트는 한국어 온라인 댓글에서 정치적 혐오표현을 식별하는 분류 모델을 학습시키기 위한 데이터세트와 코드를 제공합니다. 

## 2. 데이터 설명 (Dataset Description)
* 네이버 뉴스 사이트를 통해 2022년 한국 대통령 선거 관련 뉴스에 달린 댓글 2,750만 건을 수집했습니다.
* 전체 댓글 가운데 2만 건(약 0.07%)을 무작위 표집하여 학습 데이터세트를 구축했습니다.
* 본 데이터세트에서 혐오표현은 "특정 집단에 대한 부정적인 편견과 고정관념을 바탕으로 그 집단이나 집단의 구성원을 공개적으로 모욕·비하·멸시·위협하거나 그들에 대한 차별·폭력을 선동하는 행위"로 정의됩니다.
* 증오선동은 혐오표현 중에서도 "차별적 조치(불이익, 배제, 분리 등) 또는 물리적 폭력(공격, 박멸, 절멸)처럼 처벌적 행동을 주장하는 발언"을 의미합니다.
* 따라서 **정치적 혐오표현**은 "정치적 반대편에 대한 부정적인 편견과 고정관념을 바탕으로 상대후보 또는 그 지지자들을 모욕·비하·멸시·위협하거나 그들에 대한 차별·폭력을 선동하는 행위"로 정의되며, 그 중에서도 차별과 폭력 등 구체적인 행동을 선동하는 발언은 **정치적 증오선동**으로 구분됩니다.
  
### 2.1. 데이터세트 예제 (Examples)
  |사례|혐오 <br> 표현|증오 <br> 선동|
  |:---|:---:|:---:|
  |안은 정치 그만둬야 서울시장  나올때  대선은  안 나온다고  공언  했는데  뭐냐 할때마다 약속을 안지키냐|0|0|
  |국짐쓰레기들의  물타기작전이지 ... 한심한것들...|1|0|
  |이죄명 두모녀살인범조카놈변호나하는사이코패스놈 흥분하는거보니대장동은죄명이꺼맞네 <br> 빼박이구먼 파렴치하고독사같은놈 되려명박이가더순진했구먼 그네는쨉도안되고|1|0|
  |이땅에서 운동권주사파 좌빨 사회주이선봉자들 모조리 청소해야됩니다 그래야 자유  대한민국된다|1|1|
  |서민 피빨아먹은 여자...구속하라 사형시켜라|1|1|
    
### 2.2. 주석 방법 (Annotation Methods)
* 1명의 연구자가 댓글의 혐오표현 및 증오선동 포함 여부를 분류
* 새로운 2명의 주석자가 10%에 해당하는 2천 건의 댓글을 분류한 후 3명의 신뢰도를 평가
* 신뢰도 평가를 위해서는 급내상관계수(Intraclass Correlation Coefficient, ICC)를 계산함.
  
  ||혐오표현|증오선동|
  |:------:|:---:|:---:|
  |코더 간 신뢰도(ICC)|0.70|0.66|
* 최종적으로 2명의 연구자가 데이터세트를 검수함.
* 정치적 혐오표현 및 증오선동에 대한 판단은 개인의 정치적 성향에 따라 다를 수 있습니다. 주석자들은 편향적 판단을 피하기 위해 최선을 다했지만, 다른 판단도 존중합니다. 주석에 대해 다르게 판단하거나 오류를 발견한 분들은 Issues를 통해 보고해 주세요.
  
## 3. 모델 설명 (Model Description)
* 본 프로젝트에서는 KcELECTRA 모델을 기반으로 한 전이학습(transfer learning) 기법을 활용하였습니다.
* KcELECTRA는 한국어 온라인 댓글 3.4억 건을 사전 학습한 사전 학습 모델로, 한국어 텍스트의 자연어 처리에 특화된 모델입니다.
* 이 프로젝트에서는 정치적 혐오표현을 분류하기 위해 KcELECTRA를 정치 뉴스 댓글 2만 건에 맞춰 미세 조정(fine-tuning)하였습니다.
* 모델은 정치적 반대편을 겨냥한 혐오표현 및 증오선동을 탐지합니다.

## 4. 모델 사용법 (Model Usage)
Google colab을 통해 모델 학습과 테스트를 할 수 있는 노트북을 제공합니다.
[정치적 혐오표현 분류 모델 학습](https://colab.research.google.com/github/HyosunRyu/KPHC/blob/main/KPHC.ipynb)

### 4.2. 정치적 증오선동 분류 모델 학습

## 5. 결과 (Results)
    |사례|혐오표현|증오선동|
    |:------|:---:|:---:|
    |“가는 곳마다 다 해준다”, 이건 다 못 해준다는 말이다.|0.004|0.000|
    |쥴리와 윤텅텅은 나라 망신 시키지 말고 귀국해라|0.989|0.008|
    |좌빨들은 모조리 총살시켜야 한다! 자유대한민국을 살리는 길이다!|0.988|0.999|
