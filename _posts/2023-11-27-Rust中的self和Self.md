---
layout: post
title:  "Rust中的self和Self"
date: 2023-11-27 20:39:00 +0800
tags: Rust 
color: rgb(255,90,90)
cover: '../assets/rustacean-flat-happy.png'
---
# 关键字 `slef`
## `self`
`self` 可以表示路径是相对于当前模块的路径。`self` 仅可以用作路径的首段，前置不能用`::`。  

```rust
fn foo() {}
fn bar() {
    self::foo();  //self对于当前模块的路径
}
```  

同时，`self` 也可以表示方法入参：  

`self` ： 表示方法接受实例本身，所有权会发生移动。这意味着在方法调用之后，调用者将失去对实例的所有权。通常用于消耗实例的方法，例如将实例移动到其他地方，或者销毁实例。
```rust
struct MyStruct;

impl MyStruct {
    fn consume(self) {
        // 使用 self，消耗实例
        println!("Consuming the instance");
    }
}

fn main() {
    let my_instance = MyStruct;
    my_instance.consume(); // 实例被消耗，不能再被使用
}
```  
## `&self`
`&self` ： 则表示方法接受对实例的引用。这意味着在方法调用之后，调用者仍然保留对实例的所有权。通常用于读取实例的方法，而不是修改或消耗它。

```rust
struct MyStruct;

impl MyStruct {
    fn borrow(&self) {
        // 使用 &self，获得对实例的引用
        println!("Borrowing the instance");
    }
}

fn main() {
    let my_instance = MyStruct;
    my_instance.borrow(); // 实例被借用，仍然可以被使用
}
```    
## 二者异同
Rust中函数参数均需要注明类型，但是self则不需要，这是一个语法糖（`syntactic sugar`），以下示例中两两等价：

```rust
impl MyType{
    fn doit(self){}
    fn doit(self: Self){}

    fn doit(&self) {}
    fn doit(self: &Self){}

    fn doit(&mut self) {}
    fn doit(self: &mut Self) {}
}
```  
综上，上述的两个例子也可以表示如下：  
```rust
struct MyStruct;

impl MyStruct {
    fn consume(self: Self) {
        // 接受实例 self 作为参数，并且 self 的类型被显式注明为 Self
        println!("Consuming the instance");
    }
}

fn main() {
    let my_instance = MyStruct;
    my_instance.consume(); // 实例被消耗，不能再被使用
}
```  
以及：  
```rust
struct MyStruct;

impl MyStruct {
    fn borrow(self: &Self) {
        // 接受实例 self 的引用作为参数，并且 self 的类型被显式注明为 &Self
        println!("Borrowing the instance");
    }
}

fn main() {
    let my_instance = MyStruct;
    my_instance.borrow(); // 实例被借用，仍然可以被使用
}
```  
# 关键字 `Self`
`Self` 是指一种类型，指代 `trait` 或 `impl` 块中的实现类型，或类型定义中的当前类型。  
## 在 Trait 中表示实现类型
在 `Trait` 中，`Self` 用于表示实现该 `Trait` 的类型。这意味着在 `Trait` 的方法中，`Self` 表示调用该方法的具体类型。例如：
```rust
trait MyTrait {
    fn create_instance() -> Self;
}

struct MyStruct;

impl MyTrait for MyStruct {
    fn create_instance() -> Self {
        MyStruct
    }
}
```  
在这个例子中，`Self` 表示实现 `MyTrait` 的类型，即 `MyStruct`。

## 在 impl 块中表示当前类型
在 `impl` 块中，`Self` 用于表示当前正在实现方法的类型。这样可以在方法实现中引用当前类型，而不必明确指定类型的名称。例如：
```rust
struct MyStruct;

impl MyStruct {
    fn new() -> Self {
        MyStruct
    }
}
```
在这个例子中，`Self` 表示当前正在实现方法的类型，即 `MyStruct`。这使得在方法内部创建新实例时不必明确指定类型。

## 在类型定义中表示当前类型
在类型定义中，`Self` 用于表示当前类型本身。这可以在 `trait` 定义中用于关联类型（`associated type`）。例如：
```rust
struct MyStruct {
    elem: i32,
    // `Self` 在这里是 `MyStruct`。
    next: Option<Box<Self>>,
}
```  
在这个例子中，`Self::Output` 表示当前 `trait` 实现的类型的相关联类型。实现这个 `trait` 时，可以为 `Output` 指定具体的类型。