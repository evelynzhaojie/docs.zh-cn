# seconds_sub

## 功能

给指定的日期时间或日期减去指定的秒数。

## 语法

```Haskell
DATETIME seconds_sub(DATETIME|DATE date, INT seconds);
```

## 参数说明

`date`: 指定的时间，支持的数据类型为 DATETIME 或者 DATE。

`seconds`: 减少的秒数，支持的数据类型为 INT。

## 返回值说明

返回值的数据类型为 DATETIME。

如果 `date` 或 `seconds` 任意一者为 NULL，则返回 NULL。

## 示例

```Plain Text
select seconds_sub('2022-01-01 01:01:01', 2);
+---------------------------------------+
| seconds_sub('2022-01-01 01:01:01', 2) |
+---------------------------------------+
| 2022-01-01 01:00:59                   |
+---------------------------------------+

select seconds_sub('2022-01-01 01:01:01', -1);
+----------------------------------------+
| seconds_sub('2022-01-01 01:01:01', -1) |
+----------------------------------------+
| 2022-01-01 01:01:02                    |
+----------------------------------------+

select seconds_sub('2022-01-01', 1);
+------------------------------+
| seconds_sub('2022-01-01', 1) |
+------------------------------+
| 2021-12-31 23:59:59          |
+------------------------------+
```