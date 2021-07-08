# 结构体解构(destructure)

将一个结构体变量作为右值解构成普通变量，而非元组，并且不该变元结构体变量。

```rust
struct Person {
    name: String,
    age: u8,
    skill: String
}

let me = Person {
    name: "cooper".to_string(),
    age: 23,
    skill: "cpp".to_string()
};

let Person{
    name: a,
    age: b,
    skill: c
} = me;
assert_eq!(a, "cooper".to_string());
assert_eq!(b, 23);
assert_eq!(c, "cpp".to_string());
assert_eq!(me.name, "cooper".to_string());
```

也可以不用另外的变量名称取值，直接用结构体元变量名取值：

```rust
let Person {
    name,
    age,
    skill
} = me;
assert_eq!(name, "cooper".to_string());
assert_eq!(age, 23);
assert_eq!(skill‵, "cpp".to_string());
assert_eq!(me.name, "cooper".to_string());
```
