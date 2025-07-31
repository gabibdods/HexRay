# HexRay

# Self-Hosted Zero-Trust Endpoint Security Platform

### Description

- A comprehensive agent-server platform designed to secure enterprise devices through a Zero-Trust VPN, OS-level kernel scanning, binary reverse engineering, and real-time anomaly detection, all written in Go
- Provides visibility, control, and protection across endpoints using deep OS inspection, executable analysis, and policy enforcement
- Emphasizes feasibility, modularity, and industry-grade security while serving as an advanced systems and cybersecurity portfolio project

---

## NOTICE

- Please read through this `README.md` to better understand the project's source code and setup instructions
- Also, make sure to review the contents of the `License/` directory
- Your attention to these details is appreciated — enjoy exploring the project!

---

## Problem Statement

- Traditional endpoint protection solutions rely heavily on trust-based models, lack deep binary insight, and often require cloud dependencies
- Security-conscious environments need a self-hosted, zero-trust system that can monitor OS internals, detect hidden or injected threats, analyze binaries in-place, operate over secure VPN tunnels with policy enforcement and provide complete user visibility and control

---

## Project Goals

### Establish a Zero-Trust VPN Tunnel

- Secure communication using mTLS or WireGuard between client and server with per-device/user policy enforcement

### Build an Endpoint Agent with Binary Scanning and OS Introspection

- Scan kernel structures, analyze executables, monitor process trees, and detect anomalies in real-time

---

## Tools, Materials & Resources

### Go Libraries

- `wgctrl`, `crypto/tls`, `certmagic`, `gorilla/mux`, `grpc-go`, `gopsutil`, `debug/elf`, `go-capstone`

### Operating System Interfaces

- Linux: `/proc`, `ptrace`, `ebpf`, `sysctl`
- Cross-platform binary formats: ELF, PE, Mach-O

### Research & Inspiration

- Concepts from Elastic Agent, CrowdStrike Falcon, and Zero-Trust Architecture from NIST SP 800-207

---

## Design Decision

### Zero-Trust VPN Core

- Implemented mTLS-authenticated WireGuard-like tunnel with optional SPIFFE/SVID identity tokens per node

### Binary Scanner & Static Analysis

- Parses ELF/PE/Mach-O using Go’s native debug packages and integrates Capstone for disassembly and pattern recognition

### Kernel Monitor and Daemon Architecture

- Agent runs as a background service using `/proc`, syscall tracing, and optional eBPF hooks for runtime visibility

---

## Features

### Secure VPN Tunnel

- Encrypted communication with fine-grained device policies and rogue traffic detection

### Kernel and Process Scanner

- Reads system process trees, memory maps, and module tables for signs of tampering or code injection

### Binary Reverse Engineering Interface

- CLI or TUI tool to disassemble binaries, extract strings, inspect imports, and identify obfuscation

---

## Block Diagram

```plaintext
                    ┌─────────────────────────────┐
                    │  Central Dashboard Server   │
                    │─────────────────────────────│
                    │  - gRPC / REST API          │
                    │  - Device Registry          │
                    │  - Results DB (Scan Logs)   │
                    └───────┬─────────────────────┘
                            │
                (Encrypted mTLS Tunnel)
                            │
                    ┌───────▼─────────────────────┐
                    │     Zero-Trust VPN Agent    │
                    │─────────────────────────────│
                    │  - VPN Interface (wgctrl)   │
                    │  - Auth (mTLS / SPIFFE)     │
                    │  - Traffic Inspection       │
                    └───────┬─────────────────────┘
                            │
                            ▼
         ┌────────────────────────────┐
         │   Kernel Monitor (Linux)   │
         │────────────────────────────│
         │  - /proc / ptrace / ebpf   │
         │  - Detect injected code    │
         │  - Watch hidden processes  │
         └───────┬────────────────────┘
                 │
                 ▼
       ┌────────────────────────────┐
       │ Binary Scanner & Analyzer  │
       │────────────────────────────│
       │  - ELF/PE/Mach-O Parser    │
       │  - Capstone Disassembler   │
       │  - CLI or TUI Navigator    │
       └────────────────────────────┘

```

---

## Functional Overview

- Each device runs an agent that:
	- Establishes a VPN tunnel using mTLS
	- Scans running processes, memory maps, kernel modules
	- Analyzes static binaries for signs of packing, obfuscation, or known malicious structures
	- Reports findings to the central server
- A central dashboard collects and visualizes agent reports, scan logs, and session metrics

---

## Challenges & Solutions

### Reliable VPN Stack in Go

- Used `wgctrl` and mTLS libraries to simulate WireGuard behavior with identity-based policy enforcement

### Parsing Low-Level Binary Formats

- Leveraged Go’s `debug` package and Capstone bindings to safely extract function maps, headers, and symbol strings

---

## Lessons Learned

### Deep Systems Programming in Go

- Gained fluency in syscall interaction, memory inspection, and process table traversal using Linux internals

### Applied Security Design Patterns

- Explored practical Zero-Trust networking, identity-based authorization, and runtime anomaly detection

---

## Project Structure

```plaintext
root/
├── License/
│   ├── LICENSE.md
│   │
│   └── NOTICE.md
│
├── .gitattributes
│
├── .gitignore
│
└── README.md

```

---

## Future Enhancements

- Integrate anomaly detection models for real-time alerting
- Add dynamic binary analysis in sandboxed WebAssembly
- Auto-block binaries based on signature or behavior
- Real-time dashboard for VPN and scan telemetry
- Cross-platform support for Windows/macOS kernel monitors
- SPIRE-based SVID identity broker
