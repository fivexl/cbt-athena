# Partion Projection

- Notes: Replace `<bucket name>` and `<account_id>`. Also consider adjusting `projection.timestamp.range`. The rest should work as it is
- GA logs always inside `us-west-2` region (same is for metrics)
- Docs: https://docs.aws.amazon.com/athena/latest/ug/querying-global-accelerator-flow-logs.html#querying-global-accelerator-flow-logs-examples

```sql
CREATE EXTERNAL TABLE IF NOT EXISTS aga_flow_logs (
  version string,
  account string,
  acceleratorid string,
  clientip string,
  clientport int,
  gip string,
  gipport int,
  endpointip string,
  endpointport int,
  protocol string,
  ipaddresstype string,
  numpackets bigint,
  numbytes int,
  starttime int,
  endtime int,
  action string,
  logstatus string,
  agasourceip string,
  agasourceport int,
  endpointregion string,
  agaregion string,
  direction string
)
PARTITIONED BY ( 
  `day` string)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ' '
LOCATION
  's3://<bucket name>/AWSLogs/<account_id>/globalaccelerator/us-west-2'
TBLPROPERTIES (
  'skip.header.line.count'='1',
  'projection.day.format'='yyyy/MM/dd', 
  'projection.day.interval'='1', 
  'projection.day.interval.unit'='DAYS', 
  'projection.day.range'='2023/01/01,NOW', 
  'projection.day.type'='date', 
  'projection.enabled'='true', 
  'storage.location.template'='s3://<bucket name>/AWSLogs/<account_id>/globalaccelerator/us-west-2/${day}'
 )
```