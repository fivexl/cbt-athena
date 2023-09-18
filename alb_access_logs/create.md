# Parquet + Partion Projection

- Notes: Replace `<bucket name>` and `<account id>`'s. Also consider adjusting `projection.timestamp.range`. The rest should work as it is
- Documentation: https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html 

```sql
CREATE EXTERNAL TABLE `alb_logs`(
  `type` string COMMENT '', 
  `time` string COMMENT '', 
  `elb` string COMMENT '', 
  `client_ip` string COMMENT '', 
  `client_port` int COMMENT '', 
  `target_ip` string COMMENT '', 
  `target_port` int COMMENT '', 
  `request_processing_time` double COMMENT '', 
  `target_processing_time` double COMMENT '', 
  `response_processing_time` double COMMENT '', 
  `elb_status_code` int COMMENT '', 
  `target_status_code` string COMMENT '', 
  `received_bytes` bigint COMMENT '', 
  `sent_bytes` bigint COMMENT '', 
  `request_verb` string COMMENT '', 
  `request_url` string COMMENT '', 
  `request_proto` string COMMENT '', 
  `user_agent` string COMMENT '', 
  `ssl_cipher` string COMMENT '', 
  `ssl_protocol` string COMMENT '', 
  `target_group_arn` string COMMENT '', 
  `trace_id` string COMMENT '', 
  `domain_name` string COMMENT '', 
  `chosen_cert_arn` string COMMENT '', 
  `matched_rule_priority` string COMMENT '', 
  `request_creation_time` string COMMENT '', 
  `actions_executed` string COMMENT '', 
  `redirect_url` string COMMENT '', 
  `lambda_error_reason` string COMMENT '', 
  `target_port_list` string COMMENT '', 
  `target_status_code_list` string COMMENT '', 
  `classification` string COMMENT '', 
  `classification_reason` string COMMENT '')
PARTITIONED BY ( 
  `day` string)
ROW FORMAT SERDE 
  'org.apache.hadoop.hive.serde2.RegexSerDe' 
WITH SERDEPROPERTIES ( 
  'input.regex'='([^ ]*) ([^ ]*) ([^ ]*) ([^ ]*):([0-9]*) ([^ ]*)[:-]([0-9]*) ([-.0-9]*) ([-.0-9]*) ([-.0-9]*) (|[-0-9]*) (-|[-0-9]*) ([-0-9]*) ([-0-9]*) \"([^ ]*) (.*) (- |[^ ]*)\" \"([^\"]*)\" ([A-Z0-9-_]+) ([A-Za-z0-9.-]*) ([^ ]*) \"([^\"]*)\" \"([^\"]*)\" \"([^\"]*)\" ([-.0-9]*) ([^ ]*) \"([^\"]*)\" \"([^\"]*)\" \"([^ ]*)\" \"([^s]+?)\" \"([^s]+)\" \"([^ ]*)\" \"([^ ]*)\"') 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  's3://<bucket name>/AWSLogs/<account id>/elasticloadbalancing/eu-centra-1'
TBLPROPERTIES (
  'projection.day.format'='yyyy/MM/dd', 
  'projection.day.interval'='1', 
  'projection.day.interval.unit'='DAYS', 
  'projection.day.range'='2022/01/01,NOW', 
  'projection.day.type'='date', 
  'projection.enabled'='true', 
  'storage.location.template'='s3://<bucket name>/AWSLogs/<account id>/elasticloadbalancing/eu-central-1/${day}', 
  'transient_lastDdlTime'='1657306215')
```
