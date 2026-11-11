# Bank Marketing: 과거 접촉과 가입률 분석

## 데이터와 변수 정의

- 파일: `bank-additional-full.csv` (세미콜론 구분)
- 관측치: 41,188명
- 결과 변수 `y`: 정기예금 가입 여부 (`yes`/`no`)
- `pdays`: 이전 캠페인에서 마지막으로 연락한 뒤 지난 일수. 변수 설명서에서 `999`는 **이전 캠페인에서 연락한 적 없음**을 뜻한다.
- 범주형 변수의 `unknown`은 실제 응답 범주라기보다 알 수 없는 정보를 나타내는 코드다.

데이터 사전: [bank-additional-names.txt](https://github.com/AlexGLiu/UCI_bank_marketing/blob/master/bank-additional-names.txt)

## (a) 특수값과 `unknown` 확인

`pdays` 41,188건 중 39,673건(96.32%)이 999이고, 나머지 1,515건(3.68%)은 실제 경과일 값이다. 따라서 999를 실제 999일로 해석할 수 없다.

| 변수 | `unknown` 건수 | 전체 중 비율 |
|---|---:|---:|
| `default` | 8,597 | 20.87% |
| `education` | 1,731 | 4.20% |
| `housing` | 990 | 2.40% |
| `loan` | 990 | 2.40% |
| `job` | 330 | 0.80% |
| `marital` | 80 | 0.19% |

이 파일에서 `contact`와 `poutcome`에는 `unknown` 문자열이 없고, `poutcome`에는 과거 캠페인 결과가 존재하지 않는 경우를 나타내는 `nonexistent`가 있다. `unknown`은 결측·미확인 상태로 구별해 두며 `yes`나 `no`와 같은 의미로 취급하지 않는다.

## (b) 과거 접촉 여부와 경과일 분리

두 변수를 다음과 같이 정의했다.

- `had_prior_contact`: `pdays != 999`이면 과거 접촉 경험이 있음.
- `elapsed_days`: 과거 접촉 경험이 있는 경우에만 `pdays`를 기록하고, 없는 경우에는 결측값으로 둠.

| 통계 | 관측 수 | 평균 경과일 |
|---|---:|---:|
| 원래 `pdays` 전체 | 41,188 | 962.48일 |
| 과거 접촉 고객의 경과일 | 1,515 | 6.01일 |

원래 `pdays` 평균은 `999`라는 특수 코드에 의해 크게 올라가 있어 경과일의 평균으로 해석할 수 없다. 과거 연락을 실제로 받은 고객만 대상으로 계산한 평균 경과일은 약 6.01일이다.

## (c) 접촉 여부·경과일 구간별 가입률

가입률은 각 구간에서 `y == "yes"`인 고객의 비율로 계산했다. 과거 접촉 고객의 경과일은 0–5일, 6–10일, 11일 이상으로 나눴다.

| 과거 접촉·경과일 구간 | 표본 수 | 가입자 수 | 가입률 |
|---|---:|---:|---:|
| 과거 접촉 없음 | 39,673 | 3,673 | 9.26% |
| 0–5일 | 705 | 445 | 63.12% |
| 6–10일 | 606 | 406 | 67.00% |
| 11일 이상 | 204 | 116 | 56.86% |

### `999`를 실제 일수로 오해했을 때의 잘못된 해석

> “고객 대부분은 마지막 접촉 후 약 999일이 지났고, 오래전에 접촉한 고객일수록 가입률이 낮다.”

이는 `999`가 과거 접촉이 없다는 표시라는 사실을 놓친 해석이다. 실제로 과거 접촉이 없는 고객은 39,673명이고, 과거 접촉 고객 1,515명의 평균 경과일은 6.01일이다. 관찰된 가입률은 과거 접촉 없음 9.26%, 접촉 후 0–5일 63.12%, 6–10일 67.00%, 11일 이상 56.86%였다. 이는 구간별 단순 표본 비율이므로, 접촉이 가입을 유발했다는 인과 효과로 해석할 수 없다. 특히 접촉 경험 구간의 표본 수가 작고 구간별 고객 구성이 다를 수 있다.

## 재현 코드

아래 코드는 변수 생성, `unknown` 집계, 평균 비교 및 구간별 가입률·표본 수 계산을 재현한다.

```python
import pandas as pd

df = pd.read_csv("bank-additional-full.csv", sep=";")

# (a) pdays와 범주형 unknown 확인
print("shape:", df.shape)
print("pdays counts:\n", df["pdays"].value_counts().sort_index())

for col in df.select_dtypes(include="object").columns:
    n_unknown = df[col].eq("unknown").sum()
    if n_unknown:
        print(col, n_unknown, f"({n_unknown / len(df):.2%})")

# (b) 과거 접촉 여부와 경과일 분리
df["had_prior_contact"] = df["pdays"].ne(999)
df["elapsed_days"] = df["pdays"].where(df["had_prior_contact"])

print("원래 pdays 평균:", df["pdays"].mean())
print("과거 접촉 고객 평균 경과일:", df["elapsed_days"].mean())
print("과거 접촉 여부별 표본 수:\n", df["had_prior_contact"].value_counts())

# (c) 접촉 후 경과일 구간별 가입률과 표본 수
df["elapsed_group"] = pd.cut(
    df["elapsed_days"],
    bins=[-1, 5, 10, float("inf")],
    labels=["0–5일", "6–10일", "11일 이상"],
)
df["contact_group"] = df["elapsed_group"].astype("object")
df.loc[~df["had_prior_contact"], "contact_group"] = "과거 접촉 없음"

order = ["과거 접촉 없음", "0–5일", "6–10일", "11일 이상"]
summary = (
    df.groupby("contact_group", observed=False)
      .agg(
          표본수=("y", "size"),
          가입자수=("y", lambda s: s.eq("yes").sum()),
          가입률=("y", lambda s: s.eq("yes").mean() * 100),
      )
      .reindex(order)
)
print(summary)
```
