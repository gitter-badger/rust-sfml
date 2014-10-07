rust-sfml [![Build Status](https://api.travis-ci.org/jeremyletang/rust-sfml.png?branch=master)](https://travis-ci.org/jeremyletang/rust-sfml)
=========
[![Gitter](https://badges.gitter.im/Join Chat.svg)](https://gitter.im/jeremyletang/rust-sfml?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


SFML bindings for Rust

This is a Rust binding for SFML, the Simple and Fast Multimedia Library, developed by Laurent Gomila.

SFML website : www.sfml-dev.org

Installation
============

You must install the SFML2.1 and CSFML2.1 libraries on your computer which are used for the binding.

SFML2.1: http://www.sfml-dev.org/download/sfml/2.1/

CSFML2.1: http://www.sfml-dev.org/download/csfml/

Then clone the repo and build the library with the following command.

You can build rust-sfml using different version of Rust compiler:

| Rust version | rust-sfml
|--------------|----------
| Rust master  | [link rsfml](https://github.com/JeremyLetang/rust-sfml/)
| Rust 0.11    | [link rsfml](https://github.com/JeremyLetang/rust-sfml/releases/tag/rust0.11)
| Rust 0.10    | [link rsfml](https://github.com/JeremyLetang/rust-sfml/releases/tag/rust0.10)
| Rust 0.9     | [link rsfml](https://github.com/JeremyLetang/rust-sfml/releases/tag/rust0.9)
| Rust 0.8     | [link rsfml](https://github.com/JeremyLetang/rust-sfml/releases/tag/rust0.8)


Rust-sfml is built with make:

```Shell
> make
```

This command build rsfml, the examples, and the documentation.

You can also build them separatly:

```Shell
> make rsfml
> make examples
> make docs
```

Alternatively you can use Cargo:
```Shell
> cargo build
```

This will build rust-sfml and all the examples.



Rust-sfml works on Linux, Windows and OSX.


### Windows 32 bits bug

According to the issue #10, there is problem to use rsfml on Windows.
It seems to be a bug in the version of llvm used by Rust. Krzat made a patch to solve this problem, you can find it here: mozilla/rust#11198

OSX Specific
============

On OSX window must be launched in the main thread. You should override the Rust runtime start function.

```Rust
#[cfg(target_os="macos")]
#[start]
fn start(argc: int, argv: **u8) -> int {
    native::start(argc, argv, main)
}
```

Short example
=============

Here is a short example, draw a circle shape and display it.

```Rust
extern crate native;
extern crate rsfml;

use rsfml::system::Vector2f;
use rsfml::window::{ContextSettings, VideoMode, event, Close};
use rsfml::graphics::{RenderWindow, RenderTarget, CircleShape, Color};

#[start]
fn start(argc: int, argv: *const *const u8) -> int {
    native::start(argc, argv, main)
}

fn main () -> () {
    // Create the window of the application
    let mut window = match RenderWindow::new(VideoMode::new_init(800, 600, 32),
                                             "SFML Example",
                                             Close,
                                             &ContextSettings::default()) {
        Some(window) => window,
        None => fail!("Cannot create a new Render Window.")
    };

    // Create a CircleShape
    let mut circle = match CircleShape::new() {
        Some(circle) => circle,
        None       => fail!("Error, cannot create ball")
    };
    circle.set_radius(30.);
    circle.set_fill_color(&Color::red());
    circle.set_position(&Vector2f::new(100., 100.));

    while window.is_open() {
        // Handle events
        for event in window.events() {
            match event {
                event::Closed => window.close(),
                _             => {/* do nothing */}
            }
        }

        // Clear the window
        window.clear(&Color::new_RGB(0, 200, 200));
        // Draw the shape
        window.draw(&circle);
        // Display things on screen
        window.display()
    }
}
```


License
=======

This software is a binding of the SFML library created by Laurent Gomila, which is provided under the Zlib/png license.

This software is provided under the same license than the SFML, the Zlib/png license.

