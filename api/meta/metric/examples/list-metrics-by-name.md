# List Metrics by Name

List all metrics whose name includes `disk`

## Request

### URI

```elm
GET /api/v1/metrics?expression=name%20LIKE%20%27*disk*%27
```

### Expression

```javascript
expression=name LIKE '*disk*'
```

## Response

```json
[
    {
        "name": "aws_ec2.diskreadbytes.average",
        "enabled": true,
        "dataType": "FLOAT",
        "persistent": true,
        "tags": {},
        "retentionDays": 0,
        "invalidAction": "NONE",
        "lastInsertDate": "2016-05-18T00:35:12.000Z",
        "versioned": false
    },
    {
        "name": "aws_ec2.diskreadbytes.maximum",
        "enabled": true,
        "dataType": "FLOAT",
        "persistent": true,
        "tags": {},
        "retentionDays": 0,
        "invalidAction": "NONE",
        "lastInsertDate": "2016-05-18T00:35:12.000Z",
        "versioned": false
    },
    {
        "name": "aws_ec2.diskreadbytes.minimum",
        "enabled": true,
        "dataType": "FLOAT",
        "persistent": true,
        "tags": {},
        "retentionDays": 0,
        "invalidAction": "NONE",
        "lastInsertDate": "2016-05-18T00:35:12.000Z",
        "versioned": false
    }
]
```
