# Time-Budget Protocol for Task Execution

## Rule
Every task gets max 3 attempts or 5 min wall-clock, whichever hits first.

## For Me (The Agent)
1. Start task with expected budget (tokens + wall time)
2. If first attempt fails: retry once with different approach
3. If second attempt fails: escalate to elder (GLM 5.2 or Opus 5) with full context
4. After elder response: one more attempt. If fails: log as BLOCKED, move on
5. Budget per subtask: 3000 tokens or 120s, max

## Task Types & Budgets
| Type | Token Budget | Wall Time | Escalate After |
|------|-------------|-----------|----------------|
| Browser auth (OAuth) | 2000 | 120s | 2 failures → log as human-required |
| API call (OpenRouter) | 5000 | 180s (model response) | timeout → retry once → fallback model |
| File write/edit | 500 | 30s | N/A |
| Git operation | 1000 | 30s | permission error → log |
| Web search | 2000 | 30s | no results → try different query |
| Firecrawl scrape | 1500 | 30s | timeout → retry once → skip |

## Rules
- Do not spend more than 5 min on any single blocking task
- Log BLOCKED items with: what, why, which elder needs to advise
- Move on to next unblocked item immediately
- Never loop more than 2x on the same approach

## Elders to Escalate To
| Domain | Elder | Model |
|--------|-------|-------|
| Architecture/Design | CTO | z-ai/glm-5.2 |
| Strategic Decision | GM | moonshotai/kimi-k3 |
| Final Approval | Board | anthropic/claude-opus-5 |
| Bias Check | Supervisor | x-ai/grok-4.5 |
| Resource/Tool | HR | deepseek/deepseek-v4-pro |
