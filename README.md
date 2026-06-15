# trust-anchor

Host trust anchors automatically, for any zone, whenever and wherever you want.

## Overview

IANA hosts [root-anchors.xml](https://data.iana.org/root-anchors/root-anchors.xml) for use as trust anchors to verify DNSSEC in root servers. However, no trust anchor is maintained for other zones (TLDs, SLDs, or other zones), even though this is compliant with existing RFCs and does not violate any standards.

**trust-anchor** is a FastAPI-based server that:
- Fetches DNSSEC trust anchors (DNSKEY and DS records) for any specified zone
- Validates the DNSKEY/DS records
- Hosts the trust anchor in proper XML format (compatible with DNSSEC validators)
- Automatically refreshes the trust anchor at configurable intervals

## Features

- 🔄 **Automatic Refresh**: Periodically fetches and validates the latest DNSSEC records
- 🛡️ **DNSSEC Validation**: Validates DNSKEY and DS record relationships
- 🌍 **Zone Agnostic**: Works with any DNS zone (root, TLD, SLD, or custom zones)
- 📋 **XML Output**: Serves trust anchors in standard XML format
- ⚙️ **Configurable**: Environment variable-based configuration
- 🚀 **FastAPI**: Modern, fast, async-capable Python web framework

## Prerequisites

- Python 3.7+
- DNS zone with properly configured DNSSEC

## Installation

1. Clone the repository:
```bash
git clone https://github.com/indiainternetfoundation/trust-anchor
cd trust-anchor
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

## Configuration

Configure the server using environment variables in a `.env` file:

```env
# Zone to fetch trust anchor for (default: in.)
ZONE=example.com

# HTTP endpoint path to serve the trust anchor (default: trust-anchor.xml)
ENDPOINT=trust-anchor.xml

# Source identifier in the generated XML (default: generated)
SOURCE=my-service

# Refresh interval in seconds (default: 3600)
REFRESH_INTERVAL=3600
```

## Usage

Start the server:

```bash
python -m fastapi run src/main.py
```

Or with uvicorn directly:

```bash
uvicorn src.main:app --reload
```

The trust anchor will be available at:
```
http://localhost:8000/trust-anchor.xml
```

(or whatever path you configured in `ENDPOINT`)

## Project Structure

```
trust-anchor/
├── src/
│   ├── main.py          # FastAPI application entry point
│   └── utils.py         # DNSSEC validation utilities
├── requirements.txt     # Python dependencies
├── README.md           # This file
└── LICENSE             # Project license
```

## License

See LICENSE file for details.
