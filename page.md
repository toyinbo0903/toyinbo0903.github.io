---
title: Technical Writing Assessment
date: 2017-01-04T15:04:10.000Z
description: >-
  This assessment is to create a technical document in markdown and also share the rendered output. This technical documentation should explain how to calculate the Key Performance Indicator (KPI) Net Promotor Score using SQL
---

# Metric: Net Promoter Score

| METRIC                      | DETAILS                        |
|-----------------------------|--------------------------------|
| Metric Title                | Net Promoter Score (NPS)       |
| Metric Short Description    |                                |
| Metric Long Description     |                                |
| Metric SME                  | Jasson Ji                      |
| Product Owner               | Uddipan                        |
| Documentation Status        | IN PROGRESS                    |

Please use this template as a starting point to capture necessary descriptions as we work 
towards creating a process/tool for managing such information. Please replace **blue text** with 
content related to the metric.

**Metric Granularity:**

Qualtrics_ID

**Segments (Dimensions /Filters / Slicers):**
1. Country
2. Customer Size
3. Customer Tenure
4. Geo
5. Industry Group
6. License Compliance (WIP)
7. Named account
8. Named Account Group
9. Customer Tenure
10. Premium (WIP)

**Caveats & Clarifications**
1. Not all Qualtrics_id have an account_csn, hence not all qualtrics_id will have an 
account_name
2. For ADSK_BUSINESS_SCORE the NPS threshold is unusual, 
it's `FY21_ADSK_BUSINESS_SCORE >= 6 THEN 'PROMOTER' WHEN 
FY21_ADSK_BUSINESS_SCORE <= 3 THEN 'DETRACTOR'` for all others when `SCORE <= 
3 then Detractor and SCORE >= 9 then Promoter`
3. Some columns exclusively belong to EBA or NON-EBA, hence field EBA_FLAG is very 
important column to navigate through the data 
4. As of 11/07/2022, About 1850 qualtrics_id have multiple ACCOUNT_CSN mapped to 
them, some of these CSN's have either have multiple ACCOUNT_NAME, INDUSTRY & 
GEO or multiple values in one of these filters, so in order to ensure consistency, any 
qualtrics_id with multiple ACCOUNT_CSN will have ACCOUNT_NAME, INDUSTRY and 
GEO set to 'UNKNOWN'

**Sample Queries:**

**Use Case 1 - To fetch non-EBA Qualtrics IDs per fiscal year**

```sql
SELECT FISCAL_YEAR, 
COUNT(DISTINCT qualtrics_id) as total_qualtrics_id 
FROM "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY"
WHERE EBA_FLAG = 'FALSE'
GROUP BY 1
```
## Use Case 2 - To fetch Qualtrics IDs (EBA and non-EBA) per industry

```sql
SELECT INDUSTRY, 
COUNT(DISTINCT QUALTRICS_ID), 
COUNT(DISTINCT account_csn) 
FROM "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY"
GROUP BY INDUSTRY; 
```
## Use Case 3 - To fetch EBA Qualtrics IDs per GEO

```sql
with count_rows as 
 (select count(distinct qualtrics_id) as qualtrics_count
 from 
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" )
 --where powerbi_geo = ''_)
 ,total_promoters as (
 Select count(distinct qualtrics_id) as promoters_count from
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" where 
score >='9')
 
 ,total_detractors as (
 Select count(distinct qualtrics_id) as detractors_count from
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" where 
score <='6')
select promoters_perc - detractors_perc
from (
 select promoters_count/qualtrics_count as promoters_perc, 
detractors_count/qualtrics_count as detractors_perc 
 from count_rows,total_promoters,total_detractors
)
```
## Use Case 4 - To calculate overall NPS score (EBA and non-EBA)

```sql
with count_rows as 
 (select count(distinct qualtrics_id) as qualtrics_count
 from 
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" )
 ,total_promoters as (
 Select count(distinct qualtrics_id) as promoters_count from
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" where 
score >='9')
 
 ,total_detractors as (
 Select count(distinct qualtrics_id) as detractors_count from
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" where 
score <='6')
select promoters_perc - detractors_perc as overall_nps
from (
 select promoters_count/qualtrics_count as promoters_perc, 
detractors_count/qualtrics_count as detractors_perc 
 from count_rows,total_promoters,total_detractors
)
```

## Use Case 5 - To calculate EBA NPS score

```sql
with count_rows as 
 (select count(distinct qualtrics_id) as qualtrics_count
 from 
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" where EBA_FLAG 
= 'TRUE')
 ,total_promoters as (
 Select count(distinct qualtrics_id) as promoters_count from
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" 
 where score >='9' AND EBA_FLAG = 'TRUE')
 
 ,total_detractors as (
 Select count(distinct qualtrics_id) as detractors_count from
 "EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" 
 where score <='6' AND EBA_FLAG = 'TRUE')
select promoters_perc - detractors_perc as overall_eba_nps
from (
 select promoters_count/qualtrics_count as promoters_perc, 
detractors_count/qualtrics_count as detractors_perc 
 from count_rows,total_promoters,total_detractors
)
```

## Use Case 5 - To connect NPS score table with applicable dimensions

```sql
WITH ACCOUNT_CED AS (
SELECT 
-- site location identifying information
 t_map.account_csn as ACCOUNT_CSN
 ,SITE_TYPE
 ,SITE_COUNTRY_NAME
 ,SITE_GEO
 ,SITE_INDUSTRY_SEGMENT
 ,SITE_INDUSTRY_GROUP 
 ,PARTNER_SITE_TYPE
 ,SITE_SALES_REGION
 ,SITE_TYPE
 ,SITE_NAMED_ACCOUNT_GROUP_STATIC
 ,CORPORATE_CUSTOMER_SIZE_STATIC
from adp_publish.account_optimized.account_edp_optimized account_ced 
inner join 
adp_publish.account_optimized.transactional_csn_mapping_optimized t_map 
 on t_map.site_uuid_csn = account_ced.site_uuid_csn
) 
SELECT SCORE, ACCOUNT_CED.*
"EIO_PUBLISH"."ENGAGEMENT_SHARED"."NPS_EBA_NON_EBA_SURVEY" SURVEY
LEFT JOIN ACCOUNT_CED ON 
SURVEY.ACCOUNT_CSN = ACCOUNT_CED.ACCOUNT_CSN 
```
