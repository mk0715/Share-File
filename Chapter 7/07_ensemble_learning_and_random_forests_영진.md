---
layout: single
title:  "AdaBoost 작동 원리 이해"
---

# AdaBoost 개요
<br/>
아래와 같이 노드 하나에 두개의 리프(leaf)를 지닌 트리를 stump(가지)라고 합니다.
<br/>
<br/>
<img src='../assets/images/stump.png' align="center">
<br/>
<br/>

AdaBoost는 아래와 같이 여러 개의 stumps로 구성이 되어있습니다. 이를 forest of stumps라고 합니다. 
<br/>
<br/>
<img src='../assets/images/forest of stumps.png' align='center'>
<br/>
<br/>
Random Forest가 Full sized 트리를 만드는 것과 다르게 depth 1짜리 stump는 제대로 분류하기 어렵습니다. 이를 week learner(약한 학습기)라 부릅니다. 

또한 Random Forest는 최종 예측값을 결정할 때 모든 트리의 예측값의 평균 혹은 최빈값을 사용합니다. 즉 최종 결과 산출에 모든 트리의 가중치가 동일하게 적용됩니다. 

하지만 AdaBoost는 최종 결과값를 결정하는데 Stump마다 가중치가 서로 다릅니다. 따라서 어떤 stump가 다른 stump보다 더 중요할 수 있다는 뜻입니다. 이를 Amount of Say가 높다고 표현합니다. 

그리고, Random Forest는 모든 트리가 독립적인 반면(순서를 바꿔도 상관이 없다는 뜻) AdaBoost는 순서가 중요합니다. 왜냐하면 첫번째 Stump의 에러가 두 번째 Stump 결과에 영향을 주기 때문입니다. 이후 두번째, 세번째 .... 계속 영향을 주게 됩니다. 
<br/>
<br/>
**정리하자면, AdaBoost는 3가지 특징이 있습니다.**
- 1. 약한 학습기(week learner)가 여러개로 구성된 Forest of stumps가 분류기입니다.
- 2. 더 중요하다고 생각되는 stump가 최종 결과에 더 많은 영향을 주게 됩니다.
- 3. 각 Stump의 error는 다음 Stump 생성에 영향을 줍니다. 
<br/>
<br/>
# AdaBoost 작동 원리 
<img src='../assets/images/dataset.png' align='center'>
<br/>
<br/>
Chest Pain, Blocked Arteries, Patient Weight를 feature로 Heat Disease를 가지고 있는지에 대한 총 8개의 데이터가 있습니다.(8명의 환자) 이 경우 sample weight가 1/8입니다. 

$\text{sample weight} = \frac{1}{\text{total number of samples}} = \frac{1}{8}$
<br/>
<br/>
Chest Pain과 Heart Disease와의 관계입니다. 
<br/>
<br/>

<img src='../assets/images/chest pain stump.png' height=300>

<br/>
이 Stump는 Chest Pain이 Yes이면 Heart Disease도 Yes라고 판단하는 모델입니다. 총 8개 중 Chest Pain이 Yes라고 판단한 것 중 올바르게 판단한게 5개, 틀린게 2개입니다. 반대로 Chest Pain이 No라고 판단한 것 중 올바르게 판단한게 2개, 틀린게 1개입니다. 
<br/>
<br/>
다음은 Blocked Arteries와 Heart Disease와의 관계입니다. 
<br/>
<br/>

<img src='../assets/images/Blocked Arteries stump.png' height=300>
<br/>
<br/>
마지막으로 Weight와 Heart disease와의 관계입니다. 
<br/>
<br/>
<img src='../assets/images/Weight stump.png' height=300>
<br/>
<br/>
이 세 가지 Stump의 지니계수입니다. 
<br/>
<br/>
<img src='../assets/images/Gini of stump.png' height=300>
<br/>
<br/>
세 가지 Stump 중 Weight를 기준으로 한 stump가 가장 지니불순도가 적습니다. 따라서 이를 첫번째 분류기로 채택합니다. 

$\frac{3}{8} * \{{1-(\frac{3}{3})^2-(\frac{0}{3})^2} + \frac{5}{8}\} * 
\frac{5}{8} * \{1-(\frac{4}{5})^2-(\frac{1}{5})^2\} = 0.2$

# Amount of Say 구하기 
첫 번째 Stump를 정했다면, Total error를 계산할 차례입니다. 

$\text{Total error} = \text{sum of incorrect sample weights}$ 입니다. 

첫번째 Stump의 Incorrect는 1개밖에 없습니다. 따라서 Total Error는 1/8입니다. 

<img src='../assets/images/Total error.png' height=300>
<br/>
<br/>

Total Error를 구했다면, 다음으로 Amount of Say를 구할 수 있습니다. Amount of Say는 최종 분류에 있어 해당 Stump가 얼마만큼의 영향을 주는가를 뜻합니다. Amount of Say를 구하는 공식은 이렇습니다. 

$\text{Amount of Say} = \frac{1}{2}log(\frac{1-\text{total error}}{\text{total error}})$

그래프로 그려보면 아래와 같습니다. 

