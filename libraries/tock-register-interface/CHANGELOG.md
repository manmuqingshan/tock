# Changelog

## master

## v0.10.0

This release contains some improvements to `tock-registers`. It should
not contain breaking changes for the majority of users, except for
those who provide custom implementations of the `UIntLike` trait.

### **Breaking Changes**

To support new debug infrastructure for registers, the `UIntLike` trait now has
a supertrait-bound for the core library's `Debug` trait. To upgrade to this
version, users who have custom implementations of `UIntLike` for downstream
types will need to ensure that their types also implement the `Debug` trait.

### Register Debugging Support

https://github.com/tock/tock/pull/3771
84a281c4a640f0216112affd3da16963d804e7ef
add `debug()` method in registers for better human readable debug output

This version adds a `debug()` method to registers implementing the `Readable`
trait (through an auto-impl of the new `Debuggable` trait). It performs a read
of the underlying register value and returns a type that implements
`core::fmt::Debug` to print the register's fields and current values.

### Other changes

- #4230: add an example expansion of the `register_bitfields!` macro to the
  crate's README
- #4197: various documentation improvements
- #3901: `test_fields!`: stringify offset and size in error messages. This
  allows constant expressions to be used in the `register_structs!` macro.

## v0.9

There is a small breaking change, described below, which addresses semantic
confusion around `matches_any()`. The interface has changed, so this should
result in compile-time awareness rather than any silent behavioral changes.

### **Breaking Changes**

https://github.com/tock/tock/pull/3336
480ee65139f1e51c149236a1b3e47cd61832ac80
Rename matches_any() to any_matching_bits_set(), implement matches_any()

> The current implementation of matches_any() does not implement the
> functionality the name implies. This commit renames the existing
> implementation to a name which better describes its functionality,
> and introduces a new matches_any() function (with a different interface)
> that actually correctly implements the functionality suggested by the
> name. This commit also adds several tests to the tock-registers test
> suite to verify the new version works as expected, and removes a feature
> gate on a feature that no longer exists for the crate which was
> preventing some of the tock-registers teste suite from being run as part
> of `cargo test`.

### Other changes:

 - #3687: derive `Debug` on `Value`
 - #3582: Update rust-toolchain to nightly of 2023-07-30

## v0.8.1

A point release to allow multiple invocations of `register_fields!` in
the same module. See the PR and linked Issue for details.

 - #3230: don't encapsulate test_fields! tests in mod

## v0.8

`tock-registers` now supports stable Rust!

There is a small breaking change, documented below, required to support
Rust 2021 edition. Most of the remaining changes are improvements to the
internal self-testing infrastructure and documentation. There are also
some additions to `FieldValue` to improve ergonomics.

### **Breaking Changes**

 - #2842: tock-registers: rename TryFromValue::try_from to try_from_value
 - #2838: Update to Rust 2021 edition

### Other Changes

 - #3126: [trivial] tock-registers: mark two methods as `const`
 - #3088: tock_registers/test_fields: respect struct size padding w/ alignment
 - #3072: Update Rust nightly version + Expose virtual-function-elimination
     - libraries/tock-register-interface: Fixup register_structs documentation
 - #2988: Remove const_fn_trait_bound feature and update nightly (Mar 2022)
 - #3014: tock-registers: Implement From field enum value type for FieldValue
 - #3013: tock-register-interface: Provide none method for FieldValue type
 - #2916: tock-register-interface: improve read_as_enum documentation
 - #2922: tock-register-interface: replace register tests by const assertions

## v0.7

 - #2642: Rename `IntLike` to `UIntLike` to match semantics
 - #2618: Reorganize, document, and feature-gate modules and exports
 - #2589: Upgrade nightly for `const_fn` -> `const_fn_trait_bound`
 - #2517: Use traits for accessing / manipulating registers
 - #2512: Fix `Copy` and `Clone` implementation on `Field`
 - #2300: Add support for `usize`
 - #2220: Remove duplicate code, make local register copy read-write
 - #2210: Add `u128` to `IntLike`
 - #2197: Accept trailing comma in bitfields and bitmasks

## v0.6

 - #2095: Fix syntax errors and inconsistencies in documentation
 - #2071: Clarify bit widths in documentation examples
 - #2015: Use UnsafeCell in registers (see issue #2005)
 - #1939: Make the Field::mask and FieldValue::mask fields private
 - #1823: Allow large unsigned values as bitmasks + add bitmask! helper macro
 - #1554: Allow lifetime parameters for `register_structs! { Foo<'a> { ..`
 - #1661: Add `Aliased` register type for MMIO with differing R/W behavior

## v0.5

 - #1510
   - Register visibility granularity: don't automatically make everything
      `pub`, rather give creation macro callers visbility control.

 - #1489
   - Make `register_structs!` unit test generation opt-out, so that
     `custom-test-frameworks` environments can disable them.

 - #1481
   - Add `#[derive(Copy, Clone)]` to InMemoryRegister.

 - #1428
   - Implement `mask()` for `FieldValue<u16>` which seems to have been
     skipped at some point.
   - Implement `read()` for `FieldValue` so that individual fields
     can be extracted from a register `FieldValue` representation.

 - #1461: Update `register_structs` macro to support flexible visibility of each
   struct and each field. Also revert to private structs by default.

## v0.4.1

 - #1458: Update struct macro to create `pub` structs

## v0.4

 - #1368: Remove `new()` and add `InMemoryRegister`
 - #1410: Add new macro for generating structs

## v0.3

 - #1243: Update to Rust 2018 (nightly)
 - #1250: Doc-only: Fix some rustdoc warnings

## v0.2

 - #1161: Add `read_as_enum` to `LocalRegisterCopy`; thanks @andre-richter

## v0.1 - Initial Release
