---
title: ThinkPHP 多表事物
date: 2017-01-23 12:20:25
tags:
  - ThinkPHP
  - 多表事物
  - 事物
  - M函数
categories:
  - ThinkPHP
---

在 Thinkphp 3.2.3 下测试通过。

Thinkphp 对事务的处理非常简单。单表事务只需使用 M 函数实例化一个数据表对象，如果操作成功则提交，失败则回滚。例如：

```php
$User = M('user');
 
$User->startTrans(); // 开启事务
$id = $User->add(['name' => 'hongxuan']);
 
//
// TODO 其它操作
//
 
if ($id) { // 插入成功
    $User->commit(); // 提交
} else { // 添加失败
    $User->rollback(); // 回滚
}
```

对多表的事务处理也非常简便。先用 M 函数实例化一个空对象，使用 table 方法进行多个表的操作，如果操作成功则提交，失败则回滚。例如：

```php
$Model = M(); // 实例化一个空对象
$Model->startTrans(); // 开启事务
 
//
// TODO 其它操作
//
 
// table 方法中的数据表名要带上前缀，这里为“test_”。
$Model->table('test_user')->add(['name'=>'admin']);
$Model->table('test_key')->add(['key'=>'test']);
$Model->table('test_value')->add(['value'=>'test']);
$Model->table('test_task')->add(['task'=>'test']);
 
if (操作成功的条件) {
    $Model->commit(); // 成功则提交事务
} else {
    $Model->rollback(); // 否则将事务回滚
}
```



