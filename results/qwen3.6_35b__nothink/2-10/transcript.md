# 문제 2-10. 이메일 실험의 분모는 누구여야 할까?

- 모델: `qwen3.6:35b`
- 데이터: hillstorm_no_indices.csv
- 추론 모드: 끔
- skill: `off`
- 소요 시간: 총 153초 ((a) 37초, (b) 45초, (c) 71초)

## (a)

> (a) 이메일 미발송·남성 상품 이메일·여성 상품 이메일 집단의 고객 수를 구하라. 실험 이전의 구매 이력과 실험 이후의 방문·구매·지출 변수를 구분하라.

**도구: list_files** `{}`

<details><summary>실행 결과</summary>

```
{
  "files": [
    {
      "name": "hillstorm_no_indices.csv",
      "size_kb": 3934.5
    }
  ]
}
```

</details>

**도구: read_file** `{"filename": "hillstorm_no_indices.csv"}`

<details><summary>실행 결과</summary>

```
{
  "message": "'hillstorm_no_indices.csv'을 변수 `df`에 저장했습니다. (인코딩 utf-8, 구분자 ',')",
  "profile": {
    "var_name": "df",
    "shape": [
      64000,
      12
    ],
    "columns": [
      {
        "position": 0,
        "name": "recency",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 10
      },
      {
        "position": 1,
        "name": "history_segment",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "2) $100 - $200"
      },
      {
        "position": 2,
        "name": "history",
        "dtype": "float64",
        "n_missing": 0,
        "first_non_missing": 142.44
      },
      {
        "position": 3,
        "name": "mens",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 1
      },
      {
        "position": 4,
        "name": "womens",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 5,
        "name": "zip_code",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Surburban"
      },
      {
        "position": 6,
        "name": "newbie",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 7,
        "name": "channel",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Phone"
      },
      {
        "position": 8,
        "name": "segment",
        "dtype": "object",
        "n_missing": 0,
        "first_non_missing": "Womens E-Mail"
      },
      {
        "position": 9,
        "name": "visit",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 10,
        "name": "conversion",
        "dtype": "int64",
        "n_missing": 0,
        "first_non_missing": 0
      },
      {
        "position": 11,
        "name": "spend",
        "dtype": "float64",
        "n_missing": 0,
        "first_non_missing": 0.0
      }
    ],
    "preview": {
      "column_positions": [
        0,
        1,
        2,
        3,
        4,
        5,
        6,
        7
      ],
      "rows": [
        [
          10,
          "2) $100 - $200",
          142.44,
          1,
          0,
          "Surburban",
          0,
          "Phone"
        ],
        [
          6,
          "3) $200 - $350",
          329.08,
          1,
          1,
          "Rural",
          1,
          "Web"
        ],
        [
          7,
          "2) $100 - $200",
          180.65,
          0,
          1,
          "Surburban",
          1,
          "Web"
        ]
      ]
    },
    "missing_note": "null은 로딩된 DataFrame의 결측값입니다. 결측은 isna()/notna()로 확인하세요. 문자열은 실제 관측값입니다.",
    "keep_default_na_requested": true,
    "document_candidates": []
  }
}
```

</details>

**도구: run_python**

```python

# 컬럼명 확인 및 데이터 구조 파악
print(df.columns.tolist())
print(df.head())
print(df['segment'].value_counts())

```

<details><summary>실행 결과</summary>

```
['recency', 'history_segment', 'history', 'mens', 'womens', 'zip_code', 'newbie', 'channel', 'segment', 'visit', 'conversion', 'spend']
   recency history_segment  history  mens  womens   zip_code  newbie channel        segment  visit  conversion  spend
0       10  2) $100 - $200   142.44     1       0  Surburban       0   Phone  Womens E-Mail      0           0    0.0
1        6  3) $200 - $350   329.08     1       1      Rural       1     Web      No E-Mail      0           0    0.0
2        7  2) $100 - $200   180.65     0       1  Surburban       1     Web  Womens E-Mail      0           0    0.0
3        9  5) $500 - $750   675.83     1       0      Rural       1     Web    Mens E-Mail      0           0    0.0
4        2    1) $0 - $100    45.34     1       0      Urban       0     Web  Womens E-Mail      0           0    0.0
segment
Womens E-Mail    21387
Mens E-Mail      21307
No E-Mail        21306
Name: count, dtype: int64

```

