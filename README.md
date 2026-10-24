# Automated Web Scanner: HTTP Security Headers & CORS Auditor

## 1. Project Overview
**Automated Web Scanner** is a Python-based security auditing tool and data analysis pipeline that evaluates websites against the **OWASP Secure Headers Project** standards and actively probes endpoints for **Cross-Origin Resource Sharing (CORS)** misconfigurations. It supports multi-threaded CLI scanning, interactive Jupyter Notebook analysis, automated compliance grading (`A+` to `F`), and structured report generation (`.json`, `.csv`, and `.png` charts).

---

## 2. Dataset Link & Benchmark Sources
This project uses public domain ranking datasets to benchmark web security compliance across the internet:
* **Primary External Dataset (Tranco Top Sites List):** [https://tranco-list.eu/](https://tranco-list.eu/) (Downloadable CSV of top global domains for security research).
* **Secondary Reference Dataset (HTTP Archive Web Almanac):** [https://almanac.httparchive.org/en/2024/security](https://almanac.httparchive.org/en/2024/security)
* **Built-in Benchmark Dataset:** If no external CSV/file is passed, the script automatically audits a curated 10-domain benchmark dataset (`google.com`, `github.com`, `wikipedia.org`, `cloudflare.com`, `mozilla.org`, `example.com`, `python.org`, `owasp.org`, `httpbin.org`, and `w3.org`).

---

## 3. Key Features
* **Deep OWASP Header Inspection:** Checks presence and semantic strength of `Strict-Transport-Security` (validating `max-age` $\ge 31536000$), `Content-Security-Policy` (flagging `'unsafe-inline'`, `'unsafe-eval'`, and wildcards), `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, `Permissions-Policy`, and COOP/CORP isolation headers.
* **Active CORS Vulnerability Probing:** Sends custom `Origin` and `Origin: null` probes to detect arbitrary origin reflection, credentialed wildcard policies (`Access-Control-Allow-Origin: *` with `Allow-Credentials: true`), sandboxed iframe `null` origin bypasses, and missing `Vary: Origin` caching headers.
* **Cookie & Banner Auditing:** Inspects `Set-Cookie` headers for missing `Secure`, `HttpOnly`, and `SameSite` flags, and detects version disclosure in `Server` and `X-Powered-By` headers.
* **Quantitative Scoring & Visual Analytics:** Calculates a weighted score ($0\text{--}100$), assigns a letter grade (`A+` through `F`), exports a Pandas summary table to `scan_results.csv`, and saves a grade distribution bar chart (`scan_results_grades.png`).

---

## 4. Technologies Used
* **Programming Language:** Python 3.9+
* **Networking & Concurrency:** `requests`, `urllib3`, `concurrent.futures` (`ThreadPoolExecutor`)
* **Data Analysis & Visualization:** `pandas`, `matplotlib`, `seaborn`
* **Notebook & Documentation Export:** `jupyter`, `nbformat`, `python-docx`

---

## 5. Project Structure
```text
├── PalakpreetKaur_AutomatedWebScanner.py   # Complete project code (CLI + Jupyter cell markers)
├── PalakpreetKaur_AutomatedWebScanner.ipynb        # Jupyter Notebook (auto-generated via flag)
├── PalakpreetKaur_AutomatedWebScanner.md         # Complete Project Report source documentation
├── PalakpreetKaur_AutomatedWebScanner.docx       # Word Document Report (auto-generated via flag)
├── requirements.txt                  # Required Python libraries and dependencies
└── README.md                         # Project documentation and setup guide
```

---

## 6. Setup & Run Instructions

### Step 1: Install Dependencies
Create a virtual environment (optional) and install the required packages from `requirements.txt`:
```bash
pip install -r requirements.txt
```

### Step 2: Generate `.ipynb` and `.docx` Submission Files Automatically
Run the built-in submission exporter to automatically produce `PalakpreetKaur_AutomatedWebScanner.ipynb` and `PalakpreetKaur_AutomatedWebScanner.docx` in your folder:
```bash
python PalakpreetKaur_AutomatedWebScanner.py --generate-submission
```

### Step 3: Run the Scanner on the Benchmark Dataset
Execute the script without arguments to audit the built-in 10-site benchmark dataset and generate `scan_results.json`, `scan_results.csv`, and `scan_results_grades.png`:
```bash
python PalakpreetKaur_AutomatedWebScanner.py
```

### Step 4: Audit Custom URLs or a Tranco Dataset CSV
Audit specific URLs:
```bash
python PalakpreetKaur_AutomatedWebScanner.py https://example.com https://github.com -o custom_report.json
```
Or pass a text/CSV file of domains:
```bash
python PalakpreetKaur_AutomatedWebScanner.py -f targets.csv -w 10 -o dataset_report.json
```

### Step 5: Run in Jupyter Notebook
Open the generated notebook in Jupyter Lab or Jupyter Notebook and run all cells:
```bash
jupyter notebook PalakpreetKaur_AutomatedWebScanner.ipynb
```

---

## 7. Key Information & Sample Output
Each audited target receives an itemized breakdown sorted by severity (`HIGH`, `MEDIUM`, `LOW`, `INFO`, `PASS`) along with concrete remediation guidance:
```text
========================================================================
TARGET: https://github.com
FINAL URL: https://github.com/ (HTTP 200)
SECURITY SCORE: 95/100  |  GRADE: A+
========================================================================
[LOW]    (HEADER) Permissions-Policy: Permissions-Policy header is missing.
         -> Fix: Add Permissions-Policy to restrict sensitive browser features.
[PASS]   (HEADER) HTTPS Enforcement: Endpoint is served securely over HTTPS.
[PASS]   (HEADER) Strict-Transport-Security: Strong HSTS policy configured.
[PASS]   (HEADER) Content-Security-Policy: CSP header is present without common unsafe directives.
[PASS]   (CORS) CORS Policy: No permissive CORS headers returned for untrusted origins.
```