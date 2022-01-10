# ROC_AUC_SCORE
- FPR(거짓양성비율)과 TPR(진짜양성비율) 간의 관계 

## ROC_AUC의 맹점 (1)
|Actual Class|Predicted Class|Confidence of "response"|Type|Number of TP|Number of FP|Fraction of FP|Fraction of FP|
|---|---|---|---|---|---|---|---|
|response|response|0.9|TP|1|O|0|0.167|
|response|response|0.89|TP|2|0|0|0.333|
|response|response|0.83|TP|3|0|0|0.500|
|response|response|0.74|TP|4|0|0|0.667|
|no response|response|0.68|FP|4|1|0.25|0.667|
|response|response|0.61|TP|5|1|0.25|0.833|
|response|response|0.60|TP|6|1|0.25|1|
|no response|response|0.57|FP|6|2|0.5|1|
|no response|response|0.54|FP|6|3|0.75|1|
|no response|response|0.53|FP|6|4|1|1|
|no response|no response|0.44|TN|6|4|1|1|
- <img src='./ROC_AUC graph 1.png' height=300px width=500px>
- 근데 만일 내 모델이 예측값을 정렬했을 때 TP가 다 앞에오고, FP가 다 뒤에가 있으면 ROC_AUC는 1이 된다. 
- ***분명히 잘못 예측한 값이 있는데도 ROC_AUC가 1이 나온다는 건 이상한거고 ROC_AUC의 한계라고 생각한다.***

## ROC_AUC의 맹점 (2)
- ***위의 결과를 바탕으로 생각하면 ROC_AUC는 결국 순서가 중요하다.***
- ***게데가 ROC_AUC는 확률값이 아니다. 하지만 대부분의 분류에서는 구체적인 확률값을 구하는 것이 더 중요하다.(그래서 사과일 확률이 얼만지가 궁금하지 FPR과 TPR의 관계를 궁금해하진 않는다.)***
- 이러한 단점을 보완하기 위해서, Brier Score를 적용할 수 있다. 
> Brier Score 
> - <img src='./Brier Score formula.png/' height=70px width=200px>
> - 확률값(0~1)과 실제값(0 or 1)의 차이를 제곱한 것을 평균낸 것 (당연히 작을수록 좋음. Loss처럼)
- ROC_AUC_SCORE와 Brier Score를 동시에 고려해서 평가한다면, 예측 결과의 정답 여부와 예측값의 신빙성을 동시에 고려할 수 있다. 

## ROC_AUC는 데이터 불균형에 만능은 아니다. 
|y_true|y_prediction|
|---|---|
|1|0.8|
|0|0.5|
|0|0.4|
|1|0.3|
|0|0.2|

- Positive : 3개, Negative : 2개 따라서, 총 6개의 순서쌍
- 모델이 제대로 분류했다면, 모든 1이 0보다 앞에 나와야함
- 하지만 위의 경우, 1이 0보다 앞에 있는 경우 순서쌍이 총 4개
- 따라서 AUC_ROC = 4/6 = 0.667

근데 만일, 
|y_true|y_prediction|
|---|---|
|0|0.46|
|1|0.4|
|0|0.3|
|0|0.2|
|0|0.2|
|0 X 95|... X 95| 
- 이라면 Positive : 99개, Negative : 1개, 총 99개의 순서쌍
- 1이 0보다 앞에 있는 경우 순서쌍이 총 98개
- 따라서 AUC_ROC = 98/99 = 0.9899

- ***딱 1개 있는 데이터를 못맞췄는데 ROC_AUC는 98.99로 매우 높게 나온다.***
- ***데이터 불균형일 때 ROC_AUC를 맹신하지 말아야 하는 이유다.***

# F1 Score가 데이터 불균형일 때 가장 적절하다. 
- 1. Precision과 Recall 모두 특정 조건(실제 양성인 경우 or 양성이라고 예측한 경우)하에서의 비율이기 때문에 순수하다. 

- 2. 조화평균으로 계산하기 때문이다. 


# 다중 분류일 때 F1 Score 계산 한 눈에 보기 
> Precision = TP / (FP + TP)

> Recall = TP / (TN + TP)

<img src='./F1_Score_Multi_classification 1.png'>
<img src='./F1_Score_Multi_classification 2.png'>

--> F1 SCORE = (0.492와 0.775)의 조화 평균 = 0.601

# Decision Function과 Predict_proba의 차이 

## $f(x)$ = $\frac{1}{e^(\beta_0+\beta_1x_1+\dots+\beta_kx_k)}$

- Logistic Regression 함수식이다.
- $f(x)$가 ```predict_proba``` 이고, $e^(\beta_0+\beta_1x_1+\dots+\beta_kx_k)$ 가 ```decision_function```이다. 
- decision_function을 확률값으로 나타낸 것이 predict_proba이므로 큰 차이를 둘 필요는 없다. 

# 참고자료
1. [Understanding ROC AUC: Pros and Cons. Why is Bier Score a Great Supplement?](https://medium.com/@penggongting/understanding-roc-auc-pros-and-cons-why-is-bier-score-a-great-supplement-c7a0c976b679)
2. [14. 다중 분류 모델의 성능 측정 - Performance Measure(AUC, F1 score)](https://nittaku.tistory.com/295)
3. [What is the difference between decision_function, predict_proba, and predict function for logistic regression problem?](https://stats.stackexchange.com/questions/329857/what-is-the-difference-between-decision-function-predict-proba-and-predict-fun)