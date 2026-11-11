# 문제 3-3. 고객별 순구매액과 극단적으로 큰 구매액

데이터: `Online Retail(1).xlsx`  
분석 단위: 고객(`CustomerID`)  
관측 기간: 2010-12-01 08:26부터 2011-12-09 12:50까지

## 분석 기준

모든 고객에게 같은 달력상 관측 기간을 적용했다. 각 거래 행의 금액을 `Quantity × UnitPrice`로 계산하고, `CustomerID`가 있는 행만 고객별로 합산했다. 음수 수량으로 기록된 취소·반품 금액은 부호를 유지해 순구매액에 포함했다. 고객 식별자가 없는 행은 고객별 집계와 비중 계산에서 제외했다.

## (a) 고객별 순구매액

고객 식별이 가능한 거래는 406,829행, 고객은 4,372명이다. 고객별 순구매액의 합계는 8,300,065.81이다. 평균과 중앙값, 상위 고객 매출 비중은 다음과 같다.

| 지표 | 결과 |
|---|---:|
| 고객별 순구매액 평균 | 1,898.46 |
| 고객별 순구매액 중앙값 | 648.08 |
| 고객별 순구매액 합계 | 8,300,065.81 |
| 가장 큰 고객 순구매액 | 279,489.02 |
| 1위 고객 비중 | 3.37% |
| 상위 10명 비중 | 16.50% |
| 상위 1%(44명) 비중 | 29.92% |

평균이 중앙값보다 약 2.9배 높아 고객별 순구매액 분포는 오른쪽으로 치우쳐 있다. 고객별 순구매액을 내림차순으로 정렬한 뒤 상위 고객들의 금액 합계를 전체 고객 순구매액 합계로 나누어 비중을 계산했다.

## (b) 평균 순구매액의 부트스트랩 신뢰구간

고객별 순구매액 4,372개를 관측 단위로 삼았다. 고객 4,372명을 복원추출해 매번 평균을 계산하는 과정을 10,000회 반복하고, 부트스트랩 평균 분포의 2.5 및 97.5 백분위수를 95% 신뢰구간으로 사용했다.

- 표본 평균: **1,898.46**
- 부트스트랩 표준오차: **122.93**
- 95% 백분위수 부트스트랩 신뢰구간: **[1,678.41, 2,159.21]**

이 구간은 고객들이 유사한 고객 모집단에서 독립적으로 추출되었다는 가정 아래 평균의 표본변동을 나타낸다. 상품 거래 행을 재표집하면 한 고객의 여러 행이 서로 연관되어 있을 수 있다는 점을 무시하고, 거래가 많은 고객이 재표집에서 더 큰 비중을 갖게 된다. 거래당 평균과 고객당 평균은 다른 추정 대상이며, 고객당 평균의 불확실성을 계산하려면 고객별 합계를 만든 뒤 고객 단위로 재표집해야 한다.

## (c) 큰 고객 구매액을 제외했을 때

극단적으로 큰 구매액의 기준은 고객별 순구매액 분포의 Tukey 상한 `Q3 + 1.5 × IQR`로 정했다. `Q1 = 293.36`, `Q3 = 1,611.73`, `IQR = 1,318.36`이므로 상한은 **3,589.27**이다. 이 상한을 넘는 고객은 422명(9.65%)이다.

| 집계 대상 | 고객 수 | 평균 순구매액 | 중앙값 |
|---|---:|---:|---:|
| 전체 고객 | 4,372 | 1,898.46 | 648.08 |
| Tukey 상한 이하 고객 | 3,950 | 853.84 | 566.97 |

상한 초과 422명의 순구매액 합계는 4,927,412.53으로 전체 고객 순구매액의 59.37%다. 이들을 제외하면 평균은 1,044.62(55.0%) 낮아진다. 상한 이하 고객 집단만 대상으로 고객 단위 부트스트랩을 적용한 95% 신뢰구간은 [828.55, 879.25]다. 이 구간은 관측된 Tukey 상한을 고정한 조건부 계산이며, 상한 추정의 불확실성은 반영하지 않는다.