<img src='../assets/images/Amount of say graph.png' height=300>
<br/>
<br/>
X축이 Total error고 y축이 Amount of Say입니다. 에러율이 낮을수록 Amount of say가 높아집니다. 즉, 최종 결과에 이번 Stump가 많은 가중치를 갖을 수 있겠죠. 반대로 에러율이 높을수록 Amount of say가 낮아집니다. 만일, 음수값이 나온다면 최종 결과에서 오히려 패널티를 받게 됩니다. 
<br/>
<br/>
다시 돌아가서 Total error = 1/8이라고 했으므로, 아래그림처럼 Amount of say = 0.97이 됩니다. 
<br/>
<br/>
<img src='../assets/images/Amount of say frac18.png' height=300>
<br/>
<br/>
연습을 위해 한 번 더 예시를 들어보면, 아래그림처럼 Chest Pain으로 하면 Sample weight= 3/8이 되고, Amount of say는 0.42가 됩니다. 
<br/>
<br/>
<img src='../assets/images/Amount of say frac38.png' height=300>
<br/>
<br/>

# 샘플 가중치 설정 
이제 다음 Stump가 어떻게 생성되는지 보겠습니다. 이를 위해 첫번째 Stump에서 구한 Amount of Say를 이용하여 새로운 sample weights를 구해야합니다. 새로운 Sample Weights의 공식은 이전 Stump에서 잘못 분류된 sample과 잘 분리된 sample 두 케이스로 나누어집니다. Sample weight 공식은 아래와 같습니다. 

$\text{New Sample Weight} = \text{sample weight} * e^{\text{amount of say}})... \text{when, incorrected samples}$

$\text{New Sample Weight} = \text{sample weight} * e^{-\text{amount of say}})... \text{when, corrected samples}$
<br/>
<br/>
<br/>

잘못 분류된 sample 그래프를 아래 그래프로 그려보면,
<br/>
<br/>
<img src='../assets/images/corrected new sample weight graph.png' height=300>
<br/><br/>
amount of say가 증가할수록 new sample weight는 증가합니다. 

반대로 잘 분류된 sample 그래프를 아래 그래프로 그려보면 <br/><br/>
<img src='../assets/images/incorrected new sample weight graph.png' height=300>
<br/><br/>
amount of say가 증가할수록 new sample weight는 감소합니다.

다시 돌아가서 new sample weight의 공식에 적용시켜보면 

$0.33 = \frac{1}{8}*e^{0.97} = 0.33$

$0.05 = \frac{1}{8}*e^{0.42} = 0.05$
<br/><br/>
잘못 분류된 sample은 0.33으로, 잘 분류된 샘플은 0.05로 sample weight를 업데이트 합니다. 

그 후, 합을 1로 맞추기 위해 정규화를 진행합니다. (다 더한 수로 각 샘플을 나누면 됩니다.)
<br/><br/>
<img src='../assets/images/new sample weight update.png' height=300>
<br/><br/>
이제 new sample weight를 이용해 새로운 데이터셋을 뽑습니다. 이전의 데이터 개수 만큼 0 ~ 1 사이의 수를 랜덤하게 뽑습니다. 

만일 그 숫자가 0 ~ 0.07 사이라면 첫 번째 샘플이 뽑히게 됩니다. 

만일 그 숫자가 0.07 ~ 0.14 사이라면 두 번째 샘플이 뽑히게 됩니다. 

만일 그 숫자가 0.14 ~ 0.21 사이라면 세 번째 샘플이 뽑히게 됩니다. 

만일 그 숫자가 0.21 ~ 0.70 사이라면 네 번째 샘플이 뽑히게 됩니다. 

이런식으로, 총 8개의 샘플을 다시 뽑습니다. 
<br/><br/>
그 결과는 아래 그림처럼, 첫번째 stump에서 incorrected sample이 총 4번 뽑히게 되었습니다.(가정입니다. 실제로는 돌릴 때마다 달라질 것입니다. )
<br/><br/>
<img src='../assets/images/new sampled dataset.png' height=300>
<br/><br/>
그리고 아래 그림처럼, 새롭게 샘플링된 데이터셋에서 다시 sample weight를 초기화 해줍니다. 
<br/><Br/>
<img src='../assets/images/new dataset new sample weight.png' height=300>
<br/><br/>
이 과정을 순차적으로 계속 반복하면서 Stump를 만들어 나갑니다. 

좀 더 생각해보면 adaboost는 이 과정에서 incorrected sample에 더 가중치를 두어 새롭게 샘플링하는 데이터에 더 많이 들어가도록 합니다. 미래에는 결국 잘못 분류하는 샘플들을 더 잘 맞출 수 있게 될겁니다. 
<br/><br/>

# 최종 예측 
<br/>
최종 예측은 아래 그림처럼, 어떤 sample에 대해 생겨난 모든 Stump들을 Has Heart Disease와 No Heart Disease로 분류합니다. 그리고 각 Stump들이 가진 Amount of Say를 합산하여 더 높은 Amount of Say를 가진 범주를 최종 예측으로 삼습니다. 
<br/><Br/>
<img src='../assets/images/final prediction.png' height=300>
<br/><br/>

# 마지막 정리 
Random Forest는 Data Sampling을 무작위로 뽑아서 예측을 만들었다면, 
AdaBoost는 이전 Error를 기반으로 Data Sampling을 하여 더 정확하게 분류할 수 있도록 하는 알고리즘을 지녔습니다. 하지만 후에 조금 더 발전된 Gradient Boosting Machine이 나오면서 대체되었으나, 기본적인 아이디어는 비슷합니다. 