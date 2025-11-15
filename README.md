#### buffy_stack_format_write_module

# Buffy - Stack-Based Character Formatting and Writing/Printing (Rust)
- zero-heap string formatting and terminal output for TUI applications, embedded systems, and safety oriented code.
- License: MIT

## Buffy Format / Write

Buffy is a Rust library that provides string formatting and terminal output **without no heap allocation**. All operations use stack-allocated, pre-allocated, buffers, making it suitable for:

- **TUI (Terminal User Interface) applications** - Direct terminal output with ANSI styling
- **Embedded systems** - Predictable memory usage, no allocator required
- **Safety-critical applications** - Bounded memory, no runtime allocation failures
- **Performance-sensitive code** - Zero allocation overhead
- **Resource-constrained environments** - Fixed memory footprint

## Goal-Standards:  (like still finding bugs and oversights)
- No `String` objects created
- No `Vec` allocations
- No `.to_string()` calls
- No dynamic memory allocation
- All conversions use stack buffers
- Output written directly to destination

# Features

### Data Types Supported
- (Modular for additional needed types)
- **Unsigned integers**: `u8`, `u16`, `u32`, `u64`, `usize`
- **Signed integers**: `i8`, `i16`, `i32`, `i64`, `isize`
- **Hexadecimal**: `U8Hex`, `U16Hex`, `U32Hex`
- **Strings**: `&str` (borrowed)
- **Primitives**: `bool`, `char`
- **Paths**: `&Path`
- **Styled variants**: All types with ANSI color/formatting

### Formatting Features
- **Alignment**: Left (`{:<N}`), Right (`{:>N}`), Center (`{:^N}`)
- **ANSI Styling**: Colors, bold, underline, italic, dim
- **Direct output**: Write to stdout, stderr, files, or any `Write` trait
- **Incremental output**: Build output piece-by-piece without intermediate buffers

### Safety Features
- **No panics in production**: All errors return `Result` or `Option`
- **Bounded operations**: Max 8 arguments per format call (prevents stack overflow)
- **Explicit limits**: Max 64-character width for alignment, 20-byte number buffers
- **Defensive programming**: All conversions validated, graceful degradation


# Examples:

- Style
```
            buffy_print(
                "{}",
                &[FormatArg::U8HexStyled(
                    *byte,
                    Style {
                        fg_color: Some(RED),
                        bg_color: Some(BG_WHITE),
                        bold: true,
                        ..Default::default()
                    },
                )],
            )?;
```

- Print a line in modular sections
```

    // First part: quit, save, undo, delete
    buffy_print(
        "{}q{}uit {}s{}ave {}u{}ndo {}d{}el ",
        &[
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
        ],
    )?;

    // Second part: insert, view, hex, help
    buffy_print(
        "{}i{}ns {}v{}iew {}h{}ex {}?{}help",
        &[
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
            FormatArg::Str(RED),
            FormatArg::Str(YELLOW),
        ],
    )?;

    buffy_println("{}", &[FormatArg::Str(RESET)])?;
    buffy_println("", &[])?;
```

#### Note: With assistance from Claude Sonnet 4.5
