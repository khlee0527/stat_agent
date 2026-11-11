# 문제 2-1. 나이를 모르는 승객을 제외해도 괜찮을까?

- 모델: `qwen3-coder:30b`
- 데이터: train.csv
- 소요 시간: 총 94초 ((a) 62초, (b) 12초, (c) 20초)

## (a)

> (a) Age, Cabin, Embarked의 결측 현황을 정리하라. 특히 나이가 관측된 승객과 관측되지 않은 승객의 성별·객실 등급 구성을 비교하라.

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

# 결측치 현황 정리
missing_info = df[['Age', 'Cabin', 'Embarked']].isnull().sum()
print("결측치 현황:")
print(missing_info)

# 나이가 관측된 승객과 관측되지 않은 승객의 성별·객실 등급 구성을 비교
df['Age_known'] = df['Age'].notnull()

age_known = df[df['Age_known'] == True]
age_unknown = df[df['Age_known'] == False]

print("\n나이가 관측된 승객의 성별 구성:")
print(age_known['Sex'].value_counts())

print("\n나이가 관측되지 않은 승객의 성별 구성:")
print(age_unknown['Sex'].value_counts())

print("\n나이가 관측된 승객의 객실 등급 구성:")
print(age_known['Pclass'].value_counts())

print("\n나이가 관측되지 않은 승객의 객실 등급 구성:")
print(age_unknown['Pclass'].value_counts())
```

<details><summary>실행 결과</summary>

```
결측치 현황:
Age         177
Cabin       687
Embarked      2
dtype: int64

나이가 관측된 승객의 성별 구성:
Sex
male      453
female    261
Name: count, dtype: int64

나이가 관측되지 않은 승객의 성별 구성:
Sex
male      124
female     53
Name: count, dtype: int64

나이가 관측된 승객의 객실 등급 구성:
Pclass
3    355
1    186
2    173
Name: count, dtype: int64

나이가 관측되지 않은 승객의 객실 등급 구성:
Pclass
3    136
1     30
2     11
Name: count, dtype: int64

```

</details>

### 답변

(a) Age, Cabin, Embarked의 결측 현황은 다음과 같습니다:

- **Age**: 177명(약 19.9%)의 승객 나이가 누락되어 있습니다.
- **Cabin**: 687명(약 77.1%)의 객실 정보가 누락되어 있습니다.
- **Embarked**: 2명(약 0.2%)의 승선 항구 정보가 누락되어 있습니다.

나이가 관측된 승객과 관측되지 않은 승객의 성별·객실 등급 구성은 다음과 같습니다:

### 성별 구성:
- **나이가 관측된 승객**:
  - 남성: 453명
  - 여성: 261명

- **나이가 관측되지 않은 승객**:
  - 남성: 124명
  - 여성: 53명

### 객실 등급 구성:
- **나이가 관측된 승객**:
  - 3등급: 355명
  - 1등급: 186명
  - 2등급: 173명

- **나이가 관측되지 않은 승객**:
  - 3등급: 136명
  - 1등급: 30명
  - 2등급: 11명

결측치 현황과 성별·객실 등급 구성에서 볼 수 있듯이, 나이가 누락된 승객은 남성 비율이 높고, 3등급 객실에 많이 탑승한 것으로 보입니다. 이는 나이 정보를 제외하고 분석을 진행할 경우, 특정 집단(예: 나이가 알려지지 않은 남성 승객)의 특성이 왜곡될 수 있음을 시사합니다. 따라서 나이가 누락된 승객을 제외하는 것은 데이터 분석의 정확성에 영향을 줄 수 있으므로, 반드시 고려해야 합니다.

_소요 시간: 62초_

## (b)

> (b) (a)를 바탕으로 나이가 결측인 승객을 모두 제외했을 때, 전체 자료의 성별·객실 등급 구성과 생존율이 얼마나 달라지는지 확인하라.

**도구: run_python**

```python
# 나이가 결측인 승객을 제외한 데이터셋 생성
df_cleaned = df.dropna(subset=['Age'])

# 전체 데이터의 성별·객실 등급 구성과 생존율
overall_sex_dist = df['Sex'].value_counts()
overall_pclass_dist = df['Pclass'].value_counts()
overall_survival_rate = df['Survived'].mean()

