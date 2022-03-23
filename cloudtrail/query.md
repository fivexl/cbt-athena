# Get access errors for a certain role

```
SELECT * FROM cloudtrail_logs_pp
WHERE 
    useridentity.arn LIKE '%some_role_name%' AND
    account_id LIKE 'some_account_id' AND
    errorcode LIKE 'AccessDenied'
LIMIT 100
```

# Find an event on a certain date with certain parameters

```
SELECT * FROM cloudtrail_logs_pp 
WHERE
    timestamp LIKE '2022/03/22' AND
    eventname LIKE 'StartQueryExecution' AND
    requestparameters LIKE '%CREATE TABLE%'
LIMIT 1000
```

# Get number of AWS API call for a role per hour

```
SELECT date_trunc('HOUR', from_iso8601_timestamp(eventtime)), COUNT(*) AS count FROM cloudtrail_logs_pp
WHERE 
    useridentity.arn LIKE '%andrey%'
GROUP BY date_trunc('HOUR', from_iso8601_timestamp(eventtime))
ORDER BY date_trunc('HOUR', from_iso8601_timestamp(eventtime)) DESC
LIMIT 25
```