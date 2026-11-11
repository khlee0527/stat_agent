# 문제 2-2. Ames Housing의 지하실·차고 결측값 처리

## 분석 대상과 기준

첨부된 `train(4).csv`를 pandas로 읽었다. 자료는 1,460개 주택, 81개 변수로 구성되어 있다. 분석 대상은 `Bsmt*`, `Garage*` 변수이며, 거래가격은 `SalePrice`다.

변수 설명서에서 지하실 등급 변수의 `NA`는 지하실 없음, 차고 유형·마감·품질·상태 변수의 `NA`는 차고 없음을 뜻한다. CSV에서는 이러한 코드가 빈칸으로 저장되어 pandas에서 결측값으로 읽힌다. 따라서 빈칸을 모두 일반적인 정보 누락으로 보면 안 된다.

## (a) 변수 설명서의 NA 의미와 구분 규칙

| 시설 | 변수 | 설명서상 `NA` |
|---|---|---|
| 지하실 | `BsmtQual`, `BsmtCond`, `BsmtExposure`, `BsmtFinType1`, `BsmtFinType2` | 지하실 없음 |
| 차고 | `GarageType`, `GarageFinish`, `GarageQual`, `GarageCond` | 차고 없음 |
| 차고 건축연도 | `GarageYrBlt` | 차고 건축연도 변수. 숫자형이므로 빈칸 자체는 없음 코드가 아님 |

**구분 규칙**

- 지하실 면적 변수 `TotalBsmtSF`, `BsmtFinSF1`, `BsmtFinSF2`, `BsmtUnfSF`가 모두 0이고 지하실 등급 변수가 함께 비어 있으면 **지하실 없음**으로 분류한다.
- 지하실 면적이 양수인데 지하실 등급 변수 하나가 비어 있으면 **해당 정보 누락**으로 남긴다.
- 차고 수용량 `GarageCars`와 면적 `GarageArea`가 모두 0이고 차고 관련 범주형 변수가 비어 있으면 **차고 없음**으로 분류한다.
- 차고 수용량 또는 면적이 양수인데 차고 속성값이 비어 있으면 **해당 정보 누락**으로 남긴다. 차고가 존재하는데 `GarageYrBlt`만 비어 있는 경우도 건축연도 누락이다.

첨부 파일에서 지하실 관련 결측은 `BsmtQual` 37건, `BsmtCond` 37건, `BsmtExposure` 38건, `BsmtFinType1` 37건, `BsmtFinType2` 38건이다. 이 중 37채는 지하실 면적이 0이고 관련 항목들이 함께 비어 있어 지하실이 없는 집이다. 추가로 `Id=949`는 지하실 면적이 936인데 `BsmtExposure`가 비어 있고, `Id=333`은 지하실 면적이 3,206인데 `BsmtFinType2`가 비어 있다. 이 둘은 시설 부재가 아니라 실제 정보 누락이다.

차고 관련 변수는 `GarageType`, `GarageYrBlt`, `GarageFinish`, `GarageQual`, `GarageCond` 각각 81건이 비어 있다. 해당 81채는 모두 `GarageCars=0`, `GarageArea=0`이므로 차고가 없는 경우다. 차고가 있는 집에서 차고 속성값이 빠진 사례는 없었다.

## (b) 규칙 적용과 최빈값·중앙값 일괄 대체 비교

규칙에 따라 지하실·차고가 없는 상태를 명시적인 범주로 보존하고, 지하실이 있는데 기록되지 않은 `Id=333`과 `Id=949`의 항목은 미상으로 남긴다. `GarageYrBlt`는 차고가 없는 집에서 구조적으로 해당하지 않으므로 중앙값을 넣지 않고 결측 상태와 차고 유무를 별도로 보존한다.

비교를 위해 모든 범주형 결측을 각 변수의 최빈값으로, `GarageYrBlt` 결측을 중앙값으로 대체하면 다음 값이 들어간다.

| 변수 | 최빈값 또는 중앙값 |
|---|---|
| `BsmtQual` | `TA` |
| `BsmtCond` | `TA` |
| `BsmtExposure` | `No` |
| `BsmtFinType1` | `Unf` |
| `BsmtFinType2` | `Unf` |
| `GarageType` | `Attchd` |
| `GarageYrBlt` | 1980 |
| `GarageFinish` | `Unf` |
| `GarageQual` | `TA` |
| `GarageCond` | `TA` |

