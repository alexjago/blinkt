# Blinkt

[![crates.io](https://img.shields.io/crates/v/blinkt.svg)](https://crates.io/crates/blinkt)
[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Blinkt is a Rust library that provides an interface for the Pimoroni Blinkt!, and any similar APA102 strips or boards, on a Raspberry Pi.

Blinkt accesses the BCM2708/BCM2709 GPIO peripheral either through `/dev/gpiomem` (preferred) or `/dev/mem`. Both the original APA102, and the alternate version with a smaller, darker die, are supported. The APA102 RGB LED/driver ICs are referred to as pixels throughout the code and documentation. 

## Documentation

All documentation can be found at [doc.golemparts.com/blinkt](https://doc.golemparts.com/blinkt).

## Usage

Add a dependency for `blinkt` to your `Cargo.toml`.

```toml
[dependencies]
blinkt = "0.1"
```

Link and import `blinkt` from your crate root.

```rust
extern crate blinkt;
```

Call `Blinkt::new()` to create a new Blinkt with the default settings. In production code, you'll want to parse the result rather than unwrap it.

```rust
use blinkt::Blinkt;

let mut blinkt = Blinkt::new().unwrap();
```

## Example

```rust
extern crate blinkt;

use std::{thread, mem};
use std::time::Duration;
 
use blinkt::Blinkt;

fn main() {
    let mut blinkt = Blinkt::new().unwrap();
    let (red, green, blue) = (&mut 255, &mut 0, &mut 0);

    loop {
        blinkt.set_all_pixels(*red, *green, *blue);
        blinkt.show();

        thread::sleep(Duration::from_millis(250));

        mem::swap(red, green);
        mem::swap(red, blue);
    }
}
```

## Copyright and license

Copyright (c) 2016 Rene van der Meer. Released under the [MIT license](LICENSE).
