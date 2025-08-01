# Project Name

**PyTone FX** – an open‑source, cross‑platform, real‑time guitar‑processing engine written in Python, leveraging TorchFX for GPU‑accelerated DSP graphs and pywdf for schematic‑level component emulation.

---

## 1  Vision

Give guitarists Axe‑Fx‑class tone and tweakability without proprietary hardware, by combining modern GPU compute, open component models and a musician‑friendly desktop UI.

---

## 2  Target Users

| Persona                                     | Needs                                                                      |
| ------------------------------------------- | -------------------------------------------------------------------------- |
| **Bedroom Producer (Windows + RTX GPU)**    | Low‑latency recording, instant re‑amping, easy preset sharing.             |
| **Gigging Guitarist (MacBook + interface)** | Live reliability (< 10 ms end‑to‑end), MIDI foot control, scene switching. |
| **DSP Researcher**                          | Rapid prototyping of WDF/ML hybrids, hot‑reloading Python modules.         |

---

## 3  Value Propositions

* **Tone fidelity** – schematic‑accurate models (pywdf) or neural captures (NAM) in one graph.
* **Performance** – Metal on Apple Silicon & CUDA on Nvidia; stable ≤ 64‑sample buffers.
* **Extensibility** – add new components by subclassing `torch.nn.Module`, no C++ compile.
* **Transparency** – MIT‑licensed code; no hidden IRs or encrypted algorithms.

---

## 4  Scope

### 4.1  Minimum Viable Product (MVP)

| Category   | Requirement                                                                 | Priority |
| ---------- | --------------------------------------------------------------------------- | -------- |
| Audio I/O  | CoreAudio & ASIO @ 44.1/48 k, 32–128‑sample buffers                         | P0       |
| Processing | Load & run TorchFX graphs; pywdf R/C/L, diode, BJT, triode components       | P0       |
| Presets    | YAML‑based rigs; save/load; versioned                                       | P0       |
| UI         | Desktop shell with preset browser, drag‑drop chain editor, real‑time meters | P1       |
| MIDI       | Program‑change & CC mapping (bank/preset, knobs)                            | P2       |

### 4.2  Stretch Goals

* Dual‑amp or k-way amp parallel routing
* Build complex signal chains graphs with arbitrary number of nodes
* GPU convolution reverb
* Neural capture trainer GUI

### 4.3  Explicit *Non‑Goals*

* VST/AU plugin version (phase‑2)
* Linux ALSA support (community can add)
* Embedded hardware builds (e.g., Raspberry Pi)
* Closed‑source commercial IR packs
* Mobile (iPad) audio engine

---

## 5  Success Metrics

| Metric                                   | Target for v1.0         |
| ---------------------------------------- | ----------------------- |
| Round‑trip latency (Mac/Windows, 64‑buf) | ≤ 10 ms 95th‑percentile |
| CPU utilisation (M2 Pro, clean preset)   | < 25 %                  |
| Crash‑free sessions (>30 min play)       | 99 %                    |

---

## 6  Risks & Mitigations

| Risk                                  | Impact             | Mitigation                                               |
| ------------------------------------- | ------------------ | -------------------------------------------------------- |
| Metal / CUDA kernel jitter            | Audio drop‑outs    | Fuse TorchFX graphs; pre‑allocate tensors; profiling CI. |
| Python GIL in real‑time path          | Latency spikes     | Hard‑RT callback in C (`rtmixer`), ring‑buffer hand‑off. |
| Licensing conflicts (IRs, ML weights) | Take‑down requests | Ship only CC0 or MIT assets; script pulls user packs.    |
| Scope creep                           | Delay v1.0         | Feature freeze after MVP; roadmap reviews.               |

---

## 7  Timeline (tentative)

| Milestone                 | Date       | Deliverable                                  |
| ------------------------- | ---------- | -------------------------------------------- |
| Tech‑stack spike complete | 2025‑09‑30 | CUDA + MPS proof‑of‑concept plays clean tone |
| MVP “dogfood”             | 2025‑12‑01 | Internal jam‑session build                   |
| Public Beta               | 2026‑02‑15 | Installers, docs, preset packs               |
| v1.0 Release              | 2026‑04‑30 | Signed apps, website, Discord community      |
