# 🧩 Run All APIs - Flask Project

A small Flask-based automation dashboard that runs a sequence of BVS-related APIs (OTP, Login, BVSAccountRegistration OTP, Cash Deposit, Withdrawal, CNIC transfers) and displays each API response in the browser when you click the "Run All APIs" button.

This README replaces and expands the previous README to provide clear setup, configuration, usage, and development information so contributors and users can get started quickly.

Table of Contents
- About
- Features
- How it works
- Prerequisites
- Installation
- Configuration
- Running locally
- Project structure
- API flow / Endpoints
- UI
- Testing
- Contributing
- License

About

This repository contains a Flask web application that orchestrates a set of backend API calls used for demo/automation/testing purposes. The app sends requests to the configured API endpoints in a fixed order and renders each response in a friendly format in the browser.

Features
- Runs a predefined sequence of APIs with one click ("Run All APIs").
- Shows each API request and response in the browser, including status code and formatted JSON.
- Minimal Flask frontend to trigger runs and display results.
- Easy to configure endpoints and payloads via environment variables or a config file.

How it works

1. The user opens the web UI and clicks "Run All APIs".
2. The server performs the API calls in sequence: OTP -> Login -> BVSAccountRegistration OTP -> Cash Deposit -> Withdrawal -> CNIC Transfer (or the configured sequence).
3. Each API response (status, headers, JSON body) is captured and sent back to the frontend to be displayed.

Prerequisites
- Python 3.8+ (3.10 recommended)
- pip
- (Optional) virtualenv or venv for isolated environment

Installation

1. Clone the repository:

   git clone https://github.com/usmanfarooq317/bvs-apis-dashboard.git
   cd bvs-apis-dashboard

2. Create and activate a virtual environment (recommended):

   python -m venv venv
   # On macOS / Linux
   source venv/bin/activate
   # On Windows (PowerShell)
   venv\Scripts\Activate.ps1

3. Install dependencies:

   pip install -r requirements.txt

Configuration

The application can be configured using environment variables or a config file (depending on how you prefer to manage secrets). The most common settings:

- API_BASE_URL — Base URL for the BVS APIs (if you proxy requests through a single host)
- OTP_ENDPOINT — Relative path or full URL to request OTP
- LOGIN_ENDPOINT — Relative path or full URL to login
- BVS_REG_OTP_ENDPOINT — Relative path for BVS account registration OTP
- CASH_DEPOSIT_ENDPOINT — Endpoint for cash deposit
- WITHDRAWAL_ENDPOINT — Endpoint for cash withdrawal
- CNIC_TRANSFER_ENDPOINT — Endpoint for CNIC transfer
- AUTH_HEADERS or API_KEY — Any auth header or key required by the APIs

You can export these variables in your shell or create a .env file and load it (the project may already include dotenv support; if not, set in your environment):

   export API_BASE_URL=https://api.example.com
   export AUTH_HEADERS='{"Authorization": "Bearer TOKEN"}'

Running locally

There are two common ways to run the app depending on how the repo is structured. If the project provides an entrypoint script (app.py or run.py), use that; otherwise use Flask's CLI.

Example (if app.py exists):

   python app.py

Example (Flask CLI):

   export FLASK_APP=app.py
   export FLASK_ENV=development
   flask run --host=0.0.0.0 --port=5000

Open http://localhost:5000 in your browser and click "Run All APIs".

Project structure

A typical layout for this project (actual files may vary slightly):

- app.py or run.py — Flask application entrypoint
- requirements.txt — Python dependencies
- templates/
  - index.html — Main UI page with "Run All APIs" button
- static/ — CSS and JavaScript used by the UI
- config.py or .env — Configuration for endpoints and secrets
- api_runner.py or services/ — Logic that composes and executes the API calls

API flow / Endpoints

The app executes the API sequence in a specific order. You can customize this order inside the runner logic. Typical sequence:
1. OTP request — sends mobile or identifier, expects an OTP challenge response
2. Login — uses OTP (or test credentials) to authenticate and receive a session/token
3. BVSAccountRegistration OTP — sends registration OTP request
4. Cash Deposit — simulates a deposit transaction
5. Withdrawal — simulates a withdrawal transaction
6. CNIC Transfer — simulates a CNIC holder transfer or KYC related call

Each step captures:
- Request payload
- Request URL and method
- Response status code
- Response body (JSON formatted)
- Any error information or stack traces (kept minimal for UI display)

UI

The UI is intentionally minimal: a single page with a button to start the sequence and an area to display per-call results. The frontend uses JavaScript (fetch/XHR) to call a server endpoint that triggers the sequence and streams back results as they complete.

Testing

If the repository includes tests, run them with:

   pytest

If there are no dedicated tests, consider adding unit tests for the runner logic (mocking HTTP calls with responses or requests-mock) and basic UI tests.

Contributing

Contributions are welcome. Suggested ways to contribute:
- Improve documentation and README (this file)
- Add tests for the API runner and UI
- Make the configuration more flexible (YAML/JSON config files, per-environment configs)
- Add authentication handling and secure secret management

When opening a PR, please:
1. Fork the repository
2. Create a feature branch
3. Add tests where appropriate
4. Open a pull request with a clear description of changes

License

If you want to use a license, add a LICENSE file to the repository. If you prefer MIT, Apache-2.0, or similar, include that file and update this section.

Credits

Created and maintained by the repository owner.
