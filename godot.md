# 18

[steamdb](https://steamdb.info)

# 17

`#!\[outer_attribute\]` applies to the item immediately following it. Some examples of items are: a function, a module declaration, a constant, a structure, an enum.

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}
```

`#![inner_attribute]` applies to the enclosing item (typically a module or a crate). In other words, this attribute is interpreted as applying to the entire scope in which it's placed.

```rust
#![allow(unused_variables)]

fn main() {
    let x = 3; // This would normally warn about an unused variable.
}
```

# 17

Self和self区别  
Self是一个类型，用于引用当前的类型，而self是一个值，用于引用当前实例。
Self通常在trait和impl 块中使用，而self只能在方法定义中使用。
Self可以用于返回类型，类型参数等。而self用于方法参数列表中，表示当前实例的引用。

# 16

- **iter** - This borrows each element of the collection through each iteration. Thus leaving the collection untouched and available for reuse after the loop.
- **into_iter** - This consumes the collection so that on each iteration the exact data is provided. Once the collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop.
- **iter_mut** - This mutably borrows each element of the collection, allowing for the collection to be modified in place.

# 15

The From and Into traits are inherently linked, and this is actually part of its implementation. If you are able to convert type A from type B, then it should be easy to believe that we should be able to convert type B to type A.

# 14

Rust provides no implicit type conversion (coercion) between primitive types. But, explicit type conversion (casting) can be performed using the as keyword.

# 13

```
my-cool-project
├── godot
│   ├── project.godot
│   └── my-extension.gdextension
└── rust
    ├── Cargo.toml
    ├── src
    └── target
        └── debug
            └── (lib)?my_extension.(so|dll|dylib)
```

# 12

权限在引用生命周期结束时被返回

```rust
let mut x = 1; //x(r,w,o)
let y = &x; // x(r)
let z = *y; // x(r,w,o)
x += z; // x(r,w,o) z(r)
```

# 11

可变引用可以降级为只读引用

# 10

可变引用提供对数据“唯一”且"非拥有"访问  
不移动数据的情况下，可访问  
可变引用 &mut  
相当于常量指针 cosnt * 不移动 可以修改

# 9

```rust
let mut v: Vec<i32> = vec![1,2,3]; //v(R,W,O)
let num: &i32 = &v[2]; // v(R) num(R,O) *num(R)
v.push(r); //error v is borrowed by num
println!("Third element is {*num}"); // immutable borrow
```

# 8

RW(mut)O

# 7

可变性和别名不可同时存在(Rust Avoids Simultaneous Aliasing and Mutation)

# 6

引用是没有所有权的指针

# 5

[godot-rust](https://colinwttt.github.io/godot-rust-book-chinese/index.html)

# 4

[Rust 程序语言设计](https://www.rustwiki.org.cn/zh-CN/book/title-page.html)

# 3

```
cargo new project_name
cd project_name
cargo build
cargo run
cargo check
```

# 2

[Godot 4 & Rust](https://www.youtube.com/watch?v=G9UDVtR5AYA&list=PLPC9niKFOaRqYsXAapTVR8pqJmQColixs&index=1&ab_channel=schr3da)

# 1

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustup update
rustup self uninstall
```
