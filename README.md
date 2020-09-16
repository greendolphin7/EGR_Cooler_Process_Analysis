# EGR_Cooler_Process_Analysis

## 2020.08.16
EGR Cooler 제품 공정 데이터 분석 

EGR Cooler를 조립하는 공정에서 추출된 데이터를 이용해 설비의 시간, 성능, 품질에 대한 종합효율을 보여줄 수 있는 지표인 _OEE_ 를 시각화해서 보여줄 수 있다.


## 2020.08.18
EGR Cooler를 만드는 생산 공정에서의 데이터 추출, 분석을 통해 품질 개선, 공정 개선 및 OEE 측정을 목표로 한다.

현재까지 생각해본 사항으로 프로젝트 진행사항을 정리해보자면 크게 5가지로 나눌 수 있다.

1. 제품에 대한 이해(EGR 쿨러, EGR 밸브), 작동원리, 제품의 역할, 기대하는 품질 수준

2. 제품을 만들기 위한 공정 세부 사항
+ 부품 리스트
+ 재료와 재료 특성
+ 설비의 세부 사항
+ 해당 제품의 BOM

3. 공정 세부 사항 별 나타날 수 있는 상황
+ 예) 가동, 비가동

4. 추출해야 할 데이터 선정, 공정 상황과의 연결, 연결 근거 작성

5. 데이터 분석

(6. 대시보드 만들기) 


## 2020.08.19
프로젝트를 하며 한가지 깊게 깨닫게 된 것이 있다.

데이터 분석은 결국 모든 과정의 맨 마지막 단계라는 것이다.

어떤 데이터를 수집할지 선정하는 단계에서 

왜 그 데이터를 수집해야하는지, 어떤 장비로 수집할 수 있는지에 대해 스스로 납득할 수 있을 만큼 치열하게 고민해야한다.

공정 데이터의 경우에는 특히 그러한 점이 더 크게 작용하는 것 같다.

공정에 대한 상세한 부분까지 알고 있어야하고, 하나의 완제품을 만들기 위해 쓰이는 부품, 재료, 설비, 시간 등등과

그 제품이 하는 역할과 작동원리, 품질에서 기대하는 기대치와 허용치를 모두 다 알아야 한다.

## 2020.08.20
데이터 선별과정에서 고려해야 할 사항으로는
- 생산실적 데이터
- 설비 데이터
- 작업자 데이터 
- 환경 데이터 
- 순차적 품질데이터
- 생산시점 관리를 위한 데이터 (POP)
- 로봇 작동 데이터
- 설비가 비가동인 경우, 가동인 경우의 현황 데이터

등등이 있다. 

각 중점, 설비, 데이터 수집 장비별로 더 세분화해서 선별할 수 있다.


## 2020.08.21

데이터를 8월 20일에 데이터 선별 기준으로 8가지를 설정한 기준으로 선별하려고 한다.

공정은 조립공정이라고 가정하고 공정에 들어가는 부품리스트에 대해 찾아보았다.

부품 리스트 (EGR Cooler)
- Body
- Wavy fin
- Flange
- 냉각수 유입 유출 Tube
- Paste (부가 재료)

## 2020.08.22
오늘은 프로젝트의 분석 목적에 대해 설정해볼 필요가 있었다.

조립 공정에서의 데이터들을 생성하여, 생성된 데이터들을 Train, Test 데이터 셋으로 나눈다.

최종 Target 변수를 양품과 부적합품이라는 2개의 클래스로 나눈 다음, 머신러닝 알고리즘(의사결정나무, Random Forest, XGboost, SVM 등등)을 사용하여 학습시킨다. 

Test 데이터를 사용하여 정확도를 측정한다.

그렇게하여 나온 하나의 학습 모델을 기반으로 새로운 변수를 추가하여 모델을 더 복잡하게 만들어 볼 수 있고,

부적합품을 만들어내는 Factor 들을 추정하거나 서로 다른 데이터들의 상관관계와 인과관계를 유추해볼 수 있도록 한다.


## 2020.08.23
전체 조립공정에서 나온 완제품이 부적합품으로 판정될 수 있는 기준들을 생각해보았고, 그 기준들과 관련된 데이터들을 선별해 보았다.

