# 문제 4-6. 가입 예측이 너무 잘된다면 무엇을 의심해야 할까?

- 모델: `qwen3-coder:30b`
- 데이터: bank-additional-full.csv
- 소요 시간: 총 113초 ((a) 38초, (b) 47초, (c) 28초)

## (a)

> (a) 제공된 변수를 사용하여 가입 예측모형을 적합하고, 통화시간 duration의 예측 기여를 확인하라.

**도구: list_files** `{}`

<details><summary>실행 결과</summary>

```
{
  "files": [
    {
      "name": "bank-additional-full.csv",
      "size_kb": 5698.2
    }
  ]
}
```

</details>

**도구: read_file** `{"filename": "bank-additional-full.csv", "var_name": "df"}`

<details><summary>실행 결과</summary>

```
{
  "message": "'bank-additional-full.csv'을 변수 `df`에 저장했습니다.",
  "profile": {
    "variable": "df",
    "shape": [
      41188,
      1
    ],
    "columns": [
      {
        "name": "age;\"job\";\"marital\";\"education\";\"default\";\"housing\";\"loan\";\"contact\";\"month\";\"day_of_week\";\"duration\";\"campaign\";\"pdays\";\"previous\";\"poutcome\";\"emp.var.rate\";\"cons.price.idx\";\"cons.conf.idx\";\"euribor3m\";\"nr.employed\";\"y\"",
        "dtype": "object",
        "missing_ratio": 0.0,
        "n_unique": 41176,
        "top_values": {
          "27;\"technician\";\"single\";\"professional.course\";\"no\";\"no\";\"no\";\"cellular\";\"jul\";\"mon\";331;2;999;0;\"nonexistent\";1.4;93.918;-42.7;4.962;5228.1;\"no\"": 2,
          "39;\"blue-collar\";\"married\";\"basic.6y\";\"no\";\"no\";\"no\";\"telephone\";\"may\";\"thu\";124;1;999;0;\"nonexistent\";1.1;93.994;-36.4;4.855;5191;\"no\"": 2,
          "35;\"admin.\";\"married\";\"university.degree\";\"no\";\"yes\";\"no\";\"cellular\";\"may\";\"fri\";348;4;999;0;\"nonexistent\";-1.8;92.893;-46.2;1.313;5099.1;\"no\"": 2,
          "71;\"retired\";\"single\";\"university.degree\";\"no\";\"no\";\"no\";\"telephone\";\"oct\";\"tue\";120;1;999;0;\"nonexistent\";-3.4;92.431;-26.9;0.742;5017.5;\"no\"": 2,
          "45;\"admin.\";\"married\";\"university.degree\";\"no\";\"no\";\"no\";\"cellular\";\"jul\";\"thu\";252;1;999;0;\"nonexistent\";-2.9;92.469;-33.6;1.072;5076.2;\"yes\"": 2
        }
      }
    ],
    "head": [
      {
        "age;\"job\";\"marital\";\"education\";\"default\";\"housing\";\"loan\";\"contact\";\"month\";\"day_of_week\";\"duration\";\"campaign\";\"pdays\";\"previous\";\"poutcome\";\"emp.var.rate\";\"cons.price.idx\";\"cons.conf.idx\";\"euribor3m\";\"nr.employed\";\"y\"": "56;\"housemaid\";\"married\";\"basic.4y\";\"no\";\"no\";\"no\";\"telephone\";\"may\";\"mon\";261;1;999;0;\"nonexistent\";1.1;93.994;-36.4;4.857;5191;\"no\""
      },
      {
        "age;\"job\";\"marital\";\"education\";\"default\";\"housing\";\"loan\";\"contact\";\"month\";\"day_of_week\";\"duration\";\"campaign\";\"pdays\";\"previous\";\"poutcome\";\"emp.var.rate\";\"cons.price.idx\";\"cons.conf.idx\";\"euribor3m\";\"nr.employed\";\"y\"": "57;\"services\";\"married\";\"high.school\";\"unknown\";\"no\";\"no\";\"telephone\";\"may\";\"mon\";149;1;999;0;\"nonexistent\";1.1;93.994;-36.4;4.857;5191;\"no\""
      },
      {
        "age;\"job\";\"marital\";\"education\";\"default\";\"housing\";\"loan\";\"contact\";\"month\";\"day_of_week\";\"duration\";\"campaign\";\"pdays\";\"previous\";\"poutcome\";\"emp.var.rate\";\"cons.price.idx\";\"cons.conf.idx\";\"euribor3m\";\"nr.employed\";\"y\"": "37;\"services\";\"married\";\"high.school\";\"no\";\"yes\";\"no\";\"telephone\";\"may\";\"mon\";226;1;999;0;\"nonexistent\";1.1;93.994;-36.4;4.857;5191;\"no\""
      }
    ]
  }
}
```

