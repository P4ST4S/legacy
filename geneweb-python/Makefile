# -------- Config ----------
# Virtual environment name
VENV := .venv
# Entry point (change if needed)
ENTRY := main.py
# Requirements file (optional)
REQ := requirements.txt

# Detect OS for venv paths
ifeq ($(OS),Windows_NT)
	VENV_BIN := $(VENV)/Scripts
	RM := rmdir /S /Q
	SEP := &
else
	VENV_BIN := $(VENV)/bin
	RM := rm -rf
	SEP := ;
endif

PYTHON := $(VENV_BIN)/python
PIP    := $(VENV_BIN)/pip

# -------- Main targets ----------
.PHONY: all venv install run test lint format clean fclean re

# Default: create venv and install dependencies
all: venv install

# Create virtual environment if it does not exist
venv:
	@echo "==> Creating venv '$(VENV)' (if missing)"
	@test -d "$(VENV)" || python3 -m venv "$(VENV)"
	@echo "==> Python: $$($(PYTHON) -V)"

# Install dependencies if requirements.txt exists
install: venv
	@if [ -f "$(REQ)" ]; then \
		echo "==> Installing dependencies from $(REQ)"; \
		"$(PIP)" install -U pip wheel $(SEP) "$(PIP)" install -r "$(REQ)"; \
	else \
		echo "==> No $(REQ) found, skipping dependency install"; \
	fi

# Run the application
run: venv
	@"$(PYTHON)" "$(ENTRY)"

# Linting with Ruff only
lint: venv
	@"$(PIP)" install ruff >/dev/null
	@echo "==> Ruff"
	@"$(VENV_BIN)/ruff" check .

# Run tests
test: venv
	@"$(PIP)" install pytest >/dev/null
	@echo "==> pytest"
	@"$(VENV_BIN)/pytest" -q

# Auto-format code
format: venv
	@"$(PIP)" install ruff black >/dev/null
	@echo "==> Ruff (fix)"
	@"$(VENV_BIN)/ruff" check --fix .
	@echo "==> Black"
	@"$(VENV_BIN)/black" .

# Light cleanup: remove caches and temp files
clean:
	@echo "==> Cleaning build & caches"
	@$(RM) build dist *.egg-info 2>/dev/null || true
	@$(RM) .pytest_cache .mypy_cache 2>/dev/null || true
	@find . -type d -name "__pycache__" -exec $(RM) {} + 2>/dev/null || true
	@find . -type f -name "*.pyc" -delete 2>/dev/null || true
	@find . -type f -name "*.pyo" -delete 2>/dev/null || true

# Full cleanup: clean + remove venv
fclean: clean
	@echo "==> Removing virtualenv '$(VENV)'"
	@$(RM) "$(VENV)" 2>/dev/null || true

# Rebuild everything from scratch
re: fclean all
