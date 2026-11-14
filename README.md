# Jean-François Baudson

**Security researcher and systems engineer** — Belgium. Known online as *RedLemonBe*.

I work where security and performance meet: the layer between the network card and the
application, where a single branch, a single copy or a single unchecked length decides
whether a system holds.

---

## Security research

Independent researcher on public bug bounty programs, with published reports.

I do not scan for known vulnerability classes. I look for **asymmetries in how rules are
applied**: the check that runs on one code path and not on its twin, the limit enforced on
the way in and forgotten on the way out, the invariant that the documentation states and
the code never verifies. Most serious findings live there.

- Protocol and parser analysis: anything that reads attacker-controlled bytes
- Authorisation and trust-boundary review
- Reproduction first: a finding is a command and its raw output, not an opinion

## Systems engineering

Low-level, performance-critical software, mostly in **Rust** and **assembly**.

- Kernel-bypass networking: AF_XDP, eBPF/XDP, zero-copy datapaths
- SIMD and cache-conscious data structures; NUMA, core and interrupt placement
- DNS at the wire level: resolution, DNSSEC, encrypted transports
- Linux hardening: sandboxed services, capabilities, static binaries

## How I work

- **Measure, then claim.** No performance figure without a bench, its conditions and its
  raw data. No "fixed" without the output that shows it.
- **Reproduce before fixing.** A defect nobody has seen is not corrected, it is guessed at.
- **The RFC is the reference.** When behaviour is specified, the specification wins.
- **Predictable code.** Simple structures a modern CPU can anticipate, rather than clever
  ones that need patching.
- **No silent debt.** What is not verified is written down as not verified.

## Contact

**contact@runasm.com**

Security reports and professional enquiries are welcome. I answer in French or English.
