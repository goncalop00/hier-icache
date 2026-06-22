# Dissertation fork — refill_arbiter capture-on-valid fix

This fork of [`pulp-platform/hier-icache`](https://github.com/pulp-platform/hier-icache)
carries an instruction-cache fix required for the Streaming Engine
evaluation in the master's dissertation:

> **Configurable Streaming Engine for RISC-V Systems**
> Gonçalo Pereira — Faculdade de Engenharia da Universidade do Porto (FEUP)

The upstream `README.md` is preserved alongside this file.

## Dissertation branch

Work branch: **`axi_node_dep`** (same name as upstream).

It adds, on top of the upstream `axi_node_dep` branch:

- `RTL/L1_CACHE/refill_arbiter.sv` — in the `USE_RESP_BUFF` path, the response
  data was being captured continuously while a request was outstanding rather
  than only on the cycle the interconnect committed the correct response.
  Because the combinationally-muxed `arbiter_r_data_i` carried transient data
  from whichever bank happened to be responding at that cycle (potentially for
  a different core), a core waiting on bank 0 could intermittently latch
  bank 5's response for cores 1–7, corrupting the cache fill. Fix: capture
  only on `arbiter_r_valid_i`.
- Diagnostic `$display` blocks added during bring-up under
  `\`ifndef SYNTHESIS` guards in `share_icache_controller.sv` and
  `icache_hier_top.sv`.

The cache symptom was full-cluster sporadic mis-execution under SE-pop builds
at MINI; the bug was visible because cores 1–7 fetch concurrently during
firmware startup. The fix is paired with the AXI ID-truncation fix in the
top-level `pulp_cluster` to fully close the cluster's icache integration on
the dissertation work branch.

## Where it sits in the chain

Cloned into `ips/hier-icache/` by the top-level
[`pulp_streaming_engine`](https://github.com/goncalop00/pulp_streaming_engine)
fork through its `ips_list.yml` manifest. The IP is normally pulled
transitively as a dependency of `pulp_cluster`; the dissertation `ips_list.yml`
adds an explicit top-level pin to override the transitive clone with this
fork.

## Provenance

The original `pulp-platform/hier-icache` URL is preserved as the `upstream`
Git remote. A `git diff upstream/axi_node_dep..origin/axi_node_dep` shows the
complete dissertation delta.

## License

Inherits the Solderpad Hardware License v0.51 from the upstream repository.