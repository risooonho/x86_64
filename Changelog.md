# 0.9.2

- Remove the `cast` dependency ([#124](https://github.com/rust-osdev/x86_64/pull/124))

# 0.9.1

- Improve PageTableIndex and PageOffset ([#122](https://github.com/rust-osdev/x86_64/pull/122))

# 0.9.0

- **Breaking:** Return the UnusedPhysFrame on MapToError::PageAlreadyMapped ([#118](https://github.com/rust-osdev/x86_64/pull/118))
- Add User Mode registers ([#119](https://github.com/rust-osdev/x86_64/pull/119))

# 0.8.3

- Allow immediate port version of in/out instructions ([#115](https://github.com/rust-osdev/x86_64/pull/115))
- Make more functions const ([#116](https://github.com/rust-osdev/x86_64/pull/116))

# 0.8.2

- Add support for cr4 control register ([#111](https://github.com/rust-osdev/x86_64/pull/111))

# 0.8.1

- Fix: Add required reexport for new UnusedPhysFrame type ([#110](https://github.com/rust-osdev/x86_64/pull/110))

# 0.8.0

- **Breaking:** Replace `ux` dependency with custom wrapper structs ([#91](https://github.com/rust-osdev/x86_64/pull/91))
- **Breaking:** Add new UnsafePhysFrame type and use it in Mapper::map_to ([#89](https://github.com/rust-osdev/x86_64/pull/89))
- **Breaking:** Rename divide_by_zero field of interrupt descriptor table to divide_error ([#108](https://github.com/rust-osdev/x86_64/pull/108))
- **Breaking:** Introduce new diverging handler functions for double faults and machine check exceptions ([#109](https://github.com/rust-osdev/x86_64/pull/109))
- _Possibly Breaking:_ Make Mapper trait object safe by adding `Self: Sized` bounds on generic functions ([#84](https://github.com/rust-osdev/x86_64/pull/84))


# 0.7.7

- Add `slice` and `slice_mut` methods to IDT ([#95](https://github.com/rust-osdev/x86_64/pull/95))

# 0.7.6

- Use repr C to suppress not-ffi-safe when used with extern handler functions ([#94](https://github.com/rust-osdev/x86_64/pull/94))

# 0.7.5

- Add FsBase and GsBase register support ([#87](https://github.com/rust-osdev/x86_64/pull/87))

# 0.7.4

- Remove raw-cpuid dependency and use rdrand intrinsics ([#85](https://github.com/rust-osdev/x86_64/pull/85))
- Update integration tests to use new testing framework ([#86](https://github.com/rust-osdev/x86_64/pull/86))

# 0.7.3

- Add a new `OffsetPageTable` mapper type ([#83](https://github.com/rust-osdev/x86_64/pull/83))

# 0.7.2

- Add `instructions::bochs_breakpoint` and `registers::read_rip` functions ([#79](https://github.com/rust-osdev/x86_64/pull/79))
- Mark all single instruction functions as `#[inline]` ([#79](https://github.com/rust-osdev/x86_64/pull/79))
- Update GDT docs, add user_data_segment function and WRITABLE flag ([#78](https://github.com/rust-osdev/x86_64/pull/78))
- Reexport MappedPageTable on non-x86_64 platforms too ([#82](https://github.com/rust-osdev/x86_64/pull/82))

# 0.7.1

- Add ring-3 flag to GDT descriptor ([#77](https://github.com/rust-osdev/x86_64/pull/77))

# 0.7.0

- **Breaking**: `Port::read` and `PortReadOnly::read` now take `&mut self` instead of `&self` ([#76](https://github.com/rust-osdev/x86_64/pull/76)).

# 0.6.0

- **Breaking**: Make the `FrameAllocator` unsafe to implement. This way, we can force the implementer to guarantee that all frame allocators are valid. See [#69](https://github.com/rust-osdev/x86_64/issues/69) for more information.

# 0.5.5

- Use [`cast`](https://docs.rs/cast/0.2.2/cast/) crate instead of less general `usize_conversions` crate.

# 0.5.4

- Update dependencies to latest versions (fix [#67](https://github.com/rust-osdev/x86_64/issues/67))

# 0.5.3

- Add `PortReadOnly` and `PortWriteOnly` types in `instructions::port` module ([#66](https://github.com/rust-osdev/x86_64/pull/66)).

# 0.5.2

- Update documentation of `MappedPageTable`: Require that passed `level_4_table` is valid.

# 0.5.1

- Add `PageTable::{iter, iter_mut}` functions to iterate over page table entries.

# 0.5.0

## Breaking

- The `random` module is now a submodule of the `instructions` module.
- The `structures::paging` module was split into several submodules:
    - The `NotGiantPageSize`, `PageRange`, and `PageRangeInclusive` types were moved to a new `page` submodule.
    - The `PhysFrameRange` and `PhysFrameRangeInclusive` types were moved to a new `frame` submodule.
    - The `FrameError` and `PageTableEntry` types were moved to a new `page_table` submodule.
    - The `MapperFlush`, `MapToError`, `UnmapError`, and `FlagUpdateError` types were moved to a new `mapper` submodule.
- The `structures::paging` module received the following changes:
    - The `Mapper::translate_page` function now returns a `Result` with a new `TranslateError` error type.
    - The `NotRecursivelyMapped` error type was removed.
- The `instructions::int3` function was moved into the `instructions::interrupts` module.
- Removed some old deprecated functions.
- Made modifications of the interrupt stack frame unsafe by introducing a new wrapper type and an unsafe `as_mut` method.

## Other

- Added a new `structures::paging::MapperAllSizes` trait with generic translation methods and implement it for `MappedPageTable` and `RecursivePageTable`.
- Added a new `structures::paging::MappedPageTable` type that implements the `Mapper` and `MapperAllSizes` traits.
- Added a `software_interrupt` macro to invoke arbitrary `int x` instructions.
- Renamed the `ExceptionStackFrame` type to `InterruptStackFrame`.

# 0.4.2

- Add `RdRand::get_u{16, 32, 64}` methods
- Deprecate `RdRand::get` because it does not check for failure
- Make `RdRand` Copy

# 0.4.1

- Add support for the RdRand instruction (random number generation)

# 0.4.0

## Breaking

- Make `Mapper::map_to` and `Mapper::identity_map` unsafe because it is possible to break memory safety by passing invalid arguments.
- Rename `FrameAllocator::alloc` to `allocate_frame` and `FrameDeallocator::dealloc` to `deallocate_frame`.
- Remove `From<os_bootinfo::FrameRange>` implementation for `PhysFrameRange`
  - The `os_bootinfo` crate is no longer used by the `bootloader` crate.
  - It is not possible to provide an implementation for all `os_bootinfo` versions.

## Other

- Update to 2018 edition

# 0.3.6

- Add a `SIZE` constant to the `Page` type
- Add two interrupt tests to the `testing` sub-crate
