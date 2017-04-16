Weekly Change Log: January 30 - February 5, 2017
================================================

### ATSD

| Issue| Category    | Type    | Subject                                                                              |
|------|-------------|---------|--------------------------------------------------------------------------------------| 
| 3878 | api-network | Bug     | [series](/docs/api/network/series.md#series-command) command doens't support line break in tag value and in x (annotation) field.     |
| 3874 | api-network | Bug     | [series](/docs/api/network/series.md#series-command) command fails to overwrite value when inserted in batch with append flag.| 
| 3873 | sql         | Bug     | Malformed tag names in a [`JOIN`](/docs/api/sql#joins) query. | 
| 3870 | rule engine | Bug     | Rule expression evaluation error is propagated to inserting clients. | 
| 3869 | sql         | Bug     | Tag value encoding overflow for metrics with greater than 1K series. | 
| 3862 | rule engine | Bug     | Add html escape for variable (alias) expressions. | 
| 3861 | client      | Feature | [ATSD Python client](https://github.com/axibase/atsd-api-python#sql-queries). Add support for SQL to dataframe retrieval.| 
| 3858 | sql         | Bug     | Decimal precision sometimes ignored in scheduled [SQL reports](/docs/api/sql/scheduled-sql.md).| 
| 3854 | rule engine | Feature | Extend [Time](/docs/rule-engine/expression.md#time-functions) functions to allow custom time format.| 
| 3853 | sql         | Bug     | Keyword [`QUARTER`](/docs/api/data/series/time-unit.md#time-unit) not supported. | 
| 3851 | data-in     | Bug     | UDP server disconnects on malformed command. | 
| 3843 | sql         | Bug     | [Time/Period](/docs/api/sql#keywords) keywords should be case-insensitive. | 
| 3841 | sql         | Bug     | [`CAST`](/docs/api/sql#cast) expression in `WHERE` condition changes results. | 
| 3840 | sql         | Bug     | [`ORDER BY`](/docs/api/sql#ordering-1) not applied when using column alias. | 

### Collector

| Issue| Category    | Type    | Subject                                                                              |
|------|-------------|---------|--------------------------------------------------------------------------------------|
| 3876 | socrata     | Bug     | Job fails even if filter expression is valid. |
| 3866 | socrata     | Bug     | Configuration form refactoring. |
| 3865 | socrata     | Bug     | Time heuristics. |
| 3849 | socrata     | Bug     | Numeric field with non-standard `NaN` constant should be classified as numeric. |
| 3848 | socrata     | Bug     | Extract command time from the built-in `updated_at` field.| 
| 3826 | socrata     | Feature | Add support for series [annotation](/docs/api/network/series.md#series-tags-text-value-messages) (x:) field. | 

### Charts

| Issue| Category    | Type    | Subject                                                                              |
|------|-------------|---------|--------------------------------------------------------------------------------------| 
| 3839 | time-chart  | Bug     | Negative style not applied to marker color. |
| 3836 | bar         | Bug     | Remove onload animation in bar and pie widgets. | 
| 3794 | box         | Bug     | Fix tooltip issues. | 