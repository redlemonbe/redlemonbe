# RunSoftware

## ASM is not dead. Performance is not optional. Security is not a feature.

Infrastructure software built in Rust with SIMD, XDP kernel-bypass, and near-zero-overhead abstractions.  
Every line ships in a static binary. Every release passes a structured security audit.

---

## The Stack

| Project | Type | Description | License |
|---------|------|-------------|---------|
| [Runbound](https://github.com/redlemonbe/Runbound) | DNS server | Drop-in Unbound replacement — XDP fast-path, REST API, master/slave replication, web dashboard | AGPL-3.0 + Commercial |
| [RunNginx](https://github.com/redlemonbe/RunNginx) | HTTP server | nginx-compatible — SIMD parser, AF/XDP, io_uring, built-in SSH/SFTP engine, web dashboard | AGPL-3.0 + Commercial |
| [RunAlexDB](https://github.com/redlemonbe/RunAlexDB) | SQL database | MariaDB-compatible wire protocol — in-memory engine, XDP fast-path, built-in admin UI | AGPL-3.0 + Commercial |
| [dnsmark](https://github.com/redlemonbe/dnsmark) | DNS benchmark | DNS benchmark tool — UDP/TCP/DoT/DoH, percentiles, compare mode, JSON output | AGPL-3.0 |
| [httpmark](https://github.com/redlemonbe/httpmark) | HTTP benchmark | HTTP/HTTPS benchmark tool — QPS control, HTTP/2, compare mode, JSON output | AGPL-3.0 |
| [dbmark](https://github.com/redlemonbe/dbmark) | DB benchmark | Database benchmark tool — MySQL protocol, latency percentiles, compare mode | AGPL-3.0 |

---

## Design principles

**Military-grade security** — structured audits every release cycle, multi-source methodology ([AI-INTERNAL] + [AI-ADVERSARIAL] + [AUTOMATED-TOOL]), findings tracked to resolution, no release without a clean audit pass.

**Extreme performance** — SIMD dispatch at startup (AVX2 → SSE2 → scalar), AF/XDP kernel-bypass where the NIC supports it, io_uring for zero-copy file I/O, lockless data paths on hot paths.

**Drop-in compatibility** — existing `unbound.conf` and `nginx.conf` files work as-is. Non-standard directives are ignored gracefully. You migrate by dropping in the binary.

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
| Runbound | v0.9.41 | Experimental — not yet production-recommended |
| RunNginx | v0.1.5 | Experimental — not yet production-recommended |
| RunAlexDB | v0.1.0 | Alpha — in-memory only, persistence roadmapped |
| dnsmark | v0.1.1 | Stable |
| httpmark | v0.1.1 | Stable |
| dbmark | v0.1.0 | In development |

> ⚠️ All server software is under active development and has not yet undergone external human security audit. Not recommended for production deployments handling sensitive traffic until v1.0.

---

## Commercial licensing

Runbound, RunNginx, and RunAlexDB are dual-licensed:

- **AGPL-3.0** — free for open-source projects, self-hosted personal use, academic research, and community distributions (Debian, Ubuntu, etc.)
- **Commercial license** — for organizations that need to deploy without AGPL obligations (SaaS, proprietary integrations, OEM)

Contact: redlemonbe@codix.be

dnsmark, httpmark, and dbmark are AGPL-3.0 only — no commercial license needed for benchmark tools.

---

## Support the project

[![GitHub Sponsors](https://img.shields.io/github/sponsors/redlemonbe?style=flat&logo=github&label=Sponsor%20on%20GitHub)](https://github.com/sponsors/redlemonbe)

**Bitcoin** — `3FP8hkkiu4kwCD1PDFgAv2oq1ZTyXwy3yy`  
**Ethereum** — `0xB5eEAf89edA4204Aa9305B068b37A93439cBb680`

Security issues: redlemonbe@codix.be — private disclosure before opening a public issue.

---

`Rust` `SIMD` `XDP` `AF_XDP` `io_uring` `DNS` `HTTP` `SQL` `Linux` `Networking` `Performance` `Security`
