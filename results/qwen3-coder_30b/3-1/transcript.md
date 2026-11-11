# 문제 3-1. 생존율 차이는 얼마나 크고 불확실할까?

- 모델: `qwen3-coder:30b`
- 데이터: train.csv
- 소요 시간: 총 110초 ((a) 24초, (b) 37초, (c) 49초)

## (a)

> (a) 성별 생존율, 생존율 차이, 그 차이의 신뢰구간을 구하라. 단순한 독립 관측 가정 아래의 계산임을 명시하라.

**도구: list_files** `{}`

<details><summary>실행 결과</summary>

```
{
  "files": [
    {
      "name": "train.csv",
      "size_kb": 59.8
    }
  ]
}
```

</details>

**도구: read_file** `{"filename": "train.csv", "var_name": "df"}`

<details><summary>실행 결과</summary>

```
{
  "message": "'train.csv'을 변수 `df`에 저장했습니다.",
  "profile": {
    "variable": "df",
    "shape": [
      891,
      12
    ],
    "columns": [
      {
        "name": "PassengerId",
        "dtype": "int64",
        "missing_ratio": 0.0,
        "n_unique": 891,
        "mean": 446.0,
        "std": 257.3538,
        "min": 1.0,
        "max": 891.0
      },
      {
        "name": "Survived",
        "dtype": "int64",
        "missing_ratio": 0.0,
        "n_unique": 2,
        "mean": 0.3838,
        "std": 0.4866,
        "min": 0.0,
        "max": 1.0
      },
      {
        "name": "Pclass",
        "dtype": "int64",
        "missing_ratio": 0.0,
        "n_unique": 3,
        "mean": 2.3086,
        "std": 0.8361,
        "min": 1.0,
        "max": 3.0
      },
      {
        "name": "Name",
        "dtype": "object",
        "missing_ratio": 0.0,
        "n_unique": 891,
        "top_values": {
          "Braund, Mr. Owen Harris": 1,
          "Boulos, Mr. Hanna": 1,
          "Frolicher-Stehli, Mr. Maxmillian": 1,
          "Gilinski, Mr. Eliezer": 1,
          "Murdlin, Mr. Joseph": 1
        }
      },
      {
        "name": "Sex",
        "dtype": "object",
        "missing_ratio": 0.0,
        "n_unique": 2,
        "top_values": {
          "male": 577,
          "female": 314
        }
      },
      {
        "name": "Age",
        "dtype": "float64",
        "missing_ratio": 0.199,
        "n_unique": 88,
        "mean": 29.6991,
        "std": 14.5265,
        "min": 0.42,
        "max": 80.0
      },
      {
        "name": "SibSp",
        "dtype": "int64",
        "missing_ratio": 0.0,
        "n_unique": 7,
        "mean": 0.523,
        "std": 1.1027,
        "min": 0.0,
        "max": 8.0
      },
      {
        "name": "Parch",
        "dtype": "int64",
        "missing_ratio": 0.0,
        "n_unique": 7,
        "mean": 0.3816,
        "std": 0.8061,
        "min": 0.0,
        "max": 6.0
      },
      {
        "name": "Ticket",
        "dtype": "object",
        "missing_ratio": 0.0,
        "n_unique": 681,
        "top_values": {
          "347082": 7,
          "CA. 2343": 7,
          "1601": 7,
          "3101295": 6,
          "CA 2144": 6
        }
      },
      {
        "name": "Fare",
        "dtype": "float64",
        "missing_ratio": 0.0,
        "n_unique": 248,
        "mean": 32.2042,
        "std": 49.6934,
        "min": 0.0,
        "max": 512.3292
      },
      {
        "name": "Cabin",
        "dtype": "object",
        "missing_ratio": 0.771,
        "n_unique": 147,
        "top_values": {
          "nan": 687,
          "C23 C25 C27": 4,
          "G6": 4,
          "B96 B98": 4,
          "C22 C26": 3
        }
      },
      {
        "name": "Embarked",
        "dtype": "object",
        "missing_ratio": 0.002,
        "n_unique": 3,
        "top_values": {
          "S": 644,
          "C": 168,
          "Q": 77,
          "nan": 2
        }
      }
    ],
    "head": [
      {
        "PassengerId": "1",
        "Survived": "0",
        "Pclass": "3",
        "Name": "Braund, Mr. Owen Harris",
        "Sex": "male",
        "Age": "22.0",
        "SibSp": "1",
        "Parch": "0",
        "Ticket": "A/5 21171",
        "Fare": "7.25",
        "Cabin": "nan",
        "Embarked": "S"
      },
      {
        "PassengerId": "2",
        "Survived": "1",
        "Pclass": "1",
        "Name": "Cumings, Mrs. John Bradley (Florence Briggs Thayer)",
        "Sex": "female",
        "Age": "38.0",
        "SibSp": "1",
        "Parch": "0",
        "Ticket": "PC 17599",
        "Fare": "71.2833",
        "Cabin": "C85",
        "Embarked": "C"
      },
      {
        "PassengerId": "3",
        "Survived": "1",
        "Pclass": "3",
        "Name": "Heikkinen, Miss. Laina",
        "Sex": "female",
        "Age": "26.0",
        "SibSp": "0",
        "Parch": "0",
        "Ticket": "STON/O2. 3101282",
        "Fare": "7.925",
        "Cabin": "nan",
        "Embarked": "S"
      }
    ]
  }
}
```

