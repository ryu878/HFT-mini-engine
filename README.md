# HFT Mini Engine

A minimal low-latency trading engine skeleton in modern C++.

This project demonstrates a simplified execution pipeline typical for
high-frequency / market-making environments, focusing on performance,
determinism, and system-level thinking rather than strategy complexity.

---

## Overview

The engine models a basic trading loop:

Market Data → Strategy → Risk → Order Gateway → Acknowledgement

The goal is to showcase:

- Lock-free SPSC communication
- No dynamic allocations in hot paths
- Basic order lifecycle state machine
- Inventory and risk checks
- Latency measurement (p50 / p99)
- Thread affinity and cache awareness

This is not a production trading system.
It is an execution-focused performance exercise.

---

## Architecture

Threads:

1. Market Data Thread  
   Generates simulated tick events.

2. Strategy Thread  
   Consumes market data and produces order commands.

3. Gateway Thread  
   Simulates order sending and exchange acknowledgements.

Communication:

- Lock-free SPSC ring buffers
- Preallocated object pools
- No heap allocations in the critical path

---

## Key Components

### Infrastructure

- `spsc_ring.hpp` – single-producer single-consumer lock-free queue
- `object_pool.hpp` – fixed-size memory pool
- `cpu_affinity.hpp` – thread pinning utilities
- `metrics.hpp` – latency and throughput counters

### Market

- `md_simulator.hpp` – deterministic tick generator

### Strategy

- `simple_mm.hpp` – minimal market-making example
- Spread-based quoting logic
- Inventory tracking

### Risk

- Position limits
- Simple throttling
- Basic exposure checks

### Execution

- Order struct and lifecycle state machine
- Gateway stub with simulated ACK/FILL events
- Rate limiter (token bucket)

---

## Build

```bash
mkdir build
cd build
cmake ..
make
```
## Run
```bash
./engine
```
## Benchmarks:
```bash
./bench_spsc
./bench_latency_pipeline
./bench_false_sharing
```

## Performance Focus

The engine emphasizes:

- Deterministic latency behavior
- Elimination of unnecessary allocations
- Cache line alignment (`alignas(64)`)
- Minimal locking
- Clear execution flow

Latency is measured between:

Tick received → Order generated

With distribution reporting (p50 / p99).

## 📁 Repo layout
```bash
hft-mini-engine/
├─ README.md
├─ LICENSE
├─ .gitignore
├─ CMakeLists.txt
├─ cmake/
│  └─ toolchain.cmake              # optional
├─ include/
│  ├─ hft/
│  │  ├─ core/
│  │  │  ├─ types.hpp              # common types, aliases, enums
│  │  │  ├─ time.hpp               # timestamp, rdtsc helpers (optional)
│  │  │  ├─ utils.hpp              # small helpers (no heavy stuff)
│  │  ├─ infra/
│  │  │  ├─ spsc_ring.hpp          # lock-free SPSC ring buffer
│  │  │  ├─ object_pool.hpp        # fixed-size object pool
│  │  │  ├─ cpu_affinity.hpp       # pin thread to core
│  │  │  ├─ logger.hpp             # minimal logger (printf-style)
│  │  │  ├─ metrics.hpp            # counters + latency hist
│  │  ├─ market/
│  │  │  ├─ md_event.hpp           # market data event struct
│  │  │  ├─ md_simulator.hpp       # fake tick stream generator
│  │  ├─ strategy/
│  │  │  ├─ strategy.hpp           # interface
│  │  │  ├─ simple_mm.hpp          # simplest market making toy strategy
│  │  ├─ risk/
│  │  │  ├─ risk_checks.hpp        # limits, throttles
│  │  │  ├─ inventory.hpp          # position/inventory model
│  │  ├─ exec/
│  │  │  ├─ order.hpp              # order types, IDs, state
│  │  │  ├─ order_state.hpp        # state machine (NEW/SENT/ACK/...)
│  │  │  ├─ gateway.hpp            # send/cancel stub, acks simulator
│  │  │  ├─ rate_limiter.hpp       # token bucket (simple)
│  │  ├─ app/
│  │  │  ├─ config.hpp             # simple config struct (no parser initially)
│  │  │  └─ engine.hpp             # wires components
├─ src/
│  ├─ main.cpp                     # runs the pipeline
│  ├─ app/
│  │  └─ engine.cpp
│  ├─ infra/
│  │  ├─ cpu_affinity.cpp
│  │  └─ logger.cpp
│  └─ exec/
│     └─ gateway.cpp
├─ tests/
│  ├─ test_spsc_ring.cpp
│  ├─ test_object_pool.cpp
│  ├─ test_order_state.cpp
│  └─ CMakeLists.txt
├─ bench/
│  ├─ bench_spsc.cpp               # push/pop throughput, p50/p99
│  ├─ bench_false_sharing.cpp      # alignas(64) demo
│  ├─ bench_latency_pipeline.cpp   # tick->order latency distribution
│  └─ CMakeLists.txt
├─ tools/
│  ├─ run_perf.sh                  # perf record/report helper
│  └─ plot_latency.py              # optional: parse log -> plot histogram
└─ docs/
   ├─ architecture.md              # 1 page: threads, queues, flow
   ├─ performance.md               # what you measured, results
   └─ interview_notes.md           # short talking points
```
  
***
## About
## 📌 Quantitative Researcher | Algorithmic Trader | Trading Systems Architect

Quantitative researcher and trading systems engineer with end-to-end ownership of systematic strategies — from research and statistical validation to execution architecture and 24/7 production deployment.

Experience across crypto (CEX/DEX), FX, and exchange-traded markets.

Core focus areas:
- Systematic strategy design, validation, and robustness testing
- Market microstructure analysis (order book dynamics, liquidity, spread behavior, funding, volume delta)
- Tick-level and historical backtesting framework development
- Execution engine architecture and order lifecycle management
- Real-time market data processing pipelines
- Risk-aware system design and capital efficiency
- Production-grade trading infrastructure

## Technical Stack

- **Languages:** Python, C++, MQL5
- **Execution & Connectivity:** REST, WebSocket, FIX
- **Infrastructure:** Linux, Docker, Redis, PostgreSQL, ClickHouse
- **Analytics:** NumPy, Pandas, custom backtesting frameworks

## Contact

**Email:** ryu8777@gmail.com
***