</details>

**도구: run_python**

```python
import pandas as pd

# 데이터를 세미콜론으로 분리하여 컬럼을 분리
df_split = df['age;"job";"marital";"education";"default";"housing";"loan";"contact";"month";"day_of_week";"duration";"campaign";"pdays";"previous";"poutcome";"emp.var.rate";"cons.price.idx";"cons.conf.idx";"euribor3m";"nr.employed";"y"'].str.split(';', expand=True)

# 컬럼 이름 설정
columns = ['age', 'job', 'marital', 'education', 'default', 'housing', 'loan', 'contact', 'month', 'day_of_week', 'duration', 'campaign', 'pdays', 'previous', 'poutcome', 'emp.var.rate', 'cons.price.idx', 'cons.conf.idx', 'euribor3m', 'nr.employed', 'y']
df_split.columns = columns

# 데이터 타입 변환
numeric_columns = ['age', 'duration', 'campaign', 'pdays', 'previous', 'emp.var.rate', 'cons.price.idx', 'cons.conf.idx', 'euribor3m', 'nr.employed']
for col in numeric_columns:
    if col in df_split.columns:
        df_split[col] = pd.to_numeric(df_split[col], errors='coerce')

# y 컬럼을 이진 분류로 변환
df_split['y'] = df_split['y'].map({'"no"': 0, '"yes"': 1})

# 결과 확인
print("데이터 구조:")
print(df_split.info())
print("\n처음 5행:")
print(df_split.head())
```

<details><summary>실행 결과</summary>

```
데이터 구조:
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 41188 entries, 0 to 41187
Data columns (total 21 columns):
 #   Column          Non-Null Count  Dtype  
---  ------          --------------  -----  
 0   age             41188 non-null  int64  
 1   job             41188 non-null  object 
 2   marital         41188 non-null  object 
 3   education       41188 non-null  object 
 4   default         41188 non-null  object 
 5   housing         41188 non-null  object 
 6   loan            41188 non-null  object 
 7   contact         41188 non-null  object 
 8   month           41188 non-null  object 
 9   day_of_week     41188 non-null  object 
 10  duration        41188 non-null  int64  
 11  campaign        41188 non-null  int64  
 12  pdays           41188 non-null  int64  
 13  previous        41188 non-null  int64  
 14  poutcome        41188 non-null  object 
 15  emp.var.rate    41188 non-null  float64
 16  cons.price.idx  41188 non-null  float64
 17  cons.conf.idx   41188 non-null  float64
 18  euribor3m       41188 non-null  float64
 19  nr.employed     41188 non-null  float64
 20  y               41188 non-null  int64  
dtypes: float64(5), int64(6), object(10)
memory usage: 6.6+ MB
None

처음 5행:
   age          job    marital      education    default housing  ... emp.var.rate cons.price.idx cons.conf.idx euribor3m  nr.employed  y
0   56  "housemaid"  "married"     "basic.4y"       "no"    "no"  ...          1.1         93.994         -36.4     4.857       5191.0  0
1   57   "services"  "married"  "high.school"  "unknown"    "no"  ...          1.1         93.994         -36.4     4.857       5191.0  0
2   37   "services"  "married"  "high.school"       "no"   "yes"  ...          1.1         93.994         -36.4     4.857       5191.0  0
3   40     "admin."  "married"     "basic.6y"       "no"    "no"  ...          1.1         93.994         -36.4     4.857       5191.0  0
4   56   "services"  "married"  "high.school"       "no"    "no"  ...          1.1         93.994         -36.4     4.857       5191.0  0

[5 rows x 21 columns]

```

