# Dissertation fork — refill_arbiter capture-on-valid fix (hier-icache)

Fork of [`pulp-platform/hier-icache`](https://github.com/pulp-platform/hier-icache)
with an instruction-cache fix needed for the Streaming Engine evaluation, for
the MSc dissertation *Configurable Streaming Engine for RISC-V Systems*
(Gonçalo Pereira, FEUP).

## `axi_node_dep` branch

On top of the upstream `axi_node_dep` branch, this fork fixes
`RTL/L1_CACHE/refill_arbiter.sv` to capture the response only on
`arbiter_r_valid_i`, rather than continuously while a request is outstanding.
The original behaviour let a core latch another bank's transient response,
corrupting the cache fill under concurrent fetches. Detailed in the
dissertation's implementation chapter.

## Where it sits

Cloned into `ips/hier-icache/` by the top-level
[`pulp_streaming_engine`](https://github.com/goncalop00/pulp_streaming_engine)
fork. The `upstream` remote is kept, so
`git diff upstream/axi_node_dep..origin/axi_node_dep` shows the full delta.

## License

Solderpad Hardware License v0.51 (inherited from upstream).
