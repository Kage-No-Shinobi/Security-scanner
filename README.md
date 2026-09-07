# Security scanner

Web Security Scanner

A simple Python tool that crawls a website and checks the pages it finds for common web vulnerabilities: SQL injection, Cross-Site Scripting (XSS), and exposed sensitive information (emails, phone numbers, SSNs, API keys).

⚠️ Only scan sites you own or have explicit permission to test. Scanning sites without authorization may be illegal, even if you don't exploit anything you find. This tool actively sends attack-style payloads to the target.

What it does
Crawls the target site starting from the URL you give it, following links up to a set depth (default: 3 levels).
Tests each discovered page for:
SQL Injection — tries known SQLi payloads in URL parameters and checks the response for database error messages.
Cross-Site Scripting (XSS) — tries script payloads in URL parameters and checks whether they're reflected back unescaped.
Sensitive information exposure — scans page content for patterns matching emails, phone numbers, SSNs, and API keys.
Reports findings in the terminal (color-coded) and returns them as a list you can process further.
Requirements
Python 3.8+
requests
beautifulsoup4
colorama
Setup

It's best practice to use a virtual environment so these packages don't clash with anything else on your system.

bash

# Create a virtual environment

python3 -m venv venv

# Activate it

source venv/bin/activate # macOS/Linux
venv\Scripts\activate # Windows

# Install dependencies

python -m pip install requests beautifulsoup4 colorama
Usage
bash
python scanner.py <target_url>

Example:

bash
python scanner.py http://testphp.vulnweb.com

This runs against a public site built specifically for practicing security scanning, so it's safe to test against.

What you'll see
A blue banner when the scan starts
Red [VULNERABILITY FOUND] blocks as issues are discovered, each with the type, URL, parameter, and payload used
A green summary at the end showing total URLs scanned and total vulnerabilities found
Project structure
.
├── scanner.py # Main scanner script
└── README.md # This file
Known limitations
Error-based SQLi detection only. It won't catch blind or time-based injection where the response doesn't visibly change.
GET parameters only. It doesn't currently test data submitted via POST forms.
verify=False on requests. SSL certificate validation is disabled, which is convenient for test targets with self-signed certs but means it won't warn you about invalid certificates on real targets.
Loose scope checking. The crawler uses startswith() to decide if a link is "in scope," which can be tricked by lookalike domains (e.g. target.com.evil.net).
No POST/form-based crawling or authentication support, so pages behind a login won't be tested.
Troubleshooting
command not found / exit code 127 — Python isn't on your PATH, or your virtual environment isn't activated. Run python3 --version to check, and make sure your terminal prompt shows (venv).
No module named requests — dependencies were installed into a different Python than the one you're running. Use python -m pip install ... and python scanner.py ... consistently (not a mix of python and python3).
No connection adapters were found for '...' — check your URL starts with http:// or https:// and has no typos or extra text.
Failed to resolve / connection timeout — this is a network/DNS issue, not a scanner bug. Try ping google.com and curl -I <target_url> to confirm your machine can actually reach the target.
Disclaimer

This tool is for educational and authorized security testing purposes only. The author(s) are not responsible for misuse or damage caused by this tool. Always obtain proper authorization before testing any system you do not own.