여기서, 몇가지 상황들을 미리 가정하고 시작했다.

총 공정은 op10 ~ op60번 공정이 있다.

각각의 공정에서 부품들은 BOM 순서대로 조립된다. 

부품은 총 6개로, A - B - C - D - E - F 이다

그러므로 op10 번 공정에서는 A + B = AB (재공품)이 된다.

본 모델은 가장 간단한 모델로, 하나의 설비에서 작업하는 시간은 10초로 가정하였고, 대기 시간과 이동시간, 셋업시간은 0으로 계산하였다. (추후 더 복잡한 모델로 추가할 예정이다)

마지막 op 60 에서는 전체 완제품에 대한 비전검사를 실시해 부적합 여부를 판단한다.

이러한 공정이라고 가정하였을 때, 데이터들을 선별해 본 결과가 아래이다.

- 제품 치수 (길이, 너비, 높이)
- 조립 압력을 측정하기 위한 전류값
- 비전센서의 환경 변수를 고려하기 위한 조도 값
- 제품의 흠집 여부
- 어떤 작업팀의 관리하에 있었는지에 대한 정보
- 제품 추적을 위한 시계열 데이터
- 최종 양품 여부 (한 공정에서 다음 공정으로 넘어갈 때 적합 여부)

op 40 ~ op 50 의 경우, 용접을 통한 조립이라고 가정하여 용접 온도에 대한 데이터를 추가하였다.

각 데이터들에 대한 형태(binary, distribution)와 부적합품 기준(허용공차)에 대한 논의를 거친 후 데이터들을 랜덤으로 생성할 계획이다.

## 2020.08.24
각 데이터들에 대한 데이터 형태와 허용 공차

- 부품 치수 : 정규분포(목표값과 분산) 허용공차는 0.01mm으로 예를들어 목표 치수가 50mm 라면 49.99mm ~ 50.01mm까지 양품으로 판정한다.
- 조립 후 재공품 치수 : 설계했던 부품과 부품의 치수를 더해 나간다.
- 제품의 흠집 여부 : 비전센서를 활용해 (0, 1)로 판단한 결과값을 추출 (베르누이 분포, 확률 0.9999999로 1, 1 일 때 양호한 상태)
- 부품과 부품 또는 재공품과 부품간의 조립할 때의 조립 압력 데이터를 얻기 위해 조립 설비에서의 조립 전류값을 출한다. 
- 조립 전류 값의 경우, 일정량 이상으로 넘어가게 되면, 재공품이 파손되거나 손상되기 때문에 최대값과 최소값을 지정해 주어야 한다. (현재로서는 최대, 최소값이 있는 균일분포로 추출)
- op40 ~ op50 의 경우 용접을 통한 조립 작업이기에, 제품의 조립강도를 측정하기 위해 조립용접시 조립시 온도를 측정할 필요가 있다. 조립 전류값과 마찬가지로 최대값과 최소값이 존재하며, 최대, 최소가 있는 균일분포로 넣을 생각이다.
- 비전 센서 사용 시, 조명이 양호한지에 따라 판정 결과에 큰 영향을 받게 된다. 따라서 조도 센서가 양호한지에 대해 추출 (0, 1로 판단한다. 0일 때 제대로 판정하지 못한다고 판단, 관련해 측정력이 어느정도 떨어지는지는 차후 수정해야 한다.) 
- 어떤 작업팀에 의해 관리되고 있었는지에 대한 변수를 넣어주고 기록하기 위해 생산 작업팀에 대한 정보 (3조, 2교대라고 가정. 총 3팀이 일정시간만틈 돌아가며 작업)
- 모든 공정 데이터는 시간데이터가 있어야 제품을 추적할 수 있기 때문에 시간데이터들을 각 공정이 완료된 시점으로 넣어준다. (ex. 2020-08-24-08:12:30)
- 각 공정에서의 제품에 대한 품질 판단을 실시한다고 가정하고, 다음공정으로 넘어가기 전에 양품과 부적합품을 가려낸다. (0, 1)


## 2020.08.25

