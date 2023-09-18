# Requests in specific timeslot
```sql
SELECT  request_url, "client_ip", COUNT(client_ip) as number FROM "default"."alb_logs" 
where day='2023/08/22'
and "request_url" like  '%graph.firi.com%'
-- and "request_url" like '%TransferWithdraw%'
and "elb_status_code" = 200
AND time BETWEEN '2023-08-22T09:40:00Z' AND '2023-08-22T09:50:00Z'
GROUP BY  request_url, "client_ip"
ORDER BY number DESC
LIMIT 30
```

# Create table with smaller set of data
```sql
CREATE TABLE "alb_logs_20230822" AS 
SELECT time, client_ip, user_agent, elb_status_code, sent_bytes,  target_processing_time, actions_executed
FROM "default"."alb_logs" 
where day='2023/08/22'
and "request_url" like  '%api.example.com%'
and "request_url" like '%login%'
and ("target_processing_time" > 5 or "target_processing_time" = -1)
and "elb_status_code" = 200
AND time BETWEEN '2023-08-22T15:20:00Z' AND '2023-08-22T15:50:00Z'
```
