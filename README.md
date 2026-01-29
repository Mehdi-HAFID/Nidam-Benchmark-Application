# Nidam Benchmark Application

## Result Example

<a href="https://nidam.derbyware.com/html/11-23-12.html" target="_blank" rel="noopener noreferrer">
	Show Result in new tab
</a>

## What it is

Nidam Benchmark is a **real-world, full-stack server workload** benchmark designed to stress CPU Cores and memory, using a modern
Java + OAuth 2.0 microservice architecture. Unlike synthetic tests, Nidam Benchmark measures how
hardware performs under production-style backend load, including authentication, API processing, database access, and reverse-proxy routing.

## Downloads

[Windows x64 CLI](https://github.com/Mehdi-HAFID/Nidam-Benchmark-Application/releases/download/2.0/Nidam.Benchmark.2.0.-.Win.x64.cli.zip)

[Windows x64 GUI](https://github.com/Mehdi-HAFID/Nidam-Benchmark-Application/releases/download/2.0/Nidam.Benchmark.2.0.-.Win.x64.gui.zip)

## How To Run

### GUI Version

1. Extract the ZIP file and launch `Nidam Benchmark.exe` (`Windows`).
2. In the Threads field, enter the list of thread counts you want to benchmark, for example: `1,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32`
(No spaces allowed.) Then click **Start**.
3. When done benchmarking, Click **Stop** Button to terminate all background processes. *Simply closing the GUI will not stop them.*.

### CLI Version

This works the same as the GUI version but without the graphical interface. Ideal for automation.

1. Extract the ZIP file, open a Terminal, and run:
    ```powershell title="PowerShell Example"
    .\run-benchmark.ps1 "1,8"
    ```
    Here, `"1,8"` represents the thread sequence you want to test. You can also use longer sequences, e.g.:
    `1,2,4,6,8,10,12,14,16,18,20,22,24,26,28,30,32`

> **Windows will prompt:**
>
> Do you want to allow public and private networks to access this app? Zulu Platform x64 Architecture
>
> Click **`Allow`**, Zulu Platform is simply **Java**.


## Why hardware reviewers should care
Most server and workstation benchmarks focus on isolated CPU math, compression, or single-process throughput.
Nidam Benchmark instead answers a different question:
> How does this hardware perform when running an actual modern backend stack under load?

It exposes bottlenecks in:

* Single-core vs multi-core scaling
* Memory bandwidth and latency

## What the benchmark actually does

The benchmark deploys and exercises a complete production-style backend stack:
Reverse proxy / gateway, OAuth 2.0 Authorization Server, Backend-for-Frontend (BFF), Protected Resource Server (API),
In-Memory SQL database (H2), Single Page Application (SPA), Load generator (k6)

Simulated users authenticate, obtain tokens, and call protected APIs through the full stack. Every **iteration** traverses multiple
services, cryptographic operations, and database queries.

> 1. The benchmark uses an **in-memory database** to avoid the bottleneck of disk I/O.
> 2. Load Generator is the application that **simulates a real world user** authenticating and accessing the protected API.

---

## What Nidam Benchmark Actually Stresses

Nidam Benchmark is not a synthetic CPU stress test — it models a **real OAuth2/OIDC authentication workload** end-to-end.
Each Virtual User (VU) simulates a complete login through your system, exercising the same code paths used in production authentication servers.

Here is a breakdown of what Nidam stresses internally:

* Integer math pipelines, Branch prediction, Multiprecision multiplication
* Core frequency under sustained RSA workloads, single-thread JSON parsing and object creation
* The JVM’s thread scheduler and OS context switching

This workload is CPU-bound and can expose limits in SMT (Hyper-Threading / efficiency cores), especially at high thread counts.

### What Nidam Does Not Stress

* No disk I/O
* No external network latency (all services run locally and communicate through in-process HTTP)

---

## Defensive Password Hashing (Real-World Behavior)

In real production systems, password hashing is intentionally slow and memory-hard. Algorithms like **scrypt** (used in Nidam)
deliberately avoid hardware acceleration so attackers cannot use CPU AES/SHA instructions to brute-force logins.

Scrypt stresses:
* **CPU cores with repeated iterative work**
* **Memory bandwidth and latency**
* **RAM capacity per thread**

This makes Nidam effective for comparing CPUs, memory speeds, and memory channel configurations under real authentication workloads.

---

## Metrics reviewers get

Nidam Benchmark produces review-ready full **HTML report** for easy sharing, it shows:

* **Throughput (logins/sec):** Number of users authenticated per second across tested thread counts,
e.g., for each thread count tested—such as 1, 4, or 8 threads—you get a corresponding logins/sec value.
* **Average Latency per iteration (seconds):** Time for a full iteration—user login plus API access.
* **Thread count vs performance chart:** Visualizes how latency and throughput change under load.
* **Optimal thread score:** Indicates the ideal thread count before latency spikes without improving throughput, expressed as
 throughput at that thread count.

---

## Why it’s different from other benchmarks

| Synthetic benchmarks | Nidam Benchmark           |
| -------------------- | ------------------------- |
| Isolated CPU tests   | Full production stack     |
| Microbenchmarks      | End-to-end workload       |
| Short bursts         | Sustained load            |
| Unrealistic patterns | Real user flows           |
| Single process       | Multi-service, concurrent |

Nidam Benchmark doesn’t replace Cinebench or Geekbench — it complements them by showing what happens when hardware runs an actual
backend workload for real users.

---


## Notes
* Required Ports: `4001, 4002, 4003, 7080, 7081`
* The result of the benchmark above of my machine: **Ryzen 5800H with 32GB DDR4 RAM.**


