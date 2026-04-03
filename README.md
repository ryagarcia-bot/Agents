# Agents

A collection of AI agent definitions for specialized industrial and technical domains.

## Available Agents

| Agent | Description |
|---|---|
| [Industrial Electrical Auditor](./agents/industrial-electrical-auditor/) | Reviews industrial electrical documentation (wiring diagrams, ladder diagrams, loop drawings, one-lines, panel schematics, control narratives) for NEC/NFPA 70 compliance, functional correctness, safety integrity, and constructability. |

## Structure

Each agent lives in its own directory under `agents/` and contains:

- `system_prompt.md` — the full system prompt defining the agent's role, behavior, review method, rules, and output format
- `README.md` — usage documentation, supported document types, and example interactions
