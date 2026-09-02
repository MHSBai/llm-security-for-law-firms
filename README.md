# LLM Security for Law Firms

A practical threat model and adoption checklist for deploying large language models inside legal practice — where client confidentiality, privilege, and auditability are non-negotiable.

> Maintained by [Rich Berman](https://github.com/granolacowboy) / [MHSB Solutions](https://github.com/MHSBai). Field-tested framing, not vendor marketing. Issues and PRs welcome.

## Who this is for

Managing partners, GCs, legal-ops leads, and the engineers deploying AI for them. It assumes you are adopting AI in a **regulated, change-resistant** environment and need to answer "is this safe, and can we prove it?" before "is it clever?"

## The threat model (what actually goes wrong)

1. **Confidentiality leakage** — privileged content sent to a training-eligible or consumer endpoint; prompt/response logging outside the firm's control; context bleed between matters.
2. **Prompt injection & tool abuse** — untrusted document/email content steering an agent into exfiltration, unauthorized actions, or conflict-gate bypass.
3. **Non-determinism in the decision path** — a model, not a rule, making a conflict-screening or intake-eligibility call, with no reproducible basis.
4. **Provenance & auditability gaps** — no record of what was sent, which model/version answered, and why an automated decision was made.
5. **Data retention & residency** — vendor retention windows, sub-processors, and geography that violate an engagement letter or ethics rule.
6. **Over-broad access** — an agent with credentials to the whole DMS when it needs one matter.

## Adoption checklist

### Confidentiality routing
- [ ] Privileged/client/matter reasoning goes only to endpoints under a contract that **excludes training** and honors retention limits.
- [ ] A documented fail-closed fallback: if the approved model is unavailable, the system stops — it does **not** silently route to a consumer endpoint.
- [ ] Redaction/minimization before any external call; PII and client identifiers stripped or tokenized where feasible.

### Determinism & human-in-the-loop
- [ ] Eligibility, conflict, and validation **gates are deterministic code**, not model output. The model assists; the gate decides.
- [ ] Every automated decision has a **human override** point and is reversible.
- [ ] Untrusted content (client documents, inbound email) is treated as data, never as instructions to the agent.

### Provenance & audit
- [ ] Each automated decision logs: inputs (or their hashes), model + version, prompt, output, and the gate result.
- [ ] Logs are tamper-evident and retained per the firm's records policy.
- [ ] You can reproduce or explain any decision to a client or a regulator.

### Access & least privilege
- [ ] Agents get matter-scoped, time-boxed credentials — never blanket DMS access.
- [ ] Write/mutating actions against firm systems are gated (classify → approve → readback-proof).
- [ ] Secrets live outside prompts and repos; rotated on exposure.

### Evaluation before adoption
- [ ] A capability ships with an **eval harness** and a passing bar — "it works" means it passes, not that it demoed.
- [ ] Adversarial tests: prompt injection, conflict-gate bypass, redaction failures.
- [ ] A documented off-switch and rollback.

## Mapping to professional responsibility

This checklist is written to support (not replace) counsel's own analysis under the applicable rules — competence and technology (ABA Model Rule 1.1 cmt. 8), confidentiality (1.6), supervision of non-lawyer/AI assistance (5.3), and the 2024–2025 guidance on generative AI. Confirm your jurisdiction's rules; treat this as an engineering companion to that duty.

## Related work

- **[intake-triage-mcp](https://github.com/granolacowboy/intake-triage-mcp)** — a reference implementation of the determinism/provenance principles above for legal intake.

---

<sub>This is a living checklist. Contributions that sharpen the threat model or add jurisdiction-specific pointers are welcome. Nothing here is legal advice.</sub>
