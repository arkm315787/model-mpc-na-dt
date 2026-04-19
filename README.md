# Model MPC na dt

Interactive interview demo for a hierarchical MPC concept applied to a data-center energy orchestration problem.

The demo is intentionally lightweight:

- single-file browser dashboard
- synthetic but plausible energy, thermal, and workload signals
- upper-layer economic MPC story over 24 hours
- lower-layer tracking / thermal MPC story over 1 hour
- sliders and preset scenarios for live walkthroughs

## How to run

### Easiest way

Open [index.html](C:/Users/arshah/Model-MPC-na-dt/index.html) in your browser.

### One-click on Windows

Double-click `open-demo.bat`.

## What to show in the interview

1. Start with the architecture section and explain why the control problem is split into two layers.
2. Use the forecast charts to explain what the upper controller sees.
3. Show the upper-layer outputs: grid target, battery schedule, and thermal pre-cooling target.
4. Move the `Lower-layer zoom hour` slider to the evening peak and explain how the lower controller handles disturbance.
5. Switch presets such as `Heat Wave` or `Grid Stress` to show safety-first behavior.

## Files

- `index.html`: the self-contained interactive dashboard
- `DEMO_NOTES.md`: your speaking notes and safe interview claims
- `open-demo.bat`: opens the demo locally

## Notes

This is not presented as a production controller.

It is a demo artifact meant to communicate:

- system decomposition
- objective design
- hard vs soft constraints
- fallback logic
- observability and operational thinking
