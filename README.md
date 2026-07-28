# learning-pandapower

This repo is for Power-system study automation in Python, built as the analytical foundation for a
Protection & Control (P&C) engineering portfolio. This repository models an extremely simple yet realistic
substation-to-feeder network and runs two fundamental protective relaying tests:
**load flow** and **short-circuit (fault) analysis**.

---

## What this project demonstrates

- Building a power network in code from parameters (buses, source, transformer, feeder, load)
- Running and interpreting a Newton-Raphson 'load flow' (bus voltages, line loading, system losses)
- Running **IEC 60909 short-circuit studies** for both maximum and minimum fault conditions
- Deriving a protective-relay **pickup window** from computed load and fault currents
- Automating a **parametric study** (load-growth sweep) to observe voltage behavior under changing demand
- Using pandapower's built-in benchmark networks (CIGRE MV) and standard-type libraries

## Why it matters for P&C engineering

Every protective relay setting starts from these two studies. A relay's pickup must sit
**above** the maximum load current but **below** the minimum fault current — a window that can
only be found by computing both. This repository generates those numbers on a modeled feeder,
demonstrating the analysis performed before ever configuring a relay.

---

## The modeled network

A 138 kV utility source feeding a 12.47 kV distribution feeder through a 25 MVA substation
transformer:

```
[Utility Grid] --( 138 kV HV Bus )--[ 25 MVA XFMR, 8% ]--( 12.47 kV MV Bus )--[ 3 km Feeder ]--( 12.47 kV Load Bus )--[ 8 MW load ]
```

The external grid is modeled with both maximum (2500 MVA) and minimum (2000 MVA) short-circuit
strengths so that best-case and worst-case fault studies can both be run.

## Notebooks

| Notebook | What it does |
|---|---|
| `01-first-load-flow` | Builds the network from parameters and solves the first load flow |
| `02-load-flow-study` | Interprets results in per-unit; runs a load-growth voltage-drop sweep |
| `03-short-circuit` | IEC 60909 max/min three-phase fault currents; derives the protection window |
| `04-builtin-networks` | Explores pandapower's built-in networks (CIGRE MV benchmark) |

## Key results

- **Load flow:** load-bus voltage sags to ~0.93 pu at peak load, outside the ±5% target band typically used for these systems
- **Short circuit:** maximum fault current at the load bus is ~4.24 kA (sets equipment ratings);
  minimum is ~3.0 kA (sets relay sensitivity).
- **Protection window:** with ~0.4 kA peak load current and ~3.0 kA minimum fault current, a relay
  pickup of ~0.55 kA sits safely between normal load and the weakest fault.

## Standards referenced

- **IEC 60909** — short-circuit current calculation
- **IEC 60255 / IEEE C37.112** — inverse-time overcurrent relay characteristics

---

## Tools

Python 3.11 (conda environment `pcenv`) · pandapower · matplotlib · Jupyter

## Running it

```bash
conda activate pcenv
jupyter notebook
```

Open the notebooks in order (01 → 04). Each is self-contained and annotated with inline
notes for personal reference and theory.

---

*Part of a Protection & Control engineering portfolio. Companion projects cover a hardware
IDMT overcurrent relay, an ETAP coordination and arc-flash study, a SCADA monitoring dashboard,
a secondary current injection test set, and a substation protocol gateway.*