</details>

**도구: run_python**

```python
import pandas as pd
import numpy as np
from scipy import stats

# 성별 생존율 계산
survival_by_sex = df.groupby('Sex')['Survived'].agg(['count', 'sum', 'mean']).reset_index()
survival_by_sex.columns = ['Sex', 'Total', 'Survived', 'Survival_Rate']

print("성별 생존율:")
print(survival_by_sex)

# 남성과 여성의 생존율 차이
male_survival_rate = survival_by_sex[survival_by_sex['Sex'] == 'male']['Survival_Rate'].values[0]
female_survival_rate = survival_by_sex[survival_by_sex['Sex'] == 'female']['Survival_Rate'].values[0]
survival_difference = female_survival_rate - male_survival_rate

print(f"\n남성 생존율: {male_survival_rate:.4f}")
print(f"여성 생존율: {female_survival_rate:.4f}")
print(f"생존율 차이: {survival_difference:.4f}")

# 신뢰구간 계산 (비모집단의 평균 차이에 대한 95% 신뢰구간)
# 각 성별의 생존자 수와 전체 수를 구함
male_total = survival_by_sex[survival_by_sex['Sex'] == 'male']['Total'].values[0]
male_survived = survival_by_sex[survival_by_sex['Sex'] == 'male']['Survived'].values[0]
female_total = survival_by_sex[survival_by_sex['Sex'] == 'female']['Total'].values[0]
female_survived = survival_by_sex[survival_by_sex['Sex'] == 'female']['Survived'].values[0]

# Wilson score interval을 사용하여 각 성별의 95% 신뢰구간 계산
def wilson_score_interval(successes, trials, confidence=0.95):
    z = stats.norm.ppf((1 + confidence) / 2)
    p_hat = successes / trials
    denominator = 1 + z**2 / trials
    center = (p_hat + z**2 / (2 * trials)) / denominator
    margin = z * np.sqrt((p_hat * (1 - p_hat) / trials + z**2 / (4 * trials**2))) / denominator
    return center - margin, center + margin

male_ci_lower, male_ci_upper = wilson_score_interval(male_survived, male_total)
female_ci_lower, female_ci_upper = wilson_score_interval(female_survived, female_total)

print(f"\n남성 생존율 95% 신뢰구간: [{male_ci_lower:.4f}, {male_ci_upper:.4f}]")
print(f"여성 생존율 95% 신뢰구간: [{female_ci_lower:.4f}, {female_ci_upper:.4f}]")

# 생존율 차이의 95% 신뢰구간 계산
# 차이의 표준오차 계산 (비모집단의 평균 차이)
se_diff = np.sqrt((male_survival_rate * (1 - male_survival_rate) / male_total) + 
                  (female_survival_rate * (1 - female_survival_rate) / female_total))

# 차이의 95% 신뢰구간
z = stats.norm.ppf(0.975)
margin_error = z * se_diff
ci_lower = survival_difference - margin_error
ci_upper = survival_difference + margin_error

print(f"\n생존율 차이 (여성-남성) 95% 신뢰구간: [{ci_lower:.4f}, {ci_upper:.4f}]")
```

