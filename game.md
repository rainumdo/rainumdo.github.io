# 55

smooth follow

```
following.translation.smooth_nudge(&target.translation, decay_rate, delta_time);
```

# 54

despawn

```
commands.entity(trigger.target()).despawn() // also despawn the observer
```

# 53

- A type is `Send` if it is safe to send it to another thread.
- A type is `Sync` if it is safe to share between threads (T is Sync if and only if &T is Send).

# 52

```
App::new()
    .add_plugins(
        DefaultPlugins.set(WindowPlugin {
            primary_window: Some(Window {
                title: "I am a window!".into(),
                name: Some("bevy.app".into()),
                resolution: (500., 300.).into(),
                present_mode: bevy::window::PresentMode::AutoVsync,
                fit_canvas_to_parent: true,
                window_theme: Some(WindowTheme::Dark),
                enabled_buttons: EnabledButtons {
                    maximize: false,
                    ..default()
                },
                // visible: false,
                ..default()
            }),
            ..default()
        })
    );
```

# 51

replace .0

```
#[derive(Component, Dered, DerefMut)]
struct AnimationTimer(Timer);
```

# 50

ListNode

```
#[derive(Debug)]
pub struct ListNode {
    pub val: i32,
    pub next: Option<Box<ListNode>>,
}
impl ListNode {
    pub fn new(val: i32) -> Self {
        ListNode { val, next: None }
    }
}

pub fn main() {
    let mut node3 = ListNode::new(3);
    node3.next = None;
    let mut node2 = ListNode::new(2);
    node2.next = Some(Box::new(node3));
    let mut node1 = ListNode::new(1);
    node1.next = Some(Box::new(node2));
    let mut cur = Some(Box::new(node1));
    while let Some(n) = cur {
        println!("{:?}", &n);
        cur = n.next;
    }
}
```

# 49

error: Unknown binary 'rust-analyzer' in official toolchain 'stable-x86_64-unknown-linux-gnu'.

```
rustup component add rust-analyzer
```

# 48

[WebGPU Live Demo Editor](https://www.wgsl.dev/editor)

# 47

[bevy io for wgsl](https://github.com/bevyengine/bevy/blob/main/crates/bevy_pbr/src/render/mesh.wgsl)

# 46

[Shadertoy](https://www.shadertoy.com/)

# 45

[Option](https://doc.rust-lang.org/std/option/) => Some, None  
[Result](https://doc.rust-lang.org/std/result/index.html) => Ok, Err -> if/while let

# 44

Each bundle represents a static set of `Component` types. Currently, bundles can only contain one of each `Component`, and will panic once initialized if this is not met.

# 43

Handles act as abstract "references" to assets, whose data are stored in the `Assets<A>` resources, avoiding the need to store multiple copies of the same data.

# 42

When Ownership Moves (Transfers)

1. Assignment: `let x = y;` (where y is non-Copy type)
2. Function calls: Passing non-Copy values to functions
3. Returns: Returning values from functions
4. Collections: Inserting/removing from `Vec`, `HashMap`, etc.
5. Structs/Enums: Assigning to fields or variants
6. Pattern matching: Destructuring with `let` or `match`
7. Closures: Using move keyword or capturing values
8. Operators: Some overloaded operators (like `+` for Strings)
9. Loops: Using `into_iter()` explicitly or implicitly

# 41

[docs.rs](https://docs.rs)

# 40

trigger

```
fn attack_hits(trigger: Trigger<Attack>, name: Query<&Name>) {
    if let Ok(name) = name.get(trigger.target()) {
        info!("Attack hit {}", name);
    }
```

# 39

tests with panic

```
#[should_panic = "Rectangle width and height must be positive"]
```

# 38

generics and traits

```
impl<T: std::fmt::Display> ReportCard<T> {
    fn print(&self) -> String {
        format!(
            "{} ({}) - achieved a grade of {}",
            &self.student_name, &self.student_age, &self.grade,
        )
    }
}
```

# 37

`MeshPickingPlugin`, `observe`

# 36

unit struct is just a tag.  

# 35

parameters of function is also declaration.  
[mut](https://doc.rust-lang.org/std/keyword.mut.html)

# 34

deref coercion

# 33

The `+` operator can concatenate a `String` with `&str`

# 32

[rustlings](https://rustlings.rust-lang.org/)

# 31

[Blockchain in Rust](https://www.youtube.com/playlist?list=PLwnSaD6BDfXL0RiKT_5nOIdxTxZWpPtAv)

# 30

- `a : &T`: 引用不可变，且引用指向的内容不可修改。
- `a : &mut T`: 引用不可变，引用指向内容可以修改。
- `mut a: &T`: 引用可变（a可重新赋值），但引用指向内容不可修改。
- `mut a: &mut T` 引用可变，引用指向内容可以修改。

# 29

win install rust with china mirror

```
$ENV:RUSTUP_DIST_SERVER='https://mirrors.ustc.edu.cn/rust-static' 
$ENV:RUSTUP_UPDATE_ROOT='https://mirrors.ustc.edu.cn/rust-static/rustup'

.\rustup-init.exe
```

# 28

```
cargo build --timings
cargo tree -e feature -i <crate>
```

# 27

```
fn fill_vec(mut vec: Vec<i32>)
vec本身可变

fn fill_vec(vec: &mut Vec<i32>)
vec绑定内容可变

```

# 26

Blocking waiting for file lock on package cache

```
rm -rf ~/.cargo/.package-cache

lsof | grep .package-cache
kill -9 <PID>
```

# 25

[Hexagonal Grids](https://www.redblobgames.com/grids/hexagons/)

# 24

[Installing Linux dependencies](https://github.com/bevyengine/bevy/blob/main/docs/linux_dependencies.md)

# 23

[Making a Flappy Bird Game with Bevy & Rust in under 10 Minutes](https://www.youtube.com/watch?v=_C28kqin94c&ab_channel=BipedPotato)

# 23

[Unofficial Bevy Cheat Book](https://bevy-cheatbook.github.io/introduction.html)

# 22

[Leaving Rust gamedev after 3 years](https://loglog.games/blog/leaving-rust-gamedev/)

# 21

Option 代表可能为空可能有值的一种类型，本质上是一个枚举，有两种分别是 Some 和 None。Some 代表有值，None 则类似于 null，代表无值。

# 20

不可变引用可以重新分配，但是不能修改引用的值  
可变引用可以修改引用的值，但不能重新分配

# 19

When doing assignments (let x = y) or passing function arguments by value (foo(x)), the ownership of the resources is transferred. In Rust-speak, this is known as a move. After moving resources, the previous owner can no longer be used. This avoids creating dangling pointers.

# 18

[steamdb](https://steamdb.info)

# 17

`#![outer_attribute]` applies to the item immediately following it. Some examples of items are: a function, a module declaration, a constant, a structure, an enum.

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

可变引用提供对数据“唯一”且""访问  
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
