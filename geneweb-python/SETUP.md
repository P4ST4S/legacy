# Python environment setup (venv + requirements.txt)

Use these steps to create an isolated Python environment and manage dependencies with requirements.txt.

## Prerequisites

- Python 3.10+ and pip installed.
- Verify:
  ```bash
  python3 --version
  pip --version
  # On Windows you may use:
  # py --version
  ```

## Create and activate a virtual environment

### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### Windows (PowerShell)

```powershell
py -3 -m venv .venv
.venv\Scripts\Activate.ps1
```

## Upgrade packaging tools (recommended)

```bash
pip install --upgrade pip setuptools wheel
```

## Install project dependencies

```bash
pip install -r requirements.txt
```

## Add a new dependency

1. Activate the venv.
2. Install the package:
   ```bash
   pip install <package_name>
   ```
3. Pin the current environment to requirements.txt:
   ```bash
   pip freeze > requirements.txt
   ```
4. Commit requirements.txt.

## Update existing dependencies

- Option A: Edit versions directly in requirements.txt, then re-sync:
  ```bash
  pip install -r requirements.txt
  ```
- Option B: Upgrade a specific package, then re-freeze:
  ```bash
  pip install -U <package_name>
  pip freeze > requirements.txt
  ```

## Fresh setup (from a clean clone)

```bash
python3 -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows (PowerShell)
# .venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Deactivate the venv

```bash
deactivate
```

## Remove the venv

### macOS / Linux

```bash
rm -rf .venv
```

### Windows

```powershell
rmdir /s /q .venv
```
