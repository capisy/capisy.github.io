---
title: php-mysql_pdo
date: 2019-04-06 23:25:00
tag: php
---
#### mysql连接
> dsn:data source name
```php 
$db = [
    'dsn'=>'mysql:host=localhost;dbname=mf_demo;port=3306',
    'name' => 'root',
    'passwd'=>'m123456',
    'opts' => [PDO::ATTR_ERRMODE=>PDO::ERRMODE_EXCEPTION] // 可选：调优参数
  ];
  
  try{
    $pdo = new PDO($db['dsn'],$db['name'],$db['passwd'],$db['opts']);
  }catch(PDOException $e){
    echo $e->getMessage();
    exit;
  }
```
#### pdo执行sql语句的三种方法
query(),执行select语句，返回结果集
```php
try{
  $select_sql = "select * from mf_loving";
  $stmt = $pdo->query($select_sql);
  print_r($stmt);
}catch(PDOException $e){
  $e->getMessage();
}
```
exec(),执行update，delete，insert..返回影响的行数
```php
try{
    $update_sql = "update mf_loving set name = 'shiyuan' where id = 6";
    $affected_rows = $pdo->exec($update_sql);
    echo '执行成功，影响行数:' . $affected_rows;
  }catch(PDOException $e){
    echo $e->getMessage();
  }
```
prepare(),可执行所有的sql语句 
```php
$update_sql = "update mf_loving set name = ? where id = ?";
$stmt = $pdo->prepare($update_sql);
try{
  $res = $stmt->execute(['shiyuan',6]);
}catch(PDOException $e){
  echo $e->getMessage();	
}
```
#### 常用stmt方法
```php
 /**
 * 设置获取数据的模式
 * @ params: PDO::FETCH_NUM 或 PDO::FETCH_ASSOC
 */
 $stmt->setFetchMode(PDO::FETCH_NUM);  
```
```php
$data = $stmt->fetch(); // 一条数据 
$data = $stmt->fetchAll(); // 所有数据
```
```php
 $rowLen = $stmt->rowCount(); // 获取数据的行数
 $columnLen = $stmt->columnCount(); // 获取字段数
```
```php
// 获取返回数据的字段信息
 $fileds = [];
 for($i=0; $i<$columnLen; $i++){
   $filed = $stmt->getColumnMeta($i);
   array_push($fileds,$filed['name']);
 }
 print_r($fileds);
```

#### 常用的pdo方法
```php
// 获取最后插入的ID
$last_insert_id = $pdo->lastInsertId();
```

#### 事务处理
```php
 try{
 	// 开启事务处理
    $pdo->beginTransaction();
    $price = 50;
    $sql = "update mf_account set account =account+ ? where name = ?";
    $stmt = $pdo->prepare($sql);
    // 张三转出
    $affected_rows = $stmt->execute([-$price,'zhangsan']);
    echo ($affected_rows ? $affected_rows : '张三转出失败') . '<br>'; 
    // 李四转入
    $affected_rows = $stmt->execute([$price,'lisi']);
    echo ($affected_rows ? $affected_rows : '李四转出失败') . '<br>'; 
    // 无错误，提交事务
    $pdo->commit();
    echo '交易成功';
  }catch(PDOException $e){
    echo $e->getMessage();
    // 有错误，回滚操作
    $pdo->rollback();
  }
```