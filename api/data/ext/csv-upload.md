# CSV File Upload

## Description

Upload CSV file for parsing into series, properties, or messages with the specified CSV parser.

## Request

### Path 

```elm
/api/v1/csv
```

### Method

```
POST 
```

### Headers

|**Header**|**Value**|
|:---|:---|
| Content-Type | `text/csv` |

### Parameters

| **Field** | **Type** | **Description** |
|:---|:---|:---|
| config   | string   | [**Required**] Parser Name as configured on **Configuration:Parsers CSV** page.|
| encoding      | string   | File encoding. Default: UTF-8.|
| default-entity| string | Default entity name applied to commands contained in the file. |
| metric-prefix | string | Metric prefix prepended to metric names extracted from the file. |
| wait | boolean | Wait until processing of the file into commands is completed by the server. Default: false.<br>If wait is disabled, the file is processed by the server asynchronously. | 
| rules | boolean | Process commands in the rule engine. Default: false | 
| time | string | Date in ISO 8601 format, applied to commands if the file doesn't contain any time columns. | 
| timezone | string | Timezone applied to timestamps specified in local time. | 
| test | boolean | Parse and validate the file without actually processing and storing commands. Default: false | 

### Payload

* Payload is CSV file attached as plain text, containing an optional header line and one or multiple data rows.
* Names (metric name, property type, key names, tag names) containing non-printable characters will be normalized by replacing them with underscore. <br>Multiple underscores are collapsed into one underscore.

## Response 

### Fields

None.

### Errors

## Example 

### Request 

#### URI

```elm
POST https://atsd_host:8443/api/v1/csv?config=my-parser&default-entity=nurswgvml007
```

#### Payload

```ls
time,cpu_user,cpu_system,waitio
1423139581000,12.4,1.4,0
1423139592016,16.0,4.2,0
```

### Response

None.

## Additional Examples

