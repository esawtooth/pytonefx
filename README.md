# PyTone FX

**Real‑time, schematic‑accurate guitar processing in pure Python**

## ✨ Features

* **TorchFX DSP graph** – GPU‑accelerated FIR/IIR, delay lines, waveshapers.
* **pywdf components** – resistors, capacitors, transformers, BJTs, triodes.
* **Sub‑10 ms latency** – CoreAudio / ASIO with 32‑ to 64‑sample buffers.
* **Desktop UI** – React + Tauri editor, drag‑drop pedals/amps/cabs.
* **MIDI control** – Bank/Program select, expression pedal mapping.
* **Open assets** – CC0 cab IRs, community pedal/amp libraries.

## Quick Start (macOS + Apple Silicon)

```bash
brew install python@3.12 portaudio cmake ninja
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements-dev.txt
python scripts/check_gpu.py        # verify MPS backend
python run.py                      # launch headless engine
npm install --prefix ui && npm run --prefix ui tauri dev  # UI
```

Windows instructions are in **docs/SETUP\_WINDOWS.md**.

## Directory Layout

```
/core        • TorchFX modules & pywdf wrappers
/engine      • RT callback, ring buffers, preset loader
/ui          • React + Tauri desktop front‑end
/models      • Example amps, pedals, cabs (YAML)
/docs        • Specs, diagrams, troubleshooting
/tests       • pytest unit + latency benchmarks
```

## Contributing

1. Fork and create a feature branch.
2. `pre-commit run --all-files` must pass (`black`, `isort`, `flake8`, `mypy`).
3. Submit a pull request and fill in the checklist.

See **CODE\_OF\_CONDUCT.md** and **CONTRIBUTING.md**.

## License

MIT.  Third‑party IRs and ML weights are under their respective licenses.
