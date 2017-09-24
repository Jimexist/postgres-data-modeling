## PostgreSQL Specific Tricks

*像瑞士军刀一样多样和可靠*


### [JSON Types](https://www.postgresql.org/docs/9.6/static/datatype-json.html)

- 如果你非要打破第一范式的话
- 基本可以当做 MongoDB 来用
- 支持 array 和 object index
- 实践上，很多编程语言的 driver 支持有限，需要谨慎


### [Range Type](https://www.postgresql.org/docs/9.6/static/rangetypes.html#RANGETYPES-BUILTIN)

- 不用自己去检查 start <= end
- 支持整数、numeric、时间等
- 无穷区间和开闭区间
- 丰富的包含、重叠检测、union 和 intercept 操作
- 支持 index (GIST)


### [Geometric Types](https://www.postgresql.org/docs/9.6/static/datatype-geometric.html)

- 点线面
- 交叉、重合、切线（点）等操作
- 非 B+ 树 index
- [强大的 PostGIS](http://postgis.net/)


### 其他有用的数据类型

- UUID
- macaddr
- inet / CIDR
- enum （慎用）
- 甚至 customized type（慎用）


### 其他有用的建模工具

- [inheritance](https://www.postgresql.org/docs/current/static/ddl-inherit.html) / [partition](https://www.postgresql.org/docs/current/static/ddl-partitioning.html)
- Postgres 10 中有了 [native partition](https://wiki.postgresql.org/wiki/New_in_postgres_10#Native_Partitioning)
- [window function](https://www.postgresql.org/docs/9.6/static/functions-window.html)
- [cubing](https://www.postgresql.org/docs/9.6/static/cube.html)
- [全文搜索](https://www.postgresql.org/docs/9.6/static/textsearch.html)
- [确定精度计算](https://www.postgresql.org/docs/9.6/static/datatype-numeric.html#DATATYPE-NUMERIC-DECIMAL) 和 `money` 类型
