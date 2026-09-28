# TaCFPext-MO: Overflow-Triggered Multi-Superframe Order Adaptation for DSME MAC

Extends **STREAM-E** (a priority-ranked GTS scheduling + channel assignment scheme for
IEEE 802.15.4e DSME in constrained IIoT networks) with a coordinator-side mechanism that
grows or shrinks `macMultisuperframeOrder` (MO) to stop Critical/Delay-driven (Ct/Dt)
traffic from being silently dropped once STREAM-E's priority ranking has already done all
it can.

## Motivation

STREAM-E ranks and allocates a fixed pool of `s` GTS slots among competing devices each
multi-superframe cycle (Ct > Dt > Rt > Nt). Under sustained overload, even top-ranked Ct/Dt
devices can be denied a slot — the packet is dropped with no further recourse at the MAC
layer. TaCFPext-MO removes that ceiling by growing `s` itself (via MO) when the drop is
sustained, and shrinking it back once the extra capacity is no longer earning its keep —
without touching STREAM-E's own ranking logic, `SO`, `BO`, or CAP timing.

## Contribution (Objective 4)

Four algorithms, numbered 5–8 to continue STREAM-E's own Algorithm 1–4 sequence:

| # | Name | Role |
|---|---|---|
| 5 | Detection | Per-cycle windowed drop/utilization tracking; decides grow vs. shrink vs. hold |
| 6 | Growth | Deficit-sized, step-capped MO increase; always append-only (non-disruptive) |
| 7 | Shrink | Feasibility-checked MO decrease; never evicts active Ct/Dt GTS |
| 8 | Broadcast & Resync | Commits the change atomically at the next Enhanced Beacon, per §10.4.3 |

Full pseudocode, the standard-compliance derivation, and a 12-point failure-mode analysis
are in [`docs/TaCFPext-MO_Algorithms5-8.pdf`](docs/TaCFPext-MO_Algorithms5-8.pdf).

## Repository Structure

```
TaCFPext-MO/
├── docs/            Design document (Algorithms 5-8), references
├── src/             Implementation (platform TBD — see Status)
├── sim/             Simulation scenarios / configs
├── results/         Raw output, plots, analysis notebooks
├── presentation/    Final presentation deck
└── README.md
```

## Status

- [x] Baseline reviewed — STREAM-E Algorithms 1-4 (GTS scheduling, CAP/CFP channel
      assignment, energy-aware extension)
- [x] TaCFPext-MO design (Algorithms 5-8) — see `docs/`
- [ ] **Platform decision** — confirm a working DSME MAC implementation for the target
      simulator (see `docs/references.md`: Contiki-NG support is unverified; RIOT-OS /
      OMNeT++ have a maintained one via openDSME; STREAM-E's own C++ simulator is a
      lower-risk fallback)
- [ ] Baseline implementation + validation against STREAM-E's published numbers
- [ ] TaCFPext-MO implementation on top of the validated baseline
- [ ] Experiment design (traffic scenarios that stress Ct/Dt overflow) + parameter sweep
- [ ] Results & analysis
- [ ] Final presentation & report

## Standards & References

See [`docs/references.md`](docs/references.md).

## Getting Started

Pending the platform decision above — this section will hold build/run instructions
once the simulation target is fixed.

## License

MIT — see [`LICENSE`](LICENSE). Swap this out if your department requires a different one.