개별 공정안에서 부품과 부품이 조립되어지는 힘과 길이간의 함수관계에 대해 생각해 보았다.
op10 ~ op30 까지의 경우, 조립되어지는 전류 값에 의해 조립되어져 들어가는 길이에 영향을 준다고 생각하였다. (그 최대값은 10mm 라고 가정하였다.)
따라서, 예를들어 90 ~ 100mA 의 전류로 조립을 실시하게 되면 Body 부품과 Pipe 부품간의 결합길이는 10mm이다.

문제는 90이하인 경우, y = 0.11x 라는 선형함수 식으로 조립되어지는 길이에 영향을 준다고 설정하였다. (x는 전류값 mA, y는 조립되어 들어가는 길이, 전류값은 89~100까지 균일분포 랜덤)
이런식으로 최종적으로 op 60까지 공정에서 치수에 대한 불량을 측정함에 있어서 어떠한 불확실성을 선형함수를 통해 주는것이 맞는것인가에 대한 확신이 아직 없다.

모델을 설계하고자 하는 목적은 공정이 끝난 후 측정된 데이터들을 가져왔을 때, 이 제품은 양품인가 부적합품인가에 대한 분류를 예측하고자 함이었다.
이것보다 각 공정이 끝난 후 측정된 데이터들을 가지고 앞으로 있을 공정에서 최종 불량처리 될 확률을 미리 예측할 수 있는 방향으로 갈 수도 있겠다는 점이 고민이다. 
지금은 Base 모델을 만들고자 함인데, 모델에 어떠한 불확실성을 어떤 방법으로 줄 수 있는지에 대해 생각해봐야겠다.

1) 치수에 대한 불확실성을 선형함수관계로 미리 정해놓으면 분석하는 의미가 있을까?
2) 최종적으로 모두 측정할 데이터를 가지고 불량 양품을 예측하는것이 의미가 있을까? 차라리 공정에 들어가기 전에 미리 알 수 있어야 의미가 있지 않을까?
3) 모델의 데이터를 가지고 어떤 결과를 내고 싶은지에 대해 더 고민해보아야겠다.

## 2020.08.26

조도 센서로 측정할 수 있는 빛의 밝기와 그게 영향을 받게 되는 비전센서에서의 측정력에 집중해보았다.
결과적으로 조도센서가 0.999의 확률로 상태가 양호(0)하고, 0.001의 확률로 양호하지 않다(1)고 할 때(랜덤 베르누이 분포)
양호인 경우에는 원래 측정되어야할 값 그대로 측정되었다고 가정하였다.
반대로 양호하지 않은 경우에는 측정되어야할 값보다 더 적은 값이 측정되도록 균일분포(0 ~ -0.01)값을 곱한 값을 넣었다.

전체적으로 모은 데이터들을 엑셀파일에 저장한 것들은 모두 제품이 생산되어지고 나서 얻은 데이터 값들이라고 할 수 있다.
따라서 10000개의 랜덤 데이터들을 뽑았다는 것은 제품을 10000개 생산했다는 의미와 다르지 않다.

어제 고민했던 문제는, 그렇다면 공정이 모두 끝난 후 얻은 새로운 데이터들을 가지고 양품과 부적합품을 판변한다는 것은 의미가 없다는 생각이 들었다.
그래서 생각했던 부분은, op 10 ~ 30 까지의 과정은 조립공정 중 비용이 상대적으로 적은 축에 속하고, 반대로 op 40 ~ op 50 공정은 비용이 상대적으로 더 든다고 할 때
op 30까지의 공정에서 나온 결과값을 가지고 최종적으로 이 재공품이 양품일지에 대한 판단을 미리 내릴 수 있다면, 다음 공정으로 넘어가는 것이 합리적인지에 대한 
판단이 가능할 것이고, 곧 품질과 비용에 대한 최적화가 가능할 것이다.
따라서 분류 또는 회귀문제를 적용할 수 있겠다는 생각이 들었고, 먼저 선형회귀 모델을 사용하여 넣을 생각이다.

내일은 온도에 대한 op 40 ~ op 50 까지의 공정을 통해 최종적인 값을 알아내고, 양품 (0, 1)을 판별할 수 있는 데이터 셋을 완성시켜 보아야겠다.


## 2020.08.27
온도에 대한 데이터값은 온도 값이 500에서 최적값이기 때문에 그대로 계산하고 490 미만, 510 초과된 온도 값에서는 불량이 나오도록 하였다.
Data set을 최종적으로 완성하였다.

