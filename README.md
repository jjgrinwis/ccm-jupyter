# Akamai CCM Demo Notebook

A Jupyter notebook that demonstrates how to create a third-party certificate in [Akamai Cloud Certificate Manager](https://techdocs.akamai.com/ccm/docs/welcome) (CCM) via the API.

The notebook walks through:

1. Creating a Certificate Signing Request (CSR) via `POST /ccm/v1/certificates`
2. Signing it locally with a self-signed CA (stands in for a real CA in a demo)
3. Validating domain ownership via `POST /domain-validation/v1/domains/search` before uploading
4. Uploading the signed certificate back via `PUT /ccm/v1/certificates/{id}`
5. *(Optional)* Renewing the certificate by cloning it — fetching its configuration via `GET /ccm/v1/certificates/{id}`, creating a new CSR with the same settings, and signing and uploading the clone
6. Deleting both certificates to clean up

The renewal/clone section can be skipped by setting `demo_clone = False` in the notebook before running.

## Prerequisites

- An Akamai account with CCM enabled on at least one contract
- An API client with [CCM permissions](https://techdocs.akamai.com/ccm/reference/get-started) and credentials in `~/.edgerc`

## 1. Install Git

Git is needed to download (clone) this repository.

- **macOS**: Git is included with Xcode Command Line Tools. Run `git --version` in a terminal — if it's not installed, macOS will prompt you to install it.
- **Windows**: Download from [git-scm.com](https://git-scm.com/download/win) and run the installer.
- **Linux**: `sudo apt install git` (Debian/Ubuntu) or `sudo dnf install git` (Fedora/RHEL).

## 2. Clone this repository

Open a terminal and run:

```bash
git clone https://github.com/jjgrinwis/ccm-jupyter.git
cd ccm-jupyter
```

This downloads the notebook and all supporting files to your machine.

## 3. Install uv

`uv` is a fast Python package manager used to install dependencies and run the notebook.

**macOS / Linux:**

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows (PowerShell):**

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

After installation, restart your terminal so the `uv` command is available.

## 4. Run the notebook

Install dependencies and launch Jupyter Lab in one step:

```bash
uv run jupyter lab create-ccm-cert.ipynb
```

`uv` will automatically create a virtual environment and install all required packages on the first run.

## Configuration

All configuration is done via environment variables before running the notebook:

| Variable | Required | Description |
|---|---|---|
| `AKAMAI_EDGEGRID_SECTION` | No | Section in `~/.edgerc` to use (default: `default`) |
| `AKAMAI_ACCOUNT_SWITCH_KEY` | No | Switch to a specific account (format: `1-XXXXX:1-XXXXX`) |
| `AKAMAI_CONTRACT_ID` | Yes | Contract ID to create the certificate under |
| `AKAMAI_GROUP_ID` | Yes | Group ID to create the certificate under |
| `AKAMAI_DEMO_BASE_DOMAIN` | No | Base domain for the demo hostname (default: `great-demo.com`) |

Example:

```bash
export AKAMAI_EDGEGRID_SECTION="default"
export AKAMAI_CONTRACT_ID="1-ABC123"
export AKAMAI_GROUP_ID="12345"
```
