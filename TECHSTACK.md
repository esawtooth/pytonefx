# Technical Stack

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

```bash
# Install core tool‑chain
brew install python@3.12 git cmake ninja rustup tauri-cli
rustup default stable

# Python deps
poetry install --with dev
pre-commit install
```

Run `make latency` to execute automated latency/stability benchmarks on both CPU and GPU paths.

## Coding Standards

* **black** (line length 88, stable) for formatting
* **isort** (profile = black) for import order
* **flake8** (`--max-complexity=10`) for style/lint
* **mypy** (strict mode) for type safety
* **pylint** (score ≥ 9.0 required) for code health

All checks run in CI; failing any gate blocks the merge.

## Security & Compliance

* All dependencies are scanned via GitHub Dependabot + `pip-audit`.
* No proprietary IRs or ML weights kept in‑repo; fetched by hash on first run.
* Tauri sandbox enforces `device.audio-input` only; no unrestricted FS access.