# 나이가 결측인 승객을 제외한 데이터의 성별·객실 등급 구성과 생존율
cleaned_sex_dist = df_cleaned['Sex'].value_counts()
cleaned_pclass_dist = df_cleaned['Pclass'].value_counts()
cleaned_survival_rate = df_cleaned['Survived'].mean()

print("전체 데이터의 성별 구성:")
print(overall_sex_dist)
print("\n나이가 결측인 승객을 제외한 데이터의 성별 구성:")
print(cleaned_sex_dist)

print("\n전체 데이터의 객실 등급 구성:")
print(overall_pclass_dist)
print("\n나이가 결측인 승객을 제외한 데이터의 객실 등급 구성:")
print(cleaned_pclass_dist)

print(f"\n전체 데이터의 생존율: {overall_survival_rate:.4f}")
print(f"나이가 결측인 승객을 제외한 데이터의 생존율: {cleaned_survival_rate:.4f}")
```

<details><summary>실행 결과</summary>

```
전체 데이터의 성별 구성:
Sex
male      577
female    314
Name: count, dtype: int64

나이가 결측인 승객을 제외한 데이터의 성별 구성:
Sex
male      453
female    261
Name: count, dtype: int64

전체 데이터의 객실 등급 구성:
Pclass
3    491
1    216
2    184
Name: count, dtype: int64

나이가 결측인 승객을 제외한 데이터의 객실 등급 구성:
Pclass
3    355
1    186
2    173
Name: count, dtype: int64

전체 데이터의 생존율: 0.3838
나이가 결측인 승객을 제외한 데이터의 생존율: 0.4062

```

</details>

### 답변

(b) 나이가 결측인 승객을 모두 제외했을 때, 전체 자료의 성별·객실 등급 구성과 생존율은 다음과 같습니다:

### 성별 구성:
- **전체 데이터**: 남성 577명, 여성 314명
- **나이가 결측인 승객 제외 후**: 남성 453명, 여성 261명

### 객실 등급 구성:
- **전체 데이터**: 3등급 491명, 1등급 216명, 2등급 184명
- **나이가 결측인 승객 제외 후**: 3등급 355명, 1등급 186명, 2등급 173명

### 생존율:
- **전체 데이터**: 38.38%
- **나이가 결측인 승객 제외 후**: 40.62%

결과에서 볼 수 있듯이, 나이가 결측인 승객을 제외하면 남성 비율이 줄어들고, 3등급 객실에 탑승한 승객의 비율도 감소합니다. 또한 생존율은 약 2.24% 상승하여 40.62%로 증가했습니다. 이는 나이 정보가 결측인 승객들이 전체 데이터에서 생존률이 낮았음을 시사합니다. 따라서 나이 결측치를 제외하는 것은 분석 결과에 영향을 줄 수 있으며, 특히 생존율과 같은 중요한 지표에서 차이가 발생할 수 있습니다.

_소요 시간: 12초_

## (c)

> (c) (b)의 결과를 전체 중앙값 대체와 성별·객실 등급별 중앙값 대체 결과와 비교하라. “결측치를 처리했다”는 사실만으로 원래 자료의 특성이 복원되었다고 할 수 있는지 설명하라.

**도구: run_python**

```python
# 전체 중앙값 대체
df_median_all = df.copy()
median_age_all = df['Age'].median()
df_median_all['Age'] = df_median_all['Age'].fillna(median_age_all)

# 성별·객실 등급별 중앙값 대체
df_median_grouped = df.copy()
for sex in df['Sex'].unique():
    for pclass in df['Pclass'].unique():
        mask = (df_median_grouped['Sex'] == sex) & (df_median_grouped['Pclass'] == pclass)
        median_age = df.loc[mask, 'Age'].median()
        df_median_grouped.loc[mask & df_median_grouped['Age'].isnull(), 'Age'] = median_age

# 전체 데이터의 성별·객실 등급 구성과 생존율
overall_sex_dist = df['Sex'].value_counts()
overall_pclass_dist = df['Pclass'].value_counts()
overall_survival_rate = df['Survived'].mean()

# 전체 중앙값 대체 후의 성별·객실 등급 구성과 생존율
median_all_sex_dist = df_median_all['Sex'].value_counts()
median_all_pclass_dist = df_median_all['Pclass'].value_counts()
median_all_survival_rate = df_median_all['Survived'].mean()

