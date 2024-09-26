# rails-cookie-parser

I already wrote [something similar in Node.JS](https://github.com/joshleaves/rails5-cookie-parser), and looking at [Loco](https://loco.rs/), I felt like it would be good to write some APIs in Rust and have them read sessions set in Rails.

## Usage

Simple plug-n-use, don't forget to set your `SECRET_KEY_BASE`:

```rust
  use rails_cookie_parser::RailsCookieParser;

  let parser = RailsCookieParser::default();
  let Ok(result) = parser.decipher_cookie("my_cookie--is--very--long") else {
    panic!("An error somewhere on the way");
  }
```

Since Rails 7, the default hash digest is Sha256. If you want to use your Rails5 or Rails 6 sessions:

```rust
  use rails_cookie_parser::RailsCookieParser;

  let parser = RailsCookieParser::default_rails6();
  let Ok(result) = parser.decipher_cookie("my_cookie--is--very--long") else {
    panic!("An error somewhere on the way");
  }
```

For a more complex usage, you can fine-tune things:

```rust

  use rails_cookie_parser::{RailsCookieParser, HashDigest};

  let rails_key = RailsCookieParser::new(
    "secret_key_base"                 // Secret key base
    "authenticated encrypted cookie", // Key salt
    1000,                             // Iterations
    HashDigest::Sha256                // HashDigest: Sha1 or Sha256
  );
  let Ok(result) = parser.decipher_cookie("my_cookie--is--very--long") else {
    panic!("An error somewhere on the way");
  }
```

## License

Licensed under MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT).
