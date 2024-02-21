Rust requires developers to adhere to a few rules for how memory is used. This system, called "Ownership and Borrowing" is documented in [TRPL Ch. 4 - Understanding Ownership](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html).
# Ownership Rules ([source](https://doc.rust-lang.org/book/ch04-01-what-is-ownership.html#ownership-rules))
- Each value in Rust has an _owner_.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.
# The Rules of References ([source](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html#the-rules-of-references))
- At any given time, you can have _either_ one mutable reference _or_ any number of immutable references.
- References must always be valid.
# Issues
```dataview
list from #issue and -#issue/solved where related-to = this.file.link or contains(related-to, this.file.link)
```
## Solved Issues
```dataview
list from #issue/solved where related-to = this.file.link or contains(related-to, this.file.link)
```
