# `series` Command

## Description

Insert a timestamped value (number or text) into the specified time series, uniquely identified by a composite primary key consisting of the entity, metric, and optional `key=value` tags.

## Syntax

```css
series d:{iso-date} e:{entity} t:{tag-1}={val-1} m:{metric-1}={number}
```

The command may include multiple values for different metrics, inheriting the same entity, time, and tags.

```css
series d:{iso-date} e:{entity} t:{tag-1}={val-1} m:{metric-1}={number} m:{metric-2}={number} x:{metric-3}={text}
```

### Fields

| **Field** | **Type** | **Description** |
|:---|:---|:---|
| e         | string          | **[Required]** Entity name. |
| m         | string:number   | **[Required]** Metric name and numeric value. _Multiple._ |
| x         | string:string   | **[Required]** Metric name and text value. _Multiple._ |
| d         | string          | Time in ISO format. |
| ms        | integer         | Time in UNIX milliseconds. |
| s         | integer         | Time in UNIX seconds. |
| t         | string:string   | Tag name and text value. _Multiple._ |
| a         | boolean         | Text append flag. If set to `true`, causes the text value to be appended to the previous text value with the same timestamp. |

> At least one numeric metric `m:` or text metric `x:` is required.

> If the numeric observation was not specified for the text value with the same metric name, it is set to `NaN` (not a number).

> If the time fields `d, s, and ms` are omitted, the values are inserted with the current server time.

### ABNF Syntax

Rules inherited from [Base ABNF](base-abnf.md).

```properties
  ; entity and at least one metric is required
command = "series" MSP entity 1*(MSP [metric-numeric / metric-text]) *(MSP tag) [MSP append] [MSP time]
entity = "e:" NAME
metric-numeric = "m:" NAME "=" NUMBER
metric-text    = "x:" NAME "=" VALUE
tag = "t:" NAME "=" VALUE
append = "a:" ("true" / "false")
time = time-millisecond / time-second / time-iso
time-millisecond = "ms:" POSITIVE_INTEGER
time-second = "s:" POSITIVE_INTEGER
time-iso = "d:" ISO_DATE
```

## Data Types

New metrics will be initialized with the `float` data type by default.

To insert metric samples with another data type, create or update metric properties using [Meta API](../meta/metric/update.md) or the user interface.

## New Records

If the command references non-existent entities, metrics, or tags, such objects will be created as new records and registered automatically.

Entity, metric, and tag names are case-insensitive and will be converted to lower case when inserted, whereas tag values are case-sensitive.


## Examples

* Insert numeric value '72' for metric 'cpu_used', entity 'server001' recorded on March 4, 2015 at 15:14:40 GMT (Unix epoch seconds = 1425482080).

```ls
series e:server001 m:cpu_used=72.0 s:1425482080
```

* Insert samples for two series:
  - Insert numeric value '72' for metric 'cpu_used', entity 'server001' recorded on March 4, 2015 at 15:14:40 GMT
  - Insert numeric value '94.5' for metric 'memory_used' and the same entity and time.

```ls
series e:server001 m:cpu_used=72.0 m:memory_used=94.5 s:1425482080
```

* Same as the above, using Unix milliseconds.

```ls
series e:server001 m:cpu_used=72.0 m:memory_used=94.5 ms:1425482080000
```

* Same as the above, using ISO8601 date.

```ls
series e:server001 m:cpu_used=72.0 m:memory_used=94.5 d:2015-03-04T15:14:40Z
```

* Insert numeric value '20.5' for metric 'disk_used_percent', entity 'server001', and two tags, 'mount_point' and 'disk_name'. The value will be inserted with the current server time since the date is not specified in the command.

```ls
series e:server001 m:disk_used_percent=20.5 t:mount_point=/ t:disk_name=/sda1
```

* Insert numeric values into two series (metrics 'disk_used_percent' and 'disk_size_md') for the same entity and tags as above.

```ls
series e:server001 m:disk_used_percent=20.5 m:disk_size_mb=10240 t:mount_point=/ t:disk_name=/sda1
```

* Insert numeric value '24.4' and text value 'Provisional' (annotation) for metric 'temperature', entity 'sensor-1'.

```ls
series d:2016-10-13T08:15:00Z e:sensor-1 m:temperature=24.4 x:temperature="Provisional"
```

* Insert text value 'Shutdown by adm-user, RFC-5434' for metric 'status', entity 'sensor-1'.

```ls
series d:2016-10-13T10:30:00Z e:sensor-1 x:status="Shutdown by adm-user, RFC-5434"
```

* Same as the above. Append the specified text value to an existing value, if found.

```ls
series d:2016-10-13T10:30:00Z e:sensor-1 x:status="Shutdown by adm-user, RFC-5434" a:true
```

