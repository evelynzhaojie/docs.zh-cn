# 用户自定义变量

本文介绍如何声明和使用用户自定义变量。

自 2.5 版本起， StarRocks 支持用户声明自定义变量 (user-defined variables)。自定义变量用于存储特定的值，后续引用在 SQL 语句中，简化 SQL 语句的编写和避免重复计算。

## 使用说明

- 当前仅支持声明会话级别的自定义变量，即用户只能使用自己声明的自定义变量，且如客户端断开，那么当前会话中所有自定义变量将失效。
- StarRocks 暂不支持使用 SHOW 语句查看已有的自定义变量。
- 不支持声明 BITMAP、HLL、PERCENTILE 和 ARRAY 类型的自定义变量，JSON 类型的自定义变量会转换为 STRING 类型进行存储。

## 声明自定义变量

### 语法

```Plain
SET @var_name = expr [, ...];
```

> **说明**
>
> - 声明自定义变量时，必须加前缀 `@`。
> - 可同时声明多个自定义变量，多个变量之间用逗号 (,) 隔开。
> - 支持多次声明同一自定义变量，新声明的值会覆盖原有值。
> - 如在一个 SQL 语句中引用了没有声明过的变量，该变量值默认为 `NULL` 且为 STRING 类型。

### 参数说明

| **参数** | **必填** | **说明**                                                     |
| -------- | -------- | ------------------------------------------------------------ |
| var_name | 是       | 自定义变量名称，命名规则如下：<ul><li>必须由字母 (a-z 或 A-Z)、数字 (0-9) 或下划线 (\_) 组成。</li><li>总长度不能超过 64 个字符。</li></ul> 例如，@'my-var'、@"my-var" 和 @\`my-var\`。字符串类型的变量名称可以包含除字母、数字和下划线 (_) 以外的其他字符，例如句点 (.)。 |
| expr     | 是       | 自定义变量存储的值。支持简单值，例如 `43`；也支持复杂表达式，如 SELECT 查询返回的值。表达式计算结果的数据类型即为变量的数据类型。 |

### 示例

示例一：声明一个数值为自定义变量。

```SQL
SET @var = 43;
```

示例二：声明 SELECT 语句返回的值为自定义变量。

```SQL
SET @var = (SELECT SUM(v1) FROM test);
```

示例三：一次性声明多个变量。

```SQL
SET @v1=1, @v2=2;
```

## **使用场景**

- 简化 SQL 语句的编写。示例如下，执行 SELECT 语句时，`@var` 会被 `1` 替换。

  ```Plain
  SET @var = 1;
  SELECT @var, v1 from test;
  ```

- 避免重复计算。示例如下，执行 SELECT 语句时，`@var` 会被 `select sum(c1) from tbl` 命令的计算结果替换。

  ```Plain
  SET @var = (select sum(c1) from tbl);
  SELECT @var, v1 from test;
  ```