</details>

**도구: run_python**

```python

# 문제 해석:
# 'segment' 컬럼이 실험 그룹(이메일 발송 여부 및 상품 유형)을 나타냄.
# - No E-Mail: 이메일 미발송 집단 (대조군)
# - Mens E-Mail: 남성 상품 이메일 발송 집단
# - Womens E-Mail: 여성 상품 이메일 발송 집단

# 각 집단의 고객 수 확인
group_counts = df['segment'].value_counts()
print("각 집단의 고객 수:")
print(group_counts)

# 실험 이전의 구매 이력 변수와 실험 이후의 방문·구매·지출 변수 구분
# 컬럼명 분석:
# - history: 실험 이전의 구매 이력 (cumulative spend before experiment?)
# - visit: 실험 이후 방문 수?
# - conversion: 실험 이후 전환(구매) 여부 또는 수?
# - spend: 실험 이후 지출액?

# 각 그룹별 평균 history, visit, conversion, spend 확인
print("\n각 그룹별 기초 통계:")
print(df.groupby('segment')[['history', 'visit', 'conversion', 'spend']].mean())

```

<details><summary>실행 결과</summary>

```
각 집단의 고객 수:
segment
Womens E-Mail    21387
Mens E-Mail      21307
No E-Mail        21306
Name: count, dtype: int64

각 그룹별 기초 통계:
                  history     visit  conversion     spend
segment                                                  
Mens E-Mail    242.835931  0.182757    0.012531  1.422617
No E-Mail      240.882653  0.106167    0.005726  0.652789
Womens E-Mail  242.536633  0.151400    0.008837  1.077202

```

</details>

### 답변

데이터 파일 `hillstorm_no_indices.csv`를 분석한 결과, 이메일 실험의 분모(각 집단의 고객 수)는 다음과 같습니다.

### (a) 각 집단의 고객 수

데이터의 `segment` 컬럼은 실험 그룹을 나타내며, 총 64,000명의 고객이 다음 세 집단으로 나뉩니다.

1.  **이메일 미발송 집단 (No E-Mail)**: **21,306명**
    *   통제군(Control Group)으로, 이메일 마케팅을 받지 않은 고객들입니다.
2.  **남성 상품 이메일 발송 집단 (Mens E-Mail)**: **21,307명**
    *   남성 의류/용품 관련 이메일을 발송받은 고객들입니다.
3.  **여성 상품 이메일 발송 집단 (Womens E-Mail)**: **21,387명**
    *   여성 의류/용품 관련 이메일을 발송받은 고객들입니다.

### 변수 구분 설명

질문에서 요구한 대로 실험 이전의 구매 이력과 실험 이후의 결과를 나타내는 변수는 다음과 같이 구분됩니다.

*   **실험 이전 구매 이력**: `history` 컬럼
    *   각 집단의 평균 구매 이력은 약 240~243달러 수준으로, 집단 간 큰 차이가 없어 무작위 할당이 잘 되었음을 시사합니다.
*   **실험 이후 결과 변수**:
    *   `visit`: 실험 기간 중 웹사이트 방문 횟수 (또는 방문 여부)
    *   `conversion`: 실험 기간 중 구매 전환 여부 (또는 횟수)
    *   `spend`: 실험 기간 중 총 지출액

**결론적으로, 이메일 실험의 분모는 총 64,000명이며, 이는 'No E-Mail'(21,306명), 'Mens E-Mail'(21,307명), 'Womens E-Mail'(21,387명) 세 집단으로 균등하게 할당되었습니다.**

_소요 시간: 37초_

## (b)

> (b) (a)의 각 집단에서 전체 배정 고객을 분모로 방문율·구매율·평균 지출액을 계산하라. 방문자만을 분모로 계산한 구매율도 별도로 제시하라.

**도구: run_python**