<details><summary>실행 결과</summary>

```
성별 생존율:
      Sex  Total  Survived  Survival_Rate
0  female    314       233       0.742038
1    male    577       109       0.188908

남성 생존율: 0.1889
여성 생존율: 0.7420
생존율 차이: 0.5531

남성 생존율 95% 신뢰구간: [0.1591, 0.2229]
여성 생존율 95% 신뢰구간: [0.6909, 0.7873]

생존율 차이 (여성-남성) 95% 신뢰구간: [0.4951, 0.6111]

```

</details>

### 답변

## 문제 3-1(a) 성별 생존율 및 신뢰구간 분석 결과

### 성별 생존율
- **여성 생존율**: 74.20%
- **남성 생존율**: 18.89%
- **생존율 차이**: 55.31% (여성 - 남성)

### 신뢰구간
- **남성 생존율 95% 신뢰구간**: [15.91%, 22.29%]
- **여성 생존율 95% 신뢰구간**: [69.09%, 78.73%]
- **생존율 차이 (여성-남성) 95% 신뢰구간**: [49.51%, 61.11%]

### 분석 요약
여성의 생존율은 남성보다 약 55.31% 더 높으며, 이 차이는 통계적으로 유의미합니다. 여성의 생존율은 74.20%이며, 이 값이 95% 신뢰구간 [69.09%, 78.73%]에 포함됩니다. 반면 남성의 생존율은 18.89%로 매우 낮으며, 95% 신뢰구간 [15.91%, 22.29%]에 포함됩니다. 이 차이는 49.51%에서 61.11% 사이의 신뢰구간으로, 매우 큰 차이를 보여줍니다.

이 결과는 타이타닉 호의 생존자 중 여성과 어린이 우선 구조 원칙이 적용되었음을 시사합니다. 단순한 독립 관측 가정 하에 계산된 결과로, 실제 데이터 수집 방식이나 샘플링 오차 등 다른 요인들이 영향을 미칠 수 있습니다.

_소요 시간: 24초_

## (b)

> (b) (a)의 비교를 객실 등급별로 반복하라. 등급별 성별 구성 차이가 전체 생존율 비교에 어떤 영향을 줄 수 있는지 설명하라.

**도구: run_python**

```python
import pandas as pd
import numpy as np
from scipy import stats

# Pclass(객실 등급)과 Sex(성별)에 따른 생존율 계산
survival_by_class_sex = df.groupby(['Pclass', 'Sex'])['Survived'].agg(['count', 'sum', 'mean']).reset_index()
survival_by_class_sex.columns = ['Pclass', 'Sex', 'Total', 'Survived', 'Survival_Rate']

print("객실 등급별 성별 생존율:")
print(survival_by_class_sex)

# 각 객실 등급별로 성별 구성 비율 계산
class_sex_distribution = df.groupby(['Pclass', 'Sex']).size().reset_index(name='Count')
class_total = df.groupby('Pclass').size().reset_index(name='Total_Class')
class_sex_distribution = class_sex_distribution.merge(class_total, on='Pclass')

# 성별 구성 비율 계산
class_sex_distribution['Percentage'] = (class_sex_distribution['Count'] / class_sex_distribution['Total_Class']) * 100

print("\n객실 등급별 성별 구성:")
print(class_sex_distribution[['Pclass', 'Sex', 'Count', 'Total_Class', 'Percentage']])

# 각 객실 등급별 전체 생존율 계산
overall_survival_by_class = df.groupby('Pclass')['Survived'].agg(['count', 'sum', 'mean']).reset_index()
overall_survival_by_class.columns = ['Pclass', 'Total', 'Survived', 'Survival_Rate']

print("\n객실 등급별 전체 생존율:")
print(overall_survival_by_class)

# 각 객실 등급별 성별별 생존율 차이 계산
print("\n객실 등급별 성별 생존율 차이:")
class_survival_diff = survival_by_class_sex.pivot(index='Pclass', columns='Sex', values='Survival_Rate')
class_survival_diff['Difference'] = class_survival_diff['female'] - class_survival_diff['male']
print(class_survival_diff[['male', 'female', 'Difference']])
```

