# Trait高级

## dyn trait
rust使用`struct`和`trait`，作为面向对象的类和接口。但在将`struct`转为`trait`时无法直接转换，例如：

```rust
trait Interface{}
struct Base{}

let b = Box<Base>::new();
Interface i = Box<Interface>(dyn b);
```

## 消除trait重叠
当多个`trait`拥有同名的函数，且struct都impl这些trait时，对象在调用该函数会出现二意性。

```rust
trait RoundButton{
    fn draw(&self){}
}
trait OutlineButton{
    fn draw(&self){}
}
struct Button{
//忽略变量
}
impl RoundButton for Button{
    fn draw(&self) {
        println!("draw round btn");
    }
}
impl OutlineButton for Button {
    fn draw(&self) {
        println!("draw outline btn");
    }
}

let btnOk = Button{};
// 调用RoundButton的draw
<Button as RoundButton>::draw(&btnOk);
// 调用OutlineButton的draw
<Button as OutlineButton>::draw(&btnOk)
```


