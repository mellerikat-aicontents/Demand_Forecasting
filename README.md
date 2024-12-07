# 📖Solution 

## What is Demand Forecasting?

Demand Forecasting은 최신 AI 기술을 활용한 수요 예측 솔루션입니다. 
Google의 TFT (Temporal Fusion Transformer) 알고리즘을 기반으로 설계되어 있으며, 
관측된 변수들과 프로모션, 공유일 등 미래 계획된 변수를, 그리고 제품 스펙과 같이 변하지 않는 변수를 적절하게 처리하여 예측을 진행합니다. 이로 인하여 영업 및 마케팅 전략에 맞춰 변화하는 부품 및 제품 수요나 주가 예측 등에서 뛰어난 성능을 발휘합니다. 추가적으로 구간 예측과 XAI 기능을 통해 예측의 이해도와 신뢰성을 높임으로써 사용자에게 폭넓은 활용성을 제공합니다.


## When to use Anomaly Detection?

Demand Forecasting 솔루션의 적용이 가능한 분야는 다음과 같습니다.
 
- 수요 예측: 과거 데이터뿐만 아니라 온도, 공휴일 프로모션 계획 등 해당 시점의 값 활용에 용이합니다. 따라서 각 시점의 전략에 영향을 크게 받는 수요 예측에 적합합니다.  
- 가격 예측: 예측의 불확실성을 파악하고 예측이 도출 된 설명을 제공하기 때문에 예측의 해석이 중요한 가격 예측 분야에 적합합니다.

## Key features and benefits 

**실험계획서를 이용한 간편한 사용성**  
- 해당 모델과 기능을 사용하기 위해서는 실험계획서의 argument만 변경하면 쉽게 사용할 수 있어, 간편하면서도 효율적인 사용성을 제공합니다. 

**시물레이션 기능**  
- 해당 솔루션은 개발단계에서 Causal Information을 파악하기 위하여 Simulation기능을 제공합니다. 해당 기능을 활용하여 변수가 의도한 대로 적용되었는지를 파악할 수 있습니다.

**수요 예측 적합성**  
- 해당 솔루션은 Trainsformer와 seq2seq LSTM 구조를 바탕으로 이루어져 있습니다. 
따라서 다양한 시점의 변수(Known/Unknown/Static)를 입력하기에 적합합니다. 
- Qunantile Regression을 활용하여 점추정이 아닌 구간추정을 진행하기 때문에 예측의 불확실성을 측정할 수 있습니다. Point Wise Feature Importance Bolck과 Attention Block을 통해 각 시점 중요도와 변수 중요도를 파악할 수 있습니다.

# 💡Features

## Pipeline

AI Contents의 pipeline은 학습과 추론을 위한 Asset과 XAI의 Visualization을 위한 Output asset으로 이루어져 있습니다.

**Train pipeline**
```
Train
```

**Inference pipeline**
```
Inference - Output
```

## Assets

**Train asset**  
실험계획서에 따라 데이터를 확보해서 Dataset을 갱신하고 모델을 학습하는 모듈입니다. 

**tft_model**
기존 Dataset & Model을 로드하거나 실험 계획서에 따라 Dataset과 Model을 생성하는 모듈입니다.

**Inference asset**  
Train asset에서 만들어진 Dataset과 Model을 불러와 구간 예측을 진행합니다.
예측 도중 생성된 Point wised Feature Importance와 Attention 정보를 저장합니다.

**Inference**
Inference와 XAI를 진행하는 모듈입니다.

**Output asset**
Output asset은 Inference asset에서 도출된 예측값과 XAI값을 기반으로 Visualization을 진행합니다. 


## Experimental_plan.yaml

내가 갖고 있는 데이터에 AI Contents를 적용하려면 데이터에 대한 정보와 사용할 Contents 기능들을 experimental_plan.yaml 파일에 기입해야 합니다. AI Contents를 solution 폴더에 설치하면 solution 폴더 아래에 contents 마다 기본으로 작성되어있는 experimental_plan.yaml 파일을 확인할 수 있습니다. 이 yaml 파일에 '데이터 정보'를 입력하고 asset마다 제공하는 'user arugments'를 수정/추가하여 ALO를 실행하면, 원하는 세팅으로 데이터 분석 모델을 생성할 수 있습니다.

**experimental_plan.yaml 구조**  
experimental_plan.yaml에는 ALO를 구동하는데 필요한 다양한 setting값이 작성되어 있습니다. 이 setting값 중 '데이터 경로'와 'user arguments'부분을 수정하면 AI Contents를 바로 사용할 수 있습니다.

