# Cargo
Cargo is the [Rust](https://www.rust-lang.org/) [_package manager_](https://doc.rust-lang.org/cargo/appendix/glossary.html#package-manager "\"package manager\" (glossary entry)"). Cargo downloads your Rust [package](https://doc.rust-lang.org/cargo/appendix/glossary.html#package "\"package\" (glossary entry)")’s dependencies, compiles your packages, makes distributable packages, and uploads them to [crates.io](https://crates.io/), the Rust community’s [_package registry_](https://doc.rust-lang.org/cargo/appendix/glossary.html#package-registry "\"package registry\" (glossary entry)").

## Commands
* Create new project:
``` shell
cargo new <project name>
```
 * Build project
 ``` shell
 cargo build
 cargo build --release
```
* Build + Run
```shell
cargo run
cargo run --release
```
* Clean project
```shell
cargo clean
```

# Rust Compiler
`rustc` is the compiler for the Rust programming language, provided by the project itself. Most Rust programmers don't invoke `rustc` directly, but instead do it through [Cargo](https://doc.rust-lang.org/cargo/index.html).