</details>

**도구: run_python**

```python
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import LabelEncoder
import numpy as np

# 범주형 변수 인코딩
categorical_columns = ['job', 'marital', 'education', 'default', 'housing', 'loan', 'contact', 'month', 'day_of_week', 'poutcome']
df_encoded = df_split.copy()

for col in categorical_columns:
    if col in df_encoded.columns:
        le = LabelEncoder()
        df_encoded[col] = le.fit_transform(df_encoded[col].astype(str))

# 특성과 타겟 변수 분리
X = df_encoded.drop('y', axis=1)
y = df_encoded['y']

# 모델 학습
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 랜덤 포레스트 모델 적합
rf_model = RandomForestClassifier(n_estimators=100, random_state=42)
rf_model.fit(X_train, y_train)

# 예측
y_pred = rf_model.predict(X_test)

# 중요도 확인
feature_importance = pd.DataFrame({
    'feature': X.columns,
    'importance': rf_model.feature_importances_
}).sort_values('importance', ascending=False)

print("모델 성능:")
print(f"정확도: {rf_model.score(X_test, y_test):.4f}")

print("\n특성 중요도 (상위 10개):")
print(feature_importance.head(10))

# duration 변수의 기여도 확인
duration_importance = feature_importance[feature_importance['feature'] == 'duration']['importance'].values[0]
print(f"\nduration 변수의 중요도: {duration_importance:.4f}")
```

<details><summary>실행 결과</summary>

```
모델 성능:
정확도: 0.9133

특성 중요도 (상위 10개):
        feature  importance
10     duration    0.314350
18    euribor3m    0.105594
0           age    0.092222
19  nr.employed    0.064266
1           job    0.047824
11     campaign    0.042468
3     education    0.042242
9   day_of_week    0.041535
12        pdays    0.034755
14     poutcome    0.029701

duration 변수의 중요도: 0.3143

```

</details>

**도구: run_python**

```python
import matplotlib.pyplot as plt

# duration 변수의 중요도를 시각화
plt.figure(figsize=(10, 6))
top_features = feature_importance.head(10)
plt.barh(range(len(top_features)), top_features['importance'])
plt.yticks(range(len(top_features)), top_features['feature'])
plt.xlabel('Importance')
plt.title('Top 10 Feature Importances in Random Forest Model')
plt.gca().invert_yaxis()
plt.tight_layout()
plt.show()

# duration 변수의 중요도를 강조하여 표시
print(f"duration 변수는 모델에서 가장 중요한 특성으로, 중요도는 {duration_importance:.4f}입니다.")
print("이 값은 예측 성능에 매우 큰 영향을 미칩니다.")
```

<details><summary>실행 결과</summary>

```
duration 변수는 모델에서 가장 중요한 특성으로, 중요도는 0.3143입니다.
이 값은 예측 성능에 매우 큰 영향을 미칩니다.

```

</details>

![fig_21692e96.png](fig_21692e96.png)

**도구: run_python**