<details><summary>실행 결과</summary>

```
객실 등급별 성별 생존율:
   Pclass     Sex  Total  Survived  Survival_Rate
0       1  female     94        91       0.968085
1       1    male    122        45       0.368852
2       2  female     76        70       0.921053
3       2    male    108        17       0.157407
4       3  female    144        72       0.500000
5       3    male    347        47       0.135447

객실 등급별 성별 구성:
   Pclass     Sex  Count  Total_Class  Percentage
0       1  female     94          216   43.518519
1       1    male    122          216   56.481481
2       2  female     76          184   41.304348
3       2    male    108          184   58.695652
4       3  female    144          491   29.327902
5       3    male    347          491   70.672098

객실 등급별 전체 생존율:
   Pclass  Total  Survived  Survival_Rate
0       1    216       136       0.629630
1       2    184        87       0.472826
2       3    491       119       0.242363

객실 등급별 성별 생존율 차이:
Sex         male    female  Difference
Pclass                                
1       0.368852  0.968085    0.599233
2       0.157407  0.921053    0.763645
3       0.135447  0.500000    0.364553

```

</details>

**도구: run_python**

```python
# 각 객실 등급별로 성별 생존율 차이의 신뢰구간 계산
print("객실 등급별 성별 생존율 차이 및 95% 신뢰구간:")

class_survival_ci = []

for pclass in [1, 2, 3]:
    # 각 등급별로 남성과 여성의 생존 정보 추출
    male_data = survival_by_class_sex[(survival_by_class_sex['Pclass'] == pclass) & (survival_by_class_sex['Sex'] == 'male')]
    female_data = survival_by_class_sex[(survival_by_class_sex['Pclass'] == pclass) & (survival_by_class_sex['Sex'] == 'female')]
    
    male_total = male_data['Total'].values[0]
    male_survived = male_data['Survived'].values[0]
    female_total = female_data['Total'].values[0]
    female_survived = female_data['Survived'].values[0]
    
    male_rate = male_data['Survival_Rate'].values[0]
    female_rate = female_data['Survival_Rate'].values[0]
    diff = female_rate - male_rate
    
    # Wilson score interval을 사용하여 각 성별의 95% 신뢰구간 계산
    def wilson_score_interval(successes, trials, confidence=0.95):
        z = stats.norm.ppf((1 + confidence) / 2)
        p_hat = successes / trials
        denominator = 1 + z**2 / trials
        center = (p_hat + z**2 / (2 * trials)) / denominator
        margin = z * np.sqrt((p_hat * (1 - p_hat) / trials + z**2 / (4 * trials**2))) / denominator
        return center - margin, center + margin
    
    male_ci_lower, male_ci_upper = wilson_score_interval(male_survived, male_total)
    female_ci_lower, female_ci_upper = wilson_score_interval(female_survived, female_total)
    
    # 차이의 95% 신뢰구간 계산
    se_diff = np.sqrt((male_rate * (1 - male_rate) / male_total) + 
                      (female_rate * (1 - female_rate) / female_total))
    z = stats.norm.ppf(0.975)
    margin_error = z * se_diff
    ci_lower = diff - margin_error
    ci_upper = diff + margin_error
    
    class_survival_ci.append({
        'Pclass': pclass,
        'Male_Rate': male_rate,
        'Female_Rate': female_rate,
        'Difference': diff,
        'CI_Lower': ci_lower,
        'CI_Upper': ci_upper
    })

ci_df = pd.DataFrame(class_survival_ci)
print(ci_df[['Pclass', 'Male_Rate', 'Female_Rate', 'Difference', 'CI_Lower', 'CI_Upper']])
```

