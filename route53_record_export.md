## Export Record using AWS CLI - AWS Configure
- Export
```
aws route53 list-resource-record-sets --hosted-zone-id <your-hosted-zone-id> --max-items 1000 > route53-records.json
```
- import
```
aws route53 change-resource-record-sets --hosted-zone-id <your-hosted-zone-id> --change-batch file://route53-records.json
```

## Corrected JSON for route53-records.json:
```
{
  "Comment": "Import DNS records for example.com",
  "Changes": [
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "example.com.",
        "Type": "NS",
        "TTL": 172800,
        "ResourceRecords": [
          {
            "Value": "ns-747.awsdns-29.net."
          },
          {
            "Value": "ns-150.awsdns-18.com."
          },
          {
            "Value": "ns-1033.awsdns-01.org."
          },
          {
            "Value": "ns-1885.awsdns-43.co.uk."
          }
        ]
      }
    },
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "example.com.",
        "Type": "SOA",
        "TTL": 900,
        "ResourceRecords": [
          {
            "Value": "ns-747.awsdns-29.net. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400"
          }
        ]
      }
    },
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "www.example.com.",
        "Type": "A",
        "TTL": 60,
        "ResourceRecords": [
          {
            "Value": "197.52.54.5"
          }
        ]
      }
    },
    {
      "Action": "UPSERT",
      "ResourceRecordSet": {
        "Name": "yet.example.com.",
        "Type": "CNAME",
        "TTL": 60,
        "ResourceRecords": [
          {
            "Value": "174.2.5.5"
          }
        ]
      }
    }
  ]
}
```
