# Brief Summary in English
I participated in the data analysis contest as a team, carrying out a project dealing with data on the platform for loan comparison service. Although our team didn't make it to be nominated as a finalist, we could complete an entire project from researching the general domain (loan market) and company (its vision and mission), to analyzing the data followed by interpretation and suggestions on CRM and business plan.

The overall flow is as below:
### 1. `Classification problem`
- predicted whether the customers would apply for a loan in a specific time period
- based on given information including user data that users provide when they use the service, derived variables, and external data such as some economic indexes and interest rates.
- considering characteristics of target customers and the platform (application) itself, including dependency on medium interest rate loan and personal rehabilitation system.
- utilized various classification algorithms including random forest, catboost, adaboost and elliptic model, then tried ensembles.

### 2. `clustering problem`
- extracted insights on customized services considering the demand of target customers based on large-scale of data
- utilized various clustering algorithms including BIRCH, k-means, and so on.
- designed expectable home screen layouts for the application, which give appropriate information and advice to each cluster group.

#### 📎 Files Information
- `EDA_overall` : EDA for every given data, which include visualization of relationships between multiple variables
- `EDA_regarding_IsApplied` : EDA specifically focusing on 'IsApplied' column, the Y label, which means whether the application case was completed.
- `DataCleansing.ipynb`: codes for cleansing the data, which include imputating missing data and outliers
- `Classification_Predction`: codes for preprocessing, splitting the datasets, and modeling for classification, which includes comparisons among the candidate models based on the evaluation on validation data
- `Clustering_AppLogBased`: codes for clustering based on log data of application that indicates the user's action/behavior
- `Clustering_UserDataBased`: codes for clustering based on user data of applications, which may develop into user-based collaborative filtering, considering prospective value
- `Recommendation_byClustering`: additional algorithms that can recommend some loan products to the future user who has not applied for the loan yet


# Overview in Korean
# 앱 사용성 데이터를 통한 대출신청 예측 분석

**2022 빅콘테스트 데이터분석 분야 퓨처스 리그**

2022.08.30~2022.10.14

