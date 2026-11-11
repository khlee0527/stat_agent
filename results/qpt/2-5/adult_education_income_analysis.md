# Adult Census Income: 교육 변수와 고소득 비율 분석

## 분석 자료와 처리

adult.data(학습 분할 32,561행)와 adult.test(평가 분할 16,281행)를 합쳐 총 48,842행을 기술적으로 분석했다. adult.test 첫 설명 행을 건너뛰고, 범주형 문자열의 양끝 공백과 소득 라벨 끝의 마침표를 제거했다. 두 분할을 합친 표는 기술 요약용이며, 예측 평가를 위한 분할을 대체하지 않는다.

## (a) 변수 유형, 결측 표기, 공백, fnlwgt

| 유형 | 변수 | 해석 |
|---|---|---|
| 수치형 | age, capital-gain, capital-loss, hours-per-week | 양적 변수 |
| 수치형 가중치 | fnlwgt | 응답자가 대표하는 인구 규모를 보정하는 최종 가중치 |
| 순서형 | education, education-num | 교육 수준과 그 순위 코드 |
| 명목형 | workclass, marital-status, occupation, relationship, race, sex, native-country | 순서가 없는 범주 |
| 명목형 결과변수 | income | <=50K / >50K |

adult.names에 따르면 fnlwgt는 CPS 자료의 최종 가중치다. 주별 16세 이상 인구, 히스패닉 기원·연령·성별, 인종·연령·성별 통제치를 반영하도록 조정된다. 개인의 소득액이나 인구 수 그 자체가 아니라 가중 집계에 쓰이는 값이다.

원자료에서 문자열 앞 공백은 학습 파일의 9개 문자열 열 모두에서 각각 32,561행, 평가 파일의 같은 9개 열 모두에서 각각 16,281행 확인됐다. 문자열 뒤 공백은 발견되지 않았다. adult.test의 income 라벨에는 끝 마침표가 있어 제거했다.

| ?로 표시된 변수 | adult.data | adult.test | 합계 | 전체 중 비율 |
|---|---:|---:|---:|---:|
| workclass | 1,836 | 963 | 2,799 | 5.73% |
| occupation | 1,843 | 966 | 2,809 | 5.75% |
| native-country | 583 | 274 | 857 | 1.76% |

세 변수의 ?는 공백 제거 후 결측값으로 처리했다. 다른 변수의 결측은 없었다.

## (b) education과 education-num 대응

두 변수는 아래 순서로 일대일 대응한다. 원자료 48,842행에서 이 표준 대응과 다른 행은 0건이었다.

| education-num | education |
|---:|---|
| 1 | Preschool |
| 2 | 1st-4th |
| 3 | 5th-6th |
| 4 | 7th-8th |
| 5 | 9th |
| 6 | 10th |
| 7 | 11th |
| 8 | 12th |
| 9 | HS-grad |
| 10 | Some-college |
| 11 | Assoc-voc |
| 12 | Assoc-acdm |
| 13 | Bachelors |
| 14 | Masters |
| 15 | Prof-school |
| 16 | Doctorate |

이 데이터에서는 두 변수가 동일한 교육 수준 정보를 담는다. education은 범주명, education-num은 순위 숫자 코드다. 분석 모형에 둘 다 넣으면 정보가 중복될 수 있다.

## (c) 교육 수준별 고소득 비율

고소득은 income이 >50K인 경우로 정의했다. 단순 비율은 각 행을 동일하게 세고, 가중 비율은 고소득 행의 fnlwgt 합을 해당 교육 수준 전체 fnlwgt 합으로 나눴다. 관측 수는 교육 수준별 행 수다.

| 교육 수준 | 관측 수 | 고소득자 수 | 단순 표본 비율 | fnlwgt 가중 비율 | 차이 (%p) |
|---|---:|---:|---:|---:|---:|
| Preschool | 83 | 1 | 1.20% | 0.98% | -0.22 |
| 1st-4th | 247 | 8 | 3.24% | 2.59% | -0.65 |
| 5th-6th | 509 | 27 | 5.31% | 3.88% | -1.42 |
| 7th-8th | 955 | 62 | 6.49% | 6.08% | -0.42 |
| 9th | 756 | 41 | 5.42% | 4.64% | -0.78 |
| 10th | 1,389 | 87 | 6.26% | 6.30% | +0.04 |
| 11th | 1,812 | 92 | 5.08% | 4.54% | -0.54 |
| 12th | 657 | 48 | 7.31% | 7.77% | +0.46 |
| HS-grad | 15,784 | 2,503 | 15.86% | 15.49% | -0.37 |
| Some-college | 10,878 | 2,063 | 18.97% | 19.38% | +0.41 |
| Assoc-voc | 2,061 | 522 | 25.33% | 25.70% | +0.37 |
| Assoc-acdm | 1,601 | 413 | 25.80% | 26.18% | +0.38 |
| Bachelors | 8,025 | 3,313 | 41.28% | 41.74% | +0.45 |
| Masters | 2,657 | 1,459 | 54.91% | 56.24% | +1.33 |
| Prof-school | 834 | 617 | 73.98% | 73.77% | -0.21 |
| Doctorate | 594 | 431 | 72.56% | 73.58% | +1.02 |