Data set을 train set : test set = 8 : 2 비율로 나누었고 Decision Tree 모델을 적용해 적합품과 부적합품을 분류할 수 있도록 학습시켰다.
이전에도 언급했듯이, op30 공정은 단순 조립공정이고 op40~50 공정은 용접을 통한 조립공정이기 때문에 
용접조립 공정이 상대적으로 비용이 많이 들고 실패했을 때 실패비용이 크다는 점으로 상황을 설정하였다.
따라서 학습 시 op30까지의 결과값을 가지고 학습시켜 op 40 ~ 50까지 거친 후의 결과값을 미리 예측할 수 있으면 학습하는 의미가 있겠다는 생각이 들었다.

Decision Tree 모델을 적용하고 정확도가 98.1% 나오게 되었다.
앞으로는 XGboost과 같은 다른 알고리즘을 통해 학습시켜 결과값을 비교해보도록 하겠다.

## 2020.08.29
Data Set을 Decision Tree, Random Forest, Logistic Regression, KNN, XGboost, DNN 을 넣어보고 각각의 모델 성능을 비교해보았다. 
XGboost가 가장 뛰어난 성능을 보여주었고, Decision Tree, Random Forest가 그 다음이었다.
어떤 요인이 제품 부적합에 가장 영향을 미치는 중요 요인인지에 대해서도 선별할 수 있었다.
문제는 당초 op40~50 까지의 공정이 비용이 많이 들기 때문에 op30 까지의 결과값을 가지고 부적합여부를 판별하려고 했으나
굳이 op30 까지 공정을 직접 실행해야 하는 당위성에 의문이 들었다.

그래서 op0, 부품을 가져와서 치수 검사를 하는 단계에서 머신러닝 모델을 학습시킨다면 부품을 받아오고 나서 부적합 예측을 할 수 있을것이다.
따라서 op0의 부품 치수 데이터를 X로 두고, 공정이 끝난 후의 결과값을 Y로 두고 학습을 다시 시켜보아야겠다. 2020.08.28


처음 들어가는 부품 치수에 대한 Data_set과 그에 대한 결과값을 학습시켜 공정과정을 거치고 나왔을 때 이 부품이 최종적으로 양품일지에 대해 예측해보았다.
XGboost 모델을 학습시킨 경우 정확도 95.5%, 정밀도 0.8718, 재현율 : 0.2857 로 나오게 되었다. 불량을 양품으로 잘못 예측한 재현율이 높지 않았다.
op30 까지의 결과값을 기준으로 op 40, 50을 예측하려고 했던 것과 대조적이다.
이 결과로 인해 부품들을 미리 공정에 넣어보기 전에 어느정도의 불량이 나오게 될 지 미리 예측할 수 있다.

만약 부품을 납품하는 협력업체가 있다고 가정할 경우 부품 납품업체에게 부품의 규격이 우리 공정을 거쳤을 때 이 정도의 부적합품이 생길 수 있으니 품질 개선을 요구할 수도 있다.
하지만 반대로 납품업체 입장에서는 공정 과정에서 문제가 없었는지에 대해 증명을 요구할 것이다. 
그럴경우 어떻게 증명할 수 있을까? Random Forest를 학습시키고 난 후 결과값에 중요하게 영향을 미친 요소들을 보여주면 증명할 수 있을까?
만약 모든 부품의 치수가 이상적으로 똑같다고 가정하고 공정과정에서 나온 결과값을 구한다면 그 산포가 어느정도인지를 구할 수 있을것이고 그 산포를 비교해보면 될까?

## 2020.08.30

지금쯤해서 진행되고 있는 프로젝트를 한번 점검해볼 필요가 있을 것 같다. 
현재 우리팀에서 만들고 있는 base model의 목적은 어떤 인자가 OEE에 어느정도의 영향을 미치는지 알고 그 인자들을 개선했을 때 OEE의 변화를 시각적으로 볼 수 있는 툴, 또는 대시보드를 만든다고 할 수 있다.
그렇게하기 위해서 먼저 공정환경(조립공정)을 설정하고 설비(BOM)에서 일어나는 일들을 가정하였다. 

