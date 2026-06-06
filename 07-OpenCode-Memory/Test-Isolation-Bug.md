# Test Isolation Bug — 2026-06-06

## Problem
`test_notion_sync.py::TestNotionDashboard::test_ready_property` fails when run in full suite but passes in isolation.

## Root Cause
- `NotionDashboard.__init__` does `self.token = token or os.getenv("NOTION_TOKEN", "")`
- `test_ready_property` creates `NotionDashboard(token="")` and expects `ready is False`
- If `NOTION_TOKEN` env var is set (from a previous test), `token=""` falls back to env value
- The test ready property then returns `True` (token non-empty)

## Polluter
- `test_cli_smoke.py::test_version_flag` (and any other test calling `main()`) → some import or side effect set NOTION_TOKEN
- Bisect: only test_version_flag (a single 0.05s test) was enough to pollute

## Fix
Added `setup_method` to `TestNotionDashboard` class:

```python
def setup_method(self, method):
    """Clear Notion env vars before each test to prevent test pollution."""
    for key in ("NOTION_TOKEN", "NOTION_PARENT_PAGE", "NOTION_CAMPAIGN_DB", "NOTION_CONTENT_DB", "NOTION_ANALYTICS_DB"):
        os.environ.pop(key, None)
```

## Test Counts Progress
- After BATCH A: 906 tests
- After BATCH B-F: 988 tests
- After BATCH H: 1006 tests (16 perf)
- After E2E: 1025 tests
- After BATCH I (caption/cta/calendar): 1089 tests
- After BATCH J (CLI smoke + cache + rate burst): 1210 tests
- After pytest-asyncio config: 1131 tests (62 async fixed)
- After BATCH J: 1251 tests
- **After setup_method fix: 1252 tests, 0 failures, 4 skipped**

## Lesson Learned
- When tests rely on env vars, ALWAYS use `patch.dict(os.environ, ..., clear=True)` OR a setup_method
- Bisecting test pollution: use 21-1-1-1 approach to find culprit file in <5 runs
- Test isolation issues are hard to spot — only show up in full runs

## Pattern for Future Tests
```python
@pytest.fixture(autouse=True)
def clear_env(monkeypatch):
    for key in list(os.environ.keys()):
        if key.startswith("MY_APP_"):
            monkeypatch.delenv(key, raising=False)
```
