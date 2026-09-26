# PulseTok: Tailored Tokenization for ECG Diagnostics

PulseTok is a research/demo web application for classifying 12-lead ECG records with a hybrid 1D CNN and GraphSAGE model. It accepts WFDB record pairs, displays predicted ECG superclasses and scores, and can request an explanatory report from Google's Gemini API.

This project is for research and educational use only. It is not a medical device and must not be used to diagnose or guide treatment.

## Features

- Account registration, login, and local prediction history
- ECG record upload using matching `.hea` and `.dat` files
- Prediction scores for CD, HYP, MI, NORM, and STTC
- Optional Gemini-generated report text
- Prediction history and per-user analytics

## Requirements

- Python
- A PyTorch build compatible with your system
- The Python packages listed below

From the `App` directory, create and activate a virtual environment, then install the application dependencies:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install Flask numpy wfdb torch torch-geometric google-generativeai
```

Install a PyTorch build appropriate for your CPU/GPU before `torch-geometric` if your platform requires a specific wheel combination.

## Run

The application loads `model/ptbxl_gnn_hybrid_final.pth` at startup. From the `App` directory, set a Flask session secret and, optionally, a Gemini API key:

```powershell
$env:FLASK_SECRET_KEY = [guid]::NewGuid().ToString('N')
$env:GEMINI_API_KEY = "your-gemini-api-key"
python app.py
```

Open `http://127.0.0.1:5000`. Without `GEMINI_API_KEY`, ECG classification still works, but generated reports are unavailable. Set a stable, private `FLASK_SECRET_KEY` in any persistent or deployed environment.

Upload `.hea` and `.dat` files with the same base name. The application creates its SQLite database locally as `database.db` and removes temporary uploaded records after processing.

## Repository Contents

- `app.py` and `templates/`: Flask application and pages
- `ECG_report.py`: model, prediction, and report-generation functions
- `model/ptbxl_gnn_hybrid_final.pth`: model weights used by the web application
- `model/ptb-xl-ecg_proposed.ipynb` and `model/exsisting-system.ipynb`: related notebooks

Local credentials, the SQLite database, uploads, virtual environments, raw files under `model/test/`, and the PTB-XL dataset notebook are excluded by `.gitignore`.
