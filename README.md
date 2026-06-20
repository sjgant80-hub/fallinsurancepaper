# FallInsurancePaper · sovereign UK insurance broker document generator

Single-file tool that generates the documents a UK FCA-regulated general insurance broker has to produce to stay IDD-compliant: TOBA, demands & needs statements, IPID, policy summaries, cover notes, mid-term adjustments, renewal invitations, cancellations, claim notifications, letters of authority, complaint acknowledgements, final-response letters, commercial schedules, premium finance disclosures and vulnerable-customer notes.

Runs entirely in the browser. Client data never leaves the device. IndexedDB. Sovereign.

**Live:** https://sjgant80-hub.github.io/fallinsurancepaper/

---

## For end users · brokers, account handlers, compliance officers

**What it does.** Pick a client. Pick a policy. Pick a template. Click Generate. You get an IDD-shaped document with the client, policy, firm and adviser data interpolated and the regulatory clauses already in place.

**Fifteen templates, all included:**

| Template | Regulator hook | What it's for |
|---|---|---|
| TOBA · Terms of Business Agreement | ICOBS 4 · IDD Art 18 | First instruction · regulatory status, scope, capacity, complaints, FOS, FSCS, fees |
| Demands & Needs Statement | IDD Art 20 / ICOBS 5.2 | Records the client need · matched product · alternatives ruled out |
| Policy Summary | ICOBS 6.1 | Plain-English summary of cover (long-form products / commercial) |
| IPID · Insurance Product Information Document | IDD Art 20 / PRIIPs-style | Standardised 1-page non-life cover summary (motor / property / liability / travel) |
| Cover Note | ICOBS 5.2 / SRA cover evidence | Temporary evidence of cover pending policy schedule |
| Mid-Term Adjustment Letter | ICOBS 6.1 / 7 | Cover change · new premium · additional or refund · effective date |
| Renewal Invitation | ICOBS 6.5 (renewal transparency) | Current cover · last-year premium · this-year premium · alternatives considered · D&N reassessed |
| Cancellation Notice | ICOBS 7 · cooling-off (ICOBS 7.1) | Client or insurer-initiated · return premium calc · cooling-off note |
| Claim Notification | ICOBS 8 · claims handling | First-loss facts notified to insurer with cover reference |
| Letter of Authority | broker / market practice | Client authorises broker to act, request docs from insurer |
| Complaint Acknowledgement | DISP 1.6.1R | Within 3 business days · summary of complaint · expected timing |
| Final Response Letter | DISP 1.6.2R | Within 8 weeks · firm's decision · FOS escalation rights |
| Commercial Insurance Schedule | ICOBS 6 / commercial market | Multi-cover scheduled document for SMEs / mid-market |
| Premium Finance Disclosure | CONC 4 / 5 · CCA | Total amount payable · APR · monthly · commission earned |
| Vulnerable Customer Note | FCA FG21/1 · ICOBS 2.5 | Internal evidence of identification and accommodation |

**Regulatory clauses are locked.** The complaints text, FOS escalation, FSCS limits, cooling-off rights, IDD demands-and-needs framing, ICOBS 7 cancellation framing, capacity-of-broker disclosures and FG21/1 framing all carry a lock and can't be edited — they're the bits the FCA cares about. Everything else is yours to edit.

**Lives inside a bundle.** FallInsurancePaper listens on the `fall-insurance` BroadcastChannel:
- When **FallInsurance** (anchor · 829) records a policy, FallInsurancePaper sees it instantly.
- When **FallInsuranceOnboard** (839) captures a client, the client list updates live.
- When **FallInsurancePractice** (857) posts a claim or commission, it surfaces in the relevant section.
- Hand-off to **FallPDF** by postMessage — if it's open in another tab it renders the PDF.

If those tools aren't open, FallInsurancePaper still works on its own.

**Three export paths:**
- Markdown (filing, copy into back-office)
- Standalone HTML (open and print)
- FallPDF postMessage

**T0 Q&A built in.** Offline keyword router that answers the twelve questions brokers ask most: when is an IPID required, TOBA mandatory disclosures, DISP 1.6 timing, demands & needs under IDD Art 20, cooling-off rights, premium-finance disclosure, FOS limits for insurance, commercial-customer cooling-off, vulnerable-customer adjustments, cancellation refund calc, mid-term adjustment process, cover-note validity. T3 (BYOK) optional via Anthropic API key for anything else.

---

## For developers · sovereign / 14-pt gate

- One HTML file. `< 400 KB`. No build step. No npm. No CDN (KONOMI shim is inert and inline).
- IDB primary storage; the shared insurance schema (Policy / InsuranceClient / Adviser / Firm) is honoured; rebroadcast on every write.
- PWA manifest via `data:` URL.
- T0 offline routing, T3 optional BYOK.
- Two-audience README + MIT LICENSE.
- Mobile-first responsive.
- Disclaimer per shared insurance schema in place.
- Audit chain on by default (Mansoor P3 extended · per-mutation prevHash chain · 6-year FCA SYSC retention).
- Aesthetic: oxblood / brass / cream / void.
- Forkable brand (engine name override in settings).

**Constants:** `TOOLNAME='fallinsurancepaper'`, `VERSION='1.0.0'`, `PRIME=853`.

**IDB stores:** `firms · advisers · clients · policies · documents · templates · audit · state`.

**BroadcastChannels:** `fall-insurance` (shared bundle mesh) and `fall-signal` (low-level handshake).

**Globals exposed for verify:** `window.FALLINSURANCEPAPER = { state, render, audit, renderTemplate, TEMPLATES_BUILTIN, T0_RULES, version, prime }`.

---

Sovereign · client data never leaves the device.
