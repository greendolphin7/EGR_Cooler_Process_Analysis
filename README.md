# EGR_Cooler_Process_Analysis
EGR Cooler 제품의 공정 데이터 분석

**팀원** 

+ 김재우 https://github.com/greendolphin7
+ 정동교 https://github.com/Eliotj-4860  
+ 진익철 https://github.com/jinikcheol  
+ 장규빈 https://github.com/Binsreoun  


## 개요

3D 가상 모델링을 할 수 있도록 도와주는 디지털 트윈 소프트웨어인 Flexsim을 이용해 EGR Cooler 제품의 조립공정 시뮬레이션 모델을 만든 후 그 과정에서 생성되는 데이터들을 활용해 머신러닝 분류 알고리즘에 학습시켜 OEE를 항샹시키는 데에 활용할 수 있도록 한다.

이후 데이터들을 실시간으로 생성하고 모니터링할 수 있는 웹 애플리케이션 개발 프로젝트로 이어진다.  
( _https://github.com/greendolphin7/Assembly_process_data_web_application_ )

## 개발 진행

처음에는 조립공정에서 어떤 설비를 기준으로 잡아야 할지, 어떤 과정으로 제품 또는 재공품이 생산되는지에 대한 배경지식이 부족하여 제품 자체에 대한 정보를 찾아보았다.

우리팀은 제조과정에서 생기는 데이터들에 집중하기 시작하였고, 제조과정을 가상 시나리오로 설정하였다.

가상으로 시나리오를 설정하고나니 자연스럽게 데이터들도 어느정도 틀이 나오기 시작하였다.

그 후 Flexsim 소프트웨어를 활용해 우리가 만든 공정을 모델링하기 시작했다.

모델링 과정에서 나온 결과로 나온 데이터들을 모아 머신러닝 분류알고리즘을 적용시켜보았다.