```python
import pandas as pd
import numpy as np

# df가 이미 로드되어 있다고 가정합니다. 만약 없다면 에러가 나겠지만, 전제에 따라 존재한다고 봅니다.

# 1. 집단별 전체 통계 (분모: 배정 고객 수)
# visit, conversion, spend는 수치형 컬럼이라고 가정하고 계산합니다.
# 결측치가 있을 수 있으므로 dropna()를 적절히 사용하거나 sum()/count()를 활용합니다.

# 각 그룹별 지표 계산
group_stats = df.groupby('segment').agg(
    count=('visit', 'count'), # 배정된 고객 수 확인 (또는 group_counts 사용)
    total_visits=('visit', 'sum'),
    total_conversions=('conversion', 'sum'),
    total_spend=('spend', 'sum')
).reset_index()

# 방문율 = 총 방문 횟수 / 배정 고객 수
group_stats['visit_rate'] = group_stats['total_visits'] / group_stats['count']

# 구매율 (전체 배정 고객을 분모로) = 총 구매 횟수 / 배정 고객 수
group_stats['conversion_rate_over_all'] = group_stats['total_conversions'] / group_stats['count']

# 평균 지출액 (전체 배정 고객을 분모로) = 총 지출액 / 배정 고객 수
group_stats['avg_spend_over_all'] = group_stats['total_spend'] / group_stats['count']

# 2. 방문자만을 분모로 한 구매율 계산
# 이 경우 분모는 'visit'이 1 이상인 사람들만 해당됩니다.
# 따라서 각 그룹 내에서 visit > 0인 서브셋을 만들어야 합니다.

visitor_conversion_rate = df[df['visit'] > 0].groupby('segment').agg(
    visitor_count=('visit', 'count'),
    visitor_conversions=('conversion', 'sum')
).reset_index()

visitor_conversion_rate['conversion_rate_over_visitors'] = visitor_conversion_rate['visitor_conversions'] / visitor_conversion_rate['visitor_count']

# 결과를 병합하여 보기 좋게 정리
result_df = group_stats[['segment', 'count', 'visit_rate', 'conversion_rate_over_all', 'avg_spend_over_all']].merge(
    visitor_conversion_rate[['segment', 'conversion_rate_over_visitors']], 
    on='segment'
)

# 소수점 4자리까지 표시
pd.set_option('display.float_format', '{:.4f}'.format)
print(result_df.to_string(index=False))

```

<details><summary>실행 결과</summary>

```
      segment  count  visit_rate  conversion_rate_over_all  avg_spend_over_all  conversion_rate_over_visitors
  Mens E-Mail  21307      0.1828                    0.0125              1.4226                         0.0686
    No E-Mail  21306      0.1062                    0.0057              0.6528                         0.0539
Womens E-Mail  21387      0.1514                    0.0088              1.0772                         0.0584

```

</details>

### 답변

(b) 각 집단별 방문율, 구매율(전체 배정 고객 기준), 평균 지출액(전체 배정 고객 기준), 그리고 방문자 기준 구매율은 다음과 같습니다.

| 집단 | 배정 고객 수 (분모) | 방문율<br>(방문/배정) | 구매율<br>(구매/배정) | 평균 지출액<br>(지출/배정) | 방문자 기준 구매율<br>(구매/방문) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Mens E-Mail** | 21,307 | 18.28% | 1.25% | $1.42 | 6.86% |
| **No E-Mail** | 21,306 | 10.62% | 0.57% | $0.65 | 5.39% |
| **Womens E-Mail** | 21,387 | 15.14% | 0.88% | $1.08 | 5.84% |

**해석:**
1.  **방문율**: 이메일 발송 집단(Mens: 18.28%, Womens: 15.14%)이 미발송 집단(No E-Mail: 10.62%)보다 약 1.5~1.7배 높은 방문율을 보였습니다.
2.  **구매율 (전체 기준)**: 이메일 발송 집단의 구매율이 더 높게 나타났습니다(Mens: 1.25%, Womens: 0.88% vs No E-Mail: 0.57%). 이는 이메일이 구매 유도에 긍정적 영향을 미쳤음을 시사합니다.
3.  **평균 지출액**: 전체 배정 고객을 분모로 했을 때, Mens E-Mail 집단의 평균 지출액($1.42)이 가장 높았고, 이어 Womens E-Mail($1.08), No E-Mail($0.65) 순이었습니다.
4.  **방문자 기준 구매율**: 방문한 고객 중 실제로 구매한 비율(전환율)도 이메일 발송 집단에서 더 높았습니다(Mens: 6.86%, Womens: 5.84% vs No E-Mail: 5.39%). 이는 단순히 웹사이트를 찾게 만드는 것을 넘어, 방문한 고객을 실제 구매로 전환시키는 효과도 있음을 보여줍니다.

