## OLIST 배송 개선 프로젝트

## 1. 프로젝트 개요
### 1) Skills & Workflow

| Category | Tools |
| :--- | :--- |
| **Language** | Python (Pandas, Numpy, Matplotlib) |
| **Database** | PostgreSQL (PgAdmin) |
| **Visualization** | Tableau Public |
| **Workflow** | **Python** (Pre-processing) ➡️ **PostgreSQL** (ETL) ➡️ **Tableau** (Visualization) |
### 2) 분석 배경

OIlist의 배송 기간 표준편차가 10일에 달하고 최대 188일의 극단적 지연 사례가 발견되는 등 **배송 서비스의 불확실성이 큽니다**. 그 중 배송 지연은 전체 주문 중 **약 6.57%(6,535건)를 차지**합니다. 

이에 따라 **현재 배송 문제를 세분화**하고, 배송 지연이 고객 경험에 미치는 **영향을 정량화**하여 **운영 개선안**을 도출하고자 합니다.

### 3) 분석 목표

- OLIST에서 배송 지연 문제를 정의하고, 영향을 정량화하고 해결할 수 있는 개선안과 대시보드를 제작한다

### 4) 활용 데이터

* **Dataset:**[Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
* 2016년부터 2018년까지 브라질 전역에서 발생한 약 10만 건의 실제 주문 정보를 담은 공공 데이터셋입니다.
* 고객, 주문, 배송, 리뷰, 제품 등 총 8개의 관계형 테이블로 구성되어 있습니다.
  
## 2. 결론

### 1) 배송 지연의 영향

- **정량적 손실 확인:** 배송 지연은 리뷰 평균 점수를 2.02점 낮추고 재구매율은 대조군 대비 17.1% 감소합니다.
- **고객 만족의 임계점:**
    - 지연 발생 후 6일을 기점으로 리뷰 점수가 1점대로 진입하여 이후 회복이 불가능한 상태에 도달합니다. 이에 따라 ‘6일 이상 지연율’을 지표로 정의했습니다.

### 2) 배송 문제 세분화

- **셀러 발송 기한 준수의 중요성:**
    - 셀러의 발송 지연 시 최종 배송 지연 확률이 약 4배 급증하고, 배송 기간은 평균 7.7일 추가 소요됩니다.
        - 이에 따라 ‘셀러의 발송 지연율’을 지표로 설정하고, 지연 등급을 부여하여 고위험 셀러를 관리하는 리스트를 만들었습니다.
- **지연율이 높은 노선:**
    - 지연율 상위 5%의 노선이(20개) 전체 지연의 약 80%의 비중을 가집니다.
        - 지연 비중이 높은 핵심 노선 타겟팅이 필요하여 ‘상위 노선 리스트’를 시각화 하였습니다.

### 3) 대시보드를 활용한 운영 최적화

- **전략적 모니터링**
    - **배송 지연 KPI 및 시계열 추이:** 월별 지연율 증감 및 전월 대비 성과를 추적하여 배송 품질의 개선/악화 여부를 즉각 판단할 수 있습니다.
    - **6일 임계점 관리:** 리뷰 점수 임계점인 '6일 이상 지연율'을 집중 모니터링하여 고객 만족도 방어선을 구축할 수 있습니다.
- **배송 최적화**
    - **핵심 지연 노선 식별:** 전체 지연의 80%가 발생하는 상위 노선을 우선 순위로 선정하여 예상 도착일 조정 및 물류 파트너사 협의를 진행합니다.
- **타겟 고객 및 셀러 관리**
    - **고위험 셀러 관리 리스트:** 판매 규모와 지연율을 반영하여 지연 등급을 부여하고 고객에게 예상 초과일수를 안내하고, 패널티 부여의 근거로 활용합니다.
    - **이탈 방지 마케팅 및 운영 타겟:** 현재 지연 고객, 지연 후 재구매/미구매 고객 리스트를 추출하여 사과 쿠폰 발송 등 리텐션 캠페인 수행합니다.

## 3. 탐색적 데이터 분석 및 가설 검증 (Python)

* [🔗전처리 및 EDA 코드](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/1.%20Data%20preparation%20%2B%20EDA.ipynb)
* [🔗가설 검증 코드](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/2.Hypothesis_Testing.ipynb)
* [🔗가설 검증 정리 노션](https://plaid-paradox-1a3.notion.site/Olist-2cad569cd2e180fb926cd3c2edc3f423?source=copy_link)

### 1) 주요 전처리

- 데이터 품질을 검증
    - 결측치 확인: 각 컬럼의 결측치 비중을 파악하여 **데이터의 완전성**을 검증
    - 중복값 확인: 중복 확인을 통해 데이터의 성격과 관계를 파악
    - 유효성 확인: 기초 통계량과 분위수 분석을 통해 **이상치를 식별**
- 주문/배송 관련 날짜 계산을 위해 `datetime` 날짜형 타입 변경
- 약속 대비 실제 배송일의 격차인 `delivery_efficiency` 열, 지연 여부인 `is_delayed`열 생성

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
                                          THEN o.order_id END)::numeric / NULLIF(COUNT(DISTINCT o.order_id), 0) ,4) AS seller_late_rate,
                o.order_purchase_timestamp 					  
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
    - 내용: 전체/월별 지연율 KPI 및 증감률 모니터링
    - 인사이트:
        - KPI와 증감률을 통해 배송 품질의 개선/악화 여부를 즉각적으로 판단할 수 있음
        - 지연 일수 분포를 파악하며, 장기 지연의 발생 추이를 확인하고 개선의 우선순위를 설정할 수 있음

<img width="801" height="360" alt="image" src="https://github.com/user-attachments/assets/7e8b5169-eb88-42ea-97c2-58a22d3020cb" />


- **[중단] 지연율이 높은 노선 분석**
    - 내용: 출발-도착지별 지연율 상위 5개 노선의 지연율과 지연 건수 산출
    - 인사이트: 배송 품질이 저하된 노선을 타겟팅하여 개선 우선순위 도출

<img width="793" height="209" alt="image" src="https://github.com/user-attachments/assets/76ccd203-6a1c-4275-be28-d08d71ed643e" />


- **[하단] 상세 리스트 모니터링**
    - 내용: 상세 필터(지연 일수, 구매 금액)를 활용한 지연 후 재구매/미구매 고객, 현재 지연 고객 및 셀러 리스트 추출
    - 인사이트:
        - 고객 경험 케어: 지연 경험 고객을 식별하여 리텐션 유도를 위한 프로모션이나 지연 사과 메시지 발송 등 CS 및 마케팅 대상자로 활용 가능
        - 셀러 운영 최적화: 지연율과 판매 건수에 따라 지연 등급을 세분화하여 관리를 하고, 고객에게 예상 초과일수를 안내하여 정보 불확실성 해소
        - 데이터 정교화: 상세 필터를 활용하여 구매 금액과 지연 일수에 따라 더 정교화된 타겟을 산출할 수 있음

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