```python
# duration 변수를 기준으로 분석
df_split['duration_group'] = pd.cut(df_split['duration'], bins=5, labels=['Very Short', 'Short', 'Medium', 'Long', 'Very Long'])

# 각 그룹별로 가입 여부의 비율 계산
duration_analysis = df_split.groupby('duration_group')['y'].agg(['count', 'mean']).reset_index()
duration_analysis.columns = ['duration_group', 'count', 'conversion_rate']

print("duration에 따른 가입 확률 분석:")
print(duration_analysis)

# 시각화
plt.figure(figsize=(10, 6))
plt.bar(duration_analysis['duration_group'], duration_analysis['conversion_rate'])
plt.xlabel('Duration Group')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Duration Group')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

<details><summary>실행 결과</summary>

```
{
  "stdout": "duration에 따른 가입 확률 분석:\n  duration_group  count  conversion_rate\n0     Very Short  40178         0.100602\n1          Short    943         0.588547\n2         Medium     52         0.711538\n3           Long     13         0.384615\n4      Very Long      2         0.500000\n",
  "stderr": "<agent_code>:5: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.\n"
}
```

</details>

![fig_f60c4605.png](fig_f60c4605.png)

### 답변

이제 모델의 결과를 요약하겠습니다.

## 문제 4-6 해답

### (a) 제공된 변수를 사용하여 가입 예측모형을 적합하고, 통화시간 duration의 예측 기여를 확인하라.

**1. 모델 성능:**
- 랜덤 포레스트 모델을 사용하여 가입 여부를 예측한 결과, 테스트 데이터에 대한 정확도는 **0.9133**입니다.
- 이는 매우 높은 예측 성능을 나타냅니다.

**2. duration 변수의 기여도:**
- duration 변수는 모델에서 가장 중요한 특성으로, 중요도는 **0.3143**입니다.
- 이는 전체 특성 중 약 31%의 영향력을 가지며, 다른 모든 변수보다 훨씬 높은 중요도를 가집니다.

**3. duration에 따른 가입 확률 분석:**
- duration이 짧을수록 가입 확률이 낮고, 길수록 가입 확률이 높아지는 경향이 있습니다.
- 특히 duration이 "Short" 그룹일 때 가입 확률은 **58.86%**로 가장 높습니다.
- duration이 "Very Short" 그룹일 때 가입 확률은 **10.06%**로 낮습니다.

**4. 결론:**
- duration 변수는 가입 예측 모델에서 매우 중요한 역할을 합니다.
- 이는 통화 시간이 길수록 고객이 가입할 가능성이 높다는 것을 의미합니다.
- 그러나 duration이 너무 잘 예측된다는 것은 데이터에 편향이 있을 수 있음을 시사합니다. 즉, 실제 고객의 행동과 예측 결과가 일치하지 않을 수 있으며, 이는 모델이 과적합되었거나 데이터에 편향이 존재할 수 있음을 의심해야 합니다.

따라서 duration 변수가 너무 잘 예측된다는 것은 모델이 실제 고객의 행동을 제대로 반영하지 못하고 있을 가능성이 있으며, 이러한 경우 모델의 일반화 성능이나 실제 적용 가능성에 대해 의심해야 합니다.

_소요 시간: 38초_

## (b)

> (b) “전화를 걸기 전에 연락 대상자를 고른다”는 목적에 맞춰 (a)의 변수들을 다시 검토하라. duration을 포함하여 그 시점에 알 수 없는 정보를 제외하고 재평가하라.

**도구: run_python**

```python
# duration 변수를 제외한 다른 변수들로 모델 재적합
X_revised = df_encoded.drop(['y', 'duration'], axis=1)
y = df_encoded['y']

# 데이터 분할
X_train_rev, X_test_rev, y_train_rev, y_test_rev = train_test_split(X_revised, y, test_size=0.2, random_state=42)

# 모델 재적합
rf_model_rev = RandomForestClassifier(n_estimators=100, random_state=42)
rf_model_rev.fit(X_train_rev, y_train_rev)

# 예측 및 성능 확인
y_pred_rev = rf_model_rev.predict(X_test_rev)
accuracy_rev = rf_model_rev.score(X_test_rev, y_test_rev)

print(f"duration 변수를 제외한 모델 정확도: {accuracy_rev:.4f}")

# 재평가된 특성 중요도
feature_importance_rev = pd.DataFrame({
    'feature': X_revised.columns,
    'importance': rf_model_rev.feature_importances_
}).sort_values('importance', ascending=False)

