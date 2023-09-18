
# get all blocked requests by httprequest.uri and show terminatingrule.ruleid
```sql
WITH logs_data AS 
  (SELECT * FROM waf_logs 
    CROSS JOIN UNNEST(rulegrouplist) AS t(allrulegroups))
SELECT 
count(*) as count,
terminatingruleid, 
"allrulegroups".terminatingrule.ruleid,
httprequest.clientip, 
--httprequest.country, 
httprequest.uri 
--httprequest.headers
FROM logs_data
WHERE "date" like '2023/%/%'
and httprequest.uri like '%api%'
GROUP BY terminatingruleid,"allrulegroups".terminatingrule.ruleid,httprequest.clientip,httprequest.uri 
ORDER BY count desc

```