전체 고소득자는 11,687명(단순 비율 23.93%)이다. 교육 수준이 높을수록 고소득 비율이 대체로 높게 관측된다. 가중 비율과 단순 비율은 대부분 가까우나, 5th-6th는 가중 비율이 1.42%p 낮고 Masters는 1.33%p 높다. 이는 가중치가 교육 집단 안의 응답자 구성에 반영된 결과다. 이 비교만으로 교육의 인과효과를 말할 수는 없다.

### 모집단 비율로 일반화하기 전에 확인할 점

fnlwgt 가중 비율은 주어진 가중치를 반영한 비율이다. 이를 모집단 비율로 해석하려면 아래 사항을 확인해야 한다.

- adult.names에 따르면 자료는 1994년 Census 데이터에서 추출됐고, 연령·소득·가중치·근로시간 조건으로 사례를 선별했다. 그 선별 조건에 맞는 인구가 목표 집단인지 확인해야 한다.
- fnlwgt가 이 하위집단(교육 수준별 고소득 비율)에 적용 가능한 최종 가중치인지, 자료 추출 및 무응답 조정이 충분히 반영됐는지 확인해야 한다.
- 설계 기반 표준오차나 신뢰구간 계산에는 층화·군집(PSU) 정보 또는 복제 가중치와 적절한 분산 추정법이 필요하다. fnlwgt만으로 표본 불확실성을 계산할 수는 없다.
- 자료 설명은 6개의 중복 또는 상충 사례도 언급한다. 현재 인구로 일반화하려면 오래된 자료라는 점도 고려해야 한다.

## 재현 코드

아래 코드는 자료 정리, 교육 수준 대응 검사, 표본 비율과 가중 비율 집계를 재현한다.

~~~python
import pandas as pd

cols = [
    "age", "workclass", "fnlwgt", "education", "education-num",
    "marital-status", "occupation", "relationship", "race", "sex",
    "capital-gain", "capital-loss", "hours-per-week",
    "native-country", "income"
]

def read_adult(path):
    # adult.test 첫 설명 행은 |로 시작하므로 건너뜀
    return pd.read_csv(path, names=cols, header=None, comment="|")

train = read_adult("adult.data")
test = read_adult("adult.test")
df = pd.concat([train, test], ignore_index=True)

for col in df.select_dtypes(include="object"):
    df[col] = df[col].str.strip()
df["income"] = df["income"].str.rstrip(".")

for col in ["workclass", "occupation", "native-country"]:
    df[col] = df[col].replace("?", pd.NA)

education_order = [
    "Preschool", "1st-4th", "5th-6th", "7th-8th", "9th", "10th",
    "11th", "12th", "HS-grad", "Some-college", "Assoc-voc",
    "Assoc-acdm", "Bachelors", "Masters", "Prof-school", "Doctorate"
]
education_num_map = dict(zip(education_order, range(1, 17)))
df["education"] = pd.Categorical(
    df["education"], categories=education_order, ordered=True
)
df["education-num"] = pd.to_numeric(df["education-num"], errors="coerce")

expected_num = df["education"].astype("object").map(education_num_map)
mismatch = (
    df["education-num"].notna()
    & expected_num.notna()
    & df["education-num"].ne(expected_num)
)
print("education-num 불일치 행:", int(mismatch.sum()))

df["high_income"] = df["income"].eq(">50K")
df["weighted_high"] = df["fnlwgt"] * df["high_income"]

summary = (
    df.groupby("education", observed=False)
      .agg(
          관측수=("high_income", "size"),
          고소득자수=("high_income", "sum"),
          단순비율=("high_income", "mean"),
          가중고소득합=("weighted_high", "sum"),
          가중치합=("fnlwgt", "sum"),
      )
)
summary["가중비율"] = summary["가중고소득합"] / summary["가중치합"]
summary["차이_pp"] = (summary["가중비율"] - summary["단순비율"]) * 100
summary["단순비율"] *= 100
summary["가중비율"] *= 100
print(summary[["관측수", "고소득자수", "단순비율", "가중비율", "차이_pp"]].round(2))
~~~

## 자료

- UCI Machine Learning Repository, [Adult dataset](https://archive.ics.uci.edu/dataset/2/adult)
- 첨부된 adult.names: 변수 설명, 자료 분할, fnlwgt 설명
