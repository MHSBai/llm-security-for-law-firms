# LLM Security for Law Firms

A practical threat model and adoption checklist for deploying large language models inside legal practice, where client confidentiality, privilege, and auditability are non-negotiable.

> Maintained by [Rich Berman](https://github.com/granolacowboy) / [MHSB Solutions](https://github.com/MHSBai). Issues and PRs welcome.

**How to use this:** read it live at [mhsbai.github.io/llm-security-for-law-firms](https://mhsbai.github.io/llm-security-for-law-firms/), or print it and adopt the checklist as an internal standard you run through before signing off on an AI deployment.

## Who this is for

Managing partners, GCs, legal-ops leads, and the engineers deploying AI for them. It assumes you are adopting AI where a wrong answer is an ethics or malpractice exposure, not just a bug, and you need to answer "is this safe, and can we prove it?" before "is it clever?"

## The threat model (what actually goes wrong)

1. **Confidentiality leakage:** privileged content sent to a training-eligible or consumer endpoint; prompt/response logging outside the firm's control; context bleed between matters.
2. **Prompt injection and tool abuse:** untrusted document/email content steering an agent into exfiltration, unauthorized actions, or conflict-gate bypass.
3. **Non-determinism in the decision path:** a model, not a rule, making a conflict-screening or intake-eligibility call, with no reproducible basis.
4. **Provenance and auditability gaps:** no record of what was sent, which model/version answered, and why an automated decision was made.
5. **Data retention and residency:** vendor retention windows, sub-processors, and geography that violate an engagement letter or ethics rule.
6. **Over-broad access:** an agent with credentials to the whole DMS when it needs one matter.

## Adoption checklist

### Confidentiality routing
- [ ] Privileged/client/matter reasoning goes only to endpoints under a contract that **excludes training** and honors retention limits.
- [ ] A documented fail-closed fallback: if the approved model is unavailable, the system stops. It does **not** silently route to a consumer endpoint.
- [ ] Redaction/minimization before any external call; PII and client identifiers stripped or tokenized where feasible.

### Determinism and human oversight
- [ ] Eligibility, conflict, and validation **gates are deterministic code**, not model output. The model assists; the gate decides.
- [ ] Every automated decision has a **human override** point and is reversible.
- [ ] Untrusted content (client documents, inbound email) is treated as data, never as instructions to the agent.

### Provenance and audit
- [ ] Each automated decision logs: inputs (or their hashes), model + version, prompt, output, and the gate result.
- [ ] Logs are tamper-evident and retained per the firm's records policy.
- [ ] You can reproduce or explain any decision to a client or a regulator.

### Access and least privilege
- [ ] Agents get matter-scoped, time-boxed credentials: never blanket DMS access.
- [ ] Write and mutating actions against firm systems are gated: classify the change, require human approval, then verify it by reading the result back.
- [ ] Secrets live outside prompts and repos; rotated on exposure.

### Evaluation before adoption
- [ ] A capability ships with an **eval harness** and a passing bar, where "it works" means it passes, not that it demoed.
- [ ] Adversarial tests: prompt injection, conflict-gate bypass, redaction failures.
- [ ] A documented off-switch and rollback.

## Mapping to professional responsibility

This checklist is written to support (not replace) counsel's own analysis under the applicable rules: competence and technology (ABA Model Rule 1.1 cmt. 8), confidentiality (1.6), supervision of nonlawyer and AI assistance (5.3), and ABA Formal Opinion 512 (2024) on generative AI. Confirm your jurisdiction's rules; treat this as an engineering companion to that duty.

## Related work

- **[intake-triage-mcp](https://github.com/granolacowboy/intake-triage-mcp):** a reference implementation of the determinism and provenance principles above for legal intake.

## License

MIT. See [LICENSE](LICENSE).

---

<sub>This is a living checklist. Contributions that sharpen the threat model or add jurisdiction-specific pointers are welcome. Nothing here is legal advice.</sub>
