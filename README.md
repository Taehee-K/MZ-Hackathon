2021 MZ-CEC 인공지능 해커톤 
===========================
축인삼묘: 김태희 이효경 정성경 정희원 한유경

파일 설명
---------
    - bert_epoch15(save_model).py: 훈련 스크립트 (google colab ipynb notebook을 .py형식으로 저장한 파일)
	- saved_model_epoch15: 훈련된 모델 파일이 저장된 폴더 
	- prediction.py: 평가용 prediction.py --> test data 사용시 기존 dev.txt 읽어오는 경로 수정 필요 
	- train.txt: 훈련용 데이터 
	- dev.txt: 훈련시 검증용 데이터 

파일 준비
--------	
### 채점용 데이터(test_intent.txt, test_sent.txt) 평가용 prediction.py와 같은 경로에 저장하기

### test 데이터 경로 prediction.py 파일에 지정해 주기 (기존 dev.txt를 채점용 데이터 test.txt파일로 바꿔주기)
    line 16: test = pd.read_csv('dev.txt', sep='\t', names=['purpose','sentence'], encoding='utf-8')
    -> test = pd.read_csv('test.txt', sep='\t', names=['purpose','sentence'], encoding='utf-8')
    
    > prediction_test.py 에 경로 수정 되어 있음. 

도커에서 prediction.py 실행
--------------------------

### 도커 이미지 다운로드
    docker pull kaggle/python 
> https://hub.docker.com/r/kaggle/python 

### (local 경로 찾기 - 생략 가능)
    wslpath "D:\Taehee Kim\2021 활동\MZ-CEC 인공지능 해커톤"


### 도커 컨테이너를 인터렉티브 배쉬로 실행
    sudo docker run -it  --name MZ-CEC -v /<system_above_path>:/<container inner path> kaggle/python:latest bash

    ex) sudo docker run -it  --name MZ-CEC -v /mnt/d/Taehee\ Kim/2021\ 활동/MZ-CEC\ 인공지능\ 해커톤:/mnt kaggle/python:latest bash

### 우분투에서 pip 설치하기
    apt install python3-pip

### 파이썬 패키지 설치하기
    pip3 install tensorflow_hub
    pip3 install keras tf-models-official pydot graphviz

### 예측 파일(평가용) 실행하기
    python3 prediction.py


실행파일과 같은 경로에 예측값인 result.txt 파일이 생성된다

## 모델 코드 설명
    1. Imports 
        필요한 패키지, 모듈 불러오기
    2. Data
            - google colab 환경에서 train.txt, dev.txt 불러오기
            - train, dev 데이터에 결측치가 5개 확인됨 -> 결측치 제거
            - train 데이터에서의 중복값 제거
            - 대중소분류에 고유 코드값 부여
        A. Train/Test split
            train, dev 라벨값과 예측 데이터 분류
        B. Label Encoding
        C. Tokenization
        D. Mask and Input Type
            실제 데이터와 padding 구분하게끔 mask
        E. Remake into function for normal use
            train 데이터의 최대 길이 * 1.5 에 맞추어 모든 데이터의 길이 통일시키기(padding)
            encode tokenized data
    3. Training
        setup model using the input train data, pre-trained BERT model, and output layer based on number classes
        setup training parameters
    4. Evaluation
    5. Model Saving for Later Use
    6. Validate Saved Model
        
        
