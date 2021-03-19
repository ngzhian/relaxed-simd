# Relaxed SIMD proposal

## Summary

This proposal adds a set of useful SIMD instructions that introduce local
non-determinism (where the results of the instructions may vary based on
hardware support).

## Motivation

Applications running on Wasm SIMD cannot take full advantage of hardware
capabilities. There are 3 reasons:

1. Instruction depends on hardware support
2. Approximate instructions that are underspecified in hardware
3. Some SIMD instructions penalize particular architecture

See these [slides][slides] presented at the [2021-03-16 CG meeting][cgmeeting]
for more details.

WebAssembly aims to be [portable][portable], the [SIMD proposal][simd] has been
careful to only include instructions that behave deterministically, independent
of the underlying hardware. However, there are certain instructions that, if
specified in this manner, will be too slow to be usable. Relaxed SIMD proposal
aims to examine these instructions and unlock more performance without losing
all portability guarantees.

## Overview

Broadly, there are three categories of instructions that fit into the Relaxed SIMD proposal:

1. Integer instructions where the inputs are interpreted differently (e.g.
   swizzle,  4-D dot-product)
2. Floating-point instructions whose behavior for out-of-range and NaNs differ
   (e.g. float-to-int conversions, float min/max)
3. Floating-point instructions where the precision differ (e.g. FMA, reciprocal
   instructions)

Example of some instructions we would like to add:

- Fused Multiply Add (single rounding if hardware supports it, double rounding if not)
- Approximate reciprocal/reciprocal sqrt
- Relaxed Swizzle (implementation defined out of bounds behavior)
- Relaxed Rounding Q-format Multiplication (optional saturation)

[slides]: https://docs.google.com/presentation/d/1Qnx0nbNTRYhMONLuKyygEduCXNOv3xtWODfXfYokx1Y/edit?usp=sharing
[cgmeeting]: https://github.com/WebAssembly/meetings/blob/master/main/2021/CG-03-16.md
[portable]: https://webassembly.github.io/spec/core/intro/introduction.html#design-goals
[simd]: https://github.com/WebAssembly/simd
