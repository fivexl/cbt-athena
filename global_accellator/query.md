# Number of request between specific date and today
```sql 
SELECT clientip, COUNT(*) as num 
FROM "default"."aga_flow_logs"
WHERE "day" BETWEEN '2023/10/06' AND date_format(current_date, '%Y/%m/%d')
GROUP BY clientip
ORDER BY num DESC
```