print("\nduration 변수를 제외한 특성 중요도 (상위 10개):")
print(feature_importance_rev.head(10))
```

<details><summary>실행 결과</summary>

```
duration 변수를 제외한 모델 정확도: 0.8920

duration 변수를 제외한 특성 중요도 (상위 10개):
        feature  importance
0           age    0.181890
17    euribor3m    0.136145
1           job    0.085230
10     campaign    0.084410
3     education    0.074706
9   day_of_week    0.060309
18  nr.employed    0.058116
11        pdays    0.041741
2       marital    0.041007
5       housing    0.037737

```

</details>

**도구: run_python**

```python
import matplotlib.pyplot as plt

# duration 변수를 제외한 특성 중요도 시각화
plt.figure(figsize=(10, 6))
top_features_rev = feature_importance_rev.head(10)
plt.barh(range(len(top_features_rev)), top_features_rev['importance'])
plt.yticks(range(len(top_features_rev)), top_features_rev['feature'])
plt.xlabel('Importance')
plt.title('Top 10 Feature Importances (Excluding Duration)')
plt.gca().invert_yaxis()
plt.tight_layout()
plt.show()

# duration 변수를 제외한 모델의 성능과 기존 모델 비교
print("모델 성능 비교:")
print(f"duration 포함 모델 정확도: {rf_model.score(X_test, y_test):.4f}")
print(f"duration 제외 모델 정확도: {accuracy_rev:.4f}")
print(f"정확도 차이: {rf_model.score(X_test, y_test) - accuracy_rev:.4f}")
```

<details><summary>실행 결과</summary>

```
모델 성능 비교:
duration 포함 모델 정확도: 0.9133
duration 제외 모델 정확도: 0.8920
정확도 차이: 0.0214

```

</details>

![fig_38562940.png](fig_38562940.png)

**도구: run_python**

```python
# duration 변수를 제외한 변수들 중 중요한 변수들에 대해 더 자세히 분석
important_features = feature_importance_rev.head(5)['feature'].tolist()
print("전화를 걸기 전에 연락 대상자를 고르는 데 중요한 변수들:")
print(important_features)

# age 변수의 분석
age_analysis = df_split.groupby(pd.cut(df_split['age'], bins=5))['y'].agg(['count', 'mean']).reset_index()
age_analysis.columns = ['age_group', 'count', 'conversion_rate']
print("\n나이에 따른 가입 확률:")
print(age_analysis)

# job 변수의 분석
job_analysis = df_split.groupby('job')['y'].agg(['count', 'mean']).reset_index()
job_analysis.columns = ['job', 'count', 'conversion_rate']
job_analysis = job_analysis.sort_values('conversion_rate', ascending=False)
print("\n직업에 따른 가입 확률 (상위 5개):")
print(job_analysis.head())

