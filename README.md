# HFT mini engine (C++)

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
