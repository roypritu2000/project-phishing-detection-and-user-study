# Phishing-score calibration and URL-stability audit

This repository implements only the machine-learning experiment described in `redesigned_project_plan.md`. Work proceeds one approved step at a time and uses Jupyter notebooks by default.

## Safety

The raw dataset contains real phishing URL strings. Treat every URL as inert text: never visit, resolve, request, preview, ping, or otherwise contact any listed URL or hostname. All URL processing in this project must be local and offline.

## Setup

Python 3.13.15 was used to create the local environment.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
```

Run the Step 1 environment check from the repository root:

```bash
JUPYTER_CONFIG_DIR=/tmp/phishing-audit-jupyter-config \
JUPYTER_DATA_DIR=/tmp/phishing-audit-jupyter-data \
JUPYTER_RUNTIME_DIR=/tmp/phishing-audit-jupyter-runtime \
IPYTHONDIR=/tmp/phishing-audit-ipython \
MPLCONFIGDIR=/tmp/phishing-audit-matplotlib \
  .venv/bin/jupyter nbconvert \
  --to notebook \
  --execute notebooks/00_environment_check.ipynb \
  --output 00_environment_check.executed.ipynb \
  --output-dir /tmp
```

For interactive work:

```bash
.venv/bin/jupyter lab
```

## Repository layout

- `notebooks/`: ordered experiment and test notebooks.
- `data/raw/`: unchanged source data and its provenance record.
- `data/processed/`: reproducible split assignments and processed tables.
- `reports/`: human-readable findings and figures.
- `artifacts/`: generated model artifacts.
- `log.md`: chronological record of completed work and verification.

The raw CSV is intentionally excluded from version control. See `data/raw/README.md` for its official source, license, checksum, and handling rules.