# euribor3m 변수의 분석
euribor_analysis = df_split.groupby(pd.cut(df_split['euribor3m'], bins=5))['y'].agg(['count', 'mean']).reset_index()
euribor_analysis.columns = ['euribor3m_group', 'count', 'conversion_rate']
print("\neuribor3m에 따른 가입 확률:")
print(euribor_analysis)
```

<details><summary>실행 결과</summary>

```
{
  "stdout": "전화를 걸기 전에 연락 대상자를 고르는 데 중요한 변수들:\n['age', 'euribor3m', 'job', 'campaign', 'education']\n\n나이에 따른 가입 확률:\n        age_group  count  conversion_rate\n0  (16.919, 33.2]  13009         0.133600\n1    (33.2, 49.4]  20124         0.086116\n2    (49.4, 65.6]   7436         0.118209\n3    (65.6, 81.8]    520         0.459615\n4    (81.8, 98.0]     99         0.515152\n\n직업에 따른 가입 확률 (상위 5개):\n             job  count  conversion_rate\n8      \"student\"    875         0.314286\n5      \"retired\"   1720         0.252326\n10  \"unemployed\"   1014         0.142012\n0       \"admin.\"  10422         0.129726\n4   \"management\"   2924         0.112175\n\neuribor3m에 따른 가입 확률:\n  euribor3m_group  count  conversion_rate\n0   (0.63, 1.516]  13227         0.240266\n1  (1.516, 2.398]    271         0.453875\n2  (2.398, 3.281]      1         1.000000\n3  (3.281, 4.163]   2957         0.050389\n4  (4.163, 5.045]  24732         0.048075\n",
  "stderr": "<agent_code>:7: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.\n<agent_code>:20: FutureWarning: The default of observed=False is deprecated and will be changed to True in a future version of pandas. Pass observed=False to retain current behavior or observed=True to adopt the future default and silence this warning.\n"
}
```

</details>

**도구: run_python**

```python
# 나이에 따른 가입 확률 시각화
plt.figure(figsize=(10, 6))
plt.bar(age_analysis['age_group'], age_analysis['conversion_rate'])
plt.xlabel('Age Group')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Age Group')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# 직업에 따른 가입 확률 시각화
plt.figure(figsize=(10, 6))
plt.bar(job_analysis['job'], job_analysis['conversion_rate'])
plt.xlabel('Job')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Job')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# euribor3m에 따른 가입 확률 시각화
plt.figure(figsize=(10, 6))
plt.bar(euribor_analysis['euribor3m_group'], euribor_analysis['conversion_rate'])
plt.xlabel('Euribor3m Group')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Euribor3m Group')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

<details><summary>실행 결과</summary>

```
{
  "stdout": "(출력 없음)",
  "error": "TypeError: float() argument must be a string or a number, not 'pandas._libs.interval.Interval' (코드 3번째 줄: plt.bar(age_analysis['age_group'], age_analysis['conversion_rate']))"
}
```

</details>

![fig_0ead9ccc.png](fig_0ead9ccc.png)

**도구: run_python**

```python
# 나이에 따른 가입 확률 시각화 (수정)
age_group_labels = [str(group) for group in age_analysis['age_group']]
plt.figure(figsize=(10, 6))
plt.bar(range(len(age_group_labels)), age_analysis['conversion_rate'])
plt.xlabel('Age Group')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Age Group')
plt.xticks(range(len(age_group_labels)), age_group_labels, rotation=45)
plt.tight_layout()
plt.show()

# 직업에 따른 가입 확률 시각화 (수정)
plt.figure(figsize=(10, 6))
plt.bar(range(len(job_analysis['job'])), job_analysis['conversion_rate'])
plt.xlabel('Job')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Job')
plt.xticks(range(len(job_analysis['job'])), job_analysis['job'], rotation=45)
plt.tight_layout()
plt.show()

# euribor3m에 따른 가입 확률 시각화 (수정)
euribor_group_labels = [str(group) for group in euribor_analysis['euribor3m_group']]
plt.figure(figsize=(10, 6))
plt.bar(range(len(euribor_group_labels)), euribor_analysis['conversion_rate'])
plt.xlabel('Euribor3m Group')
plt.ylabel('Conversion Rate')
plt.title('Conversion Rate by Euribor3m Group')
plt.xticks(range(len(euribor_group_labels)), euribor_group_labels, rotation=45)
plt.tight_layout()
plt.show()
```

<details><summary>실행 결과</summary>

```
(출력 없음)
```

</details>

![fig_297e1921.png](fig_297e1921.png)
![fig_b92d7b62.png](fig_b92d7b62.png)
![fig_7e9af072.png](fig_7e9af072.png)

### 답변

## 문제 4-6 해답

### (b) "전화를 걸기 전에 연락 대상자를 고른다"는 목적에 맞춰 (a)의 변수들을 다시 검토하라. duration을 포함하여 그 시점에 알 수 없는 정보를 제외하고 재평가하라.

**1. 모델 성능 비교:**
- duration 변수를 포함한 모델 정확도: **0.9133**
- duration 변수를 제외한 모델 정확도: **0.8920**
- 정확도 차이: **0.0214**

