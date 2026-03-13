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

<br>

### 3) 문제 정의

배송 지연은 고객 경험을 저해하는 핵심 요인입니다.  
이에 따라 고객 경험에 미치는 **영향을 정량화** 하고, 배송 지연이 발생하는 **주요 원인을 분석**하여 운영 관점에서 **개선 가능한 지점을 도출**하고자 합니다.

<br>

## 2. 데이터 소개

- **활용 데이터:**[Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)
- 2016년부터 2018년까지 **브라질** **Olist 이커머스 플랫폼의 약 10만 건의 실제 주문 정보**를 담은 **공공 데이터셋**으로, 고객, 주문, 리뷰 등 **8개의 관계형 테이블**로 구성되어 있습니다.
- **선정 이유:** 고유 식별자(ID)를 기준으로 **병합**하여, 지연이 발생한 구체적인 노선과 원인 등을 **입체적으로 분석**할 수 있습니다.
<br>

## 3. 분석 과정
* [🔗전처리 및 EDA 코드](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/1.%20Data%20preparation%20%2B%20EDA.ipynb)
* [🔗가설 검증 코드](https://github.com/eunsung20311-dot/Olist-E-commerce-Analysis/blob/main/2.Hypothesis_Testing.ipynb)
* [🔗가설 검증 정리 노션](https://plaid-paradox-1a3.notion.site/Olist-2cad569cd2e180fb926cd3c2edc3f423?source=copy_link)

### 1) 데이터 품질 관리

- **데이터 전처리: 데이터 품질**을 진단하고, **컬럼 내 관계 분석**을 하는 **자동화 전처리 함수**를 설계하여 반복적인 전처리를 수행하였습니다.
- **열 생성:** 약속된 배송일과 실제 배송일 격차를 의미하는  `delivery_efficiency`열과, 지연 여부를 확인하는 `is_delayed`열을 생성했습니다.
<br>

### 2) 분석 요약
**(1) 배송 지연 현황** 

- **최근 악화 추세:** 이번 달 배송 지연율은 6.04%로 전월 대비 82.6% 급증하며 불안정한 양상을 보이고 있습니다.
- **장기 지연 구조:** 지연 주문 중 절반 이상인 **57.61%**가 6일 이상 지연이며**,** 21일 이상의 극단적 지연 사례도 12.17%에 달합니다.
- **긍정적 신호:** 전체 지연율은 상승했으나, 가장 치명적인 6일 이상 지연율은 **전월 대비 47.6% 감소**하며 개선되는 추세를 보이고 있습니다.
<img width="1178" height="477" alt="image" src="https://github.com/user-attachments/assets/a57d1d3d-a921-4a43-bee8-a031376f230b" />
<img width="937" height="391" alt="image" src="https://github.com/user-attachments/assets/106fd090-0cbb-4ee5-923d-52bb9cc79cfc" />


<br>

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


<br>

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

<br>

**(4) 핵심 지표(KPI) 정의** 

분석에 기반하여, 실시간 모니터링을 위해 아래의 세가지 지표를 핵심 KPI로 정의내렸습니다.
| **KPI** | **정의** | 설명 |
| --- | --- | --- |
| **배송 지연율** | 실제 도착일 > 예상 도착일 | 배송 품질의 진단 지표 |
| **셀러 발송 지연율** | 택배사 전달일 > 셀러 발송 기한 | 셀러 관리 지표 |
| **6일 이상 지연율** | 예상 도착일 + 6일 이상 | 리뷰 스코어 관리 지표 |

<br>

## 4. 인사이트
### 1) 배송 지연은 일부 병목 노선에 집중되는 구조입니다.
- 전체 지연의 80%가 상위 5% 노선에서 발생
- 배송 거리가 증가할수록 지연율 상승

### 2) 셀러 발송 지연은 배송 지연 위험을 크게 높이는 운영 리스크 요인입니다.
- 셀러 발송 지연 시 배송 지연 확률 약 4배 증가
- 평균 배송 기간 7.7일 증가

### 3) 배송 지연 6일 이후 고객 만족도가 1점대에 고착되는 구간이 확인됩니다.
- 6일 이후 리뷰 점수 1점대 진입
- 고객 만족도 회복 어려움

<br>

## 5. 실행 전략
### 1)  병목 노선 최적화

- **실행 전략:** 거리 기반 리스크 완화
  - **근거리 매칭 알고리즘:** 동일 권역 내 셀러 상품을 우선 노출하는 알고리즘으로, 물리적 운송 거리를 최소화 합니다.
  - **풀필먼트 거점 구축:** 지연이 집중되는 노선에 재고를 선배치하여,  거리 기반 리스크를 줄입니다.
- **기대효과:**
  - 2,000km 이상의 장거리 노선(주문 6,423건, 지연율 11.5%)을 풀필먼트 거점을 통해 500km 이내 구간(지연율 5.9%) 수준으로 개선할 경우 **전체 배송 지연 건수의 약 5.5%가 개선됩니다.**
    - (계산식: 지연 개선 건수 360건 / 현재 전체 지연 건수 6,535건)
  - 장거리 운송 비중 감소를 통해 **배송의 변동성을 줄이고 플랫폼의 전반적인 배송 안정성을 개선할 수 있습니다.**  

### 2)  셀러 발송지연

- **실행 전략:** 셀러 배송 품질 관리 시스템 도입
    - **지연 페널티:** 상습 지연 셀러에게는 검색 가중치를 하향 및 상세페이지에 평균 발송 소요 시간을 자동 노출하여 고객의 기대를 조정합니다.
    - **지연 리포트:** 주간 지연율 상위 10% 셀러를 선별하여, 기회비용 분석 리포트와 발송 프로세스 최적화 가이드를 제공하여 자발적 개선을 유도합니다.
- **기대효과:** 
  - **검색 노출 가중치 조정**을 통해 자연스럽게 배송 품질이 높은 셀러의 주문 비중이 증가합니다.
  - 주간 지연 리포트를 통해 셀러가 자신의 배송 성과를 인지하게 되며 **발송 리드타임 단축을 유도할 수 있습니다.**

### 3)  지연 6일차 임계점

- **실행 전략:** 고객 경험 방어 전략
    - **지연 알림:** 2일/ 4일차에 지연 예상일을 실시간으로 발송하여 정보 비대칭으로 인한 고객 불안을 해소합니다.
    - **선제적 보상:** 5일차에 사과 메시지와 재구매 할인 쿠폰을 발송하여 부정적 감정을 완화합니다.
    - **리뷰 UX 개선 :** 6일차 이상 지연 건의 리뷰 작성 화면에 1:1 문의 팝업을 고정 노출합니다.
- **기대효과**
    - 지연 정보를 사전에 안내함으로써 **정보 비대칭으로 인한 고객 불안을 완화**할 수 있습니다.
    - 리뷰 작성 이전에 1:1 문의로 유도하여 **공개 리뷰 페이지에 부정 리뷰가 확산되는 것을 예방**할 수 있습니다.

<br>

## 6. 대시보드 모니터링
* [🔗배송 지연 모니터링 대시보드](https://public.tableau.com/app/profile/.62143999/viz/_17659544557070/2_1)
* Tableau 대시보드로 배송지연 현황을 시각화하여, 실시간 배송 관제 시스템으로 활용합니다. 

### 1) 대시보드 내용
- **[상단] 배송 지연 현황 모니터링**
    - **내용:** 전체/월별 지연율 KPI 및 증감률 추적
<img width="1200" height="530" alt="image" src="https://github.com/user-attachments/assets/62ac2e77-16fb-4997-a246-dea6462812a7" />

- **[중단] 지연율이 높은 노선**
    - **내용:**  출발-도착지별 지연율 상위 5개 노선의 지연율과 지연 건수 산출

<img width="793" height="209" alt="image" src="https://github.com/user-attachments/assets/76ccd203-6a1c-4275-be28-d08d71ed643e" />


- **[하단] 상세 리스트 모니터링**
    - **내용:** 상세 필터(지연 일수, 구매 금액)를 활용한 지연 후 재구매/미구매 고객, 현재 지연 고객 및 셀러 리스트 추출 

<img width="797" height="281" alt="image" src="https://github.com/user-attachments/assets/59734c37-e1e5-454b-9047-d8fbfd051e7e" />


### 2) 기대 효과
- 배송 지연 KPI와 증감률을 실시간으로 모니터링하여 **배송 품질 악화 여부를 빠르게 인지할 수 있습니다.**
- 지연율이 높은 노선을 즉시 파악하여 **지연이 발생하는 물류 구간을 신속하게 진단하고, 개선 우선순위를 설정할 수 있습니다.**
- 현재 지연 고객 및 셀러 리스트를 확인하여 **CS 대응, 셀러 관리 등 운영팀의 즉각적인 대응이 가능해집니다.**
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
