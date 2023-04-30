# ETRI_2023_AI_KCC
제2회 2023 ETRI 휴먼이해 인공지능 논문경진대회 

## Abstract
> 인간은 언어적, 반언어적 요소를 통합적으로 파악하며, 감정을 인식합니다. 본 연구에서는 발화자의 텍스트와음성 및 생체 신호 데이터를 종합적으로 활용하여 중립, 기쁨, 슬픔, 놀람, 공포, 분노, 혐오 총 7가지 감정을 분류하는 멀티 모달리티 모델을 제안하였습니다. 그 결과, 단일 모달리티 모델보다 더 우수한 성능을 얻을 수 있었습니다. 따라서 감정 인식에 있어서 여러 모달리티를 종합적으로 활용하는 것이 효과적인 것을 알 수 있었습니다.

## DATA
> 

## 코드 내용
> Feature Extraction : 텍스트와 음성 데이터의 특징 벡터를 추출하고 단일 모델을 학습하는 파일입니다.
> - 텍스트 벡터 추출
>   - ratsnlp, Korpora, Konlpy, Transformers, pytorch 라이브러리를 사용하여 모델 구현하였습니다.
>   - ratsnlp의 경우 local에서 install 할 시 라이브러리, 의존성관리를 통해 버전 맞추기에 어려움이 있으므로 colab 환경에서 돌리는 것을 권장합니다.
>
> - 음성 벡터 추출
>   -
>
> KFold_fusion_model : 텍스트 특징 벡터, 음성 특징 벡터, 생체 신호 데이터를 활용하여 Early fusion, Late fusion 방식으로 멀티모달 모델을 생성하는 파일입니다.<br>
> - Early fusion 구현
>   - MLP 모델을 사용하여 텍스트 , 음성 특징 벡터 값을 input 값으로 넣고 grid search를 통해 파라미터 값을 활성화 시켜 모델을 학습 시켰습니다.
>
> - Late fusion 구현
>   - Optuna 라이브러리를 사용하여 음성과 텍스트 각각의 logit 값에 대해 최적화를 진행하였습니다.
>
> Preprocessing : 생체 신호 데이터를 전처리하고, 음성 데이터에 wav파일의 path를 추가하는 파일입니다.<br>
> -IBI 값 전처리
>   - NeuroKit2 라이브러리를 활용한 ECG 데이터를 IBI 값으로 변환하였습니다.
>
> (**데이터에 대한 정보는 해당 대회 주최측에서 비공개를 요청함**)

## Late, Early fusion Structure
### Early fusion
 ![image](https://user-images.githubusercontent.com/64082236/235346905-297c4f7d-c001-48af-8783-98246763683f.png)

### Late fusion
 ![image](https://user-images.githubusercontent.com/64082236/235346916-45cd612e-bbe9-45f5-b02c-1a8d1837ebef.png)


## Performance
> K-fold는 subset 1,2,3,4,5 데이터로 따로 저장
> 각각 subset에 대해 test를 진행하고 합산하여 평균값을 냄
> |Model|Setting|F-1 score|
> |:----:|:----:|:----:|
> |KcBert|Text|0.736|
> |Wav2vec|Audio|0.731|
> |MLP|Early fusion (Text+Audio)|0.752|
> |MLP|Early fusion (Text+Audio+Signal)|0.747|
> |Optuna+weight sum|Late fusion (Text+Audio)|0.778|
