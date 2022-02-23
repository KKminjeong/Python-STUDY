# Python



## 범주형 데이터 다루기 : Encoding 기법



### 범주형 데이터란?

* 수치형 변수 : 연령 변수와 같이 어떠한 범위 내 수치 값을 갖는 변수를 수치형 변수

* 범주형 변수 : 성별 변수와 같이 고정된 목록 중 하나의 값을 갖는 변수

  * 명목형 변수 : 성별과 혈액형처럼 값이 달라짐에 따라 좋거나 나쁘다고 할 수 없는 경우

    (여러 카테고리들 중 하나의 이름에 데이터를 분류할 수 있을 때 사용)

  * 순서형 변수 : 학점과 만족도 조사처럼 값이 커지거나 작아짐에 따라 좋거나 나쁘다고 할 수 있는 경우

    (카테고리들이 순서가 있을 때 사용  ex. 상/중/하,  A/B/C...)

    

------



### 인코딩을 사용하는 이유

* 분석을 위해 사용하는 모델들은 수치형 데이터만 다룰 수 있음

* String형으로 된 범주형 변수는 모델이 인식할 수 있도록 일련의 변환 과정을 거쳐야 함  -> **인코딩(Encoding)**

  

------



### 인코딩 기법

#### 1. Label Encoding (라벨인코딩)

* Label Encoding은 알파벳 순서에 따라 문자형 데이터를 unique한 숫자형으로 매핑하는 기법

* 단순히 문자형을 정수형으로 변환시켜주므로 데이터의 크기 및 shape이 변하지 않음

  

##### 1-1. Python에서 Label Encoding을 사용할 수 있는 방법 2가지

* **sklearn.processing의 LabelEncoder**

  ```
  import pandas as pd
  import numpy as np
  
  data = {
  "Temperature": ["Hot", "Cold", "Very Hot", "Warm", "Hot", "Warm", "Warm", "Hot", "Hot", "Cold"],
  "Color": ["Red", "Yellow", "Blue", "Blue", "Red", "Yellow", "Red", "Yellow", "Yellow", "Yellow"], 
  "Target": [1, 1, 1, 0, 1, 0, 1, 0, 1, 1]}
  
  df = pd.DataFrame(data, columns = ["Temperature", "Color", "Target"])
  df
  ```

  ```
  from sklearn.preprocessing import LabelEncoder    # Label Encoding
  
  print(df['Temperature'].unique())
  print()
  
  encoder = LabelEncoder()    # LabelEncoder를 객체로 생성
  encoder.fit(df['Temperature'])    # fit, transform 메소드를 통한 레이블 인코딩
  
  df["Temp_Label_Encoder"] = encoder.transform(df['Temperature'])
  df
  ```

  * 어떤 카테고리가 어떠한 값으로 Label Encoding 되었는지 알 수 없을 때 

    : Inverse_transform() 메소드 사용할 수 있음

    

* **Pandas의 factorize**

  ```
  # Pandas의 factorize
  
  df.loc[:, "Temp_Factorize"] = pd.factorize(df["Temperature"])[0].reshape(-1,1)
  df
  ```

  * factorize 메소드는 인코딩하고자 하는 범주형 변수에서 카테고리가 등장하는 순서를 기반으로 매핑해줌

    

------



### 2. One-Hot Encoding

* One-Hot Encoding은 각 카테고리를 0과 1로 구성된 벡터로 표현하는 기법
* 이때, 카테고리의 수만큼 벡터가 생성되므로 각 카테고리가 새로운 변수가 되어 표현되며 숫자의 크고 작은 특성(중요도)을 없앨 수 있음
* 카테고리가 너무 많은 변수의 경우 데이터의 cardinality를 증가시켜 모델의 성능을 저하시킬 수 있다는 단점이 있음
  * 데이터의 cardinality : 특정 데이터 집합의 unique한 값의 개수



##### 2-1. Python에서 One-Hot Encoding을 사용할 수 있는 방법 2가지

* **sklearn.processing의 OneHotEncoder**

  ```
  from sklearn.preprocessing import OneHotEncoder
  
  oh = OneHotEncoder()
  encoder = oh.fit_transform(df['Temperature'].values.reshape(-1,1)).toarray() 
  # 인코딩 하기 전에 2차원 데이터로 변환
  
  df_OneHot = pd.DataFrame(encoder, columns=["Temp_OneHot_Encoder_" + str(oh.categories_[0][i]) for i in range (len(oh.categories_[0]))])
  
  df1 = pd.concat([df, df_OneHot], axis=1)
  df1
  ```

  * One-Hot Encoder의 경우 Input 데이터가 2차원이어야 하므로 reshape()을 통한 변환 과정이 필요



* **Pandas의 get_dummies**

  ```
  df1 = pd.get_dummies(df1, prefix=["Temp_Get_Dummies"], columns=["Temperature"])
  df1
  ```

  * 2차원 데이터를 Input으로 사용하는 OneHotEncoder와 달리 Get_dummies()의 경우 데이터 프레임을 바로 사용할 수 있다는 장점이 있음