**데이터 경로 입력(external_path)**  
external_path의 parameter는 불러올 파일의 경로나 저장할 파일의 경로를 지정할 때 사용합니다. save_train_artifacts_path와 save_inference_artifacts_path는 입력하지 않으면 default 경로인 train_artifacts, inference_artifacts 폴더에 모델링 산출물이 저장됩니다.
```
external_path:
    - load_train_data_path: ./solution/sample_data/train
    - load_inference_data_path:  ./solution/sample_data/test
    - save_train_artifacts_path:
    - save_inference_artifacts_path:
```

|파라미터명|DEFAULT|설명 및 옵션|
|---|----|---|
|load_train_data_path|	./sample_data/train/|	학습 데이터가 위치한 폴더 경로를 입력합니다.(csv 파일 명 입력 X)|
|load_inference_data_path|	./sample_data/test/|	추론 데이터가 위치한 폴더 경로를 입력합니다.(csv 파일 명 입력 X)|

**사용자 파라미터(user_parameters)**  
user_parameters 아래 step은 asset 명을 의미합니다. 아래 step: input은 input asset단계임을 의미합니다.
args는 input asset(step: input)의 user arguments를 의미합니다. user arguments는 각 asset마다 제공하는 데이터 분석 관련 설정 파라미터입니다. 이에 대한 설명은 아래에 User arguments 설명을 확인해주세요.
```
user_parameters:
    - train_pipeline:
        - step: input
          args:
            - file_type
            ...
          ui_args:
            ...

```

# 📂Input and Artifacts

## 데이터 준비

**학습 데이터 준비**  
1. 예측에 활용하고자 하는 모든 변수들이 모인 csv파일을 준비합니다.
2. csv 파일은 시간축, target, 독립변수를 포함해야 합니다.
3. csv 파일은 시간축과 값이 누락이 모두 없어야 합니다. 
4. 다수의 제품을 예측하는 경우 그룹 구분자 변수가 존재해야 합니다.

**Input data directory 구조 예시**  
- ALO를 사용하기 위해서는 train과 inference 파일이 분리되어야 합니다. 아래와 같이 학습에 사용할 데이터와 추론에 사용할 데이터를 구분해주세요.
- 하나의 폴더 아래 있는 모든 파일을 input asset에서 취합해 하나의 dataframe으로 만든 후 모델링에 사용됩니다. (경로 밑 하위 폴더 안에 있는 파일도 합쳐집니다.)
- 하나의 폴더 안에 있는 데이터의 컬럼은 모두 동일해야 합니다.
```
./{train_folder}/
    └ train_data1.csv
    └ train_data2.csv
    └ train_data3.csv
./{inference_folder}/
    └ inference_data1.csv
    └ inference_data2.csv
    └ inference_data3.csv
```

## 산출물

학습/추론을 실행하면 아래와 같은 산출물이 생성됩니다.  

**Train pipeline**
```
./alo/train_artifacts/
    └ output/train/models.train/
        └ dataset.ckpt
        └ tft_algorithm.ckpt
        └ train_config.yaml
    └ experimental_plan.yaml
```

**Inference pipeline**
```
 ./alo/inference_artifacts/
    └ output/inference/
        └ result_melt.csv
    └ extra_output/inference/
        └ result_weight_og.pickle
        └ weight_avg.csv
        └ weight_pivt.csv
    └ extra_output/output/
        └ decoder.html
        └ encoder.html
        └ static.html
    └ score/
        └ inference_summary.yaml

```

각 산출물에 대한 상세 설명은 다음과 같습니다.  

**result_melt.csv**
예측 결과값을 담고있는 csv 파일입니다.

**result_weight_og.pickle**
예측 시 도출 된 XAI 정보를 담고 있는 pickle 파일입니다

**weight_avg.csv**
XAI정보를 시점별로 평균화한 값입니다.

**weight_pivt.csv**
예측 진행 당 XAI 정보를 평균화 한 값입니다. 

**decoder.html**
weight_pivt을 바탕으로 Decoder의 XAI값을 visulaization한 그래프입니다.

**encoder.html**
weight_pivt을 바탕으로 Encoder의 XAI값을 visulaization한 그래프입니다.

**static.html**
weight_pivt을 바탕으로 Static의 XAI값을 visulaization한 그래프입니다.
