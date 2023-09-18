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
