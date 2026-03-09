## OLIST 배송 개선 프로젝트

## 1. 프로젝트 개요
![Period](https://img.shields.io/badge/기간-2025.12--2026.01-blue?style=flat-square) 
![Duration](https://img.shields.io/badge/소요-약_5주-green?style=flat-square)
### 1) Skills & Workflow

| Category | Tools |
| :--- | :--- |
| **Language** | Python (Pandas, Numpy, Matplotlib) |
| **Database** | PostgreSQL (PgAdmin) |
| **Visualization** | Tableau Public |
| **Workflow** | **Python** (Pre-processing) ➡️ **PostgreSQL** (ETL) ➡️ **Tableau** (Visualization) |


### 2) 분석 배경
<table style="border: none; border-collapse: collapse;">
  <tr style="border: none;">
    <td style="border: none;"width="50%">
      Olist는 브라질 전역을 연결하는 이커머스 플랫폼이지만, 물류의 불안정성이 존재합니다.
      <ul>
        <li><b>배송 지연 현황:</b> 전체 주문의 <b>6.57% (6,535건)</b>가 지연됩니다.</li>
        <li><b>불안정성:</b> 배송 기간의 <b>표준 편차 10일</b>, 최대 <b>188일의 극단적 지연</b> 등 일관된 배송 경험을 제공하지 못합니다.</li>
      </ul>
    </td>
    <td style="border: none;" width="50%">
      <img src="https://github.com/user-attachments/assets/15e11cee-5ef7-4973-b676-7f8ebcc761a0" width="80%"><br></td>
  </tr>
</table>
<img width="900" height="378" alt="image" src="https://github.com/user-attachments/assets/6b6e166d-15b4-4471-abe1-4664bccfb5a9" />


### 3) 문제 정의

배송 지연은 고객 경험을 저해하는 핵심 요인입니다.  
이에 따라 고객 경험에 미치는 **영향을 정량화** 하고, 배송 지연이 발생하는 **주요 원인을 분석**하여 운영 관점에서 **개선 가능한 지점을 도출**하고자 합니다.


## 2. 데이터 소개

- **활용 데이터:**[Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- 2016년부터 2018년까지 **브라질** **Olist 이커머스 플랫폼의 약 10만 건의 실제 주문 정보**를 담은 **공공 데이터셋**으로, 고객, 주문, 리뷰 등 **8개의 관계형 테이블**로 구성되어 있습니다.
- **선정 이유:** 고유 식별자(ID)를 기준으로 **병합**하여, 지연이 발생한 구체적인 노선과 원인 등을 **입체적으로 분석**할 수 있습니다.



## 3. 분석 과정
* [🔗전처리 및 EDA 코드](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/1.%20Data%20preparation%20%2B%20EDA.ipynb)
* [🔗가설 검증 코드](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/2.Hypothesis_Testing.ipynb)
* [🔗가설 검증 정리 노션](https://plaid-paradox-1a3.notion.site/Olist-2cad569cd2e180fb926cd3c2edc3f423?source=copy_link)

### 1) 데이터 품질 관리

- **데이터 전처리: 데이터 품질**을 진단하고, **컬럼 내 관계 분석**을 하는 **자동화 전처리 함수**를 설계하여 반복적인 전처리를 수행하였습니다.
- **열 생성:** 약속된 배송일과 실제 배송일 격차를 의미하는  `delivery_efficiency`열과, 지연 여부를 확인하는 `is_delayed`열을 생성했습니다.


### 2) 분석 요약
**(1) 배송 지연 현황** 

- **최근 악화 추세:** 이번 달 배송 지연율은 6.04%로 전월 대비 82.6% 급증하며 불안정한 양상을 보이고 있습니다.
- **장기 지연 구조:** 지연 주문 중 절반 이상인 **57.61%**가 6일 이상 지연이며**,** 21일 이상의 극단적 지연 사례도 **12.17%**에 달합니다.
- **긍정적 신호:** 전체 지연율은 상승했으나, 가장 치명적인 6일 이상 지연율은 **전월 대비 47.6% 감소**하며 개선되는 추세를 보이고 있습니다.
<img width="1178" height="477" alt="image" src="https://github.com/user-attachments/assets/a57d1d3d-a921-4a43-bee8-a031376f230b" />
<img width="937" height="391" alt="image" src="https://github.com/user-attachments/assets/106fd090-0cbb-4ee5-923d-52bb9cc79cfc" />


**(2) 배송 지연과 고객 영향 분석**

- **고객 영향 정량화:** 배송 지연 시, 리뷰 점수는 **2.02점 하락**하며, 재구매율은 **17.1% 감소**합니다.
- **임계점 발견:** 지연 일수 **6일차 부터 리뷰 점수가 1점대로 떨어지며,** 만족도가 오르지 않는 **1점대 유지 구간에 진입**합니다.
- **재구매 행동의 역설:** 지연 후 재구매한 고객(2.53%, 162건)의 재구매 주기가 정시 도착 그룹보다 **24.5일 빠른 현상이 확인**되었습니다.
    - 이는 플랫폼 의존도가 높은 **소수의 충성 고객군이 존재함**을 의미합니다.
<table style="border: none; border-collapse: collapse;">
  <tr style="border: none;">
    <td style="border: none;" width="33%">
      <img width="399" height="561" alt="image" src="https://github.com/user-attachments/assets/ecff06e1-bece-4732-94d1-1807d558c1ef" />
    </td>
    <td style="border: none;" width="33%">
      <img width="417" height="564" alt="image" src="https://github.com/user-attachments/assets/9428631b-2324-4318-a87a-a81202ac95ba" />
    </td>
    <td style="border: none;" width="33%">
      <img width="413" height="564" alt="image" src="https://github.com/user-attachments/assets/29c26bd0-4e4d-4592-9b7e-ca5a7ddd2a7e" />
    </td>
  </tr>
</table>
<img width="1178" height="578" alt="image" src="https://github.com/user-attachments/assets/4070e642-8bd0-41ef-97e1-6063292f25e4" />


**(3) 배송 지연의 원인 분석**

- **셀러 지연:** 셀러가 발송 기한을 초과하면, 배송 지연 확률이 **약 4배 급증**하고 배송 기간은 **7.7일 추가 소요**됩니다.
- **병목 노선:** 동일 주 대비 타 주 배송 지연율이 **1.73배 높고,** 배송 거리에 따라 **지연율이 증가**하며, **전체 지연의 80%가 상위 5% 노선에서 발생**하고 있습니다.
<table style="border: none; border-collapse: collapse;">
  <tr style="border: none;">
    <td style="border: none;" width="33%">
      <img width="377" height="577" alt="image" src="https://github.com/user-attachments/assets/5c9c919f-a4b0-493c-ba9c-b18effd83780" />
    </td>
    <td style="border: none;" width="33%">
      <img width="412" height="565" alt="image" src="https://github.com/user-attachments/assets/785cb4c3-b106-4b39-8dad-ccd7c6d5f210" />
    </td>
    <td style="border: none;" width="33%">
      <img width="402" height="565" alt="image" src="https://github.com/user-attachments/assets/d4482af3-e3bb-40d9-a882-3788708dabd3" />
    </td>
  </tr>
</table>
<img width="977" height="378" alt="image" src="https://github.com/user-attachments/assets/40a09552-4527-4cc7-908d-e1de59b02e72" />


**(4) 핵심 지표(KPI) 정의** 

분석에 기반하여, 실시간 모니터링을 위해 아래의 세가지 지표를 핵심 KPI로 정의내렸습니다.
| **KPI** | **정의** | 설명 |
| --- | --- | --- |
| **배송 지연율** | 실제 도착일 > 예상 도착일 | 배송 품질의 진단 지표 |
| **셀러 발송 지연율** | 택배사 전달일 > 셀러 발송 기한 | 셀러 관리 지표 |
| **6일 이상 지연율** | 예상 도착일 + 6일 이상 | 리뷰 스코어 관리 지표 |


## 3. 탐색적 데이터 분석 및 가설 검증 (Python)



### 1) 주요 전처리 및 데이터 품질 검증

분석의 신뢰도를 확보하고 반복적인 EDA 작업을 효율화하기 위해 **자동화 함수**를 설계하여 전처리를 수행했습니다.

#### 데이터 전처리 자동화 함수

* **`preprocessing(df)` : 데이터 품질 진단**
    * **완전성 검증:** 각 컬럼의 결측치 수와 비중을 자동 계산하여 데이터 누락 상태를 파악했습니다.
    * **고유성 및 PK 식별:** 중복도를 체크하여 **기본키 후보를 식별**하고 데이터의 고유성을 검증했습니다.
    * **타입 요약:** 수치형/범주형 데이터를 자동 분류하여 데이터 타입의 적절성을 확인했습니다.

* **`check_relationship(df, col_a, col_b)` : 컬럼 내 관계 분석**
    * 테이블 내 식별자 간 관계(1:1, 1:N, N:M)를 분석하여 데이터 모델의 구조를 파악했습니다.
    * 이를 통해 `customer_unique_id`와 `customer_id`가 **1:N(일대다) 관계**임을 식별, **재구매 고객의 존재**를 데이터 구조적으로 증명했습니다.

* **`box_plot(df)` : 시각적 분포 탐색 및 이상치 식별**
    * 연속형 수치 데이터에 대해 **Box plot과 Violin plot을 자동 생성**하여 이상치를 한눈에 파악했습니다.
    * 고유값 개수를 기준으로 필터링하여 분석 효율성을 높였습니다.


#### 데이터 최적화 및 파생 변수 생성

1.  **시계열 데이터 최적화:** 주문, 승인, 배송 관련 5개 컬럼을 `datetime` 타입으로 일괄 변환하여 시간 흐름에 따른 분석이 가능하도록 했습니다.
2.  **물류 효율 지표 산출:**
    * `delivery_efficiency`: 약속된 배송일과 실제 배송일의 격차를 계산하여 물류 속도를 수치화했습니다.
    * `is_delayed`: 약속된 날짜를 초과한 건에 대해 지연 여부 컬럼을 생성하여 핵심 분석 지표로 활용했습니다.



### 2) 탐색적 분석

- [고객] 재구매율 및 리뷰 분석:
    - 전체 재구매율(3.04%)을 산출하고, 동일 주문 내 리뷰 재작성 유저의 점수 변화를 확인
<img width="949" height="434" alt="image" src="https://github.com/user-attachments/assets/401147a0-f413-47ff-bac4-c97a40cfe726" />
    
    
- [배송] 예측 정확도 분석:
    - 예측일 대비 실제 도착일을 시각화하여 지연 이상치가 긴 분포로 극단적 지연 사례가 신뢰도를 낮출 수 있음을 확인
<img width="1176" height="477" alt="image" src="https://github.com/user-attachments/assets/d6491cfa-c329-4abf-9a13-83b5356a8ec3" />

        
- [매출] 지역 및 카테고리:
    - 상파울루(SP) 등 브라질의 특정 주와 도시에 고객층이 극도로 밀집된 분포와 매출이 높은 카테고리를 확인
<img width="1178" height="378" alt="image" src="https://github.com/user-attachments/assets/26a9ccbe-ac5a-4862-812e-ccd704f34a03" />
<img width="1184" height="578" alt="image" src="https://github.com/user-attachments/assets/2fbb58ad-b9f8-46ae-85e8-44b8f77eb239" />

        

### 3) 가설 검증
* [🔗가설 검증 정리 노션](https://plaid-paradox-1a3.notion.site/Olist-2cad569cd2e180fb926cd3c2edc3f423?source=copy_link)

**총 5가지의 가설을 설정**하여 배송 지연의 영향과 원인을 분석했습니다. 

- **가설 1:** [발송 기한] 셀러의 발송 기한 준수 여부와 최종 배송 지연의 상관관계
    - **→ 지연 확률 4배 증가, 배송일 +7.7일**
- **가설 2:** [지역 편차] 지역별 배송 인프라 차이 및 지연 집중 노선 존재 여부
    - →  **상위 5% 구간에 지연 80% 집중**
- **가설 3:** [고객 만족] 배송 지연 일수와 리뷰 점수 간의 상관관계
    - →  **리뷰 점수 -2.02점 급락 (6일 이후 1점대 진입)**
- **가설 4:** [비즈니스 영향] 배송 지연이 재구매율에 미치는 부정적 영향
    - →  **재구매율 17.1% 하락**
- **가설 5:** [재구매 패턴] 지연 경험 고객의 재구매 주기 변화 분석
    - →  **유효 표본 부족(162건) 및 예상과 다른 재구매 주기 확인**

<img width="1178" height="578" alt="image" src="https://github.com/user-attachments/assets/254b0596-8182-4cd9-9455-20dc95f0e4b1" />


## 4. 데이터 추출 (SQL)

* [🔗SQL 추출 쿼리](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/3.%20SQL_delivery_analysis)

### 1) 사용 목적

태블로 시각화를 위해 실시간 모니터링에 필요한 데이터를 총 8개의 쿼리로 추출

### 2) 핵심 쿼리

- **월별 KPI와 증감률**
    - 분석 목적: 지난 달과의 비교를 통해 **변화를 확인할 수 있도록** 월별 추세를 추출함
    - 핵심 로직:
        - 월별 기초 집계(CTE1): `DATE_TRUNC`로 데이터를 월 단위로 그룹화하고, `CASE WHEN`절을 사용해 월별 KPI 산출
        - 지난 달 지표(CTE2): `LAG` 윈도우 함수를 사용해 전월의 KPI수치를 행에 결합
        - 증감률 계산: 현재 월과 전월의 지표 차이를 전월 지표로 나누어 증감률 산출
    - 주요 함수: `LAG` (지난 달의 KPI값을 현재 행으로 가져옴), `NULLIF` (0이 나오지 않게 에러 방지)
    - 쿼리
        
        ```sql
        WITH monthly_kpi AS (
            SELECT 
        				DATE_TRUNC('month', o.order_purchase_timestamp) AS month,
                COUNT(DISTINCT CASE WHEN delivery_efficiency < 0 THEN o.order_id END)
                / NULLIF(COUNT(DISTINCT o.order_id), 0) AS delay_rate,
                COUNT(DISTINCT CASE WHEN order_delivered_carrier_date::DATE > shipping_limit_date::DATE THEN o.order_id END)
                / NULLIF(COUNT(DISTINCT o.order_id), 0) AS seller_delay_rate,
                COUNT(DISTINCT CASE WHEN delivery_efficiency <= -6 THEN o.order_id END)
                / NULLIF(COUNT(DISTINCT CASE WHEN delivery_efficiency < 0 THEN o.order_id END), 0) AS long_days_delay_rate
            FROM orders o 
            LEFT JOIN items i ON o.order_id = i.order_id 
            GROUP BY 1
        ), 
        prev_kpi AS (
        	SELECT *,
        		LAG(delay_rate) OVER (ORDER BY month) AS prev_delay_rate,
        		LAG(seller_delay_rate) OVER (ORDER BY month) AS prev_seller_rate,
        		LAG(long_days_delay_rate) OVER (ORDER BY month) AS prev_long_days_delay_rate
        	FROM monthly_kpi
        )
        SELECT 
            month,
            round(delay_rate::numeric, 4) AS delay_rate,
            round(((delay_rate - prev_delay_rate) 
                / NULLIF(prev_delay_rate, 0))::numeric, 4) AS delay_rate_mom,
            
            round(seller_delay_rate::numeric, 4) AS seller_delay_rate,
            round(((seller_delay_rate - prev_seller_rate)
                / NULLIF(prev_seller_rate, 0))::numeric, 4) AS seller_delay_rate_mom,
            
            round(long_days_delay_rate::numeric, 4) AS long_days_delay_rate,
            round(((long_days_delay_rate - prev_long_days_delay_rate) 
                / NULLIF(prev_long_days_delay_rate, 0))::numeric, 4) AS long_days_delay_rate_mom   
        FROM prev_kpi
        ORDER BY 1;
        ```
        
- **지연 후 재구매 고객 리스트**
    - 분석 목적:
        - 배송 지연이라는 부정적 경험에도 재구매를 선택한 고객군을 식별하고, 이들의 구매 특성을 요약한 마케팅 리스트를 구축
    - 핵심 로직:
        - 주문 순서 정의(CTE1): `ROW NUMBER` 윈도우 함수로 고객별 주문 순서를 정의 내림
        - 첫구매 지연 고객군 선별(CTE2): `WHERE` 조건문을 사용해서 지연이고, 첫 주문인 고객을 선별
        - 지연 여부 정의: delivery_efficiency가 음수인 경우 지연 주문으로 정의
        - 재구매 주기 산출: 조건문을 사용해 두번째 주문과 첫 주문의 일차를 `EXTRACT` 함수로 추출
    - 주요 함수:
        - `ROW_NUMBER()` 윈도우함수(고객별 주문 순서), `EXTRACT` (재구매 주기)
    - 쿼리
        
        ```sql
        WITH order_revenue AS (
        		SELECT
        				order_id, SUM(payment_value) AS revenue
        		FROM payments
        		GROUP BY 1
        ), 
        customer_orders_ranked AS (
        		SELECT 
        				c.customer_unique_id,
        				o.order_id,
        				o.order_purchase_timestamp,
        				o.order_delivered_customer_date,
        				o.order_estimated_delivery_date,
        				r.revenue,
        				CASE WHEN o.delivery_efficiency <0 THEN 1 ELSE 0 END AS is_delayed,
        				ROW_NUMBER() OVER (PARTITION BY c.customer_unique_id ORDER BY o.order_purchase_timestamp ASC, o.order_id ASC) AS order_rank
        		FROM customers c
        		JOIN orders o ON c.customer_id = o.customer_id
        		LEFT JOIN order_revenue r ON o.order_id = r.order_id
        		WHERE o.order_status = 'delivered'
        ),
        first_late_order AS (
        		SELECT
        				customer_unique_id,
        				order_purchase_timestamp AS first_date,
        				order_delivered_customer_date,
        				order_estimated_delivery_date,
        				(order_delivered_customer_date::date - order_estimated_delivery_date::date) AS late_days
        		FROM customer_orders_ranked
        		WHERE is_delayed = 1
        		AND order_rank = 1
        )
        SELECT  
        		f.customer_unique_id,
        		SUM(cor.revenue) AS total_revenue,
        		MAX(cor.order_purchase_timestamp) AS last_purchase_date,
        		COUNT(DISTINCT cor.order_id) AS order_counts,
        		f.late_days,
        		EXTRACT(DAY FROM (MAX(CASE WHEN cor.order_rank = 2 THEN cor.order_purchase_timestamp END) - f.first_date)) AS repurchase_interval
        		
        FROM first_late_order f
        JOIN customer_orders_ranked cor ON f.customer_unique_id = cor.customer_unique_id
        GROUP BY f.customer_unique_id, f.first_date,f.late_days
        HAVING COUNT(cor.order_id) >= 2
        ```
        
- **셀러 지연 리스트**
    - 분석 목적:
        - 배송 지연된 셀러의 지연 일수와 같은 특성을 확인할 수 있는 관리 리스트를 구축
    - 핵심 로직:
        - 전처리: 주문 현황이 'canceled', 'unavailable'일 경우 제외
        - 셀러 발송 지연 정의: 배송사에 전달된 날짜가 셀러의 발송 기한보다 더 클 경우
        - 셀러별 평균 발송 기한 초과일: `CASE WHEN`절을 사용하여 셀러 지연인 경우에 배송사 전달 날짜와 발송 기한을 뺀 후, 평균값을 냄
    - 주요 함수:
        - `CASE WHEN`절을 사용하여 실제 배송사 전달일이 기한 보다 늦어졌을 때 평균 초과일수를 계산
        - `HAVING COUNT` (지연 셀러 판별)
    - 쿼리
        
        ```sql
        WITH seller_processed AS (
            SELECT 
                i.seller_id,
        		SUM(i.price + i.freight_value) AS total_payments_value,
                COUNT(DISTINCT o.order_id) AS total_order,
                COUNT(DISTINCT CASE WHEN o.order_delivered_carrier_date::date > i.shipping_limit_date::date
                                    THEN o.order_id END) AS seller_late_count,
                ROUND(AVG(CASE WHEN o.order_delivered_carrier_date::date > i.shipping_limit_date::date
                               THEN o.order_delivered_carrier_date::date - i.shipping_limit_date::date END), 2) AS avg_excess_days,
                ROUND(COUNT(DISTINCT CASE WHEN o.order_delivered_carrier_date > i.shipping_limit_date
                                          THEN o.order_id END)::numeric / NULLIF(COUNT(DISTINCT o.order_id), 0) ,4) AS seller_late_rate	  
            FROM orders o
            INNER JOIN items i ON o.order_id = i.order_id
            WHERE o.order_status NOT IN ('canceled', 'unavailable')
              AND o.order_delivered_carrier_date IS NOT NULL
            GROUP BY 1
            HAVING COUNT(DISTINCT CASE WHEN o.order_delivered_carrier_date::date > i.shipping_limit_date::date
                                       THEN o.order_id END) > 0 
        )
        SELECT *
        FROM seller_processed
        order by seller_late_rate DESC, avg_excess_days DESC
        ```
        

---

## 5. 데이터 시각화 (Tableau)

* [🔗배송 지연 모니터링 대시보드](https://public.tableau.com/app/profile/.62143999/viz/_17659544557070/2_1)

- **[상단] 배송 지연 현황 모니터링**
    - **내용:** 전체/월별 지연율 KPI 및 증감률 추적
    - **액션플랜:**
        - **시즈널 선제 대응:** 지연 급증이 예상되는 특정 시즌(11월, 2~3월) 직전, 셀러들에게 '사전 발송 가이드'를 배포하고 시스템상 배송 예정일을 자동으로 조정하여 고객 기대치 관리
        - **장기 지연 집중 케어:** 리뷰 점수 임계점인 6일을 넘기기 전, **D+5에** 셀러에 자동 알림 발송

<img width="801" height="360" alt="image" src="https://github.com/user-attachments/assets/7e8b5169-eb88-42ea-97c2-58a22d3020cb" />


- **[중단] 지연율이 높은 노선**
    - **내용:** 지연 비중이 가장 높은 상위 5개 노선 식별
    - **액션 플랜:** 
        - **물류 파트너사 협상 및 교체:** 고위험 노선에 대해 배송 품질 개선 요구 및 지속 시 교체 검토
        - **운영 리소스 효율화:** 지연 건수가 많으면서 지연율도 높은 병목 노선을 우선적으로 물류 관리 리소스 집중 투입

<img width="793" height="209" alt="image" src="https://github.com/user-attachments/assets/76ccd203-6a1c-4275-be28-d08d71ed643e" />


- **[하단] 상세 리스트 모니터링**
    - **내용:** 상세 필터(지연일, 구매액)를 통한 4대 핵심 타겟군 추출
    - **액션 플랜:**
        - **고위험 셀러 관리:** 지연 등급 상위 10% 셀러 대상 판매 제한 및 광고 중단
        - **고객 안심 프로세스:** 6일 임계점 초과 시 예상 초과일 안내 및 유사 상품 추천 (안전 이별 전략)
        - **VIP/충성고객 리텐션:** 고액 구매자 및 지연 후 재구매 고객 대상 1:1 케어 및 전용 감사 사은품 제공
        - **이탈 고객 회복:** 지연 후 미구매 고객 대상 **24시간 내 발송 보장 상품** 큐레이션 및 보상 쿠폰

<img width="797" height="281" alt="image" src="https://github.com/user-attachments/assets/59734c37-e1e5-454b-9047-d8fbfd051e7e" />


## 6. 기대 효과

1. **운영 비용 최적화:** 전체 노선의 5%에서 전체 지연의 80%가 발생하는 병목 지점을 식별하여, 개선 리소스를 우선적으로 투입합니다.
2. **셀러 발송 준수 유도:** 예상 초과일수 안내 및 패널티 체계를 통해 자발적으로 발송 기한을 준수하도록 유도하여 셀러 지연을 낮춥니다.
3. **리뷰 점수 방어:** 만족도 임계점인 '6일' 이내에 배송을 완료하거나 선제적인 보상을 제공함으로써, **리뷰 점수 급락(2.02점 하락)을 방어**하고 플랫폼의 대외적 신뢰도를 유지합니다.
<br>


### 📝 KPT 회고
```text
Keep

- 임계점 도출한 점: 배송 지연의 영향을 확인할 때 리뷰 점수의 차이를 확인한 후, 리뷰 점수가 1점대로 떨어지는 6일이라는 임계점을 확인하고 모니터링 지표로 정의내린 점
- 운영 지표를 단계별로 설계한 점: 셀러 지연의 영향을 확인한 후, 셀러 발송 지연율을 지표로 설정하고, 각 셀러별 지연 등급을 부여하여 셀러 관리 리스트를 구축

Problem

- 초기 분석 시, 초 단위 초과로도 지연이 정의됨 → 분석 목적에 맞춰 다시 일 단위로 지연 정의 재설정함
- 테이블 간 N:N 관계를 충분히 파악하지 못한 채 병합하여 중복 데이터 발생

Try

- 본격적인 분석 전, 사전 검증으로 논리적인 결함을 조기에 검증하는 단계를 필수적으로 포함할 것.
- 데이터 구조도를 분석 시작 전에 직접 그려보며, 각 테이블 간 연결 형태를 이해하는 습관을 기를 것.