각 상황과 설비에 맞는 데이터들을 뽑아와서 (최종 분석하기 전까지) 분석가능한 데이터로 변환하고 정리까지 한 결과로 엑셀이라는 파일로 만들었다는 시나리오로 진행되고 있다.
현실에서는 분석하기 위한 하나의 csv파일은 각각의 설비에 부착된 센서나 하드웨어들에서 데이터들을 추출하고 그것들을 데이터베이스에 저장한 후 변환, 적재 (ETL)과정을 거쳐 가공된 형태로 만든것이다. 

결국에는 이러한 일련의 파이프라인을 구축하는 것이 최종적으로 데이터를 분석하는데에 있어서 아주 중요한 요소라는 것을 알게되었다. 
따라서 그러한 부분에 대해서 공부를 해야한다는 점도 알게되었다.  

다시 프로젝트로 돌아와서, 하나의 분석하기 위한 엑셀파일을 Base Data Set이라고 가정하고 현재까지는 품질과 관려된 데이터들을 추출, 가공, 저장하였다.
OEE 는 시간, 성능, 품질에 대한 현재 설비 또는 공정의 현재 상황을 하나의 수치로서 표현한다. 

따라서 Base Data Set에는 시간과 성능에 관련된 데이터들을 추가해야한다. 그래야 OEE에 영향을 끼치는 요소들을 추려낼 수 있고 문제점 및 개선점을 찾을 수 있을 것이다.
다음주 월요일부터는 설비의 가동시간, 비가동시간과 관련된 상황들과 그에 맞는 데이터들을 생각하고 추가해보는 시간을 가져야겠다.


## 2020.08.31

시간에 대한 모델을 넣기 위해 알아야할 몇가지 용어들을 정리해보았다.

조업시간 : 일정기간내 작업단위의 근무시간으로 설비운전에 관계없이 임금이 지불되는 시간

부하시간 : 설비가 제품을 생산하도록 계획된 시간.
부하시간 = 조업시간 - 휴지시간

가동시간 : 설비가 가동되어야 할 부하시간 중에서 정지시간을 제외한 설비의 순수운전시간
가동시간 = 부하시간 - 정지시간

정미가동시간 : 설비가 실제로 생산에 기여한 시간으로 제품 생산에 소요되는 시간
정미가동시간 = 이론C/T 실제생산수량 = 가동시간- 속도LOSS

가치가동시간 : 설비가 양품만을 생산하는데 소요되는 시간
가치가동시간 = 이론C/T 양품생산수량 = 정미가동시간 - 불량LOSS

휴지시간 : 조업시간중에 설비를 비가동 시킴으로써 발생하는 LOSS시간 (설비의 기능과 관계없이 정지하는 시간)
- 회의, 조회 : 작업중 계획이나 필요에 의한 회의나 조회로 설비가 정지한 시간
- 교육, 훈련 : 회사가 인정한 사내 외 교육 및 훈련에 참가함으로써 발생한 설비의 정지시간
- 정전(단수) : 전기공급(단수) 중단으로 설비를 가동할 수 없는 시간
- 품절 : 생산도중 자재의 품절로 인해 생산이 지속적으로 이루어지지 않아 설비가 정지한 시간(관리 LOSS)

정지시간 : 다음과 같은 요인으로 설비가 가동하지 못한 시간
- 기계고장 : 우발적으로 발생한 기계고장으로 설비가 생산을 할수 없게된 시간
- 기종변경 : 자재불량, 품절등 우발적 요인이나 생산계획에 의해 Model을 변경할때 발생하는 설비정지시간
- 준비, 조정 : 제품의 생산종료시 또는 시작시 치공류의 착탈 및 조정, 정리, 청소등 일련의 작업에 의한 정지시간
- 순간정지, 공전 : 콘베이어상의 제품이 막히거나, 가공 테이블 위에 가공품이 걸려 공전하거나, 또는 검출장치가 작동해 정지된 시간
- 자재불량 : 불량자재로 인하여 투입전에 선별, 수리하거나 생산도중의 수정으로 설비가 정지한 경우
- 불량재작업 : 품질수준, 사양등에 미달하여 불합격된 제품의 수정 및 중간검사 후 수리작업에 소요된 설비 가동시간
- 공정불균형 : 전,후 공정(또는 전,후 설비)에서 다른 요인에 의해 작업이 정지하였거나 해당 설비가 후공정(설비)의 생산물량 또는 목표량을 달성하고 정지 하였을 경우의 시간
- 기타 : 상기 이외의 원인으로 설비가 정지한 시간

