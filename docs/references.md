# References

## Standard
- IEEE 802.15.4-2024, §10.4 (Deterministic and Synchronous Multi-channel Extension, DSME) —
  used for the `SO ≤ MO ≤ BO` constraint, the multi-superframe/N formula, the DSME PAN
  Descriptor IE, and the §10.4.6.3 GTS deallocation procedure that Algorithm 7 relies on.

## Base project
- Ray & Moulik, "STREAM-E: A Streamlined Resource Allocation in Multichannel MAC for DSME
  With Energy-Aware Extension for Constrained-IIoT," IEEE Transactions on Industrial
  Informatics, 2025. https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=11152312
  — source of Algorithms 1-4 (GTS scheduling, CAP/CFP channel assignment, energy-aware
  extension) that Algorithms 5-8 extend.
- STREAM-E's own C++ DSME MAC simulator, cited as ref. [29] in the paper above:
  `github.com/ray-of-Code/DSME_MAC_Simulation` — not independently verified reachable at
  time of writing; check it directly before committing to it as an implementation base.

## Related work (naming context, not the same mechanism)
- Lee et al., "Traffic-Adaptive CFP Extension for IEEE 802.15.4 DSME MAC," IEEE Access,
  2021 — the original "TaCFPext," a distributed, per-node mechanism that converts part of
  the CAP into an extended CFP. Our TaCFPext-MO borrows the name but is a different,
  coordinator-centric mechanism built on `macMultisuperframeOrder` — worth a explicit
  contrast paragraph in the final report/related-work section.

## Candidate DSME implementations (platform decision — see Status in root README)
- **openDSME** — `github.com/RIOT-OS/openDSME`. Open, portable IEEE 802.15.4 DSME
  implementation from Hamburg University of Technology / HAW Hamburg. Confirmed native
  support in **RIOT-OS** and an **OMNeT++/INET** integration (`opp_env install
  opendsme_allinone-latest`). Academic literature on openDSME also mentions an
  integration into "Contiki" (legacy, pre-Contiki-NG) with Cooja — **this needs direct
  verification against current Contiki-NG** before relying on it; the two codebases
  diverged significantly after the Contiki-NG fork.
- If no clean Contiki-NG path exists, the lowest-risk option is extending STREAM-E's own
  C++ simulator directly (see above) — it already implements the exact baseline
  (Algorithms 1-4) this project builds on, and keeps results directly comparable to
  STREAM-E's published figures.
