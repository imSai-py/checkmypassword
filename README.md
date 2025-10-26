# pwned-password-checker

A small Python CLI tool that checks whether a password has appeared in known data breaches using the Have I Been Pwned (HIBP) Pwned Passwords API (k-Anonymity).

---

## Table of Contents

* [About](#about)
* [Features](#features)
* [Requirements](#requirements)
* [Installation](#installation)
* [Usage](#usage)
* [How it works](#how-it-works)
* [Security & Privacy](#security--privacy)
* [Example output](#example-output)
* [Contributing](#contributing)
* [License](#license)
* [Acknowledgements](#acknowledgements)

---

## About

This project implements a simple password leak checker in Python. It uses the HIBP Pwned Passwords API with the k-Anonymity model: the script sends only the first 5 characters of the SHA-1 hash of a password and compares the remainder locally to determine whether the password has been seen in breaches.

## Features

* Minimal, dependency-light implementation
* Does not transmit full passwords to any third party
* Easy to run from the command line

## Requirements

* Python 3.7+
* `requests` library

Install the dependency with pip if you don't have it:

```bash
pip install requests
```

## Installation

1. Clone the repository:

```bash
git clone https://github.com/<your-username>/pwned-password-checker.git
cd pwned-password-checker
```

2. (Optional) Create and activate a virtual environment:

```bash
python -m venv venv
# macOS / Linux
source venv/bin/activate
# Windows (PowerShell)
venv\Scripts\Activate.ps1
```

3. Install requirements:

```bash
pip install -r requirements.txt
```

> If you prefer not to use a requirements file, just `pip install requests`.

## Usage

Run the script from the command line and pass one or more passwords as arguments. Example:

```bash
python check_password.py hunter2
python check_password.py password123 "correct horse battery staple"
```

You can also use it with a small wrapper or from another script by importing `pwned_api_check`.

### Example (programmatic):

```python
from check_password import pwned_api_check

count = pwned_api_check('hunter2')
if count:
    print(f'Found {count} times')
else:
    print('Not found')
```

## How it works

1. The script computes the SHA-1 hash of the input password and converts it to uppercase.
2. It sends the first 5 characters of the hash to the HIBP Pwned Passwords API.
3. The API returns a list of matching hash suffixes and occurrence counts. The script compares the suffix locally and returns the leak count if found.

This is the k-Anonymity approach recommended by HIBP and ensures the full password/hash never leaves the client.

## Security & Privacy

* **Never** hardcode real user passwords into public repositories.
* This tool sends only the first 5 characters of a SHA-1 hash (k-Anonymity) — the full password is not sent.
* Use this script for learning and small audits; for production or scanning many passwords, follow HIBP terms and consider rate limits and API best practices.

## Example output

```
$ python check_password.py password123
password123 was found 15507 times... you should probably change your password!

$ python check_password.py thisIsMyUniquePass!
thisIsMyUniquePass! was NOT found. Carry on!
```

## Contributing

Contributions are welcome! Some ideas:

* Add unit tests
* Add an interactive TUI or GUI
* Add a batch mode that reads passwords from an encrypted file
* Improve error handling and retry/backoff for network errors

Please open an issue or submit a pull request.

## License

This repository does not include a license by default. If you want to make it open source, add a license such as the MIT License (`LICENSE` file) and include a short `LICENSE` section here.

## Acknowledgements

* [Have I Been Pwned](https://haveibeenpwned.com/) — for providing the Pwned Passwords API and guidance about k-Anonymity.

---

*Generated README for a simple Pwned Password Checker — edit any section to match your repo name and file names.*
