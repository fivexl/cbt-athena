# Parquet + Partion Projection

Replace `<bucket name>` and `<account id>`'s. Also consider adjusting `projection.timestamp.range`. The rest should work as it is

```
CREATE EXTERNAL TABLE IF NOT EXISTS `vpc_flow_logs` (
  `version` int, 
  `account_id` string, 
  `interface_id` string, 
  `srcaddr` string, 
  `dstaddr` string, 
  `srcport` int, 
  `dstport` int,
  `protocol` bigint, 
  `packets` bigint, 
  `bytes` bigint, 
  `start` bigint, 
  `end` bigint, 
  `action` string, 
  `log_status` string, 
  `vpc_id` string, 
  `subnet_id` string, 
  `instance_id` string, 
  `tcp_flags` int, 
  `type` string, 
  `pkt_srcaddr` string, 
  `pkt_dstaddr` string, 
  `region` string, 
  `az_id` string, 
  `sublocation_type` string, 
  `sublocation_id` string, 
  `pkt_src_aws_service` string, 
  `pkt_dst_aws_service` string, 
  `flow_direction` string, 
  `traffic_path` int 
)
PARTITIONED BY (`timestamp` string, `region_name` string, `account` string)
ROW FORMAT SERDE 
  'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
STORED AS INPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION 's3://<bucket name>/AWSLogs/'
TBLPROPERTIES (
'skip.header.line.count'='1',
'projection.enabled'='true',
'projection.timestamp.format'='yyyy/MM/dd', 
'projection.timestamp.interval'='1',
'projection.timestamp.interval.unit'='DAYS',
'projection.timestamp.range'='2021/09/01,NOW',	
'projection.timestamp.type'='date',
'projection.region_name.type'='enum',
'projection.region_name.values'='us-east-2,us-east-1,us-west-1,us-west-2,af-south-1,ap-east-1,ap-south-1,ap-northeast-3,ap-northeast-2,ap-southeast-1,ap-southeast-2,ap-northeast-1,ca-central-1,cn-north-1,cn-northwest-1,eu-central-1,eu-west-1,eu-west-2,eu-south-1,eu-west-3,eu-north-1,me-south-1,sa-east-1',
'projection.account.type'='enum',
'projection.account.values'='<account_id>,<account_id>',
'storage.location.template'='s3://<bucket name>/AWSLogs/${account}/vpcflowlogs/${region_name}/${timestamp}'
);
```