<details><summary>실행 결과</summary>

```
객실 등급별 성별 생존율 차이 및 95% 신뢰구간:
   Pclass  Male_Rate  Female_Rate  Difference  CI_Lower  CI_Upper
0       1   0.368852     0.968085    0.599233  0.506535  0.691931
1       2   0.157407     0.921053    0.763645  0.672032  0.855258
2       3   0.135447     0.500000    0.364553  0.275303  0.453803

```

</details>

### 답변

## 문제 3-1(b) 객실 등급별 성별 생존율 분석 결과

### 객실 등급별 성별 생존율

**1등급 (First Class):**
- 여성 생존율: 96.81%
- 남성 생존율: 36.89%
- 생존율 차이: 59.92%

**2등급 (Second Class):**
- 여성 생존율: 92.11%
- 남성 생존율: 15.74%
- 생존율 차이: 76.36%

**3등급 (Third Class):**
- 여성 생존율: 50.00%
- 남성 생존율: 13.54%
- 생존율 차이: 36.46%

### 객실 등급별 성별 구성

**1등급:**
- 여성: 43.52% (94명)
- 남성: 56.48% (122명)

**2등급:**
- 여성: 41.30% (76명)
- 남성: 58.70% (108명)

**3등급:**
- 여성: 29.33% (144명)
- 남성: 70.67% (347명)

### 객실 등급별 전체 생존율

**1등급:** 62.96%
**2등급:** 47.28%
**3등급:** 24.24%

### 신뢰구간

**1등급:**
- 생존율 차이 95% 신뢰구간: [50.65%, 69.19%]

**2등급:**
- 생존율 차이 95% 신뢰구간: [67.20%, 85.53%]

**3등급:**
- 생존율 차이 95% 신뢰구간: [27.53%, 45.38%]

### 분석 요약

1. **성별 생존율의 차이는 객실 등급에 따라 다르게 나타납니다**:
   - 1등급과 2등급에서는 여성의 생존율이 남성보다 현저히 높습니다 (약 60% 이상 차이)
   - 3등급에서는 여성과 남성의 생존율 차이가 가장 작습니다 (약 36% 차이)

2. **객실 등급별 성별 구성의 영향**:
   - 1등급과 2등급에서는 여성 비율이 상대적으로 낮지만, 여성의 생존율이 매우 높아 전체 생존율이 높습니다
   - 3등급에서는 남성 비율이 매우 높기 때문에 전체 생존율이 낮습니다 (70.67%가 남성)
   - 이는 성별 구성이 전체 생존율에 큰 영향을 미친다는 것을 보여줍니다

3. **전체 생존율과 성별 차이의 관계**:
   - 1등급과 2등급에서는 성별 차이가 크지만, 전체 생존율도 높습니다
   - 3등급에서는 성별 차이가 적지만, 전체 생존율도 낮습니다
   - 이는 객실 등급이 생존에 중요한 요인임을 시사합니다

4. **성별 구성의 중요성**:
   - 3등급에서 남성이 70.67%로 매우 많기 때문에, 전체 생존율이 낮은 것으로 나타납니다
   - 이는 단순히 성별에 따른 생존율 차이만을 고려하는 것이 아니라, 그 차이가 어떤 집단 구성에서 발생했는지를 고려해야 함을 의미합니다

따라서 전체 생존율 비교에서 성별 차이를 단순히 비교할 경우, 객실 등급의 영향을 고려하지 않으면 잘못된 결론을 도출할 수 있습니다.

_소요 시간: 37초_

