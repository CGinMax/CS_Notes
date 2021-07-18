Option错误处理
========================

# unwrap/expect

如果option有值这返回值，否则抛出`panic`。

# ?获取Option值

如果要将一个`Option<i32>`转换为`Option<String>`时，可以使用match的方法：

```rust
fn to_string(v: Option<i32>) -> Option<String>
{
    match v {
        Some(n) => Some(format!("value is {}", n)),
        None,
    }
}
```

这样的代码看起来不简介，rust提供了`?`语法糖，但只能使用在返回Option/Result的函数中。

```rust
fn to_string(v: Option<i32>) -> Option<String>
{
    let tmp = v?;// v=None，直接返回None
    Some(format!("value is {}", tmp))
}
```

