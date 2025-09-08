# Geneweb — build locally (macOS, Linux, Windows)

This repository contains the Geneweb sources under `geneweb/`. Below are minimal steps to build and run the server locally with opam and dune across platforms.

## Prerequisites (macOS)

- Homebrew (recommended)
- opam (>= 2.1), dune, ocamlfind

Optional (via Homebrew):

- `brew install opam`
- Then run `opam init` as shown below

## Quick start (macOS)

All commands run from the `geneweb/` directory.

```bash
# 1) Go to the project root
cd geneweb

# 2) Initialize opam (first time only)
opam init --disable-sandboxing -y
# Load opam env in current shell session
eval "$(opam env --switch=default)"

# 3) Ensure base tools are installed
opam install -y ocamlfind dune

# 4) Install project dependencies (incl. test deps)
opam install . -y --deps-only --with-test

# 5) Generate project config
ocaml ./configure.ml

# 6) Build
# (a) Full project
make build-geneweb
# or (b) only server/client binaries
make gwd

# 7) Run tests (optional)
dune runtest
```

Notes:

- The make targets automatically generate required `dune` files and `lib/version.ml`.
- If you prefer dune directly: `dune build bin/gwd bin/gwc bin/gwu bin/setup`.

## Linux

Tested on Debian/Ubuntu-like distributions. Other distros are similar (use your package manager for optional system deps).

Optional system packages (recommended for smoother builds):

```bash
sudo apt-get update
sudo apt-get install -y opam m4 pkg-config libgmp-dev libpcre3-dev
```

Then follow the same opam/dune flow (from the `geneweb/` directory):

```bash
# Initialize opam (first time only)
opam init -y
eval "$(opam env)"

# Base tools
opam install -y ocamlfind dune

# Project deps
opam install . -y --deps-only --with-test

# Configure and build
ocaml ./configure.ml
make build-geneweb  # or: make gwd

# Tests (optional)
dune runtest
```

If a package is missing at link-time (e.g., gmp/pcre), install the corresponding `-dev` system package and rebuild.

## Windows

You have two options:

1. Recommended: WSL (Windows Subsystem for Linux)

- Install Ubuntu from the Microsoft Store and open a WSL shell.
- Follow the Linux section above inside WSL.

2. Native Windows (MSYS2/opam)

- Install opam for Windows (MSYS2-based) and open the MSYS2/OCaml environment shell.
- From the `geneweb/` directory, run:

```powershell
# Initialize opam (first time only)
opam init --disable-sandboxing -y
opam env | Invoke-Expression

# Base tools
opam install -y ocamlfind dune

# Project deps
opam install . -y --deps-only --with-test

# Configure and build
ocaml ./configure.ml
make build-geneweb    # or: make gwd

# Tests (optional)
dune runtest
```

Notes (Windows):

- Do not use `-daemon` (Unix-only). Prefer foreground mode or run as a service.
- You may add `-wd .` to set the working directory used for sockets and access count.

## Running the server

The following runs the server with assets from `hd/` and databases in the current directory, in English, as a daemon on port 2317:

```bash
./_build/default/bin/gwd/gwd.exe -hd hd -bd . -lang en -daemon
```

- Open http://localhost:2317 in your browser.
- Omit `-daemon` to run in the foreground (handy for logs).
- To change port: add `-p 8080` (for example).

Tip: add `-setup_link` to display a link to `gwsetup` in the page footer.

Windows example (no daemon, optional `-wd`):

```powershell
./_build/default/bin/gwd/gwd.exe -hd hd -bd . -lang en -wd .
```

## Creating or loading a base

Easiest path is via the web UI (with `-setup_link`) to create a new base. You can also use the tools:

- `gwc.exe` compiles a `.gw` textual database into a base directory.
- `gwb2ged.exe` / `ged2gwb.exe` convert to/from GEDCOM.

Example (from inside `geneweb/`):

```bash
# Compile a base from a valid file mybase.gw into directory "mybase"
./_build/default/bin/gwc/gwc.exe mybase.gw -o mybase

# Then point the server to the parent directory of your base with -bd
./_build/default/bin/gwd/gwd.exe -hd hd -bd . -lang en
```

Note: `.gw` format is specific; ensure your file follows Geneweb syntax or import a GEDCOM via `ged2gwb`.

## Troubleshooting

- unknown option '-bases': use `-bd <DIR>` (bases directory), not `-bases`.
- opam env not loaded: run `eval "$(opam env)"` in your shell.
- Missing `dune` files: run `ocaml ./configure.ml` then `make build-geneweb` (or `make gwd`).
- macOS permissions on first run: allow incoming connections for `gwd.exe` if prompted.
- Linux link errors (gmp/pcre): install `libgmp-dev`/`libpcre3-dev` (Debian/Ubuntu) or distro equivalents.
- Windows: skip `-daemon`; consider adding `-wd .`. Allow the app through Windows Defender Firewall on first run.

## Clean

```bash
make clean
```

This removes generated files and `_build/` artifacts.
