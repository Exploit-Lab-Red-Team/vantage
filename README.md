<p align="center">
  <img src="vantage.jpg" alt="Vantage" width="600">
</p>

# Vantage

**Continuous attack surface intelligence for startups, SaaS companies, and security teams.**

Discover, map, and monitor internet-facing assets to understand what your organization exposes from the outside.

[![Go Version](https://img.shields.io/badge/Go-1.22+-00ADD8?logo=go\&logoColor=white)](https://golang.org)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS-blue.svg)](https://github.com/VantageASM/vantage)

---

## What Vantage Does

* **Subdomain enumeration** — subfinder, assetfinder, optional DNS bruteforce
* **HTTP probing** — live host detection, technology fingerprinting, TLS information
* **Port scanning** — connect scanning, top-100/1000/custom ports
* **Banner grabbing** — service and version detection from real TCP banners
* **Attack surface mapping** — 50+ technology, path, and port signals with actionable notes
* **Screenshots** — automatic screenshots of live hosts
* **Network expansion** — discovered IP → `/24` expansion in aggressive mode
* **JavaScript analysis** — extract endpoints, API keys, and potential secrets
* **Secrets detection** — cloud keys, JWTs, SSH/PGP keys, API tokens, entropy filtering, and context capture
* **Technology checks** — WordPress, Laravel, Django, Next.js, Jenkins, GitLab, Grafana, Kubernetes, Elasticsearch, Docker, and more
* **Cloud asset discovery** — S3/Azure/GCP storage, cloud IP ranges, Kubernetes endpoints, Docker APIs
* **Alerts** — Telegram notifications with deduplication, rate limiting, and severity filtering
* **Change tracking** — record newly discovered assets, hosts, ports, and other changes
* **Exports** — Caido, Burp Suite, Metasploit, CSV, target lists, and JSON
* **Dashboard** — local web dashboard with live scan activity

---

## Scan Profiles

| Profile      | Ports            |   Rate | Banner | Screenshot | Bruteforce | Net Expand |
| ------------ | ---------------- | -----: | ------ | ---------- | ---------- | ---------- |
| `stealth`    | 80,443,8080,8443 |   50/s | No     | No         | No         | No         |
| `standard`   | top-100          |  500/s | Yes    | Yes        | No         | No         |
| `aggressive` | top-1000         | 2000/s | Yes    | Yes        | Yes        | Yes        |

---

## Requirements

* Linux (x86_64 or arm64)
* Go 1.22+
* gcc
* libsqlite3-dev

---

## Installation

### Quick Install

```bash
git clone https://github.com/VantageASM/vantage.git
cd vantage
chmod +x scripts/install.sh
./scripts/install.sh
```

### Build from Source

```bash
sudo apt-get install -y gcc libsqlite3-dev build-essential

# Install Go 1.22+
# Then install the required reconnaissance tools

export PATH=$PATH:$(go env GOPATH)/bin

go install github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install github.com/tomnomnom/assetfinder@latest
go install github.com/projectdiscovery/httpx/cmd/httpx@latest
go install github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
go install github.com/projectdiscovery/dnsx/cmd/dnsx@latest
go install github.com/sensepost/gowitness@latest

# Naabu raw socket permission
sudo setcap cap_net_raw+ep $(which naabu)

# Build
CGO_ENABLED=1 go build -mod=vendor -o vantage ./cmd/
```

### Docker

```bash
docker compose up -d
```

---

## Usage

```bash
# One-shot scans
./vantage scan -d target.com
./vantage scan -d target.com -p stealth
./vantage scan -d target.com -p aggressive

# Dashboard
./vantage serve

# Headless monitoring
./vantage monitor

# Scan configured targets
./vantage scan
```

The dashboard runs locally at:

```text
http://127.0.0.1:8080
```

---

## Dashboard Pages

| Page               | What It Shows                                                              |
| ------------------ | -------------------------------------------------------------------------- |
| **Dashboard**      | Statistics, recent scans, and live activity                                |
| **Assets**         | Discovered subdomains, IPs, types, and first-seen data                     |
| **Live Hosts**     | HTTP/S hosts, status, titles, technologies, and screenshots                |
| **Ports**          | Open ports, services, versions, and banners                                |
| **Attack Surface** | Per-host attack surface signals and notes                                  |
| **Interesting**    | Admin panels, login pages, APIs, development environments, and risky ports |
| **JS Analysis**    | Discovered endpoints, secrets, and API keys                                |
| **Tech Checks**    | Technology-specific security checks                                        |
| **Cloud Assets**   | Cloud storage, Kubernetes, Docker, and cloud infrastructure findings       |
| **Alerts**         | Notification history                                                       |
| **Changes**        | Newly discovered or changed assets and services                            |
| **Scans**          | Scan history and exports                                                   |

---

## Exports

| Export           | Use For                                     |
| ---------------- | ------------------------------------------- |
| Caido scope JSON | Import targets into Caido                   |
| Burp Suite XML   | Import targets into Burp Suite              |
| Metasploit `.rc` | Load discovered services into Metasploit    |
| CSV              | Reporting and analysis                      |
| URL list         | Feed URLs into other security tools         |
| IP:port list     | Feed discovered services into other tooling |
| JSON             | Full scan snapshot                          |

---

## Configuration

Copy the example configuration:

```bash
cp vantage.example.yaml vantage.yaml
nano vantage.yaml
```

Key configuration sections:

* `alerting` — Telegram notifications, severity thresholds, and event types
* `tech_checks` — enabled checks, threads, and timeouts
* `cloud_recon` — cloud discovery and timeout settings

See `vantage.example.yaml` for the complete configuration reference.

---

## Vantage Pro

**Vantage Pro** is the hosted version of Vantage for organizations that need continuous external attack surface visibility without managing scanning infrastructure themselves.

Vantage Pro adds capabilities including:

* Continuous monitoring
* Scheduled scanning
* Organization and team management
* Authorized asset verification
* Historical exposure tracking
* Change alerts
* Security reports
* Integrations
* Managed scanning infrastructure

The Vantage open-source engine is the foundation of Vantage Pro.

---

## Responsible Use

Vantage is intended for authorized security testing, attack surface discovery, and defensive security operations.

Only scan systems and infrastructure you own or have explicit authorization to assess.

You are responsible for complying with applicable laws, regulations, contracts, and testing policies.

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for contribution guidelines.

---

## License

Vantage is released under the [MIT License](LICENSE).
