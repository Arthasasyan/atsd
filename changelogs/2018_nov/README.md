# Monthly Change Log: November 2018

## ATSD

**Issue**| **Category**    | **Type**    | **Subject**
-----|-------------|---------|----------------------
5800 | rule editor | Bug | [Rule Editor](../../rule-engine/): **Test** tab does not display error. |
5799 | license | Bug | License: Record incoming commands in `command_discarded.log` if license expired. |
5798 | license | Bug | License: `NullPointerException` in license check. |
5797 | sql | Bug | SQL: Column alias partially unsupported in the [`WHERE`](../../sql/README.md#where-clause) clause. |
5796 | api rest | Feature | [Series Query](../../api/data/series/query.md#series-query): Implement support for `IN`/`NOT IN` in `tagExpression`. |
5792 | sql | Feature | SQL: Implement [`RANK`](../../sql/README.md#partition-ordering) and [`DENSE_RANK`](../../sql/README.md#partition-ordering) analytic functions. |
5789 | sql | Feature | SQL: Implement offset and default for [`LAG`](../../sql/README.md#lag) and [`LEAD`](../../sql/README.md#lead) functions. |
5786 | administrator | Bug | Core: Version Check not performed. |
5781 | portal | Bug | [Portals](../../portals/README.md#portals): Icon setting does not work for **View By Name**. |
5774 | UI | Bug | UI: Erroneous number formatting in the **Data Entry** command field. |
5770 | sql | Feature | SQL: Implement [`PI()`](../../sql/README.md#mathematical-functions) function.  |
5769 | jdbc | Bug | [JDBC](https://github.com/axibase/atsd-jdbc/): Conflicting headers set on request. |
5768 | sql | Bug | [SQL](../../sql/api.md): return error message if query is missing or duplicate. |
5767 | jdbc | Bug | [JDBC](https://github.com/axibase/atsd-jdbc/): Provide details in exception message for error response code `401`. |
5766 | security | Bug | Security: `findUserByUsername` throws exception. |
5765 | security | Bug | [Security Incidents](../../rule-engine/functions-security.md#security-functions): Add `User-Agent` logging. |
5764 | sql | Feature | SQL: Implement [trigonometric functions](../../sql/README.md#trigonometric-functions). |
5748 | sql | Bug | SQL: Incorrect data type inferred by [`ISNULL`](../../sql/README.md#isnull) and [`COALESCE`](../../rule-engine/functions-text.md#coalesce) functions for `datetime` and `time` columns. |
5736 | sql | Bug | [SQL](../../sql/): Precision loss in big integer arithmetics. |
5730 | core | Feature | Implement ATSD co-processor to retrieve co-processor version. |
5690 | UI | Feature | UI: Multiple enhancements. |
5648 | log_aggregator | Bug | Aggregation Logger: No need to create HTTP/HTTPS connection.  |
5352 | security | Bug | [Security](../../administration/user-authentication.md#http-basic-authorization-examples): User cannot log in through HTTP if logged in through HTTPS. |
5086 | sql | Feature | SQL: [`ENDTIME`](../../sql/README.md#endtime) calculations to align with [`CALENDAR`](../../administration/workday-calendar.md#workday-calendar). |
4294 | sql | Bug | SQL: [`OUTER JOIN`](../../sql/examples/outer-join.md#outer-join) includes rows for excluded entities. |
4180 | sql | Bug | SQL: filter by tag not applied in [`OUTER JOIN`](../../sql/examples/outer-join.md#outer-join). |
4033 | sql | Feature | SQL: [`BETWEEN`](../../sql/README.md#interval-subqueries) subquery with multiple intervals for the same series key. |
