# ipapp • v0.1.0

Tiny **stdlib-only** client for [ip.app](https://ip.app).
Query your public IP address, ASN data, request headers, user-agent string, and more — programmatically *or* from the command line.

---

## 🛠 Installation

```bash
# From PyPI (after you publish)
pip install ipapp

# Local development / editable install (with test extras)
python -m pip install -e .[dev]
```

Python ≥ 3.8, no external runtime dependencies.

### Using a virtual environment (recommended)

If your system Python is *externally managed* (PEP 668) you’ll need an isolated environment instead of installing into the OS packages. The fastest path is a **virtual env**:

```bash
python3 -m venv .venv
source .venv/bin/activate
python3 -m pip install -e '.[dev]'
```

(Upgrade `pip` inside the venv if you like: `python -m pip install --upgrade pip`.)

Alternatively you can keep things completely separate with **pipx**:

```bash
pipx install --editable . --pip-args='.[dev]'
```

---

## 🚀 Quick-start (Python)

Each endpoint gets its own snippet so you can copy/paste exactly what you need.

### Public IP

```python
import ipapp
print(ipapp.get_ip())          # → "203.0.113.42"
```

### Public IP via HEAD (fastest)
```python
import ipapp
print(ipapp.get_ip(head=True))     # "203.0.113.42" via X-Ipapp-Ip header
```

### ASN (plain-text)

```python
import ipapp
print(ipapp.get_asn())         # → "AS12345"
```

### ASN (JSON)

```python
import ipapp
info = ipapp.get_asn(json=True)
# {'asn': 12345, 'holder': 'Example ISP', 'country': 'DE', ...}
```

### ASN with caller IP merged (JSON only)

```python
import ipapp
info = ipapp.get_asn(json=True, include_ip=True)
# {'asn': 12345, 'holder': 'Example ISP', ..., 'ip': '203.0.113.42', 'ip_version': '4'}
```

### User-Agent string

```python
import ipapp
print(ipapp.get_user_agent())  # → "python-urllib/3.12"
```

### User-Agent string (JSON)

```python
import ipapp
print(ipapp.get_user_agent(json=True))  # {"ua": "python-urllib/3.12", ...}
```

### User-Agent with caller IP merged (JSON only)

```python
import ipapp
print(ipapp.get_user_agent(json=True, include_ip=True))
# {"ua": "python-urllib/3.12", "ip": "203.0.113.42", "ip_version": "4"}
```

### Request headers (JSON)

```python
import ipapp
headers = ipapp.get_headers()  # dict with all inbound headers
```

All helpers raise `ipapp.IPAppError` on network problems or invalid responses.

---

## 🖥 Quick-start (CLI)

Every function is mirrored by a sub-command.
No arguments prints the IP (nice for scripts):

```bash
ipapp                        # 198.51.100.7
```

Run each endpoint separately:

```bash
ipapp ip                     # same as default

ipapp asn                    # AS12345
ipapp asn --json             # {"asn": 12345, ...}

ipapp loc                    #

ipapp headers                # {"User-Agent": "...", ...}
ipapp headers --json         # explicit, same output
```

---

## 🧪 Running the test suite

Tests are network-free (they monkey-patch the internal fetcher) so they run instantly:

```bash
pytest -q
# 4 passed in 0.02s
```

---

## 📦 Building & checking the package

```bash
# Ensure build tools are present
python -m pip install build twine

# Build wheel + sdist into ./dist/
python -m build

# Verify metadata is valid (no upload)
twine check dist/*
```

When you're ready to release, upload with Twine (or wire this into CI):

```bash
twine upload dist/*
```

---

## 🤝 Contributing

* Fork, create a feature branch, add tests for any new behaviour.
* Keep code **PEP 8** compliant (the codebase sticks to stdlib only).
* Open a PR – CI will run `pytest` and basic metadata checks.

---

MIT licensed – have fun!
