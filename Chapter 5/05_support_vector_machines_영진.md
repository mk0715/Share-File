# SVM for Anomaly Detection 
- 우리가 배운 SVMClassifier는 이진 분류 혹은 다중 분류를 할 때 사용합니다. 하지만 이상 탐지에서 쓰는 SVM은 클래스 한 개만 구분하는 One-Class 문제에서 사용됩니다. 

## One class란 
- Anomoly Detection의 내용을 살펴보면 가장 기본적인 내용은 정상과 비정상을 분류할 수 있는 알고리즘들입니다. 
- 예를 들어, 어떤 공정에서 정상과 비정상을 분류하는 작업이 필요하다고 하면, 정상 데이터를 확보하는 일은 매우 쉽습니다. 하지만 비정상 데이터를 확보하는 일은 어려울 겁니다. 그리고 그 비정상의 케이스가 모든 비정상의 케이스를 대변한다고 볼 수도 없습니다. 그래서 이진 혹은 다중 분류로 접근할 수 없습니다. 
- 그래서 정상치만을 구분할 수 있는 방법이 필요했고 그 아이디어가 하나의 클래스를 분류할 수 있는 One-Class 분류 문제입니다. 

# SVM에서 One-Class를 다루는 법
[![image](https://user-images.githubusercontent.com/66348480/149745184-29e0fd16-b07d-4322-b6d0-d62e10fd62fa.png)](https://youtu.be/3liCbRZPrZA)


![image](https://user-images.githubusercontent.com/66348480/149746625-9fd6a8e9-ec5a-409b-9eb7-d1bae62f0371.png)

![image](https://user-images.githubusercontent.com/66348480/149747131-d673e462-5e57-43bb-a83b-57a9db828469.png)

# SVM과 ANN 비교 
- NN은 여러 Layer를 추가해 특징 사이의 복잡한 관계를 학습 가능하지만, SVM은 피쳐 엔지니어링을 수동으로 잘 해야한다. 
- Local Minumum과 saddle point를 피할 수 있다면 global minumum을 찾을 수 있다. 그리고 이는 고차원에서도 엄청나게 많은 학습을 한다면 대부분 global minimum을 찾을 수 있다는 것이 현재 메타이다. 

- NN은 훈련에 엄청나게 많은 데이터가 필요하지만, SVM은 작은 데이터셋에서도 잘 작동한다 (훈련속도도 빠르다.)
- SVM은 GPU가 요구되지 않는다. 
- CNN과 같은 NN은 '공간'개념이 필요하다. RNN은 '시간.순서'의 개념이 필요하다. 즉 일반적인 테이블 데이터는 시공간 속성이 없는 CNN, RNN이 유효하지 않다.
- SVM은 초매개변수 튜닝할 게 NN보다 훨씬 적다. 


# 잡담 
## Local Minimum 이라는게 왜 생길까 ? 
- ~~만일 wx+b을 yhat이라고 하고 이를 mse식에 넣으면 cost function은 2차 함수로 convex 함수가 되고 이러면 global minima를 찾을 수 있다. 
하지만 x를 고차원에 mapping 한다면, 즉 polonomial feature로 만들어 F 공간에서 ax^2 + bx + c가 yhat 이라면, 이를 mse식에 대입하면 cost function은 단숨에 4차원으로 늘어난다. 4차원 그래프를 떠올리면 극소값이 2개이다. 그리고 a와 b를 랜덤하게 초기화하면 최소값이 아닌 다른 극소값에 머무를 수 있게 되는 것이다. 만일 변수가 100개라면 mse 식에 대입하면, 100*100, 10000차원의 cost function이 정의되고 이러면 5000개의 local minima가 생기게 된다. 5000개의 local minima를 다 피해서 global minima에 도달 할 자신이 있는가 ?~~

## 중요한 건 Local minima가 아니라 saddle point다. 
- ~~local minima는 어떤 cost function을 사용하느냐에 따라 다르겠지만, 적어도 mse에서는 d/2게 존재한다. 그럼 그 부분만 넘으면 되는거 아니냐 하는 쉬운 얘기도 가능하겠다. 하지만 중요한 건 local minima가 아니라 saddle point다. saddle point는 1차 미분값이 0에 가까운 지점들을 말한다. 이 경우 backpropagation에서 gradient vanishing 문제가 발생하기 때문에 optimization 속도가 매우 느려지게 된다. 완전 0이 되는 지점은 아니기에 무한 시간을 들이면 수렴에 가까워지겠지만, 현실에선 그럴 수 없다. 그래서 이러한 saddle point를 쉽게 넘기기 위한 cost function에서의 다양한 탐색 방법이 필요하다.~~


<참고>
- https://www.youtube.com/watch?v=OmK_GQ40yko
- <http://rvlasveld.github.io/blog/2013/07/12/introduction-to-one-class-support-vector-machines/>
- <https://hyunw.kim/blog/2017/11/01/Optimization.html>
