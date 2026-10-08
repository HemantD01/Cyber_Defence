# CyberDefense Backend

Flask REST API backend for `cyber_defense_project_front_end.html`.

## Setup

```bash
pip install -r requirements.txt
python app.py
```

Server runs on **http://localhost:5000**

---

## API Reference

### Overview
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/overview` | Dashboard summary stats |
| GET | `/api/health` | Health check |

### Phishing
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/phishing/analyse` | Analyse an email for phishing |
| GET  | `/api/phishing/history` | Past analysed emails |
| POST | `/api/phishing/simulate` | Generate a phishing simulation |

**POST /api/phishing/analyse — body:**
```json
{
  "sender":  "ceo@company-secure.net",
  "subject": "Urgent: verify your account",
  "body":    "Click here http://malicious.com to reset"
}
```

### Network
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/network/scan`    | Run a port scan simulation |
| GET  | `/api/network/results` | Get latest scan results |
| GET  | `/api/network/summary` | Risk counts for metric cards |

**POST /api/network/scan — body:**
```json
{ "target": "192.168.1.0/24" }
```

### Ransomware
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/ransomware/monitor/start`          | Start directory monitoring |
| POST | `/api/ransomware/monitor/stop`           | Stop monitoring |
| POST | `/api/ransomware/test`                   | Trigger a test detection |
| GET  | `/api/ransomware/incidents`              | List all incidents |
| POST | `/api/ransomware/playbook/<id>/advance`  | Advance playbook step |

### Logs
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/logs`        | Fetch logs (filterable) |
| GET | `/api/logs/export` | Download logs as CSV |

**GET /api/logs — query params:**
- `level` — CRIT / WARN / INFO / OK
- `source` — phishing / network / ransomware / system
- `search` — keyword filter
- `limit` — max rows (default 50)

---

## Connecting to the Front-End

Add this script block to your HTML to wire up the buttons:

```js
const API = "http://localhost:5000";

// Example: wire up the phishing analyse button
document.querySelector('.btn-accent').addEventListener('click', async () => {
  const res = await fetch(`${API}/api/phishing/analyse`, {
    method: 'POST',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({
      sender:  document.querySelector('[name=sender]').value,
      subject: document.querySelector('[name=subject]').value,
      body:    document.querySelector('[name=body]').value,
    })
  });
  const data = await res.json();
  console.log(data); // { score, verdict, spf_pass, ... }
});
```
