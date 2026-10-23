# SecureLog Sentinel

A lightweight, defensive security toolkit built for a cybersecurity bootcamp project. It has two modules:

1. **Log Analyzer** – parses Linux SSH auth logs, detects brute-force attacks (sliding time window) and flags IPs that log in successfully after repeated failures (possible compromise).
2. **Password Auditor** – estimates entropy, checks common passwords and patterns, and gives improvement tips.

## Features
- Detects brute-force attempts by IP, with configurable threshold and time window
- Severity levels: `HIGH` (brute force) and `CRITICAL` (success after failures)
- Exports alerts to JSON
- No external runtime dependencies (Python standard library only)
- Unit tests included

## Installation
```bash
git clone <your-repo-url>
cd securelog-sentinel
python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install -r requirements.txt                   # only needed for tests
```

## Usage
```bash
# Analyze a log: 5+ failures within 10 minutes triggers an alert
python sentinel.py logs sample_auth.log --threshold 5 --window 10 --json alerts.json

# Audit a password
python sentinel.py password "MyP@ssw0rd!"
```

Example output:
```
[CRITICAL] 203.0.113.45 - 8 failures (admin) - Successful login after failures - possible compromise
[HIGH] 192.0.2.99 - 6 failures (test) - Brute-force pattern detected
```

## Run tests
```bash
pytest -q
```

## Project structure
```
securelog-sentinel/
├── sentinel.py          # main tool (CLI)
├── test_sentinel.py     # unit tests
├── sample_auth.log      # synthetic sample data (documentation IPs)
├── requirements.txt
├── README.md
└── PROJECT_REPORT.md
```

## Ethical use
This tool is **defensive** and only reads log files you own or are authorized to analyze. The sample log is synthetic. Never test passwords you do not own.

## Limitations & future work
- Supports OpenSSH `auth.log` format only; no distributed-attack correlation
- Add GeoIP enrichment, email/Slack alerts, and a firewall (fail2ban-style) response
- 
