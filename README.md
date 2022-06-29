### T 국가 세관 신고 데이터 기반 위법물 탐지 서비스 개발

<img src="https://github.com/erdosnumber0/project2/blob/master/practice/%EC%8B%9C%EC%97%B0%20gif.gif"/>

<video src='https://github.com/erdosnumber0/project2/blob/master/practice/NA1_%EB%8D%B0%EB%AA%A8%EC%8B%9C%EC%97%B0%EC%98%81%EC%83%81.mp4'>
</video>


- ppt: (https://www.slideshare.net/secret/GKYPRluBlVnRmr)




### About: 

- 팀명: NA1
- 팀장: 예상일
- 팀원: 전정재, 이보람, 안아련
- 작업 기간: 21.03.29 ~ 21.05.09
- Stacks
```
- Language: Python, SQL
- IDE : DataGrip, SQL developer,Jupyter Notebook, VSCode, Pycharm, Colab
- Library: Pandas, Numpy, Matplotlib, Plotly, Seaborn, statsmodels, 
           Scikit-learn, Keras, Tensorflow
- Framework: Flask
```



### 상황 설명:

2017.01~2020.11의 T국가 세관신고서를 이용하여 위법물(관세 포탈, 밀수 등) 적발 패턴을 분석 및 학습 \
이를 토대로 탐지 모형 개발.


### 이슈:

 - 약 210만개의 세관 신고 데이터에서 적발된 데이터는 약 1만건(0.65%)에 불과함(Imbalanced dataset)
 - T 국가는 통관 절차에 관한 인프라와 인력이 부족한 상황, 현실적으로 효용성이 있는 지표를 사용해야 한다. 



### 해결 과정:
- Oversampling: 범주형 변수에서 없던 숫자가 생성되는 데이터 왜곡 발생. \
  Undersampling: 소수의 샘플링 된 데이터가 전체를 대표할 만하다고 보기 어렵다. \
  Boost 알고리즘, *scale_pos_weight* 파라미터를 사용했지만 성능지표가 떨어진다.
- 계통 추출법: StratifiedKfold와 train_test_split의 파라미터(stratify=y)를 사용하여 불균형한 클래스를 일정하게 유지하려고 노력.\
  (StratifiedKfold는 데이터 불균형은 해결했지만 성능이 좋지 않았음)
- RandomForest, CatBoost, LightGBM, MLP, DNN 다양한 모델링 및 결과 비교.



### 결론:
- Train_test_split의 **stratify=y 파라미터를 사용하여 클래스의 비율을 유지**.
- **LightGBM 사용**: flask 구현 시, 속도가 가장 빠르고 평가지표 점수가 가장 높음.
- 평가 지표 **precision** 사용: 위법물이라고 판단한 사례 중 실제 위법물의 비율은 어느 정도인가? \
  현실적으로 의심되는 모든 상자를 열기 어렵다. \
  그러므로 precision을 중심 지표로 삼고, 이 후 파라미터 튜닝을 통해 recall 개선.
- Flask로 웹 제작 


### 개선할 점:
- 많은 노력에도 불구하고 여전한 과적합.
- 구현된 페이지는 저장된 pkl 파일을 사용, 데이터베이스에서 바로 추출할 수 있도록 개선해야 함.
