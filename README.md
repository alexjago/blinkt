# Blinkt_Spidev

[![MIT licensed](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Blinkt_Spidev is a Rust library that provides an interface for the Pimoroni Blinkt!, and any similar APA102 or SK9822 strips or boards, on a Raspberry Pi.

Blinkt_Spidev accesses the BCM283x GPIO peripheral through `/dev/spidev0.0` or similar. Both the original APA102 and the SK9822 clone are supported. The APA102 RGB LED/driver ICs are referred to as pixels throughout the code and documentation.

## Documentation

Documentation for the latest release of `blinkt_spidev` can be found at [docs.golemparts.com/blinkt](https://docs.golemparts.com/blinkt). Documentation for earlier releases is stored at [docs.rs/blinkt](https://docs.rs/blinkt).

This fork is quite similar, with the exception that you must specify the Linux path to the SPI device. Also, it's not published in the usual Rust infrastructure. 

## Usage

If running on a [Raspberry Pi](https://www.raspberrypi.org/documentation/hardware/raspberrypi/spi/) or similar be sure you have SPI enabled.

Add a dependency for `blinkt_spidev` to your `Cargo.toml`.

```toml
[dependencies]
blinkt_spidev = {path = /path/to/blinkt_spidev}
```

Link and import `blinkt_spidev` from your crate root.

```rust
extern crate blinkt_spidev;
```

Call `BlinktSpidev::new()` to create a new Blinkt with the default settings. In production code, you'll want to parse the result rather than unwrap it.

```rust
use BlinktSpidev::BlinktSpidev;

let mut blinkt = BlinktSpidev::new().unwrap();
```

## Example

```rust
extern crate blinkt_spidev;

use std::{thread, mem};
use std::time::Duration;

use BlinktSpidev::BlinktSpidev;

fn main() {
    let mut blinkt = BlinktSpidev::new().unwrap();
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

Blinkt Copyright (c) 2016-2018 Rene van der Meer. Released under the [MIT license](LICENSE).

Blinkt_Spidev adapted 2018 by Alex Jago. 
