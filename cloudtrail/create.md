
# Manually create table

Replace bucket_name and org_id (assuming you are using AWS Organisations which you defenetely should) and you should be good to go

```
CREATE EXTERNAL TABLE cloudtrail_logs_pp(
    eventVersion STRING,
    userIdentity STRUCT<
        type: STRING,
        principalId: STRING,
        arn: STRING,
        accountId: STRING,
        invokedBy: STRING,
        accessKeyId: STRING,
        userName: STRING,
        sessionContext: STRUCT<
            attributes: STRUCT<
                mfaAuthenticated: STRING,
                creationDate: STRING>,
            sessionIssuer: STRUCT<
                type: STRING,
                principalId: STRING,
                arn: STRING,
                accountId: STRING,
                userName: STRING>>>,
    eventTime STRING,
    eventSource STRING,
    eventName STRING,
    awsRegion STRING,
    sourceIpAddress STRING,
    userAgent STRING,
    errorCode STRING,
    errorMessage STRING,
    requestParameters STRING,
    responseElements STRING,
    additionalEventData STRING,
    requestId STRING,
    eventId STRING,
    readOnly STRING,
    resources ARRAY<STRUCT<
        arn: STRING,
        accountId: STRING,
        type: STRING>>,
    eventType STRING,
    apiVersion STRING,
    recipientAccountId STRING,
    serviceEventDetails STRING,
    sharedEventID STRING,
    vpcEndpointId STRING
  )
PARTITIONED BY (timestamp string, account_id string, region string)
ROW FORMAT SERDE 'com.amazon.emr.hive.serde.CloudTrailSerde'
STORED AS INPUTFORMAT 'com.amazon.emr.cloudtrail.CloudTrailInputFormat'
OUTPUTFORMAT 'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION 's3://bucket_name/AWSLogs/org_id/'
TBLPROPERTIES (
  'projection.enabled'='true', 
  'projection.timestamp.format'='yyyy/MM/dd', 
  'projection.timestamp.interval'='1',
  'projection.timestamp.interval.unit'='DAYS', 
  'projection.timestamp.range'='2021/09/01,NOW',	
  'projection.timestamp.type'='date',
  'projection.account_id.type' = 'enum',
  'projection.account_id.values' = 'XXXXXXX,YYYYYY,ZZZZZZZ',
  'projection.region.type' = 'enum',
  'projection.region.values' = 'us-east-2,us-east-1,us-west-1,us-west-2,af-south-1,ap-east-1,ap-south-1,ap-northeast-3,ap-northeast-2,ap-southeast-1,ap-southeast-2,ap-northeast-1,ca-central-1,cn-north-1,cn-northwest-1,eu-central-1,eu-west-1,eu-west-2,eu-south-1,eu-west-3,eu-north-1,me-south-1,sa-east-1',
  'storage.location.template'='s3://bucket_name/AWSLogs/org_id/${account_id}/CloudTrail/${region}/${timestamp}/')
  
```