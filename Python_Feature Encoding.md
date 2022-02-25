#  Python



## 범주형 데이터 다루기 : Feature Encoding



* 범주형 변수를 모델에 적용하기 위해 변환해주는 과정을 Encoding 이라 함
* 가장 많이 사용되는 인코딩 방법은 Label Encoding과 One-Hot Encoding이지만 항상 만능은 아님
* Encoding 방식에 따라 모델의 성능이 달라질 수 있기 때문에 데이터 손실과 차원을 최소화하기 위해 데이터에 맞는 방법을 사용해야 함



### 1. Encoding을 할 때 고려해야하는 경우

* 트리 계열 알고리즘에서 숫자로 된 변수를 사용한 경우(Label Encoding)

  * Label Encoding은 문자형 데이터를 정수형 데이터로 변환해주는 기법

    주의! 일괄적으로 숫자로 바꿔주는 과정에서 데이터가 가중치로 인식될 수 있다는 점

  * ex)

    [A, B ,AB, O]의 혈액형 변수를 Label Encoding을 통해 [1,2,3,4]로 매핑한다면 해당 알고리즘들은

    A, B, AB, O형 사이의 대소 관계를 부여하게 됨

    (중요도 측면에서 A형이 B형보다 작고, O형은 AB형보다 크다고 인식)

    그렇기때문에 카테고리 간에 순서가 없는 명목형 변수를 Label Encoding을 통해 매핑 후 학습한다면, 

    정확한 정보를 학습하고 있다 확신하기 어려울 것임

  * 회귀 모형과 같이 선형적 특징을 지니는 알고리즘의 경우 Label Encoding을 사용하지 않는 것이 좋음



* 트리 알고리즘에서 One-Hot Encoding을 사용하는 경우

  * One-Hot Encoding은 하나의 변수를 여러 개의 변수로 분할하는 기법

    고유값에 해당하는 변수만 1로 표시하고 나머지 변수에는 0으로 표시하는 방법임

    특정 변수가 카테고리를 N개 가지고 있다면 1개의 변수가 N개의 변수로 증가하게 됨

  * 반면에 트리 계열의 알고리즘은 분기점에서 하나의 변수를 선택해 정보 이득을 계산함

  * 트리 알고리즘에 One-Hot Encoding을 적용하게 되면 특정 변수에 대한 정보 이득이 N개의 변수로 찢어지므로, 해당 변수가 높은 정보 이득을 가질 가능성이 낮아짐

    결국, 알고리즘에 대해 잘 선택되지 않고, 중요도 역시 낮아짐.
    
    

------



### 2. 인코딩 기법

#### 2 - 1. Binary Encoding

* Binary Encoding은 0과 1을 사용하여 인코딩하는 기법

* One-Hot Encoding과 유사해보일 수 있지만 차이점이 있음

  * **One-Hot Encoding** : 고유값에 해당하는 변수만 1로 표시 -> 1개의 변수가 N개의 변수로 분할됨
  * **Binary Encoding** : 이진법을 통해 표시 -> 1가의 변수가 log_{2}{N}log2N개의 변수로 분할됨

* Binary Encoding은 훨씬 더 적은 변수만 필요로 한다는 장점을 지님

  

#### 2 - 2. Original Encoding

*  Original Encoding은 문자형 데이터를 숫자형 데이터로 변환해주는 기법
* 데이터셋에 등장하는 순서 혹은 알파벳 순서대로 매핑하는 Label Encoding과 달리 변수의 순서 정보를 담을 수 있음

```
# Mapping을 활용환 Ordinal Encoding

temp_dict = {"Cold": 1, "Warm": 2, "Hot": 3, "Very Hot": 4}

df["Temp_Ordinal"] = df['Temperature'].map(temp_dict)

df
```

* 참고) Label Encoding은 알파벳 순으로 Cold - 0, Hot - 1, Very Hot - 2, Warm - 3으로 인코딩 됨