전체 평균은 관측 기간 동안 고객 식별이 가능한 고객 전체를 대상으로 한다. Tukey 상한 이하 평균은 순구매액이 해당 기준 이하인 고객 집단만 대상으로 하므로 두 값은 같은 추정 대상이 아니다. 큰 구매액이 실제 거래라면 이를 제외하는 것은 단순한 오류 정정이 아니라 추정 대상의 변경이다. 참고로 상위 1%(44명)만 제외하면 남은 고객의 평균은 1,343.87로, 제외 기준에 따라 평균이 달라진다.

따라서 전체 고객의 평균 구매액을 설명할 때는 실제 고액 고객을 포함한 평균을 사용한다. 상한 이하 평균은 그 기준에 해당하는 고객 집단의 평균으로만 해석한다. 일반화 범위도 고객 식별이 가능한 고객과 자료에 나타난 구매 환경으로 제한해야 한다. 이는 전체 방문자나 미래 고객, 고객별 연간 또는 평생 구매액을 곧바로 나타내지 않는다. 같은 달력 기간을 적용했더라도 첫 구매 시점이 달라 고객별 실제 관측 활동 기간은 같지 않을 수 있다.

## 재현용 pandas 코드

아래 코드는 파일을 읽어 (a)와 (c)의 요약치를 계산하고, (b)와 (c)의 고객 단위 백분위수 부트스트랩을 수행한다.

```python
import numpy as np
import pandas as pd

df = pd.read_excel("Online Retail(1).xlsx")
df["LineNet"] = df["Quantity"] * df["UnitPrice"]

identified = df.loc[df["CustomerID"].notna()]
customer_net = identified.groupby("CustomerID")["LineNet"].sum().astype(float)

# (a) 기술통계와 상위 고객 비중
total_net = customer_net.sum()
print("행 수:", len(identified))
print("고객 수:", len(customer_net))
print("총 순구매액:", total_net)
print("평균:", customer_net.mean())
print("중앙값:", customer_net.median())

for n in (1, 10, int(np.ceil(0.01 * len(customer_net)))):
    top_sum = customer_net.nlargest(n).sum()
    print(f"상위 {n}명 비중:", top_sum / total_net)

# 고객 단위 백분위수 부트스트랩
def bootstrap_mean_ci(values, B=10_000, seed=20261031, batch_size=250):
    values = np.asarray(values, dtype=float)
    rng = np.random.default_rng(seed)
    boot_means = np.empty(B)
    n = len(values)

    for start in range(0, B, batch_size):
        b = min(batch_size, B - start)
        idx = rng.integers(0, n, size=(b, n))
        boot_means[start:start + b] = values[idx].mean(axis=1)

    ci = np.quantile(boot_means, [0.025, 0.975])
    return boot_means.std(ddof=1), ci

se_all, ci_all = bootstrap_mean_ci(customer_net.to_numpy())
print("전체 고객 부트스트랩 SE, CI:", se_all, ci_all)

# (c) Tukey 상한 초과 고객 제외
q1 = customer_net.quantile(0.25)
q3 = customer_net.quantile(0.75)
iqr = q3 - q1
upper_fence = q3 + 1.5 * iqr
ordinary = customer_net.loc[customer_net <= upper_fence]
extreme = customer_net.loc[customer_net > upper_fence]

print("Q1, Q3, IQR, Tukey 상한:", q1, q3, iqr, upper_fence)
print("제외 고객 수:", len(extreme))
print("제외 고객 비중:", len(extreme) / len(customer_net))
print("제외 고객 순구매액 비중:", extreme.sum() / total_net)
print("전체 평균:", customer_net.mean())
print("상한 이하 평균:", ordinary.mean())
print("전체 중앙값:", customer_net.median())
print("상한 이하 중앙값:", ordinary.median())

se_ordinary, ci_ordinary = bootstrap_mean_ci(ordinary.to_numpy())
print("상한 이하 고객 부트스트랩 SE, CI:", se_ordinary, ci_ordinary)

# 상위 1%(44명) 제외 민감도
n_top1 = int(np.ceil(0.01 * len(customer_net)))
top1_ids = customer_net.nlargest(n_top1).index
print("상위 1% 제외 평균:", customer_net.drop(top1_ids).mean())
```
