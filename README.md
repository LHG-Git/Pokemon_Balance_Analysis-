<div align="center">
  <h1>🌳 토이프로젝트<br><br>
  ⚡ 포켓몬스터 밸런스 분석</h1>
</div>

<h4> 💭 Language : Python <br><br>
     📝 Library : Pandas, Numpy, Matplotlib, seaborn <br><br>
     🛠  Tool : Google Colab <br><br>
     📅 진행기간 : 2023.03.23 ~ 2023.06.07</h4>
     
### 👨‍👦‍👦 팀원소개
<table>
<tbody>
  <tr>
    <td align="left"><img src="" width="20px;" alt=""/><br /><b>팀원 : 이희구</b></a><br /></td>
   <tr/>
</tbody>
</table>
<br>


# 💡 분석 기획
* 포켓몬스터의 능력치 밸런스 분석<br>
* 포켓몬스터의 일반적인 능력 유형 조합 분석<br>
* 전설 포켓몬스터의 일반적인 능력 유형 조합 분석<br>
* 세대에 따른 기본 유형의 차이 분석<br>
* 가장 잡기 쉬운 포켓몬스터 및 세대 분석<br>
* 포켓몬스터가 가진 능력 개수 분석<br>
* 포켓몬스터의 BMI 분석<br>
* 가장 좋은 유형의 포켓몬스터 분석<br>
* 최고의 포켓몬스터 분석<br><br>
<br>

# 🔎 데이터 전처리
|전처리|방법|
|------|:-------------:|
|불필요한 컬럼 삭제|분석에 불필요한 컬럼들은 전부 삭제|
|값 대체|'Is_Legendary'의 0은 "Non-legendary", 1은 "Legendary"로 대체|
|컬럼 결합|'Type1' 과 'Type2'를 단일 유형으로 결합|
|형태 변환|능력 컬럼의 값 리스트형태로 변환|
|컬럼 생성|포켓몬별 능력의 개수 컬럼 생성|
|컬럼 생성|BMI 컬럼 생성|
|Nan 대체|포획률의 30 (Meteorite)255 (Core) 값 Nan으로 변경|

<br><br>

# 🔎 QGIS를 이용한 데이터 매핑
## 1) 관측소 및 갯끈풀 위치 시각화
<h3 align="center"><img src="https://github.com/LHG-Git/project/assets/100845169/3e1da9b8-d9ca-4ba1-9640-91a3269f9fed"></h3> <br><br>

* 위의 그림과 같이, 비슷한 구역에 관측소가 붙어있는 것을 확인할 수 있음
* 이런 경우 바다 격자 정보를 이용하여 해안 구역을 설정한 뒤, 한 격자에 들어오는 관측소는 하나의 관측소로 통합 시킬 것을 고려함
<br>

## 2) 바다 격자 정보 세분화
<h3 align="center"><img src="https://github.com/LHG-Git/project/assets/100845169/0d3cb4ee-9998-49bf-93a5-9ef65ccec982"></h3> <br><br>

* 한 격자내에 속해있는 관측소는 하나의 관측소로 간주하기 위한 작업
* 수집한 격자정보의 경우 3단계 격자 정보에 해당하였고, 3단계의 정보를 이용하여 해안 구역을 선정할 경우 반경 범위가 상당히 광범위했기 때문에 구체적인 지역 선정이 어렵다고 판단하여, 더 세분화하기 위한 작업인 4단계 격자로 지역을 나누는 작업 진행
<br>

## 3) 역거리 가중 보간법(IDW)을 이용한 최종 데이터셋 구성
<h3 align="center"><img src="https://github.com/LHG-Git/project/assets/100845169/9a86e60b-87c0-4276-b0fa-ea90423a100f"></h3> <br><br>

* 해양환경측정망, 해양생태계 데이터와 갯끈풀의 위치 속성을 QGIS에 표시
  
* 표시된 각 포인트 지점 사이에 비어있는 사이값을 구하기 위하여, 공간보간법을 사용할 것을 고려
  
* 특정 포인트와 속성값을 이용하여 연속적인 공간의 속성값을 도출하여, 한 격자내에 포함된 포인트는 하나의 관측소로 매핑하고자 하였고, 이때 공간보간법중 하나인 역거리 가중 보간법(IDW)를 활용
  
* 포인트 및 속성별 연속적인 래스터 데이터를 생성한 뒤 벡터로 변환하여, 이전에 세분화시킨 4단계 격자에 벡터 데이터를 할당
  
* 그 후, 작게 분할된 벡터 데이터의 변수들을 평균값을 취해 격자별 관측소 및 갯끈풀 속성 데이터를 생성하여 최종데이터셋을 완성

<br><br>

# 📊 EDA(탐색적 데이터 분석)
## 1) 상관관계 분석
<h3 align="center"><img src= "https://github.com/LHG-Git/project/assets/100845169/455f32d5-2172-4441-b1c4-04de9ea380bf"></h3>

