# Python Logging Lab

A hands-on introduction to Python's built-in `logging` library, extended with production-relevant logging patterns.

## Contents

| Section | Topic |
|---|---|
| 1 | Importing the Logging Library |
| 2 | Basic Logging Configuration |
| 3 | Logging Messages (DEBUG, INFO, WARNING, ERROR, CRITICAL) |
| 4 | Using Custom Loggers |
| 5 | Logging Exceptions |
| 6 | Logging to a File |
| 7 | Controlling Log Levels |
| 8 | Log Handlers (StreamHandler + FileHandler) |
| 9 | ✨ Rotating File Handler |
| 10 | ✨ Structured JSON Logging |
| 11 | ✨ ML Training Logger |

✨ = added extensions

---

## Extensions (My Additions)

### 9. Rotating File Handler
Uses `RotatingFileHandler` to cap log file size at 1MB with 3 backup files. Simulates 100 system heartbeat entries to demonstrate log rotation in action — a standard pattern in production services to prevent unbounded disk usage.

**Output:** `rotating_app.log`

### 10. Structured JSON Logging
Implements a custom `JSONFormatter` class that outputs each log record as a JSON object with fields: `timestamp`, `level`, `logger`, `module`, and `line`. Supports attaching arbitrary extra fields (e.g. `user_id`, `endpoint`, `latency_ms`) — compatible with ELK Stack and cloud logging pipelines.

**Output:** `app_structured.log`

Sample output:
```json
{"timestamp": "2026-03-30T23:02:56.724485+00:00", "level": "INFO", "logger": "json_logger", "message": "Service started", "module": "...", "line": 36}
{"timestamp": "2026-03-30T23:02:56.725401+00:00", "level": "INFO", "logger": "json_logger", "message": "User request processed", "user_id": "u_789", "endpoint": "/predict", "latency_ms": 42}
```

### 11. ML Training Logger
Simulates a `GradientBoostingClassifier` training loop on the Wine Dataset, logging `loss`, `accuracy`, and `f1` at each epoch. Includes:
- Loss spike detection with `WARNING` alerts
- Early-epoch `DEBUG` messages for low accuracy warm-up
- Intentional `RuntimeError` for a missing evaluation file — caught and logged gracefully
- Output to both console and a rotating `training.log` file

**Output:** `training.log`

Sample output:
```
2026-03-30 19:02:56,740 [INFO] Epoch [1/10] | loss=0.9284 | acc=0.6255 | f1=0.6100
2026-03-30 19:02:56,740 [DEBUG] Epoch 1: accuracy still warming up (0.6255)
...
2026-03-30 19:02:56,749 [INFO] Epoch [10/10] | loss=0.1758 | acc=0.9569 | f1=0.9348
2026-03-30 19:02:56,749 [ERROR] Evaluation failed — training loop completed but eval skipped
```

---

## Setup

```bash
pip install notebook
jupyter notebook logging_lab.ipynb
```

No additional dependencies required — uses Python standard library only.

---

## Files Generated

| File | Description |
|---|---|
| `app.log` | Basic file log output (Section 6) |
| `rotating_app.log` | Rotating log, capped at 1MB with 3 backups (Section 9) |
| `app_structured.log` | JSON-formatted structured log (Section 10) |
| `training.log` | ML training run log with per-epoch metrics (Section 11) |