일괄 대체는 실제로 지하실이 없는 37채에도 지하실 품질·상태·유형 값을 채운다. 차고가 없는 81채에는 붙박이 차고(`Attchd`), 1980년 건축, 미완성(`Unf`), 평균 품질·상태(`TA`) 값을 채운다. 예를 들어 `Id=40`은 지하실 면적과 차고 면적·수용량이 모두 0인데, 일괄 대체 후에는 지하실 및 차고 속성값이 있는 것처럼 표현된다. 이는 0 면적·0 수용량과 모순된다.

실제 지하실이 있는 `Id=333`의 `BsmtFinType2`에는 `Unf`, `Id=949`의 `BsmtExposure`에는 `No`가 들어간다. 하지만 이 값들은 관측된 사실이 아니라 최빈값으로 만든 추정치다.

## (c) 시설 보유 여부별 거래가격 비교

보유 여부는 지하실의 경우 `TotalBsmtSF > 0`, 차고의 경우 `GarageCars > 0` 또는 `GarageArea > 0`으로 판정했다. 최빈값 대체 자료에서는 범주형 변수의 값이 채워졌는지를 기준으로 보유 여부를 분류했다. 가격은 평균과 중앙값을 계산했다.

| 시설 | 처리 방식 | 보유 주택 수 | 보유 평균 가격 | 보유 중앙값 | 미보유 주택 수 | 미보유 평균 가격 | 미보유 중앙값 |
|---|---|---:|---:|---:|---:|---:|---:|
| 지하실 | 규칙 적용 | 1,423 | $182,878 | $165,000 | 37 | $105,653 | $101,800 |
| 지하실 | 최빈값 대체 | 1,460 | $180,921 | $163,000 | 0 | — | — |
| 차고 | 규칙 적용 | 1,379 | $185,480 | $167,500 | 81 | $103,317 | $100,000 |
| 차고 | 최빈값 대체 | 1,460 | $180,921 | $163,000 | 0 | — | — |

최빈값 대체 후에는 미보유 집단이 사라지고, 지하실이 없는 37채와 차고가 없는 81채가 보유 집단에 합쳐진다. 그 결과 보유 집단의 가격 요약도 낮아진다. 예를 들어 차고 보유 집단의 평균은 규칙 적용 시 $185,480이지만, 대체 후에는 $180,921이다. 전체 거래가격의 평균과 중앙값은 대체 전후 같으며, 달라지는 것은 시설 보유 집단의 구성과 그 집단별 요약값이다.

**분석 목적에 적절한 방식은 규칙 적용이다.** 시설 보유 여부별 거래가격을 비교하려면 시설 없음이라는 구조적 상태를 보존해야 한다. 최빈값·중앙값 일괄 대체는 없는 시설을 있는 것처럼 만들고, 가격이 낮은 미보유 주택을 보유 집단에 섞어 비교 결과를 왜곡한다. 일반 정보 누락은 미상으로 유지하거나, 예측 분석에서 별도의 결측 지표와 함께 목적에 맞게 처리해야 한다.

## 재현용 pandas 코드

```python
import pandas as pd

df = pd.read_csv("train(4).csv")

# 시설 존재 기준
df["HasBasement"] = df["TotalBsmtSF"].gt(0)
df["HasGarage"] = df["GarageCars"].gt(0) | df["GarageArea"].gt(0)

# 결측 수 확인
basement_cols = [
    "BsmtQual", "BsmtCond", "BsmtExposure", "BsmtFinType1", "BsmtFinType2"
]
garage_cols = [
    "GarageType", "GarageYrBlt", "GarageFinish", "GarageQual", "GarageCond"
]
print(df[basement_cols + garage_cols].isna().sum())

# 가격 요약
for feature in ["HasBasement", "HasGarage"]:
    print(df.groupby(feature)["SalePrice"].agg(["count", "mean", "median"]))

# 일괄 대체 기준값
categorical_cols = basement_cols + [
    "GarageType", "GarageFinish", "GarageQual", "GarageCond"
]
modes = {c: df[c].mode(dropna=True).iloc[0] for c in categorical_cols}
garage_year_median = df["GarageYrBlt"].median()
```

## 참고한 변수 설명서

- [Ames Housing Data Documentation](https://github.com/eugeneyan84/Ames-Housing-Data-Analysis/blob/main/DataDocumentation.txt)
- [CMU Statistical & Data Science Data Repository: Ames Housing](https://cmustatistics.github.io/data-repository/money/ames-housing.html)
