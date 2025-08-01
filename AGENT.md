# Instructions for the Programming Agent

This file provides guidelines for any automation or AI agent contributing to **PyTone FX**. Follow these steps to build and test the project.

## Overview

PyTone FX is a real-time guitar-processing engine written in Python. The project uses TorchFX for DSP graphs and pywdf for schematic-level components. Refer to `README.md` for a feature overview and directory layout.

## Toolchain and Dependencies

The preferred tool versions and rationale are captured below (mirroring `TECHSTACK.md`):

| Layer                 | Choice                                               | Rationale                            | Explicitly **Not** Used             |
| --------------------- | ---------------------------------------------------- | ------------------------------------ | ----------------------------------- |
| **Language**          | Python 3.12 (CPython)                                | Mature ecosystem, TorchFX support    | Python < 3.10, PyPy                 |
| **DSP Engine**        | Torch 2.3 + TorchFX<br/>pywdf 0.7                    | GPU‑accelerated, flexible graphs     | JUCE/C++ hard‑dependency            |
| **GPU Back‑Ends**     | • Apple MPS (Metal)<br/>• CUDA 12.4                  | Native on M‑series, RTX cards        | OpenCL (deprecated), Vulkan‑compute |
| **Audio I/O**         | rtmixer (PortAudio CFFI)                             | Hard‑RT callback in C; lock‑free     | PyAudio, sounddevice                |
| **Preset / Config**   | YAML 1.2 + Pydantic v3                               | Human‑readable, schema validation    | XML, INI files                      |
| **Desktop UI**        | React 18 + Tauri 2.0                                 | Lightweight, Rust core, native menus | Electron (heavier), Tkinter, PyQt   |
| **State Mgmt**        | Redux Toolkit + Zustand                              | Time‑travel debug; React‑friendly    | MobX, custom globals                |
| **MIDI / HID**        | `mido` (CoreMIDI/WinMM)                              | Cross‑platform                       | rtmidi‑python (threading issues)    |
| **Build / Packaging** | Poetry 1.8, Hatchling, PyInstaller<br/>Tauri bundler | Reproducible env, signed apps        | setup.py only, Conda                |
| **CI/CD**             | GitHub Actions + Codesign‑Notarize                   | macOS & Windows build/test matrix    | Travis CI                           |
| **Static Analysis**   | black • isort • flake8 • mypy • pylint‑strict        | Enforced via pre‑commit              | autopep8 (less opinionated)         |
| **Documentation**     | MkDocs Material, diagrams.net                        | Live site + editable schematics      | Sphinx (overkill here)              |
| **Testing**           | pytest • pytest‑benchmark                            | Unit & latency perf gates            | nosetests                           |
| **Profiling**         | torch.profiler • scalene                             | CPU/GPU breakdown                    | cProfile only                       |

## Development Environment

Use the following commands to set up a Debian/Ubuntu environment:

```bash
# Install core tool‑chain (Debian/Ubuntu)
sudo apt-get update
sudo apt-get install -y python3.12 python3.12-venv python3-pip git cmake ninja-build cargo

# Rust & Tauri CLI
curl https://sh.rustup.rs -sSf | sh -s -- -y
source "$HOME/.cargo/env"
cargo install tauri-cli

# Python deps
pip3 install poetry
poetry install --with dev
pre-commit install
```

Run `make latency` to execute automated latency/stability benchmarks on both CPU and GPU paths.

## Coding Standards

* **black** (line length 88, stable) for formatting
* **isort** (profile = black) for import order
* **flake8** (`--max-complexity=10`) for style/lint
* **mypy** (strict mode) for type safety
* **pylint** (score ≥ 9.0 required) for code health

All checks run in CI; failing any gate blocks the merge.

## Security & Compliance

* Dependencies are scanned via GitHub Dependabot and `pip-audit`.
* No proprietary IRs or ML weights are stored in the repository; they are fetched by hash on first run.
* The Tauri sandbox restricts access to `device.audio-input` only.

## Build & Test Workflow

1. Ensure the dependencies above are installed.
2. Run `pre-commit run --all-files` before committing changes.
3. Execute `pytest` to run unit and benchmark tests.
4. Use `npm` inside the `ui` directory to build or launch the Tauri front-end.

This file is provided so that any automation or AI assistant can reproduce the environment and follow the same standards as human contributors.
