# Dissertation fork — refill_arbiter capture-on-valid fix

A fork of [`pulp-platform/hier-icache`](https://github.com/pulp-platform/hier-icache)
carrying an instruction-cache fix needed for the Streaming Engine
evaluation in the master's dissertation:

> **Configurable Streaming Engine for RISC-V Systems**
> Gonçalo Pereira — Faculdade de Engenharia da Universidade do Porto (FEUP)

The upstream `README.md` is preserved alongside this file.

## The `axi_node_dep` branch

On top of the upstream `axi_node_dep` branch, this fork fixes
`RTL/L1_CACHE/refill_arbiter.sv` to capture the response only on
`arbiter_r_valid_i`, rather than continuously while a request is
outstanding. The original behaviour let a core latch another bank's
transient response, corrupting the cache fill under concurrent fetches. The
fix is detailed in the dissertation's implementation chapter.

## Where it sits

Cloned into `ips/hier-icache/` by the top-level
[`pulp_streaming_engine`](https://github.com/goncalop00/pulp_streaming_engine)
fork through its `ips_list.yml` manifest. The `upstream` Git remote is
preserved, so `git diff upstream/axi_node_dep..origin/axi_node_dep` shows the
complete delta.

## License

Inherits the Solderpad Hardware License v0.51 from the upstream repository.