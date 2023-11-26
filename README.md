# 一些学习记录
## RUST
### lines()函数作用
在 Rust 中，lines() 是字符串类型 &str 的一个方法，用于按行迭代字符串。这个方法返回一个迭代器，该迭代器产生字符串切片，每个字符串切片都代表原始字符串中的一行。

rust
Copy code
fn main() {
    let contents = "Line 1\nLine 2\nLine 3";

    for line in contents.lines() {
        println!("{}", line);
    }
}
在这个例子中，contents 是一个包含三行文本的字符串。通过调用 lines() 方法，我们可以遍历这个字符串的每一行，并使用 for 循环逐行打印。

输出结果将是：

mathematica
Copy code
Line 1
Line 2
Line 3
lines() 方法在处理文本文件等每行以换行符分隔的文本时非常有用。它能够自动处理不同操作系统的行结束符（例如，\n 或 \r\n），并返回一个适应这些差异的迭代器。