# 성별·객실 등급별 중앙값 대체 후의 성별·객실 등급 구성과 생존율
median_grouped_sex_dist = df_median_grouped['Sex'].value_counts()
median_grouped_pclass_dist = df_median_grouped['Pclass'].value_counts()
median_grouped_survival_rate = df_median_grouped['Survived'].mean()

print("전체 데이터의 성별 구성:")
print(overall_sex_dist)
print("\n전체 중앙값 대체 후의 성별 구성:")
print(median_all_sex_dist)
print("\n성별·객실 등급별 중앙값 대체 후의 성별 구성:")
print(median_grouped_sex_dist)

print("\n전체 데이터의 객실 등급 구성:")
print(overall_pclass_dist)
print("\n전체 중앙값 대체 후의 객실 등급 구성:")
print(median_all_pclass_dist)
print("\n성별·객실 등급별 중앙값 대체 후의 객실 등급 구성:")
print(median_grouped_pclass_dist)

print(f"\n전체 데이터의 생존율: {overall_survival_rate:.4f}")
print(f"전체 중앙값 대체 후의 생존율: {median_all_survival_rate:.4f}")
print(f"성별·객실 등급별 중앙값 대체 후의 생존율: {median_grouped_survival_rate:.4f}")
```

<details><summary>실행 결과</summary>

```
전체 데이터의 성별 구성:
Sex
male      577
female    314
Name: count, dtype: int64

전체 중앙값 대체 후의 성별 구성:
Sex
male      577
female    314
Name: count, dtype: int64

성별·객실 등급별 중앙값 대체 후의 성별 구성:
Sex
male      577
female    314
Name: count, dtype: int64

전체 데이터의 객실 등급 구성:
Pclass
3    491
1    216
2    184
Name: count, dtype: int64

전체 중앙값 대체 후의 객실 등급 구성:
Pclass
3    491
1    216
2    184
Name: count, dtype: int64

성별·객실 등급별 중앙값 대체 후의 객실 등급 구성:
Pclass
3    491
1    216
2    184
Name: count, dtype: int64

전체 데이터의 생존율: 0.3838
전체 중앙값 대체 후의 생존율: 0.3838
성별·객실 등급별 중앙값 대체 후의 생존율: 0.3838

```

</details>

### 답변

(c) 결측치를 처리한 결과를 비교해 보면 다음과 같습니다:

### 성별 구성:
- **전체 데이터**: 남성 577명, 여성 314명
- **전체 중앙값 대체 후**: 남성 577명, 여성 314명
- **성별·객실 등급별 중앙값 대체 후**: 남성 577명, 여성 314명

### 객실 등급 구성:
- **전체 데이터**: 3등급 491명, 1등급 216명, 2등급 184명
- **전체 중앙값 대체 후**: 3등급 491명, 1등급 216명, 2등급 184명
- **성별·객실 등급별 중앙값 대체 후**: 3등급 491명, 1등급 216명, 2등급 184명

### 생존율:
- **전체 데이터**: 38.38%
- **전체 중앙값 대체 후**: 38.38%
- **성별·객실 등급별 중앙값 대체 후**: 38.38%

결측치를 처리한 후에도 성별 구성, 객실 등급 구성, 그리고 생존율은 모두 동일하게 유지됩니다. 이는 결측치가 데이터의 구조에 영향을 주지 않거나, 결측치를 대체하는 방식이 데이터의 분포를 그대로 유지했기 때문입니다.

하지만, "결측치를 처리했다"는 사실만으로 원래 자료의 특성이 복원되었다고 할 수는 없습니다. 결측치가 존재하는 이유(예: 나이 정보가 누락된 승객이 특정 조건을 가진 경우)는 분석에 영향을 줄 수 있으며, 단순히 평균이나 중앙값으로 대체하는 방식은 실제 데이터의 분포를 완전히 재현하지 못할 수 있습니다. 따라서 결측치 처리는 단순히 데이터를 유지하는 것을 넘어서, 그 처리 방법이 데이터의 진실성을 얼마나 잘 반영하는지를 고려해야 합니다.

_소요 시간: 20초_