이론 C/T : 설비 제작회사에서 제시한 또는 설비가 설치된후 최적의 상태에서 단위제품이 생산되는 시간을 의미한다.  
실제 C/T : 작업환경과 제반여건을 감안하여 실제로 가동시켜 본 결과 평균적으로 사용되고 있는 가공속도를 의미한다.

위의 용어들을 숙지하고 미리 설정해두었던 공정 라인에 대입하여 시간과 관련된 데이터들을 설정하여야 한다. 


## 2020.09.01

위의 시간가동율과 성능가동율을 알아볼 수 있는 가장 좋은 방법으로 Flexsim 이라는 소프트웨어를 사용하기로 하였다.
 Flexsim 소프트웨어는 3D 가상공장을 설계하고 시뮬레이션 할 수 있는 도구이다.
 
 Flexsim을 활용해서 EGR Cooler의 조립공정을 가장 잘 표현할 수 있도록 만들어보려고 한다.  
 가상공장을 만들고 시뮬레이션 할 수 있다면 시간가동율과 성능가동율을 계산할 수 있다.
 
 한가지 더 생각해보아야 할 점은 Flexsim 소프트웨어를 어느 선 까지 활용해야하는 점을 고민해보아야 한다.
 우리가 설계했던 공장의 모든 내용을 거의 정확하게 시뮬레이션하고 구현하는 방법과 단순히 시간가동율과 성능가동율의 결과값을 얻어 OEE 계산에 활용하는 것 중에 선택해야 한다.
 
 만약 조립공정의 모든 내용을 시뮬레이션한다고 했을 때 알아봐야할 내용
 1. 시뮬레이션한 결과를 excel 파일로 받을 수 있는 방법
 2. 랜덤으로 생성했던 부품 data를 공정에 input할 수 있는 방법
 3. flowitem이 공정을 지나갈 때 마다 미리 설정해두었던 함수관계를 표현하여 치수데이터에 변형을 줄 수 있는 방법
 4. 작업이 끝난 후 time_stamp를 기록할 수 있는 방법
 
## 2020.09.02

Flexsim을 최대한 많이 사용해서 데이터를 뽑아오기 위한 세뮬레이션 모델을 만들기로 팀토의 결과 결정되었다.
오늘은 그 모델을 만드는 작업을 진행하였고, 미리 설정해두었던 공정 상황을 구현하기 위해 trigger 설정하는데에 많은 시간을 들였다.
이번주까지 모델을 완성하는것이 목표이고 다음주에는 이 모델을 활용해서 데이터추출, 분석한 결과를 대시보드 형태로 보여주는 것으로 계획하였다.  

## 2020.09.04

하나의 시뮬레이션 모델을 만들어 놓으면 OEE를 계산하고 데이터를 받아오는데에도 용이하다. 

하지만 어제와 오늘 시뮬레이션 모델을 만드려고 하는데에 생각보다 많은 시간이 걸리고 있다.

오늘은 어제 만드는 중 생겼던 오류들의 원인을 파악하고 될 수 있으면 오류를 해결해 실시간 공정상황 체크와 데이터를 받아오는데까지 하는 것을 목표로 잡아야겠다.

## 2020.09.05

몇일 간 계속되는 시도에도 불구하고 계속해서 발생하는 오류의 해결책을 찾지 못하였다.

실시간으로 재공품이 각 공정을 돌면서 데이터를 기록하는 모델을 만드려고 했지만 프로젝트의 진행을 위해서 차선책을 잡아야했다.

하나의 제품이 완성되고 난 후에 재공품에 저장되어있던 데이터들을 한번에 데이터 테이블에 저장시키는 방법으로 베이스 모델을 완성하려 한다.

## 2020.09.06

하나의 제품이 완성되고 난 후에 데이터를 업로드시키는 방식의 모델로 만드는 것이 하나의 돌파구가 되었다.

현재까지 op10 ~ op30 까지의 공정을 구현하였다.

