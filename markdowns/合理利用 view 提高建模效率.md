## 合理利用 view 提高建模效率


### View 是一种「快捷方式」

- View 本身不承载数据，只是对 SQL 的重命名。
- 如果说 normalization 是为了数据一致性而「拆」，那么 view 就可以为了查询方便性而「合」
- 运行分析完全透明，由底层 table 决定


### View 的建立

```sql
create table users (
  id text primary key,
  first_name text not null,
  last_name text not null,
  age int
);
```

```sql
create table movies (
  id text primary key,
  title text not null,
  year int
  -- and others
);
```

```sql
create table fav_movies (
  user_id text references users (id) on delete cascade,
  movie_id text references movies (id) on delete cascade,
  primary key (user_id, movie_id)
);
```


可以用 view 实现复杂查询：
```sql
create view movie_fans as select
  u.id as user_id,
  u.first_name || ' ' || u.last_name as user_name,
  u.age > 18 as is_adult
  from users as u
  where u.id in (
    select user_id from fav_movies
    group by 1 having count(*) > 100
  );
```

考虑用代码实现：
- 所需时间（开发、测试）？
- 运行效率？
- 如果再在上面增加呢？`movie_fans` 的好友？


### View 的好处：

- 没有 dup 数据（永远 update to date）
- read only
- 数据层的封装（隐藏复杂业务逻辑模型）
- 性能？
  - 不能建立 index，依赖 underlying tables
  - 如果太多 join、数据量大、真的 care 性能怎么办？


### Materialized View

*和 create view 一样，只是多了一个 materialized，works like a cache*

- 数据会真正 copy，不再需要 hit the real tables，但也不再实时刷新
- 可以在其上面建立 index
- 支持 refresh 功能
- 这里讨论的是数据建模，性能优化就不详细讲了
