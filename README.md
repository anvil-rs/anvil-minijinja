# anvil-minijinja

An [Anvil](https://github.com/anvil-rs/anvil) integration for [MiniJinja](https://github.com/mitsuhiko/minijinja) templates.

## Installation

```toml
[dependencies]
anvil-minijinja = "0.2.1"
minijinja = "2.8.0"
minijinja-embed = "2.8.0"  # Required for template portability
serde = { version = "1.0", features = ["derive"] }
```

## Usage

```rust
use anvil::Forge;
use anvil_minijinja::prelude::*;  // Import extension traits and functions
use serde::Serialize;

// Define a serializable template context
#[derive(Serialize)]
struct MyTemplate {
    name: String,
}

// Use macro to implement the Shrine trait
make_minijinja_template!(MyTemplate, "greeting.j2");

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let template = MyTemplate { name: "World".to_string() };
    
    // Generate a new file
    generate(&template).forge("hello.txt")?;
    
    // Append to an existing file
    append(&template).forge("log.txt")?;
    
    Ok(())
}
```

## Template Portability with minijinja-embed

For portable templates embedded in your binary:

1. Create a templates directory in your project root
2. Setup build.rs:
```rust
// build.rs
fn main() {
    minijinja_embed::export_templates!("templates");
}
```

3. The `make_minijinja_template!` macro automatically loads templates from this embedded source.
