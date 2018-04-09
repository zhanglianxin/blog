# 本应该关注性能问题

在 TJ 办理离职那天下午，遇到前项目组里的同事，说领导夸我写得代码好，没有 bug。当时是沾沾自喜的～

前阵子闲下来整理一些书，看到《Modern PHP》又捡起来翻了一下。恥ずかしい。

**Case 0. 手动解析 CSV 文件**

```php
<?php
// my version
$handle = fopen($filename, 'r');
while (($data = fgetcsv($handle, 0)) !== false ) {
    print_r($data);
}
```

```php
<?php
// good practice
function getRows($file) {
    $handle = fopen($file, 'rb');
    if ($handle === false) {
        throw new Exception();
    }
    while (feof($handle) === false) {
        yield fgetcsv($handle);
    }
    fclose($handle);
}
foreach (getRows($filename) as $row) {
    print_r($row);
}
```
**Case 1. 大数据量渲染**

离职前的项目有一个问题是 Vue 渲染列表，两三千条数据却花了几分钟。当时有人怀疑是 AJAX 请求后端查询数据库耗时，我在浏览器端做了测试，返回数据只花了几秒时间。简单学习过 Vue 基础加上对前端的一些认识，让我断定问题是出在页面渲染上，类似于 Android 中的 RecycleView，默默地 Google 解决方案，其他公司都是写一个特定的插件来解决的，分享出来供大家使用。Vue 刚入门理解不深，拿着别人的插件硬往上套始终不成功，我拯救不了全世界，还是别挣扎了，交给技术大牛吧。

**Reflection**

即使是传统软件开发行业，大家做出来东西都是要经过谨慎详细地测试的，此时手动测试的弊端就暴露出来，数据量小的问题，测试数据都是大家一条一条做出来的，做好几千条数据就是纯体力活。数据量不足，有某些问题就无法发现。

而作为一名开发人员，只把功能实现是不够的。一直认为，只要不是智商明显欠缺，把业务逻辑给实现完全不成问题。More step forward，需要关注语言特性（比如上面的生成器）、考虑性能问题和安全问题来把代码写得更优雅，以此谨记。