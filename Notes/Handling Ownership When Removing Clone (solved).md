---
tags:
  - issue
  - issue/solved
related-to:
  - "[[Rust Ownership & Borrowing]]"
  - "[[Encabulate Performance Optimization]]"
---

> [!NOTE] NOTE
> This is an intentional copy of the [[Handling Ownership When Removing Clone]] note, with added text at the bottom to show what a solved issue note looks like. In your real workflows, you should not duplicate notes to indicate that they are solved, you should edit the existing issue note with the solution to the issue and add the #issue/solved tag.

To ensure the `Encabulate` endpoint performs as well as possible, we're removing all unnecessary `.clone()` calls from the RPC. This is easy enough to do in principle, but we run into some problems with the borrow checker while doing it in practice.

Previously, the code looked like this:

```rust
fn encabulate(amulate: String) -> Result<Encabulated<String>> {
	panadermic_verification(amulate.clone())?;
	ambifactant_processing(amulate.clone())?;

	do_encabulation(amulate)
}
```

But now we need to remove the two `.clone()` calls and everything is breaking!

```rust
fn encabulate(amulate: String) -> Result<Encabulated<String>> {
	panadermic_verification(amulate)?;
	ambifactant_processing(amulate)?;
	
	do_encabulation(amulate)
}
```

 I did what the ticket said and removed the `.clone()` calls but now the code won't compile, and the compiler is just telling me to add the `.clone()` back!
 
```
error[E0382]: use of moved value: `amulate`
 --> src/lib.rs:3:25
  |
1 | fn encabulate(amulate: String) -> Result<Encabulated<String>> {
  |               ------- move occurs because `amulate` has type `String`, which does not implement the `Copy` trait
2 |     panadermic_verification(amulate)?;
  |                             ------- value moved here
3 |     ambifactant_processing(amulate)?;
  |                            ^^^^^^^ value used here after move
  |
note: consider changing this parameter type in function `panadermic_verification` to borrow instead if owning the value isn't necessary
 --> src/lib.rs:8:37
  |
8 | fn panadermic_verification(amulate: String) -> Result<()> {
  |    -----------------------          ^^^^^^ this parameter takes ownership of the value
  |    |
  |    in this function
help: consider cloning the value if the performance cost is acceptable
  |
2 |     panadermic_verification(amulate.clone())?;
  |                                    ++++++++
```

I'm so confused. But I noticed the message `consider changing this parameter type in function 'panadermic_verification' to borrow instead if owning the value isn't necessary`. I wonder if the ticket meant for me to "add borrowing" instead of "remove cloning." Let's try that.

```rust
fn encabulate(amulate: String) -> Result<Encabulated<String>> {
	panadermic_verification(&amulate)?;
	ambifactant_processing(&amulate)?;
	
	do_encabulation(amulate)
}

fn panadermic_verification(amulate: &str) -> Result<()> {
	...
}

fn ambifactant_processing(amulate: &str) -> Result<()> {
	...
}

fn do_encabulation(amulate: String) -> Result<Encabulated<String>> {

}
```

This seems to work, so I think this was it! I guess "remove clone" goes hand in hand with "do more borrowing."