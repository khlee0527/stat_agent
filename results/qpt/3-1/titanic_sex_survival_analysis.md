# 3-1. 타이타닉 성별 생존율 비교

- 데이터: `train(5).csv` (891명)
- 분석 변수: `Survived` (생존 1, 사망 0), `Sex`, `Pclass`
- 생존율 차이의 방향: 여성 생존율 − 남성 생존율
- 신뢰구간: 별도 언급이 없는 한 95% 정규근사(Wald) 신뢰구간
- 가정: 승객 관측치들이 서로 독립이라고 보는 단순 계산이다. 가족·동일 티켓 승객 간 관련성은 반영하지 않는다.

## (a) 성별 생존율과 차이

성별 생존율은 해당 성별 생존자 수를 해당 성별 전체 승객 수로 나누었다.

| 성별 | 생존자 / 전체 | 생존율 |
|---|---:|---:|
| 여성 | 233 / 314 | 74.2% |
| 남성 | 109 / 577 | 18.9% |

여성 생존율과 남성 생존율의 차이는 **55.3%포인트**이다.

두 비율 차이의 표준오차와 95% 신뢰구간은 다음과 같이 계산했다.

```math
\widehat{p}_F-\widehat{p}_M \pm 1.96\sqrt{\frac{\widehat{p}_F(1-\widehat{p}_F)}{n_F}+\frac{\widehat{p}_M(1-\widehat{p}_M)}{n_M}}
```

계산 결과 표준오차는 0.0296, 차이의 95% 신뢰구간은 **49.5~61.1%포인트**이다. 이 구간은 독립 관측 가정 아래의 근사 구간이다.

## (b) 객실 등급별 성별 생존율

같은 생존율과 신뢰구간 계산을 객실 등급별로 반복했다.

| 객실 등급 | 여성 생존자 / 승객 수 (생존율) | 남성 생존자 / 승객 수 (생존율) | 차이 (95% 신뢰구간) |
|---|---:|---:|---:|
| 1등석 | 91 / 94 (96.8%) | 45 / 122 (36.9%) | 59.9%p (50.7~69.2%p) |
| 2등석 | 70 / 76 (92.1%) | 17 / 108 (15.7%) | 76.4%p (67.2~85.5%p) |
| 3등석 | 72 / 144 (50.0%) | 47 / 347 (13.5%) | 36.5%p (27.5~45.4%p) |

세 등급 모두 여성 생존율이 남성보다 높았다. 차이는 2등석에서 가장 컸고 3등석에서 가장 작았다.

여성 중 1·2등석 승객은 54.1%였고 남성 중에서는 39.9%였다. 반대로 3등석 승객은 여성의 45.9%, 남성의 60.1%였다. 전체 생존율이 1등석 63.0%, 2등석 47.3%, 3등석 24.2%로 등급에 따라 다른 점을 고려하면, 여성의 상대적으로 높은 1·2등석 비중은 전체 성별 생존율 격차를 키우는 방향으로 작용할 수 있다.

## (c) 공통 객실 등급 분포로 표준화

공통 가중치는 전체 891명에서 관측한 객실 등급 비중으로 정했다.

| 객실 등급 | 공통 가중치 | 여성 등급별 생존율 | 남성 등급별 생존율 |
|---|---:|---:|---:|
| 1등석 | 216 / 891 = 24.2% | 96.8% | 36.9% |
| 2등석 | 184 / 891 = 20.7% | 92.1% | 15.7% |
| 3등석 | 491 / 891 = 55.1% | 50.0% | 13.5% |

각 성별의 표준화 생존율은 **등급별 생존율 × 공통 가중치**를 등급 전체에 걸쳐 합산했다.

\[
\widehat p_{F,\mathrm{std}}=\sum_{k=1}^{3} w_k\widehat p_{F,k}=70.0\%,
\qquad
\widehat p_{M,\mathrm{std}}=\sum_{k=1}^{3} w_k\widehat p_{M,k}=19.7\%.
\]

표준화 생존율 차이는 **50.4%포인트**이다. (a)의 실제 전체 생존율 차이 55.3%포인트보다 약 **4.9%포인트 작다**. 이 자료에서 관측된 객실 등급 분포 차이는 전체 격차를 키우는 방향으로 작용했다.

이 표준화 결과를 성별의 인과효과로 해석할 수는 없다. 객실 등급 분포를 같게 맞춘 기술적 비교이며, 무작위 배정 자료가 아니다. 연령, 가족 동반 여부 등 다른 요인의 영향이 남아 있을 수 있고, 객실 등급을 조정하는 것이 어떤 인과 경로를 차단하거나 남기는지도 별도로 정해야 한다. 인과 해석에는 명확한 인과 추정 대상과 추가 가정 및 교란 요인에 대한 검토가 필요하다.

## 재현용 Python 코드

```python
import math
import pandas as pd

df = pd.read_csv("train(5).csv")
z = 1.959963984540054

# (a) 전체 성별 생존율 및 Wald 신뢰구간
overall = df.groupby("Sex")["Survived"].agg(n="size", survivors="sum", rate="mean")
p_f = overall.loc["female", "rate"]
p_m = overall.loc["male", "rate"]
se = math.sqrt(
    p_f * (1 - p_f) / overall.loc["female", "n"]
    + p_m * (1 - p_m) / overall.loc["male", "n"]
)
difference = p_f - p_m
ci = (difference - z * se, difference + z * se)

print(overall)
print("Female - male:", difference)
print("95% CI:", ci)

# (b) 등급별 생존율 차이와 Wald 신뢰구간
for pclass, group in df.groupby("Pclass", sort=True):
    rates = group.groupby("Sex")["Survived"].agg(n="size", survivors="sum", rate="mean")
    pf, pm = rates.loc["female", "rate"], rates.loc["male", "rate"]
    d = pf - pm
    se = math.sqrt(
        pf * (1 - pf) / rates.loc["female", "n"]
        + pm * (1 - pm) / rates.loc["male", "n"]
    )
    print(pclass, rates, "difference:", d, "95% CI:", (d - z * se, d + z * se))

# (c) 전체 표본의 객실 등급 분포를 공통 가중치로 사용
weights = df["Pclass"].value_counts(normalize=True).sort_index()
stratum_rates = df.groupby(["Pclass", "Sex"])["Survived"].mean().unstack("Sex").sort_index()
standardized = stratum_rates.mul(weights, axis=0).sum(axis=0)

print("Common weights:")
print(weights)
print("Standardized rates:")
print(standardized)
print("Standardized female - male:", standardized["female"] - standardized["male"])
```