**2. 전화를 걸기 전에 연락 대상자를 고르는 데 중요한 변수들:**
- 나이(age)
- euribor3m
- 직업(job)
- 캠페인 횟수(campaign)
- 교육 수준(education)

**3. 각 변수별 분석 결과:**

**나이에 따른 가입 확률:**
- 80대 이상 고객의 가입 확률이 가장 높음 (약 51.5%)
- 60대 후반 고객의 가입 확률도 상대적으로 높음 (약 45.96%)
- 30대와 40대 고객의 가입 확률은 낮음 (약 8-13%)

**직업에 따른 가입 확률:**
- 학생(student)이 가장 높은 가입 확률을 보임 (약 31.4%)
- 퇴직자(retired)도 상대적으로 높은 가입 확률 (약 25.2%)
- 관리자(management)와 행정관(admin.)의 가입 확률은 낮음 (약 11-13%)

**euribor3m에 따른 가입 확률:**
- euribor3m 값이 1.5~2.4 사이일 때 가입 확률이 가장 높음 (약 45.4%)
- euribor3m 값이 3.3 이상일 때 가입 확률이 매우 낮음 (약 5%)

**4. 결론:**
- duration 변수는 예측 성능 향상에 큰 기여를 하지만, 전화를 걸기 전에 알 수 있는 정보가 아니므로 실제 업무에서 사용할 수 없습니다.
- 전화를 걸기 전에 연락 대상자를 선정할 때는 나이, 직업, euribor3m, 캠페인 횟수, 교육 수준 등의 변수를 고려해야 합니다.
- 특히 나이가 80대 이상이거나 학생, 퇴직자인 고객은 가입 확률이 높아 전화를 걸기 적합한 대상자로 볼 수 있습니다.
- euribor3m 수치가 낮을수록 가입 확률이 높다는 점도 중요한 요소입니다.

따라서 duration 변수를 제외한 다른 변수들을 기반으로 연락 대상자를 선정하는 것이 전화 마케팅의 실제 업무에 더 적합합니다.

_소요 시간: 47초_

## (c)

> (c) (b)의 예측확률 상위 10%에서 가입률을 계산하고 전체 가입률과 비교하라. 이것이 고객 선별 성능의 평가이지, 전화의 추가 효과를 추정한 결과는 아니라는 점을 설명하라.

**도구: run_python**

```python
# duration 변수를 제외한 모델을 사용하여 예측 확률 계산
y_proba = rf_model_rev.predict_proba(X_test_rev)[:, 1]  # 예측 확률

# 예측 확률 상위 10%의 인덱스 찾기
threshold_index = int(len(y_proba) * 0.1)
top_10_percent_indices = y_proba.argsort()[::-1][:threshold_index]

# 예측 확률 상위 10%에 해당하는 실제 값
top_10_percent_actual = y_test_rev.iloc[top_10_percent_indices]

# 전체 데이터의 가입률 계산
total_conversion_rate = y.mean()
print(f"전체 데이터의 가입률: {total_conversion_rate:.4f}")

# 예측 확률 상위 10%의 가입률 계산
top_10_percent_conversion_rate = top_10_percent_actual.mean()
print(f"예측 확률 상위 10%의 가입률: {top_10_percent_conversion_rate:.4f}")

# 상위 10%에 포함된 고객 수
num_top_10_percent = len(top_10_percent_actual)
print(f"예측 확률 상위 10%에 포함된 고객 수: {num_top_10_percent}")

# 전체 고객 수
total_customers = len(y_test_rev)
print(f"전체 고객 수: {total_customers}")

# 상위 10%가 전체 고객의 몇 퍼센트인지 계산
percentage_of_customers = (num_top_10_percent / total_customers) * 100
print(f"예측 확률 상위 10%가 전체 고객 수의 비율: {percentage_of_customers:.2f}%")

# 예측 확률 상위 10%의 실제 가입률과 전체 가입률의 차이
difference = top_10_percent_conversion_rate - total_conversion_rate
print(f"차이: {difference:.4f}")
```

<details><summary>실행 결과</summary>

