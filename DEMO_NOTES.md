# Loadwise interview demo notes

## What this demo is

This is a self-contained browser demo for explaining a hierarchical MPC architecture for a data-center energy orchestration problem.

It is intentionally not a production controller.

It is a communication artifact that shows:

- how I would split strategic optimization from real-time control
- which signals each layer consumes
- what outputs pass from the upper layer to the lower layer
- which constraints are hard versus soft
- how I think about safety, fallback, and observability

## What to say first

"I built this as a simplified HMPC storyboard for a data center with PV, BESS, flexible compute, and cooling assets. The point is not that this browser page is the production optimizer. The point is that the control decomposition, timescales, objectives, and safeguards are the ones I would carry into a real system."

## The core architecture

Upper layer:

- strategic or economic MPC
- horizon: 24 hours
- step: 15 minutes
- inputs: day-ahead price, carbon intensity, weather / PV forecast, IT workload forecast, battery state, thermal state
- objective: minimize net energy cost, weighted carbon, peak import, and battery degradation while respecting asset and SLA constraints
- outputs: grid power target, battery SOC target, battery dispatch plan, thermal buffering target

Lower layer:

- tracking or thermal MPC
- horizon: 30 to 60 minutes
- step: 5 minutes
- inputs: real-time inlet temperature, actual PV, actual IT load, current SOC, ambient conditions
- objective: track the upper-layer plan while maintaining thermal safety and respecting actuator limits
- outputs: fan speed, chiller loading, battery trim, alarm / rollback state

## Why hierarchical MPC here

- The market problem is slow and anticipatory. It needs forecasts and longer horizons.
- The plant problem is fast and local. It needs tracking and hard safety.
- If one controller tries to do both, it becomes harder to validate, harder to debug, and harder to operate safely.
- The interface between layers is clean: the upper layer gives targets, the lower layer executes safely.

## What is synthetic versus real

Synthetic in the demo:

- the actual time series
- the exact objective weights
- the browser-side optimization heuristics

Grounded in real-world assumptions:

- German wholesale market framing and quarter-hour resolution
- negative / low midday prices and expensive evening ramps
- data-center thermal envelope thinking
- battery reserve policy and dispatch logic
- production concerns like rollback, drift, observability, and actuator constraints

## Safe claims you can make

- "I used synthetic signals because interview demos should emphasize architecture and reasoning, not pretend to be a certified live controller."
- "In production I would replace the browser heuristics with a proper service in Python, probably with Pyomo, CVXPY, CasADi, or a similar stack depending on the formulation."
- "I would want simulation, backtesting, and hardware-in-the-loop style validation before touching a real facility."
- "The lower layer would be conservative by design and always able to give up economics to preserve thermal safety."
- "The most important thing is the contract between layers and the fallback behavior when forecasts are wrong."

## Strong interview moments

If they ask why pre-cooling matters:

- "Because thermal mass is a real storage medium. If energy is cheap at 04:00 and expensive at 18:00, pre-cooling lets us shift part of the cooling cost in time while staying within the recommended inlet range."

If they ask why not use a single optimization:

- "I would still decompose it. Even if the mathematics is unified, operations, validation, and failure handling are cleaner with a strategic layer and a tracking layer."

If they ask what makes this production-grade:

- "Not the exact optimizer in this file. Production-grade means monitored data pipelines, model versioning, simulation coverage, rollback, anomaly detection, reserve margins, and explainable control decisions."

If they ask what the next step would be:

- "I would formalize the asset models, define the state vector, turn the objectives and constraints into an explicit optimization problem, and build an offline replay environment with historical market and plant data."

## Suggested demo flow

1. Start on `Baseline` and explain the two layers.
2. Point at the 24-hour forecasts and explain what the upper layer sees.
3. Show the upper-layer outputs: battery charging, grid target, and pre-cooling.
4. Move the `Lower-layer zoom hour` to the evening peak and explain the disturbance handling.
5. Switch to `Heat Wave` and show how the thermal target shifts earlier.
6. Switch to `Grid Stress` and talk about reserve policy and battery trim.

## References you can mention if they ask where the framing came from

- SMARD market data: https://www.smard.de/en
- EPEX 15-minute products: https://www.epexspot.com/en/15-minute-products-market-coupling
- ASHRAE handbook data center chapter: https://handbook.ashrae.org/Handbooks/A19/IP/a19_ch20/a19_ch20_ip.aspx
