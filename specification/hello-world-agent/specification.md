# Specification: hello-world-agent

> **Guidelines**: Read all applicable guidelines before executing ANY tasks below:
> - [guidelines.md](../guidelines.md) — Universal execution rules
> - [guidelines-agent.md](../guidelines-agent.md) — Universal agent patterns
> - [guidelines-agent-python.md](../guidelines-agent-python.md) — Python implementation details
> - [guidelines-agent-skills.md](../guidelines-agent-skills.md) — Runtime skills patterns
> - [guidelines-agent-mcp.md](../guidelines-agent-mcp.md) — MCP integration patterns

---

## Basic Setup

- [ ] Read `product-requirements-document.md` and `intent.md` for full context
- [ ] Bootstrap agent code in `assets/hello-world-agent/` using instructions from the `sap-agent-bootstrap` section (invoke from inside `assets/hello-world-agent/`, use copy commands — do NOT create files manually)
- [ ] Install dependencies, validate the agent starts and responds at `/.well-known/agent.json`

---

## Runtime Skills

No runtime skills required — the Hello World agent is a simple, single-step greeter with no complex workflow or domain-specific rules.

---

## Project-Specific Tasks

### Agent System Prompt

- [ ] Set the agent system prompt to introduce itself as a Hello World agent and instruct it to respond with a friendly greeting to any incoming message
- [ ] The greeting must include "Hello, World!" or equivalent in every response

### Hello World Response Handler

- [ ] Implement the core agent response logic in `assets/hello-world-agent/app/agent.py`:
  - The agent receives any user message
  - Returns a Hello World greeting that includes the user's message or name if provided
  - Response must always be polite, friendly, and include "Hello"
- [ ] No external tools or MCP servers are required for this agent — tool list is empty

### Solution Setup

- [ ] Invoke the `setup-solution` skill to create `solution.yaml` and `asset.yaml` for the `hello-world-agent` asset
- [ ] Validate `solution.yaml` and `assets/hello-world-agent/asset.yaml` exist and are well-formed

---

## Business Instrumentation

- [ ] Instrument milestone M1 (Agent Bootstrapped): emit `M1.achieved: agent project bootstrapped successfully` after bootstrap completes, or `M1.missed: agent bootstrap did not complete` on failure
- [ ] Instrument milestone M2 (Hello World Response): emit `M2.achieved: hello world response returned successfully` when the agent returns a greeting, or `M2.missed: agent did not return a hello world response` on failure
- [ ] Instrument milestone M3 (Tests Passing): emit `M3.achieved: all pre-built tests passed` after test suite passes, or `M3.missed: one or more tests failed` on failure
- [ ] Add OpenTelemetry spans for each milestone step using the decorator form on regular async methods or context manager form inside non-generator async functions
- [ ] Verify `auto_instrument()` is called at the top of `main.py` before any AI framework imports

---

## MCP Tool Integration

No MCP tool integration required for this agent — no SAP API interactions needed.

- [ ] Confirm `mcp-mock.json` is not required (no MCP tools used); skip mock generation

---

## Testing

- [ ] Install test dependencies: `pip install -r requirements-test.txt` from `assets/hello-world-agent/`
- [ ] Write unit test in `assets/hello-world-agent/tests/test_hello_world.py`:
  - Test that the agent returns a response containing "Hello" for any input message
  - Mock LLM responses (AI Core is NOT available in tests)
  - Run immediately after writing: `pytest tests/test_hello_world.py`
- [ ] Write one integration test in `assets/hello-world-agent/tests/test_integration.py`:
  - Invoke the agent's `invoke` function end-to-end with a sample message
  - Mock all LLM calls
  - Verify a valid Hello World response is returned
- [ ] Run full test suite: `pytest` from `assets/hello-world-agent/` (no extra flags)
- [ ] Verify coverage ≥ 70%; add tests if below threshold
- [ ] Verify `assets/hello-world-agent/app/agent.py` has exactly 9 decorated functions:
  ```bash
  grep -c "^@agent_model\|^@agent_config\|^@prompt_section" assets/hello-world-agent/app/agent.py
  ```
  Confirm it returns 9. Remove any extras and replace with plain Python constants.
- [ ] Run `pytest` again (no args) to generate final `test_report.json`
- [ ] Verify `test_report.json` exists in `assets/hello-world-agent/`
