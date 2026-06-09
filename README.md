# RunASM

**[www.runasm.com](https://www.runasm.com)** — high-performance infrastructure in Rust.

## ASM is not dead. Performance is not optional. Security is not a feature.

Infrastructure software built in Rust with SIMD, XDP kernel-bypass, and near-zero-overhead abstractions.  
Every line ships in a static binary. Every release passes a structured security audit.

---

## The Stack

| Project | Type | Description | License |
|---------|------|-------------|---------|
| [Runbound](https://github.com/redlemonbe/Runbound) | DNS server | Drop-in Unbound replacement — XDP fast-path, REST API, master/slave replication, web dashboard | AGPL-3.0 + Commercial |
| [RunX540](https://github.com/redlemonbe/RunX540) | Proxmox datapath | Host-side XDP datapath for Proxmox VMs — 10 GbE line-rate VM→external, same-host VM↔VM in RAM, zero-config, no SR-IOV / passthrough | AGPL-3.0 + Commercial |
| [dnsmark](https://github.com/redlemonbe/dnsmark) | DNS benchmark | DNS benchmark tool — UDP/TCP/DoT/DoH, percentiles, compare mode, JSON output | AGPL-3.0 |
| [RunPerf](https://github.com/redlemonbe/RunPerf) | Network benchmark | TCP/UDP throughput & packet-rate generator — AF_XDP line-rate, per-CPU + REUSEPORT, JSON output | AGPL-3.0 |

---

## Design principles

**Structured security audits** — multi-cycle reviews every release, multi-source methodology ([AI-INTERNAL] + [AI-ADVERSARIAL] + [AUTOMATED-TOOL]), findings tracked to resolution, no release without a clean audit pass.

**Extreme performance** — SIMD dispatch at startup (AVX2 → SSE2 → scalar), AF/XDP kernel-bypass where the NIC supports it, io_uring for zero-copy file I/O, lockless data paths on hot paths.

**Drop-in compatibility** — existing `unbound.conf` files work as-is. Non-standard directives are ignored gracefully. You migrate by dropping in the binary.

**Operability** — live config reload without restart, REST API on every service, built-in browser dashboard with no external dependencies, structured JSON logs, Prometheus endpoints.

**Static binaries** — musl targets ship with zero runtime dependencies. Copy the binary. Run it. Done.

---

## Architecture

All server projects share the same fast-path design:

```
Network card
    │
    ├─ XDP program (BPF, runs in NIC driver)
    │      ↓
    │   AF_XDP socket — zero-copy ring buffer
    │      ↓
    │   Tokio async runtime — SIMD protocol parser
    │      ↓
    │   In-memory engine (DNS cache / HTTP handler / SQL engine)
    │      ↓
    └─ Slow path fallback (standard Linux TCP/UDP stack)
```

XDP runs in SKB mode if the NIC doesn't support DRV mode (e.g. virtio-net). The slow path is always available as a fallback.

---

## Status

| Project | Version | Status |
|---------|---------|--------|
| Runbound | v0.16.9 | Stable |
| RunX540 | v1.0.0 | Audited (8 cycles incl. cross-review + live host); proven on 10G |
| dnsmark | v2.1.3 | Stable |
| RunPerf | v0.2.1 | Early — AF_XDP line-rate generation proven |

> ⚠️ All server software is under active development and has not yet undergone external human security audit. Not recommended for production deployments handling sensitive traffic until v1.0.

---

## Commercial licensing

Runbound and RunX540 are dual-licensed:

- **AGPL-3.0** — free for open-source projects, self-hosted personal use, academic research, and community distributions (Debian, Ubuntu, etc.)
- **Commercial license** — for organizations that need to deploy without AGPL obligations (SaaS, proprietary integrations, OEM)

Contact: **contact@runasm.com** · [www.runasm.com](https://www.runasm.com)

dnsmark and RunPerf are AGPL-3.0 only — no commercial license needed for benchmark tools.

---

## Support the project
[![GitHub Sponsors](https://img.shields.io/github/sponsors/redlemonbe?style=flat&logo=github&label=Sponsor)](https://github.com/sponsors/redlemonbe)

**Bitcoin** — `3FP8hkkiu4kwCD1PDFgAv2oq1ZTyXwy3yy`  
**Ethereum** — `0xB5eEAf89edA4204Aa9305B068b37A93439cBb680`

---