다음주까지는 모델을 완성하고 여러번 데이터를 수집하여 분석에 사용해보는 것을 목표로 잡았다.

그런다음 그 데이터들을 대시보드 형태로 볼 수 있도록 만들 생각이다.


## 2020.09.09

현재까지 op10~ op50 공정까지 모델을 완성하였고, 재공품에 데이터를 저장해둔 상태이다. 

오늘까지 팀원들과 마지막 op60에서 테스트한 값을 저장할 수 있는 것까지 만들 생각이다.

단순한 모델이 아닌 꽤 복잡한 시뮬레이션 모델을 만든다는 것 자체가 굉장히 고된일이라는 것을 느끼고 있다.

## 2020.09.10

오늘 드디어 공정을 시뮬레이션 하기 위한 Base Model을 만들었고, 데이터를 생성할 수 있게되었다.

항상 에러들이 생겨 힘들었지만 하나하나 조금씩 해결해나가 모델을 완성하였다.

이제 이 시뮬레이션 모델을 가지고 여러가지 변수들에게 변형을 가하거나 데이터 분석을 진행할 예정이다.

## 2020.09.11

base model 에서 생겼던 오류를 해결했고 모델을 가지고 데이터를 생성하는 실험을 진행하였다.

시뮬레이션 실행 시간을 하루 8시간(3600초 * 8시간), 1주일에 5일 작업한다고 가정했을 때, 4주 동안 총 144800초라는 계산이 나오게 되었다.

이 시간이 한번 시뮬레이션을 실행하는 시간이 되고, 이 한번 실행에 보통 14388개의 제품이 만들어졌다. 동일한 모델은 총 4번 반복 실행하였다.

이 중에 현재 공정 모델에서는 대략 4000개 정도의 부적합품이 생기게 되었다. 조치가 필요한 상황이다.

완제품이 부적합품으로 취급될 때는 치수 부적합인 경우가 대부분이고, 이 값은 전기값과 온도값에 영향을 받는다고 추측하였다.

따라서 온도값과 전기값을 각각의 수준으로 나누고 모델에 변화를 줄 때마다 4번 반복 시행하였다.

base model에서의 전기값과 온도값의 분포는 각각 89 < 전기값 < 100, 489 < 온도값 < 511 으로 설정되어 있었고, 

그 분포의 간격을 더 축소하여 불량률을 줄여나갈 생각이다.

## 2020.09.14

Base Model - 89 < 전류값 < 99.9, 489 < 온도값 < 511 / 반복 4번  
결과 : 불량 갯수 (4693, 4828, 4782, 4843) / 14388 (총 생산 갯수)  
평균 불량률 : 약 33 %  

Electricity Model - 
(1) 89.9 < 전류값 < 99.9 / 반복 2번  
(2) 89.99 < 전류값 < 99.9 / 반복 4번  
(3) 89.9999 < 전류값 < 99.9 / 반복 2번  
(4) 89.9999999 < 전류값 < 99.9 / 반복 2번  

결과 : 불량 갯수
(1) 2984, 2956  
(2) 2625 2815 2793 2756  
(3) 2735 2855  
(4) 2691 2766  

평균 불량률 : 
(1) 약 20 %   
(2) 약 19.0 %  
(3) 약 19.4 %  
(4) 약 18.9 %   

결론 : 전류값의 범위를 축소 시켰을 때 불량률의 감소를 보이지만, 일정 수준으로 이하로는 떨어지지 않고 수렴하는 경향이 있다.


## 2020.09.16

어제 시뮬레이션 데이터를 생성해 여러가지 분류 알고리즘을 통해 분석을 실시하였지만, 시뮬레이션 모델에 데이터 생성 부분에서 일부 문제가 있어 다시 데이터 수집을 실시해야 한다.  

몇일 전에 했던 실험 모두 수정한 모델을 통해 데이터 생성을 실시한 후, 우선 Base Model을 가지고 데이터 분석을 실시한다.   

앞으로 해야 할 순서  

(1) Base Model 에서 데이터 수집 후 데이터 분석(상관관계, 분류 예측 모델)  

(2) 원인 파악 후 개선을 위한 실험 실시  

(3) 개선 모델을 통한 예측 모델 설계  