* feature간의 상관관계가 높은 경우가 있어, 다중공선성이 우려<br>

* 추후 상관계수가 높은 각 입력 변수를 제거/추가하며 회귀계수의 변동정도 파악, 차원축소 PCA 적용, 정규화, VIF(Variance Inflation Factor)를 이용한 변수선택 등 다양한 방법론을 적용시킬 것을 고려<br>

* 상관관계 분석 시각화를 통해 target값인 화학적 산소농도와 상관관계 값이 0.15미만에 해당하는 컬럼은 전부 삭제를 진행

<br>



## 2) 관측소별 화학적 산소농도 차이 시각화
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/b16cd70a-8f7e-4527-9e8d-2d286685670d></h3>

* 동해에 위치한 모든 관측치에서는 화학적 산소농도값이 정상범위(약 1.0)을 기록

* 서해의 논산과 인천부근 그리고 남해의 부산과 창원부근의 관측치에서는 정상 수치보다 높은 화학적 산소 농도값이 기록된 것을 통해 화학적 산소농도 값이 위치적 특성에 따라 영향을 받는다는 것을 확인

<br><br>

# 📄 Modeling
## 1) 군집화
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/e22674c7-b20e-4298-b8bc-b7b9f2f4583a></h3>

* 최적의 k값 도출을 위해 <strong>실루엣 계수</strong>를 사용<br> 
* 이때 실루엣 계수 평균만을 고려하지 않았고 figure5를 통해 도출된 인사이트를 함께 고려<br>
* 그 결과 cluster별 실루엣 계수 평균이 가장 높지는 않지만, 실루엣 계수의 너비가 비교적 균일한 지점에서 <strong>최적의 k(k=6)값을 도출</strong><br><br>

<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/909e7b1d-0b40-4745-abc5-f3652ef06a84></h3>

* 최적의 K값을 통해 위치별 군집화 결과, figure 5에서 인천 부근의 서해에 위치한 관측치와, 부산 부근의 남해에 위치한 관측치에서 화학적 산소농도 수치가 높게 기록
<br>

## 2) 모델 성능 지표 선정
* 성능 지표의 경우 본 프로젝트의 주제 자체가 ‘해양정보를 활용한 해양오염 예측’이기 때문에, 모델의 설명력을 나타내는 R2값 보다 실제 예측 오차의 크기인 MAE가 본 프로젝트와 맞는 지표라고 생각하여, <strong>MAE값을 기준으로 최종 모델을 선정</strong>
<br>

## 3) 최종 모델 선정
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/da4b5525-0513-4615-a954-9778eadeda55></h3>

* 최종 예측결과 전체 모델에서 훈련세트에 <strong>약간의 과적합 존재</strong><br>
* <strong>CatBoost모델의 MAE값이 가장 준수</strong>
* 해양오염 예측에 사용될 모델을 <strong>CatBoostRegressor로 선정</strong>
<br>

## 4) 하이퍼파라미터 튜닝
* CatBoostRegressor모델의 특성상 하이퍼파라미터 튜닝을 진행하여도 모델 성능 개선에 크게 영향을 미치지 않음
* 오히려 파라미터의 default값으로 모델 예측을 수행하였을 때, 성능이 가장 높게 측정됨
<br>

## 5) K-Fold 교차검증
* 최종 선정된 모델(CatBoostRegressor)의 경우 train과 text의 성능 지표에서 약간의 과적합이 발생
* 과적합을 방지하기 위해 valid data를 생성
* 해당 데이터셋을 이용하여 가장 흔하게 사용되는 <strong>K-Fold 교차검증을 진행</strong>
* <strong>K값은 5로 지정</strong>하였으며, 모델 진행시에 골고루 데이터의 특성을 반영하기 위하여 <strong>shuffle을 진행</strong>
<br>

## 6) 모델평가 및 검증
<h3 align="center"><img src= https://github.com/LHG-Git/project/assets/100845169/1f3ca8e2-9710-404d-9425-d9a9d3f64cdf></h3>

* <strong>하이퍼파라미터 튜닝 및 K-Fold교차검증을 통하여 모델 성능 최적화를 진행하여 과적합 개선</strong>

* 그 결과 이전의 결과에서 보다 <strong>과적합이 많이 개선</strong>되었음을 확인하였고 <strong>모델의 성능 또한 향상됨</strong>

* 성능 지표의 경우 본 프로젝트의 주제가 ‘해양정보를 활용한 해양오염 예측’ 이기 때문에, 모델의 설명력을 나타내는 R2값 보다 실제 예측 오차의 크기인 MAE가 본 프로젝트와 맞는 지표라고 판단

* <strong>최종 모델링 결과 약 MAE = 0.076으로 실제값과 약 0.076이 차이가 나는 모델 완성</strong>

* 최근 10년동안 국내 연안의 화학적 산소농도가 연평균 1.13~1.82mg/L인 것을 고려했을 때, 꽤 정확도가 높은 모델이라고 설명할 수 있음