## (c)

> (c) 공통의 객실 등급 분포를 기준으로 성별 생존율을 표준화하고, (a)의 결과와 비교하라. 이것만으로 성별의 인과효과를 추정했다고 할 수 있는지 판단하라.

**도구: run_python**

```python
import pandas as pd
import numpy as np
from scipy import stats

# 전체 데이터에서 각 객실 등급의 비율 계산
class_distribution = df['Pclass'].value_counts(normalize=True)
print("전체 데이터에서 객실 등급 비율:")
print(class_distribution)

# 각 객실 등급별로 성별 생존율을 계산
survival_by_class_sex = df.groupby(['Pclass', 'Sex'])['Survived'].agg(['count', 'sum', 'mean']).reset_index()
survival_by_class_sex.columns = ['Pclass', 'Sex', 'Total', 'Survived', 'Survival_Rate']

# 표준화된 생존율 계산 (공통 객실 등급 분포 기준)
# 각 성별의 표준화된 생존율을 계산하기 위해, 전체 데이터에서 각 객실 등급의 비율을 사용
class_weights = df['Pclass'].value_counts(normalize=True)

# 각 성별에 대해 표준화된 생존율 계산
standardized_survival_rates = {}

for sex in ['male', 'female']:
    weighted_survival = 0
    total_weighted = 0
    
    for pclass in [1, 2, 3]:
        class_data = survival_by_class_sex[(survival_by_class_sex['Pclass'] == pclass) & (survival_by_class_sex['Sex'] == sex)]
        if len(class_data) > 0:
            rate = class_data['Survival_Rate'].values[0]
            weight = class_weights[pclass]
            weighted_survival += rate * weight
            total_weighted += weight
    
    standardized_survival_rates[sex] = weighted_survival

print("\n표준화된 생존율 (공통 객실 등급 분포 기준):")
print(f"남성 표준화 생존율: {standardized_survival_rates['male']:.4f}")
print(f"여성 표준화 생존율: {standardized_survival_rates['female']:.4f}")

# 표준화된 차이 계산
standardized_difference = standardized_survival_rates['female'] - standardized_survival_rates['male']
print(f"\n표준화된 생존율 차이: {standardized_difference:.4f}")

# 원래의 전체 데이터에서의 성별 생존율과 비교
original_survival_by_sex = df.groupby('Sex')['Survived'].mean()
print("\n원래 전체 데이터에서의 성별 생존율:")
print(f"남성 생존율: {original_survival_by_sex['male']:.4f}")
print(f"여성 생존율: {original_survival_by_sex['female']:.4f}")

# 원래 차이 계산
original_difference = original_survival_by_sex['female'] - original_survival_by_sex['male']
print(f"원래 생존율 차이: {original_difference:.4f}")

# 표준화된 차이의 신뢰구간 계산
# 각 객실 등급별로 표준화된 차이의 신뢰구간 계산
def calculate_standardized_ci():
    # 각 등급별로 남성과 여성의 생존율을 구함
    class_survival = []
    
    for pclass in [1, 2, 3]:
        male_data = survival_by_class_sex[(survival_by_class_sex['Pclass'] == pclass) & (survival_by_class_sex['Sex'] == 'male')]
        female_data = survival_by_class_sex[(survival_by_class_sex['Pclass'] == pclass) & (survival_by_class_sex['Sex'] == 'female')]
        
        if len(male_data) > 0 and len(female_data) > 0:
            male_rate = male_data['Survival_Rate'].values[0]
            female_rate = female_data['Survival_Rate'].values[0]
            diff = female_rate - male_rate
            
            # 각 등급의 표본 수
            male_total = male_data['Total'].values[0]
            female_total = female_data['Total'].values[0]
            
            class_survival.append({
                'Pclass': pclass,
                'Male_Rate': male_rate,
                'Female_Rate': female_rate,
                'Difference': diff,
                'Male_Total': male_total,
                'Female_Total': female_total
            })
    
    # 각 등급별로 차이의 신뢰구간 계산
    class_diff_ci = []
    for item in class_survival:
        # 남성 생존율의 95% 신뢰구간
        def wilson_score_interval(successes, trials, confidence=0.95):
            z = stats.norm.ppf((1 + confidence) / 2)
            p_hat = successes / trials
            denominator = 1 + z**2 / trials
            center = (p_hat + z**2 / (2 * trials)) / denominator
            margin = z * np.sqrt((p_hat * (1 - p_hat) / trials + z**2 / (4 * trials**2))) / denominator
            return center - margin, center + margin
        
        male_ci_lower, male_ci_upper = wilson_score_interval(
            item['Male_Total'] * item['Male_Rate'], 
            item['Male_Total']
        )
        
        female_ci_lower, female_ci_upper = wilson_score_interval(
            item['Female_Total'] * item['Female_Rate'], 
            item['Female_Total']
        )
        
        # 차이의 95% 신뢰구간
        diff_ci_lower = female_ci_lower - male_ci_upper
        diff_ci_upper = female_ci_upper - male_ci_lower
        
        class_diff_ci.append({
            'Pclass': item['Pclass'],
            'Difference': item['Difference'],
            'CI_Lower': diff_ci_lower,
            'CI_Upper': diff_ci_upper
        })
    
    # 가중 평균을 통한 표준화된 차이의 신뢰구간 계산
    weighted_diff = 0
    total_weight = 0
    
    for item in class_diff_ci:
        weight = class_weights[item['Pclass']]
        weighted_diff += item['Difference'] * weight
        total_weight += weight
    
    # 표준화된 차이의 신뢰구간 계산 (복잡한 방법으로 계산)
    # 간단하게 각 등급별 차이의 가중 평균과 표준편차를 사용하여 근사
    differences = [item['Difference'] for item in class_diff_ci]
    weights = [class_weights[item['Pclass']] for item in class_diff_ci]
    
    # 가중 평균 계산
    weighted_mean = np.average(differences, weights=weights)
    
    # 표준편차 계산 (간단한 근사)
    std_error = np.sqrt(np.sum([weights[i] * (differences[i] - weighted_mean)**2 for i in range(len(differences))]))
    
    z = stats.norm.ppf(0.975)
    margin_error = z * std_error
    
    return weighted_mean, weighted_mean - margin_error, weighted_mean + margin_error

standardized_mean, ci_lower, ci_upper = calculate_standardized_ci()
print(f"\n표준화된 생존율 차이 95% 신뢰구간: [{ci_lower:.4f}, {ci_upper:.4f}]")

# 인과관계 추정 가능성 판단
print("\n인과효과 추정 가능성 판단:")
print("1. 표준화된 생존율 차이는 객실 등급 구성의 영향을 제거한 결과입니다.")
print("2. 원래 데이터에서는 3등급에서 남성이 많아 전체 생존율이 낮았지만,")
print("   표준화된 결과에서는 이 영향이 제거되었습니다.")
print("3. 그러나 인과관계를 추정하기 위해서는 다음과 같은 요소들이 필요합니다:")
print("   - 시간적 순서 (성별이 생존에 영향을 미쳤는가)")
print("   - 공통 원인 변수의 제거 (객실 등급은 생존에 직접적인 영향을 미치므로)")
print("   - 실험적 설계 또는 강력한 관측 연구")
print("4. 이 데이터는 관측 연구이므로, 인과관계를 단정하기 어렵습니다.")
print("5. 표준화된 결과는 성별 차이가 객실 등급에 따라 다르다는 것을 보여주지만,")
print("   실제 인과관계를 추정하기 위해서는 추가적인 정보나 실험적 접근이 필요합니다.")
```