_소요 시간: 45초_

## (c)

> (c) 두 구매율이 서로 다른 질문에 답한다는 점을 설명하라. 이메일의 효과를 비교할 때 방문자만 남기면 어떤 선택 문제가 생길 수 있는지 논의하라.

### 답변

(c) 두 구매율(전체 배정 고객 기준 vs 방문자 기준)의 차이와 선택 문제(Illustration of Selection Bias)에 대한 분석은 다음과 같습니다.

### 1. 두 구매율이 서로 다른 질문에 답한다는 점

두 지표는 마케팅 효과의 **서로 다른 단계**를 측정합니다.

*   **전체 배정 고객 기준 구매율 (`conversion_rate_over_all`)**:
    *   **질문**: "이메일을 발송하는 것이 전체 고객 중 실제 구매로 이어지는 효과를 갖는가?"
    *   **의미**: 이메일 발송이라는 개입(Intervention)이 최종 비즈니스 목표(판매)에 미치는 **순수한 효과(Overall Impact)**를 측정합니다. 여기에는 이메일이 웹사이트 방문 자체를 유도하는 효과와, 방문한 후 구매로 전환시키는 효과가 모두 포함됩니다.
*   **방문자 기준 구매율 (`conversion_rate_over_visitors`)**:
    *   **질문**: "웹사이트에 도달한 고객 중 이메일 발송 집단이 미발송 집단보다 더 높은 구매 전환율을 보이는가?"
    *   **의미**: 웹사이트 트래픽을 유도한 후, 그 트래픽을 얼마나 효율적으로 판매로 연결시키는지에 대한 **전환 효율(Conversion Efficiency)**만 측정합니다. 방문 자체의 효과는 배제됩니다.

### 2. 방문자만 남겼을 때 발생하는 선택 문제 (Selection Bias)

이메일 마케팅 효과를 평가할 때 '방문자' 집단만으로 분석하면 심각한 **선택 편향(Selection Bias)**이 발생합니다. 그 이유는 다음과 같습니다.

1.  **자기 선택(Self-Selection)의 개입**:
    *   이메일을 받고 웹사이트를 방문하는 고객은 이미 이메일에 관심을 보인 '적극적인 고객'입니다. 반면, 이메일을 받지 않았거나 받아도 클릭하지 않은 고객은 분석에서 제외됩니다.
    *   즉, '방문자' 집단은 무작위 할당(Random Assignment)된 전체 모집단이 아니라, **이메일의 매력도에 의해 선별된 집단**이 됩니다.

2.  **효과 과대평가 또는 왜곡**:
    *   만약 이메일 디자인이나 콘텐츠가 매우 훌륭하여 클릭률이 높다면, 방문자 집단은 '관심도가 높은 고객'들로만 구성됩니다. 이 집단의 구매율은 자연히 높아질 수밖에 없습니다.
    *   반대로, 이메일이 흥미롭지 않아 클릭률이 낮다면, 방문자 집단은 극소수의 강성 팬이나 우연히 찾은 사람들로 구성될 수 있어 구매율이 왜곡될 수 있습니다.
    *   따라서, **방문자 기준 구매율만 보면 이메일 발송의 유인 효과(Impact on Visits)를 무시하게 되어, 전체적인 마케팅 ROI(투자 대비 수익)를 과소평가하거나 잘못 해석할 위험**이 있습니다.

3.  **무작위화의 붕괴**:
    *   실험의 핵심은 무작위 할당을 통해 인과관계를 규명하는 것입니다. 방문자만 남기면 무작위 할정의 장점이 사라지고, '방문 여부'라는 새로운 선택 기준이 개입되어 두 집단 간 비교가 더 이상 공정한 대조군 비교가 되지 않습니다.

**결론적으로**, 이메일 마케팅의 진정한 효과를 평가하려면 **전체 배정 고객을 분모로 한 구매율**을 사용해야 합니다. 이는 이메일 발송이라는 행동 자체가 최종 판매에 기여하는 총체적인 영향을 반영하기 때문입니다. 방문자 기준 구매율은 웹사이트 최적화(CRO) 관점에서는 유용할 수 있으나, 이메일 캠페인의 전반적 성과를 평가하는 데는 적합하지 않습니다.

_소요 시간: 71초_
