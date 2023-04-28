

**生成器yield关键字不是返回值，只是生成一个值**。 生成器会返回一个 Generator 类的对象。 foreach对该对象每一次迭代，PHP会通过 Generator 实例计算出下一次需要迭代的值。这样 foreach 就知道下一次需要迭代的值了。
在运行中 for 循环执行后，会立即停止。等待 foreach 下次循环时候循环才会再执行一次，然后立即再次停止，直到不满足条件不执行结束。


```
function getI() {
    for ($i = 0; $i < 100; $i++) {
        yield $i;
    }
}


$a = getI();
foreach ($a as $value) {
    echo $value . "\r\n";
    if($value == 20) {
        break;
    }
}
$a = getI(); //不写此行代码会Cannot rewind a generator that was already run
//foreach开始执行时会调用rewind函数重置开头。这样就会出错。
foreach ($a as $value) {
    usleep(10);
    echo $value ;


    if($value == 100) {
        break;
    }
}
```


