## Constraints, Checks, and Triggers


### Nulls

*最重要一种 check，经常被忽略*

*null 不是 0，不是「空值」*

**null 代表不知道**

avoid nulls whenever possible


问题：小明20岁，小红18岁，小白年龄不知道，请问他们的平均年龄？

SQL will do the right thing:

```
\d users;

name | age
------+-----
小明 |  20
小红 |  18
小白 |   ¤
```

```
select avg(age) from users;

         avg
---------------------
 19.0000000000000000
(1 row)
```


- avg 等 aggregation 函数会处理好 null
- create table 时应该注意 `not null` constraint，避免无效数据 *sneak in*
- order by 的时候可以指定 null first/last


### Other Constraints

```sql
create table users (
  id text not null primary key,
  name text not null check (length(name) > 0),
  -- non-empty
  age int not null check (age >= 18 and age < 130),
  -- must be adult
  email text null unique,
  -- must be unique
  phone text null unique
    check (phone is not null or email is not null),
  -- must have at least email or phone registered
  created_at timestamptz not null default now()
);
```

考虑在代码里面实现：
- 单元测试
- 「最后一道关卡」


### Triggers

- 自动更新 `updated_at`
- 自动删除不再被引用的值
- 自动更新 `count` 等
- 自动 refresh materialized view
- 功能强大，但是也「把逻辑隐藏在背后」，需要慎用


### 好例子

[Instagram's sharding and ID generation](https://engineering.instagram.com/sharding-ids-at-instagram-1cf5a71e5a5c)
