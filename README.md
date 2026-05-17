# 🔍 RO-Setup-Finder
> A high-efficiency, asynchronous network crawler designed to algorithmically brute-force, validate, and catalog official Ragnarok Online client installers, regional patches, and setup binaries.

## [![Ask DeepWiki](https://deepwiki.com/badge.svg)](https://deepwiki.com/AoShinRO/RO-Setup-Finder)

![Python](https://img.shields.io/badge/Language-Python_3.12-blue?style=for-the-badge&logo=python)
![Engine](https://img.shields.io/badge/Asynchronous-aiohttp_//_asyncio-purple?style=for-the-badge)
![Automation](https://img.shields.io/badge/CI%2FCD-GitHub_Actions-green?style=for-the-badge&logo=githubactions)

Finding official client files for Ragnarok Online across different regional distributors (kRO, twRO, jRO) is a major challenge due to unindexed directories and rotating filenames. **RO-Setup-Finder** automates link validation by generating multi-format date permutations over historical ranges, executing low-overhead async requests, and committing validated endpoints directly into a structured live database.

---

## 🚀 Live Data Stream

📅 Not Default Pattern Valid Links |
------|
http://rofull.gnjoy.com/KR_RO1_Live_20251001_122947.tar
[![Monthly Update Valid Links](https://img.shields.io/badge/%E2%96%B6%20Monthly%20Update%20Valid%20Links%20Here-e8b84b?style=for-the-badge&labelColor=cc0000)](https://github.com/AoShinRO/RO-Setup-Finder/blob/main/links_validos.md)
---

## ⚙️ Core Architecture & Operational Logic

### ⚡ Non-Blocking High-Throughput Scraper
The scanning engine avoids sequential execution bottlenecks by deploying an asynchronous HTTP pool handled via `aiohttp` and structured coroutines.

* **Rate-Limit Guard:** Includes a `0.15s` artificial thread cooldown combined with a strict concurrency bottleneck restriction (`asyncio.Semaphore(6)`). This prevents target Content Delivery Networks (CDNs) from flagging the runtime environment as a malicious DoS vector.
* **Date Mask Parsing:** Automatically rolls dates sequentially starting from `January 1st, 2020` up through the execution timestamp, mapping strings into alternating short-year masks (`%y%m%d` -> `d6`) and full-century masks (`%Y%m%d` -> `d8`).

### 🗺️ Monitored Regional CDNs & Distribution Footprints

The pattern matrix targets official distribution nodes, mapping formats across servers for varying environments:

| Regional Publisher / Tag | Core Domain Endpoint | Formats Searched | Target Infrastructure |
| :--- | :--- | :--- | :--- |
| **Gravity kRO / Zero** | `rofull.gnjoy.com` | `.exe`, `.zip`, `.bin` | Main live clients, Sakray test beds, and Zero nodes. |
| **Gravity Game Taiwan** | `twcdn.gnjoy.com.tw` | `.exe` | Taiwanese localized setups (`RO_Install`, `RAGNAROK`). |
| **GungHo Japan (jRO)** | `patch2.gungho.jp` | `.tar` | Full client tarballs (`JP_Live`). |

---

## 🛠️ Automated Cron Infrastructure

The scanner handles automated collection tasks independently via standard CI/CD pipelines.

The built-in GitHub Actions workflow (`.github/workflows/main.yml`) uses a cron trigger configured for the **1st day of every month** (`0 0 1 * *`). It performs clean dependencies setup, executes the multi-threaded network check, compares output deltas, and automatically commits fresh results to `links_validos.md` using isolated runner tokens.

---

## 🖥️ Local Execution & Environment Setup

### Prerequisites
* Python `3.9+` (Tested on `3.12`)
* Network access to target domains

### Installation
1. Clone the repository:
```bash
   git clone https://github.com/AoShinRO/RO-Setup-Finder.git
   cd RO-Setup-Finder
```

2. Install the necessary network hooks:
```bash
pip install aiohttp
```

3. Run the generator:
```bash
python3 rosetupfinder.py
```

4. Check the results in the newly compiled file: `links_validos.md`

---

## ⚖️ Disclaimer & Compliance Notice

This tool is designed strictly for research, digital preservation, and educational archiving purposes. **RO-Setup-Finder does not pull down or store the actual game binaries** — it issues connection requests to identify active download links. All checked links resolve to official, unmodified distribution systems owned by Gravity Co., Ltd. and their associated regional operational partners.

---

## 📜 License

This project is open-source and licensed under the terms of the [GNU General Public License v3.0](https://www.google.com/search?q=LICENSE).