[빅콘테스트 홈페이지 바로가기](https://www.bigcontest.or.kr/main.php)

---

# 💡 프로젝트 상세

### 팀 소개

- 신재원 [jwoneeeee@ewhain.net](mailto:jwoneeeee@ewhain.net)
- 이지원 [jiddoly@gmail.com](mailto:jiddoly@gmail.com)
- 최주희 [lovejoohero@gmail.com](mailto:lovejoohero@gmail.com)
- 최지원 [jiii111@ewhain.net](mailto:jiii111@ewhain.net)

---

## 1️⃣ 분석주제 및 방향

### 대출 비교 플랫폼, Finda 분석

: 금융 **정보 불균형 문제**를 해소하여, **현명하고 주체적인 금융 선택**을 돕는 기업

- **주요 기능**
    - 고객의 소득, 재직, 신용점수 등을 60여개 제휴사의 신용평가모델에 적용하여 조건에 맞는 대출상품 비교 및 신청 서비스 제공
    - 신용점수 및 상환플랜 등 대출 통합 관리 서비스 제공
- **주요 고객**
    - 금리 상승기 이자 부담으로 인한 **대출 비교 플랫폼에 대한 니즈**를 느끼는 사용자
    - 양극화된 대출시장에서 **중금리 대출 상품의 부재**를 느끼는 중간 신용자

### 분석 과제 분석

: Finda가 빅데이터를 기반으로 고객별 니즈를 고려한 **맞춤형 서비스**를 제공하도록 인사이트를 도출하는 것

1. **분류 과제**
    - Finda 홈 화면 진입 고객 중, 특정 기간에 **대출을 신청할 고객 예측**
2. **군집화 과제**
    - Finda 홈 화면 진입 고객들을 군집화하여 **군집 별 특성 분석**을 통한 **맞춤형 서비스 메시지 제안**

---

## 2️⃣ 활용데이터
![Untitled](https://user-images.githubusercontent.com/101344070/210193867-7d803a6e-60ea-4a7f-a6ed-f5a38692823f.png)  
- **제공 데이터**: 유저 스펙 테이블, 대출 결과 테이블, 유저 로그 데이터
- **외부 데이터**: 대출 금리 데이터, 경제 심리 지수 데이터, 한국은행 기준 금리 데이터

---

## 3️⃣ 데이터 클렌징

### 1. 이상치 처리

- 각 Feature를 boxplot으로 시각화 한 후, 분포 상 극단적인 이상치가 존재하는 경우 **평균값 대체 또는 삭제** 진행

### 2. 결측치 처리

1. 동일 유저 ID 기준으로 처리
2. **논리 • 인과적 추론**을 통한 처리
3. 다중 대체법(MICE)을 통한 처리

![Untitled 1](https://user-images.githubusercontent.com/101344070/210194000-34eb746a-0e23-47f3-8878-3df92ff18602.png)  

---

## 4️⃣ 모델링1 : 분류

### 파생변수

**대출 승인 변수**

- 신청서별 **평균 금리**
- 신청서별 **평균 승인한도**

**부채 수준 변수**

- **실제 대출 원금**
- 총 지불 **원리금**
- 총 **예상** 부채

**외부 파생변수**

- 경제심리지수, 소비자동향지수
- 한국은행 공시 **기준 금리**
- 대출 **상품 유형별 대출 금리** 데이터


![Untitled 2](https://user-images.githubusercontent.com/101344070/210194016-a1aaef88-1141-46f4-805c-a91cc636d359.png)  
유저 스펙과 부채 관련 파생변수 상세

### 전처리

- **수치형 변수** : 로그변환, Robust Scaling, Min-Max Scaling
- **범주형 변수**(모델 특성에 맞게 처리): 수치형 score 파생변수, 수치형 label화, 원핫인코딩, 라벨인코딩
- **Sampling** : **1000만 행이 넘는 불균형 데이터**
    - Validation set 분리 후, 남은 train set과 test set의 대출 신청 비율을 유지하며 **언더샘플링 진행**
    - 언더 샘플링을 통해 데이터 불균형 비율이 17.5:1에서 5.8:1로 완화
    ![Untitled 3](https://user-images.githubusercontent.com/101344070/210194036-15c425be-af34-48d7-afa7-a365d1d6ab49.png)  
    
    

### 모델링

1. 교차검증을 통해 트리기반 모델, 선형 모델, 신경망 모델 등의 **성능을 전반적으로 확인**
 - 트리 기반 모델( XGBoost, Catboost, lightGBM, AdaBoost)과 인공 신경망 모델(Elliptic)이 괜찮은 성능을 보임  
  ![Untitled 4](https://user-images.githubusercontent.com/101344070/210194179-1fab8aad-eb3d-414a-af1d-9925d020211c.png)  

- 선형 및 인공 신경망 모델  
  ![Untitled 5](https://user-images.githubusercontent.com/101344070/210194186-51474348-3e5d-4efd-8972-a48f5b44f48b.png)  


- 트리 기반 모델의 성능

1. 성능이 좋은 5개 모델의 **하이퍼파라미터 튜닝** 
    - 모든 모델에서 비슷한 **변수 중요도** 결과가 확인됨
        - 유저 고유 ID를 제외하면, **대출 승인 금리, 신용점수, 대출 승인 한도, 금융사 점수(파생변수)** 등이 높은 변수 중요도를 보임
        
        ![Untitled 6](https://user-images.githubusercontent.com/101344070/210194260-f10bd956-f14d-499b-b8e7-6e680b82d820.png)  
        
2. Stacking 기법과 soft voting 기법을 활용하여 튜닝된 5개의 모델의 **앙상블 모델 생성**
![Untitled 7](https://user-images.githubusercontent.com/101344070/210194272-16d4675e-5bfe-4fde-9fd0-816d72441c0a.png)  
![Untitled 8](https://user-images.githubusercontent.com/101344070/210194284-2da1de31-548c-43a7-8bfd-844e4c2b0a8d.png)  
앙상블 모델 구조

1. 앙상블 모델과 개별모델의 성능을 비교하여 **가장 좋은 모델 채택**
    - **Elliptic Envelope 단독 모델이 가장 높은 성능을 보임**
    - 
![Untitled 9](https://user-images.githubusercontent.com/101344070/210194294-833ba254-0ba2-44cf-b9ef-20ef213f291c.png)  

- **Elliptic Envelope 모델**
    
    : 이상치 탐지 알고리즘으로 데이터가 가우시안 분포를 따른다고 가정할 때, 타원 분포를 벗어나는 샘플을 이상치로 분류
    

---

## 5️⃣ 모델링2 : 군집화

### 파생변수

**행동군집화**

- 대출신청유무
- 기대출보유유무
- 이탈지수
- 총 방문일 수
- 최근성
- 전체 이벤트 중 주요 이벤트 차지 비율

**고객군집화**

- 고객기존정보 : 나잇대, 근로형태, 고용형태, 주거소유형태
- 부채 수준 : 기대출수, 개인회생자, 신용점수, 연소득
- 희망 대출조건 : 목적, 대출희망금액
- 승인 대출조건 : 평균 승인한도, 평균 금리, 총 신청서 수

![Untitled 10](https://user-images.githubusercontent.com/101344070/210194326-a5fd0255-b8ac-44af-a72f-a3c84752055c.png)
유저 로그 데이터 관련 파생변수 상세

### 전처리

- 고객별 로그 통계량 기준 **Q1 이하의 로그 개수를 가진 유저 삭제**
- 로그가 유실되었다고 판단된 **유저 삭제**
- **수치형 변수**: 로그 변환, Min-Max Scaling 진행
- **범주형 변수**: 라벨 인코딩 진행

### 모델링

1. **행동군집화**
    - 대출, 로그, Event 관련 변수를 통해 고객의 행동을 기반으로 한 군집화
    - 사용한 모델 : **BIRCH 모델**
    
    ![Untitled 11](https://user-images.githubusercontent.com/101344070/210194372-62499c49-a3c5-42d0-ba93-c9c7e4bba9e5.png)  
    행동군집화 군집 분석 결과
    
2. **고객군집화**
    - 고객기본정보, 부채 수준, 대출조건 관련 변수를 통해 고객의 특성을 기반으로 한 군집화
    - 사용한 모델 : **Gaussian Mixture Model**
     
    ![Untitled 12](https://user-images.githubusercontent.com/101344070/210194386-c227d605-fda9-4613-9239-63d5339cfc4f.png)  
    

### 서비스 제안 - [핀다 앱 진입 화면 서비스 메세지 제안]


<img width="984" alt="Screen Shot 2023-01-02 at 1 06 44 PM" src="https://user-images.githubusercontent.com/101344070/210194483-da55ef87-5fb0-4d62-8613-3725ec57d3d3.png">  
왼쪽부터 순서대로 잠재고객군, 취소위험/신규고객군, 이탈고객군, 충성고객군

---

## 6️⃣ 활용방안 및 기대효과

- 다수의 제휴사 및 고객에 대한 빅데이터를 기반으로 **고객별 니즈를 고려한 맞춤형 서비스 제공**
- **금융시장의 불균형과 비효율적 구조를 개선**하고자 하는 기업의 방향성 실현
