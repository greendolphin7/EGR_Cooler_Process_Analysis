# EGR_Cooler_Process_Analysis

EGR Cooler 제품 공정 데이터 분석 2020.08.16




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

2020.08.18



프로젝트를 하며 한가지 깊게 깨닫게 된 것이 있다.

데이터 분석은 결국 모든 과정의 맨 마지막 단계라는 것이다.

어떤 데이터를 수집할지 선정하는 단계에서 

왜 그 데이터를 수집해야하는지, 어떤 장비로 수집할 수 있는지에 대해 스스로 납득할 수 있을 만큼 치열하게 고민해야한다.

공정 데이터의 경우에는 특히 그러한 점이 더 크게 작용하는 것 같다.

공정에 대한 상세한 부분까지 알고 있어야하고, 하나의 완제품을 만들기 위해 쓰이는 부품, 재료, 설비, 시간 등등과

그 제품이 하는 역할과 작동원리, 품질에서 기대하는 기대치와 허용치를 모두 다 알아야 한다. 

2020.08.19



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

2020.08.20



데이터를 8월 20일에 데이터 선별 기준으로 8가지를 설정한 기준으로 선별하려고 한다.

공정은 조립공정이라고 가정하고 공정에 들어가는 부품리스트에 대해 찾아보았다.

부품 리스트 (EGR Cooler)
- Body
- Wavy fin
- Flange
- 냉각수 유입 유출 Tube
- Paste (부가 재료)

2020.08.21



오늘은 프로젝트의 분석 목적에 대해 설정해볼 필요가 있었다.

조립 공정에서의 데이터들을 생성하여, 생성된 데이터들을 Train, Test 데이터 셋으로 나눈다.

최종 Target 변수를 양품과 부적합품이라는 2개의 클래스로 나눈 다음, 머신러닝 알고리즘(의사결정나무, Random Forest, XGboost, SVM 등등)을 사용하여 학습시킨다. 

Test 데이터를 사용하여 정확도를 측정한다.

그렇게하여 나온 하나의 학습 모델을 기반으로 새로운 변수를 추가하여 모델을 더 복잡하게 만들어 볼 수 있고,

부적합품을 만들어내는 Factor 들을 추정하거나 서로 다른 데이터들의 상관관계와 인과관계를 유추해볼 수 있도록 한다.

2020.08.22



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

2020.08.23



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

2020.08.24



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

2020.08.25



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

2020.08.26



온도에 대한 데이터값은 온도 값이 500에서 최적값이기 때문에 그대로 계산하고 490 미만, 510 초과된 온도 값에서는 불량이 나오도록 하였다.
Data set을 최종적으로 완성하였다.

Data set을 train set : test set = 8 : 2 비율로 나누었고 Decision Tree 모델을 적용해 적합품과 부적합품을 분류할 수 있도록 학습시켰다.
이전에도 언급했듯이, op30 공정은 단순 조립공정이고 op40~50 공정은 용접을 통한 조립공정이기 때문에 용접조립 공정이 상대적으로 비용이 많이 들고 실패했을 때 실패비용이 크다는 점으로 상황을 설정하였다.
따라서 학습 시 op30까지의 결과값을 가지고 학습시켜 op 40 ~ 50까지 거친 후의 결과값을 미리 예측할 수 있으면 학습하는 의미가 있겠다는 생각이 들었다.
Decision Tree 모델을 적용하고 정확도가 98.1% 나오게 되었다.
앞으로는 XGboost과 같은 다른 알고리즘을 통해 학습시켜 결과값을 비교해보도록 하겠다.

2020.08.27



Data Set을 Decision Tree, Random Forest, Logistic Regression, KNN, XGboost, DNN 을 넣어보고 각각의 모델 성능을 비교해보았다. 
XGboost가 가장 뛰어난 성능을 보여주었고, Decision Tree, Random Forest가 그 다음이었다.
어떤 요인이 제품 부적합에 가장 영향을 미치는 중요 요인인지에 대해서도 선별할 수 있었다.
문제는 당초 op40~50 까지의 공정이 비용이 많이 들기 때문에 op30 까지의 결과값을 가지고 부적합여부를 판별하려고 했으나
굳이 op30 까지 공정을 직접 실행해야 하는 당위성에 의문이 들었다.
그래서 op0, 부품을 가져와서 치수 검사를 하는 단계에서 머신러닝 모델을 학습시킨다면 부품을 받아오고 나서 부적합 예측을 할 수 있을것이다.
따라서 op0의 부품 치수 데이터를 X로 두고, 공정이 끝난 후의 결과값을 Y로 두고 학습을 다시 시켜보아야겠다.

2020.08.28
