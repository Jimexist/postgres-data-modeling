## Normalization


### 范式的递进

规范了如何把实体和关系对应成 tables 和 keys

建模目标：准确 ✅

分为不同等级，层层递进，前者是后者的前提

- 1NF
- 3NF
- **BCNF**
- 4NF
- etc.


### 1NF

所有的 column 都只存简单不可分的值


从复合字段

<table>
  <caption>Customer</caption>
  <tbody><tr>
  <th>Customer ID</th>
  <th>Name</th>
  </tr>
  <tr>
  <td>123</td>
  <td>Pooja Singh</td>
  </tr>
  <tr>
  <td>456</td>
  <td>San Zhang</td>
  </tr>
  <tr>
  <td>789</td>
  <td>John Doe</td>
  </tr>
  </tbody>
</table>

变成简单字段

<table>
  <caption>Customer</caption>
  <tbody><tr>
  <th>Customer ID</th>
  <th>First Name</th>
  <th>Surname</th>
  </tr>
  <tr>
  <td>123</td>
  <td>Pooja</td>
  <td>Singh</td>
  </tr>
  <tr>
  <td>456</td>
  <td>San</td>
  <td>Zhang</td>
  </tr>
  <tr>
  <td>789</td>
  <td>John</td>
  <td>Doe</td>
  </tr>
  </tbody>
</table>


从 array 类型

<table>
  <caption>Customer</caption>
  <tbody><tr>
  <th>Customer ID</th>
  <th>First Name</th>
  <th>Surname</th>
  <th>Telephone Number</th>
  </tr>
  <tr>
  <td>123</td>
  <td>Pooja</td>
  <td>Singh</td>
  <td>555-861-2025, 192-122-1111</td>
  </tr>
  <tr>
  <td>456</td>
  <td>San</td>
  <td>Zhang</td>
  <td>(555) 403-1659 Ext. 53; 182-929-2929</td>
  </tr>
  <tr>
  <td>789</td>
  <td>John</td>
  <td>Doe</td>
  <td>555-808-9633</td>
  </tr>
  </tbody>
</table>

变成

<table>
  <caption>Customer</caption>
  <tbody><tr>
  <th>Customer ID</th>
  <th>First Name</th>
  <th>Surname</th>
  <th>Telephone Number1</th>
  <th>Telephone Number2</th>
  </tr>
  <tr>
  <td>123</td>
  <td>Pooja</td>
  <td>Singh</td>
  <td>555-861-2025</td>
  <td>192-122-1111</td>
  </tr>
  <tr>
  <td>456</td>
  <td>San</td>
  <td>Zhang</td>
  <td>(555) 403-1659 Ext. 53</td>
  <td>182-929-2929</td>
  </tr>
  <tr>
  <td>789</td>
  <td>John</td>
  <td>Doe</td>
  <td>555-808-9633</td>
  <td></td>
  </tr>
  </tbody>
</table>


或者一对多映射（不确定有几个值）

<table>
  <tbody><tr>
  <td valign="top">
  <table class="wikitable">
  <caption>Customer Name</caption>
  <tbody><tr>
  <th><u>Customer ID</u></th>
  <th>First Name</th>
  <th>Surname</th>
  </tr>
  <tr>
  <td>123</td>
  <td>Pooja</td>
  <td>Singh</td>
  </tr>
  <tr>
  <td>456</td>
  <td>San</td>
  <td>Zhang</td>
  </tr>
  <tr>
  <td>789</td>
  <td>John</td>
  <td>Doe</td>
  </tr>
  </tbody></table>
  </td>
  <td valign="top">
  <table class="wikitable">
  <caption>Customer Telephone Number</caption>
  <tbody><tr>
  <th>Customer ID</th>
  <th><u>Telephone Number</u></th>
  </tr>
  <tr>
  <td>123</td>
  <td>555-861-2025</td>
  </tr>
  <tr>
  <td>123</td>
  <td>192-122-1111</td>
  </tr>
  <tr>
  <td>456</td>
  <td>(555) 403-1659 Ext. 53</td>
  </tr>
  <tr>
  <td>456</td>
  <td>182-929-2929</td>
  </tr>
  <tr>
  <td>789</td>
  <td>555-808-9633</td>
  </tr>
  </tbody></table>
  </td>
  </tr>
  </tbody>
