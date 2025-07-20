# fuze-kpi-compliance-dashboard

> **Live CEX KPI observability for Web3 tokens**  
> Next.js (React 18) frontend ＋ Grafana + TimescaleDB stack, shipped as a one‑command Docker bundle.

[![build](https://github.com/fuze-ac/fuze-kpi-compliance-dashboard/actions/workflows/ci.yml/badge.svg)](./actions)
[![Next.js 14](https://img.shields.io/badge/Next.js-14-brightgreen)](https://nextjs.org/)
[![license](https://img.shields.io/badge/license-MIT-lightgrey.svg)](#license)

---

## ✨  Features

| Module                         | What It Does                                                |
|--------------------------------|-------------------------------------------------------------|
| **KPI Collector**              | Ingests CEX REST + WS data (volume, spread, depth) every 60 s |
| **Rule Engine**                | Evaluates exchange‑specific delist thresholds; raises alerts |
| **Grafana Dashboards**         | Pre‑built boards for volume trend, spread heat‑map, uptime   |
| **Next.js Portal**             | Auth‑gated token overview, risk bar, badge share links       |
| **Webhooks / Telegram bot**    | `delist_warning` & `kpi_recovered` notifications            |

---

## 🚀  Quick Start

```bash
git clone https://github.com/fuze-ac/fuze-kpi-compliance-dashboard.git
cd fuze-kpi-compliance-dashboard
cp .env.sample .env          # add API keys, DB creds
docker compose up            # spins Timescale, Grafana, Next.js
````

Then visit **[http://localhost:3000](http://localhost:3000)** → log in with the admin password set in `.env`.

---

## 📦  Directory Layout

```
.
├─ apps/
│  ├─ next-portal/          # Next.js 14 + shadcn/ui
│  └─ grafana/              # Provisioned dashboards JSON
├─ packages/
│  ├─ collector/            # Rust async-kafka ingestor
│  └─ rules-engine/         # TypeScript rule evaluators
├─ docker/
│  └─ compose.yml
└─ docs/                    # setup, schema diagrams
```

---

## 🛠️  Config

`config/kpi-rules.yaml`

```yaml
binance:
  volume_24h_min: 30000
  spread_avg_max: 0.03
mexc:
  volume_24h_min: 20000
  spread_avg_max: 0.025
```

Rules hot‑reload when the file changes.

---

## 🔐  Security & Auth

* Next.js API routes protected by JWT (hooks in `/auth`).
* Grafana behind reverse proxy; signed‑cookie single‑sign‑on from portal.
* Prometheus‑style metrics exposed on `:9001/metrics`.

---

## 📈  Screenshots

| Token Overview                 | Spread Heat‑Map                | Delist‑Watch Feed          |
| ------------------------------ | ------------------------------ | -------------------------- |
| ![portal](docs/img/portal.png) | ![spread](docs/img/spread.png) | ![feed](docs/img/feed.png) |

---

## 🧪  Tests

```bash
pnpm test        # Jest for rules engine
cargo test -p collector
```

Integration tests spin up a local Timescale container with sample KUCOIN data.

---

## 🤝  Contributing

1. Fork → feature branch (`feat/my-awesome-widget`)
2. Run `pnpm lint`, `cargo fmt`
3. Submit PR against `develop`

> We welcome new CEX adapters & dashboard panels!

---

## 📝  License

MIT © 2025 FUZE Foundation