<details><summary>실행 결과</summary>

```
전체 데이터에서 객실 등급 비율:
Pclass
3    0.551066
1    0.242424
2    0.206510
Name: proportion, dtype: float64

표준화된 생존율 (공통 객실 등급 분포 기준):
남성 표준화 생존율: 0.1966
여성 표준화 생존율: 0.7004

표준화된 생존율 차이: 0.5039

원래 전체 데이터에서의 성별 생존율:
남성 생존율: 0.1889
여성 생존율: 0.7420
원래 생존율 차이: 0.5531

표준화된 생존율 차이 95% 신뢰구간: [0.1828, 0.8249]

인과효과 추정 가능성 판단:
1. 표준화된 생존율 차이는 객실 등급 구성의 영향을 제거한 결과입니다.
2. 원래 데이터에서는 3등급에서 남성이 많아 전체 생존율이 낮았지만,
   표준화된 결과에서는 이 영향이 제거되었습니다.
3. 그러나 인과관계를 추정하기 위해서는 다음과 같은 요소들이 필요합니다:
   - 시간적 순서 (성별이 생존에 영향을 미쳤는가)
   - 공통 원인 변수의 제거 (객실 등급은 생존에 직접적인 영향을 미치므로)
   - 실험적 설계 또는 강력한 관측 연구
4. 이 데이터는 관측 연구이므로, 인과관계를 단정하기 어렵습니다.
5. 표준화된 결과는 성별 차이가 객실 등급에 따라 다르다는 것을 보여주지만,
   실제 인과관계를 추정하기 위해서는 추가적인 정보나 실험적 접근이 필요합니다.

```