* Insert 'NaN' (Not-a-Number) for metric 'temperature', entity 'sensor-1'

```ls
series d:2016-10-13T08:45:00Z e:sensor-1 m:temperature=NaN
```

## Number Representation

* The numeric value can be a real number or `NaN` (Not a Number).
* The string representation of an inserted number consists of an optional sign, '+' ('\u002B') or '-' ('\u002D'), followed by a sequence of zero or more decimal digits ("the integer"), optionally followed by a fraction, optionally followed by an exponent.
* The exponent consists of the character 'e' ('\u0065') or 'E' ('\u0045') followed by an optional sign, '+' ('\u002B') or '-' ('\u002D'), followed by one or more decimal digits. The value of the exponent must lie between -2147483647 and 2147483647, and is inclusive.
* The fraction consists of a decimal point followed by zero or more decimal digits. The string must contain at least one digit in either the integer or the fraction.
* The number formed by the sign, the integer, and the fraction is referred to as the [**significand**](https://en.wikipedia.org/wiki/Significand).
* The **significand** value stripped from trailing zeros should be within the Long.MAX_VALUE `9223372036854775807` the and Long.MIN_VALUE  `-9223372036854775808` (19 digits). Otherwise the database will throw an **llegalArgumentException: BigDecimal significand overflows the long type** for decimal metrics or round the value for non-decimal metrics. For example, significand for `1.1212121212121212121212121212121212121212121` contains 44 digits and will be rounded to `1.121212121212121212` if inserted for a non-decimal metric.

## Series Tags, Text Value, Messages

The difference between series tags and text values inserted with the `x:` field is that tag names/values are converted to unique identifiers when stored.

Series tags are part of each series composite primary key, whereas the text value is not.

Since the total number of unique tag value identifiers is [limited](README.md#schema) to `16,777,215`, series tag values are not well suited for values with high cardinality such as random values or continuously incrementing values (time, counters).

A text value, on the other hand, is stored `as is`, without converting it to an identifier. A text value can be used to annotate a numeric observation without changing the series' primary key.

```ls
series d:2016-10-13T08:00:00Z e:sensor-1 m:temperature=20.3
series d:2016-10-13T08:15:00Z e:sensor-1 m:temperature=24.4 x:temperature="Provisional"
```

In this example, the temperature reading at `2016-10-13T08:15:00Z` is characterized as `Provisional`.

A text value can also be used to record observations for series that contain only text values, in which case their numeric values are set to `NaN`.

```ls
series d:2016-10-13T10:30:00Z e:sensor-1 x:status="Shutdown by adm-user, RFC-5434"
```

Unlike [message](message.md) commands, series text values are available for SQL querying using the `text` column.

```sql
SELECT entity, datetime, value, text
  FROM atsd_series
WHERE metric IN ('temperature', 'status') AND datetime >= '2016-10-13T08:00:00Z'
```

```ls
| entity   | datetime             | value | text                           |
|----------|----------------------|-------|--------------------------------|
| sensor-1 | 2016-10-13T08:00:00Z | 20.3  | null                           |
| sensor-1 | 2016-10-13T08:15:00Z | 24.4  | Provisional                    |
| sensor-1 | 2016-10-13T10:30:00Z | NaN   | Shutdown by adm-user, RFC-5434 |
```


## Text Append

The `append` flag applies to text values specified with the `x:` field.

If the append flag is set to `true`, ATSD checks the previous text value for the same timestamp. If the previous value is found, the new value is appended at the end using `;\n` (semi-colon followed by line feed) as a separator.

In order to prevent **duplicate** values, the database checks the existing value for duplicates by splitting the stored value into a string array and discarding the new value if it is equal to one of the elements in the array.

```ls
series d:2017-01-20T08:00:00Z e:sensor-1 x:status="Shutdown by adm-user, RFC-5434"
series d:2017-01-20T08:00:00Z e:sensor-1 x:status="Restart" a:true
```

The new value will be equal to:

```
Shutdown by adm-user, RFC-5434;
Restart
```

## Versioning

[Versioning](http://axibase.com/products/axibase-time-series-database/data-model/versioning/) enables tracking of time-series value changes for the purpose of establishing an audit trail and traceable data reconciliation.

Versioning is disabled by default. It can be enabled for particular metrics by applying the Versioning check box on the Metric Editor page.

To insert versioning fields use reserved tags:

* `$version_source`
* `$version_status`

```ls
series s:1425482080 e:e-vers m:m-vers=13 t:$version_status=OK t:$version_source=collector-1
```

If the metrics are versioned,  `$version_source` and `$version_status` tags will be ignored as regular series tags and instead will be converted to corresponding versioning tags.