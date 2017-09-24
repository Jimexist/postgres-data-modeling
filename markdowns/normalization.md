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


### 动机

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


### 3NF


### BCNF
