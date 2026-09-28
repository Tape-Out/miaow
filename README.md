# miaow

The University of Wisconsin's [MIAOW](https://github.com/VerticalResearchGroup/miaow), a compute unit for the AMD Southern Islands ISA, taken as a black box.

![maturity](https://img.shields.io/badge/maturity-planned-lightgrey) ![license](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0%20OR%20MulanPSL--2.0-blue) ![upstream](https://img.shields.io/badge/upstream-BSD--3--Clause-lightgrey)

Part of the [Tape-Out](https://github.com/Tape-Out) IP library, wired up by [`xirang`](https://github.com/Tape-Out/xirang). One submodule, not modified: `third_party/miaow`.

## What this repository adds

Thirty of upstream's sources are `.vp` files, Verilog with embedded Perl, and exist as Verilog only after `scripts/veriperl.pl` has run. The manifest's `setup: vp` task runs it into `build/vp/`, and the gate runs that task before anything else. Four of the thirty also ship as checked-in `.v`; upstream's Makefile copies those over the generated ones, and so does this task.

`veriperl.pl` runs each Perl block as `./<output>-veriperl-N.pl`, so an absolute output path makes every block fail without an error and leaves the Perl in place as comments. The task writes relative paths and refuses any output that did not grow.

The top is `compute_unit`, the ASIC flavour. The FPGA one needs a `block_ram` that only Vivado generates. The macros in `*_definitions.v` are used across files, so the sources form one compilation unit.

```console
$ ran run miaow vp
$ ran test miaow
```

## Endpoints

`dispatch` takes wavefronts from a dispatcher, `ifetch` reads instructions and `mem` is the load–store unit's port to memory. The other 156 outputs feed upstream's trace monitor and are not declared.

## Limits

One compute unit, with its configuration fixed in the macros. Upstream's testbench needs VCS and its DirectC models, so none of it runs here: the gate elaborates the design and checks the declaration against it.

## License

This repository: 任选其一 [MIT](LICENSE-MIT) · [Apache 2.0](LICENSE-APACHE) · [木兰宽松许可证 第2版](LICENSE-MULAN).

`third_party/miaow` stays under **BSD-3-Clause**; see its `COPYING`.