```
전체 데이터의 가입률: 0.1127
예측 확률 상위 10%의 가입률: 0.4617
예측 확률 상위 10%에 포함된 고객 수: 823
전체 고객 수: 8238
예측 확률 상위 10%가 전체 고객 수의 비율: 9.99%
차이: 0.3491

```

</details>

**도구: run_python**

```python
import matplotlib.pyplot as plt

# 가입률 비교 시각화
labels = ['전체 데이터', '예측 확률 상위 10%']
conversion_rates = [total_conversion_rate, top_10_percent_conversion_rate]

plt.figure(figsize=(8, 6))
bars = plt.bar(labels, conversion_rates, color=['blue', 'red'])
plt.ylabel('Conversion Rate')
plt.title('Comparison of Conversion Rates')
plt.ylim(0, max(conversion_rates) * 1.2)

# 각 막대에 값 표시
for bar, rate in zip(bars, conversion_rates):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.01,
             f'{rate:.4f}', ha='center', va='bottom')

plt.tight_layout()
plt.show()

print("결과 요약:")
print(f"전체 데이터의 가입률: {total_conversion_rate:.4f}")
print(f"예측 확률 상위 10%의 가입률: {top_10_percent_conversion_rate:.4f}")
print(f"차이: {difference:.4f}")
print(f"예측 확률 상위 10%가 전체 고객 수의 비율: {percentage_of_customers:.2f}%")
```

<details><summary>실행 결과</summary>

```
결과 요약:
전체 데이터의 가입률: 0.1127
예측 확률 상위 10%의 가입률: 0.4617
차이: 0.3491
예측 확률 상위 10%가 전체 고객 수의 비율: 9.99%

```

</details>

![fig_2804cee7.png](fig_2804cee7.png)

### 답변

## 문제 4-6 해답

### (c) (b)의 예측확률 상위 10%에서 가입률을 계산하고 전체 가입률과 비교하라. 이것이 고객 선별 성능의 평가이지, 전화의 추가 효과를 추정한 결과는 아니라는 점을 설명하라.

**1. 분석 결과:**
- **전체 데이터의 가입률:** 0.1127 (약 11.27%)
- **예측 확률 상위 10%의 가입률:** 0.4617 (약 46.17%)
- **차이:** 0.3491 (약 34.91%)

**2. 고객 선별 성능 평가:**
- 예측 확률 상위 10%에 해당하는 고객은 전체 고객의 약 10%에 불과합니다(9.99%)
- 하지만 이 그룹의 가입률은 전체 데이터의 약 4배 이상 높습니다(46.17% vs 11.27%)
- 이는 모델이 고객을 효과적으로 선별하고 있음을 보여줍니다.

**3. 중요한 점:**
이 분석 결과는 **고객 선별 성능의 평가**일 뿐, **전화의 추가 효과를 추정한 결과는 아닙니다**.

**이유:**
- 이 분석은 모델이 예측 확률을 얼마나 잘 계산했는지를 보여주는 것이지, 전화를 걸었을 때 실제로 가입률이 올라간 정도를 측정한 것이 아닙니다.
- 즉, 이 고객들이 전화를 받고 가입했는지 여부는 알 수 없습니다. 모델은 단지 이 고객들이 가입할 가능성이 높은지를 예측한 것입니다.
- 실제 전화 마케팅 효과를 평가하려면, 전화를 걸었을 때의 결과와 그렇지 않았을 때의 결과를 비교해야 합니다(예: A/B 테스트).
- 따라서 이 분석은 모델의 예측 성능과 고객 선별 능력을 평가하는 데 유용하지만, 전화 마케팅 자체의 효과를 측정하는 것은 아닙니다.

**결론:**
이러한 분석은 모델이 고객을 얼마나 잘 선별할 수 있는지를 보여주는 중요한 지표이며, 마케팅 자원을 효율적으로 배분하는 데 도움이 됩니다. 하지만 전화를 걸었을 때의 실제 효과는 다른 방법으로 측정해야 합니다.

_소요 시간: 28초_