</details>

### 답변

## 문제 3-1(c) 표준화된 성별 생존율 분석 결과

### 객실 등급 비율
- 3등급: 55.11%
- 1등급: 24.24%
- 2등급: 20.65%

### 표준화된 생존율 계산 결과

**표준화된 생존율 (공통 객실 등급 분포 기준):**
- 남성 표준화 생존율: 19.66%
- 여성 표준화 생존율: 70.04%
- 표준화된 생존율 차이: 50.39%

**원래 전체 데이터에서의 성별 생존율:**
- 남성 생존율: 18.89%
- 여성 생존율: 74.20%
- 원래 생존율 차이: 55.31%

### 신뢰구간
- 표준화된 생존율 차이 95% 신뢰구간: [18.28%, 82.49%]

### 분석 요약

1. **표준화된 결과의 변화**:
   - 원래 데이터에서는 여성 생존율이 74.20%, 남성은 18.89%로 차이가 55.31%였습니다.
   - 표준화된 결과에서는 여성 생존율이 70.04%, 남성은 19.66%로 차이가 50.39%로 줄었습니다.
   - 이는 3등급에서 남성이 많아 전체 생존율이 낮게 나타났던 영향이 제거되었음을 보여줍니다.

2. **객실 등급의 영향**:
   - 3등급에서는 여성과 남성 모두 생존율이 낮았지만, 남성 비율이 매우 높아 전체 생존율이 낮게 나타났습니다.
   - 표준화를 통해 이 구성 차이의 영향을 제거하면 성별 간의 실제 차이가 더 명확해집니다.

3. **인과관계 추정 가능성**:
   - 표준화된 결과는 성별에 따른 생존율 차이가 객실 등급 구성의 영향을 받았다는 것을 보여줍니다.
   - 그러나 이 데이터는 관측 연구이므로, 다음과 같은 한계가 있습니다:
     - 시간적 순서를 확인할 수 없습니다 (성별이 생존에 영향을 미쳤는지)
     - 객실 등급은 생존에 직접적인 영향을 미치므로 공통 원인 변수로 작용합니다
     - 인과관계를 단정하기 위해서는 실험적 설계나 더 강력한 관측 연구가 필요합니다

4. **결론**:
   - 표준화된 결과는 성별 차이가 객실 등급에 따라 다르다는 것을 보여줍니다.
   - 그러나 이 데이터만으로는 성별이 생존에 직접적인 인과효과를 미쳤다고 단정할 수 없습니다.
   - 실제 인과관계를 추정하기 위해서는 추가적인 정보나 실험적 접근이 필요합니다.

_소요 시간: 49초_
