# The Game of Life on the micro::bit v2

## Description

This is a basic Rust implementation for John Conway's "Game of Life" running on the
micro:bit v2 microcontroller's 5x5 LED matrix. The program rules are as follows:

- The game starts with a random world state and runs at 10 frames per second.
- As long as the A button is held, the board will generate a random world state every frame.
- Otherwise, if the B button is held, the board will invert its state and block the B button
  for 5 frames (or half a second).
- If no button is held, the board will take a normal game of life step.
- If the game of life terminates (i.e. all cells are dead), the board will wait either for
  a button press or for 5 frames, upon which a random world state is generated and the game
  begins once again.

## Build and run

- To build the application without flashing: `cargo build --release` from repo root
- To build the application and flash to the micro::bit: `cargo embed --release` from repo root

## Implementation details

Board randomization utilizes the micro:bit v2's hardware random number generator,
which is imported through the `microbit::hal::Rng` module.

Reset buffers--such as the 5-frame lockout of the B button and the 5-frame reset
of the world state following completion--are handled by tracking integer values.
These values start at the frame reset count value (i.e. 5), and are checked
at the beginning of every frame. If their value is less than 5, it is incremented;
otherwise nothing happens. When the game reaches a completion state or the B button
is pressed, the corresponding buffer count is reset to 0. This implementation
could probably be done more elegantly with a timer, but this solution works fine
enough!


## Attribution

All code in the `life.rs` file was provided by [Bart Massey](https://github.com/BartMassey).
