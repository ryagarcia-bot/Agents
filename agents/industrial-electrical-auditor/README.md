# Industrial Electrical Auditor

An AI audit agent for industrial electrical documentation review, including wiring diagrams, ladder diagrams, loop drawings, one-line diagrams, panel schematics, and control narratives.

## What It Does

The Industrial Electrical Auditor reviews vendor-supplied and project-generated electrical documentation against:

- **NEC / NFPA 70 compliance** — conductor sizing, protection, disconnecting means, grounding, control circuit protection, equipment ratings, wiring methods, and hazardous area requirements
- **Functional correctness** — sequence of operation, NO/NC logic, seal-in circuits, permissives, shutdowns, alarm and trip behavior, relay and PLC logic consistency
- **Safety and interlock integrity** — E-stop logic, hardwired interlocks, fail-safe design, shutdown chains, bypass risks, software override of hardware safety
- **Constructability** — field termination clarity, wire and terminal numbering, JB interface, factory vs field scope split
- **Startup and maintenance readiness** — documentation quality sufficient for commissioning and troubleshooting
- **Consistency** — device tag alignment across IFC drawings, vendor submittals, and field records

## Document Types Supported

| Document | Audit Scope |
|---|---|
| Wiring diagrams | Signal continuity, terminal numbering, device tags, shielding |
| Ladder diagrams | Logic correctness, NO/NC usage, interlocks, shutdown chains |
| Loop drawings | Signal path, JB terminations, grounding, PLC interface |
| One-line diagrams | Power distribution, protection, disconnecting means |
| Panel schematics | Control power, device layout, incoming power |
| Panel schedules | Load assignments, breaker ratings, feeder sizing |
| I/O lists | Tag consistency, signal types, I/O assignment |
| Terminal schedules | Wire numbers, source/destination, signal type |
| Interlock narratives | Logic vs diagram agreement, fail-safe behavior |
| Vendor manuals / as-builts | Factory assumptions, interface requirements |

## Output Format

Every audit response is structured as:

1. **Executive Summary** — overall assessment
2. **Functional Review** — what the circuit does and whether it works logically
3. **Code / Compliance Findings** — each issue with NEC article or standard reference
4. **Safety / Interlock Findings** — critical safety concerns (listed first)
5. **Documentation Gaps** — missing information that blocks approval or clean installation
6. **Constructability / Startup Risks** — installation and commissioning concerns
7. **Recommended Actions** — specific next steps

Each finding includes: what was found, why it matters, supporting evidence, applicable code reference, and a recommended action.

## Usage

### As a System Prompt

Load [`system_prompt.md`](./system_prompt.md) as the system message when initializing the agent with any LLM that supports system-level instructions (e.g., OpenAI GPT-4, Anthropic Claude, Azure OpenAI).

```python
with open("agents/industrial-electrical-auditor/system_prompt.md", "r") as f:
    system_prompt = f.read()

# Pass system_prompt as the system message in your LLM API call
```

### Example Request

```
Review the attached wiring diagram for the compressor package E-stop circuit.
Identify any safety logic gaps, missing wire numbers, and NEC compliance issues.
```

### Example Response Structure

```
### Executive Summary
[Overall pass/fail/conditional assessment]

### Safety / Interlock Findings
- Finding: E-stop contact wired NC but drawn as NO on sheet 14.
  Why it matters: Wiring as drawn results in a non-functional E-stop — the motor will not de-energize on E-stop activation.
  Evidence: Sheet 14, rung 4, contact symbol vs field device specification FS-101.
  Code / standard reference: NFPA 79 Section 9.2.5.4 (machine E-stop requirements); NEC Article 430.132
  Recommended action: Correct drawing to reflect NC contact. Verify field device configuration before energization.

### Code / Compliance Findings
...

### Documentation Gaps
...
```

## Scope and Limitations

- This agent identifies deficiencies, conflicts, risks, and likely noncompliance based on the documentation provided.
- Final engineering approval remains with the responsible licensed Engineer of Record.
- Findings are based on the information present in the submitted documents. Incomplete submittals will generate documentation gap findings.
- The agent does not replace a physical field inspection.

## Related Standards

- NEC / NFPA 70 — National Electrical Code
- NFPA 79 — Electrical Standard for Industrial Machinery
- ISA-5.1 — Instrumentation Symbols and Identification
- ISA-5.4 — Instrument Loop Diagrams
- IEEE 519 — Harmonic Control
- IEC 60529 — IP Ratings (where enclosure classification is relevant)