</table>


### 1NF 动机

- 简单类型比复杂类型更好处理（可以分开检查、去重、查询、索引、更新等）：准确性 ✅
- 可以方便合成和模拟复杂的值：灵活性 ✅
  <dl>
    <dt>Sarah Zhang</dt>
    <dd><code>fname || ' ' || lname</code></dd>
    <dt>Zhang, Sarah</dt>
    <dd><code>lname || ', ' || fname</code></dd>
    <dt>还原成数组</dt>
    <dd><code>array_agg(phones)</code></dd>
  </dl>


### 函数依赖

*极其简化版本*

- 定义： *A → B*
- 代表：如果列 A 的值相同，那么 B 的值也相同
- 注意：
  - 逆命题不成立，如果 B 值相同，那么 A 的值可能不同
  - 否命题不成立，如果 A 的值不同，那么 B 的值可能相同
  - 比如身份证号和姓名


### 3NF

*"Nothing but the key"*


从

<table>
  <caption>Tournament Winners</caption>
  <tbody><tr>
  <th><u>Tournament</u></th>
  <th><u>Year</u></th>
  <th>Winner</th>
  <th>Winner Date of Birth</th>
  </tr>
  <tr>
  <td>Indiana Invitational</td>
  <td>1998</td>
  <td>Al Fredrickson</td>
  <td>21 July 1975</td>
  </tr>
  <tr>
  <td>Cleveland Open</td>
  <td>1999</td>
  <td>Bob Albertson</td>
  <td>28 September 1968</td>
  </tr>
  <tr>
  <td>Des Moines Masters</td>
  <td>1999</td>
  <td>Al Fredrickson</td>
  <td>21 July 1975</td>
  </tr>
  <tr>
  <td>Indiana Invitational</td>
  <td>1999</td>
  <td>Chip Masterson</td>
  <td>14 March 1977</td>
  </tr>
  </tbody>
</table>

变成

<table>
  <tbody>
  <tr>
  <td valign="top">
  <table class="wikitable">
  <caption>Tournament Winners</caption>
  <tbody><tr>
  <th><u>Tournament</u></th>
  <th><u>Year</u></th>
  <th>Winner</th>
  </tr>
  <tr>
  <td>Indiana Invitational</td>
  <td>1998</td>
  <td>Al Fredrickson</td>
  </tr>
  <tr>
  <td>Cleveland Open</td>
  <td>1999</td>
  <td>Bob Albertson</td>
  </tr>
  <tr>
  <td>Des Moines Masters</td>
  <td>1999</td>
  <td>Al Fredrickson</td>
  </tr>
  <tr>
  <td>Indiana Invitational</td>
  <td>1999</td>
  <td>Chip Masterson</td>
  </tr>
  </tbody></table>
  </td>
  <td valign="top">
  <table class="wikitable">
  <caption>Winner Dates of Birth</caption>
  <tbody><tr>
  <th><u>Winner</u></th>
  <th>Date of Birth</th>
  </tr>
  <tr>
  <td>Chip Masterson</td>
  <td>14 March 1977</td>
  </tr>
  <tr>
  <td>Al Fredrickson</td>
  <td>21 July 1975</td>
  </tr>
  <tr>
  <td>Bob Albertson</td>
  <td>28 September 1968</td>
  </tr>
  </tbody></table>
  </td>
  </tr>
  </tbody>
</table>


### 3NF 动机

- 减少了数据的重复,数据更加一致（准确性 ✅）
  - 设想你要去修改其中的一个人的生日
  - 设想输入的时候其中一条名字输入错误
- 业务逻辑更加围绕「实体」构建（灵活性 ✅）
  - 业务概念重用（比赛 + 人）



### BCNF

*Boyce–Codd normal form*

比 3NF 略强，比 4NF 略弱，这里略过不讲。
