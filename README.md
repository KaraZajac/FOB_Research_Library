# FOB Research Library

A community collection of **.fob** capture files for [**KAT**](https://github.com/KaraZajac/KAT) (Keyfob Analysis Toolkit). This library supports protocol analysis, decoder development, and security research on automotive keyfob signals.

---

## About KAT

**[KAT — Keyfob Analysis Toolkit](https://github.com/KaraZajac/KAT)** is a terminal-based RF signal analysis tool for capturing, decoding, and retransmitting automotive keyfob signals. Built in Rust with a real-time TUI.

- **Hardware:** HackRF One (or compatible) for full RX/TX; RTL433 / RTL-SDR for receive-only.
- **Protocols:** Kia V0–V6, Ford, Fiat, Subaru, Suzuki, VAG, PSA, Scher-Khan, Star Line, KeeLoq fallback, and more.
- **Export:** `.fob` (versioned JSON with full signal + vehicle metadata) and `.sub` (Flipper Zero).

Use KAT to record keyfob signals, then contribute your captures here to grow this research library.

---

## Purpose

This repository is a **shared research library** of high-quality keyfob captures in KAT’s `.fob` format. It is intended for:

- **Researchers** — protocol analysis, timing, and encoding studies  
- **Security practitioners** — authorized testing, replay/rollback research, CVE validation  
- **Decoder developers** — test vectors and real-world samples for KAT and other tools  

Captures are organized by **manufacturer** (e.g. `Subaru/`, `Chrysler/`) and include vehicle metadata (year, make, model, region, command, notes) to support filtering and analysis.

---

## Repository layout

- **By manufacturer:** Each top-level folder is the vehicle **Make** (e.g. `Subaru/`, `Chrysler/`).
- **Filenames:** `Year_Make_Model_Region_Command_<8-char-hex>.fob`  
  Example: `2014_Subaru_Impreza_NA_Unlock_AD8797F1.fob`
- **Format:** KAT `.fob` v2.0 (see below).

---

## .fob format (v2.0)

Each `.fob` file is a JSON document with:

| Section   | Contents |
|----------|----------|
| **signal** | Protocol, frequency, modulation, RF type, encryption, data bits/hex, serial, key, button, counter, CRC, encoder capability |
| **vehicle** | Year, make, model, region, command (e.g. Lock/Unlock/Trunk/Panic), notes |
| **capture** | Timestamp, optional raw data hex, raw level/duration pairs for replay |

Example (abbreviated):

```json
{
  "version": "2.0",
  "format": "kat-fob",
  "signal": {
    "protocol": "Kia V3/V4",
    "frequency": 433920000,
    "frequency_mhz": "433.92MHz",
    "modulation": "PWM",
    "rf_modulation": "AM/FM",
    "encryption": "KeeLoq",
    "button_name": "Lock",
    "encoder_capable": true
  },
  "vehicle": {
    "year": 2023,
    "make": "Kia",
    "model": "Sportage",
    "region": "NA",
    "command": "Lock",
    "notes": ""
  },
  "capture": {
    "timestamp": "2026-02-07T12:00:00Z",
    "raw_pairs": [{"level": true, "duration_us": 400}, ...]
  }
}
```

For unknown protocols, the filename includes a unique 8-character hex suffix; `vehicle.command` stores the button/command label.

---

## Contribute — we need your captures

High-quality captures from real keyfobs are what make this library useful. **If you have KAT and hardware (HackRF or RTL-SDR), please consider contributing.**

### Quick workflow

1. **Open KAT** and ensure your radio (HackRF or RTL-SDR) is connected.
2. **Record a signal** — tune to the right frequency, start RX, press the keyfob.
3. **Enter metadata** — select the capture, press **`i`**, fill in year, make, model, region, command, notes.
4. **Export** — open the signal menu (Enter), choose **Export .fob**, confirm filename and metadata, export.
5. **Verify** — open the `.fob` in a text editor and confirm protocol, vehicle, and command look correct.
6. **Contribute** — add the file to this repo (see below) and open a pull request.

### Step-by-step in KAT

**1. Record a signal**

- Set frequency (e.g. 433.92 MHz or 315 MHz) via **Tab** → Radio settings or **`:freq 433.92`**.
- Press **`r`** to start receive. Press the keyfob button (Lock, Unlock, Trunk, Panic, etc.).
- When a decode appears in the list, press **`r`** again to stop RX if desired.

**2. Enter metadata (important for the library)**

- Select the capture in the list (j/k or arrow keys).
- Press **`i`** to open the metadata form.
- Fill in: **Year**, **Make**, **Model**, **Region**, **Command** (e.g. Lock, Unlock, Trunk, Panic), **Notes**.
- Press **Enter** to confirm.

**3. Export .fob**

- With the capture selected, press **Enter** to open the signal action menu.
- Choose **Export .fob**, check the filename (KAT adds a unique 8-character hex suffix for all protocols).
- Confirm vehicle fields and export.

**4. Verify the file**

- Open the `.fob` in a text editor; confirm `signal.protocol`, `vehicle`, and `capture` data look correct.

### Adding your capture to this repository

- **Folder:** Use a top-level folder named after the vehicle **Make** (e.g. `Subaru/`, `Chrysler/`, `Kia/`).
- **Filename:** `Year_Make_Model_Region_Command_<8-char-hex>.fob`  
  Examples: `2014_Subaru_Impreza_NA_Unlock_AD8797F1.fob`, `2010_Chrysler_Town_&_Country_NA_Lock_45026C8A.fob`
- **Submit:** Fork this repo, add your `.fob`(s) under the correct Make folder, commit, push, and open a **Pull Request**. In the PR, mention vehicle (year, make, model, region), commands added, protocol if known, and any notes.

### Quality tips

- **Clean recordings** — reduce noise so KAT can decode or capture usable raw pairs.
- **Complete metadata** — year, make, model, region, and command make captures searchable.
- **One command per capture** — one button press per .fob (e.g. one file for Lock, one for Unlock).
- **Legal use only** — only capture and contribute signals from **vehicles and systems you own or have explicit permission to test**. You are responsible for complying with local laws.

**Questions:** Open an issue here for contributing or repo structure. For KAT usage or protocol questions, see [KAT](https://github.com/KaraZajac/KAT).

---

## License

See [LICENSE](LICENSE). By contributing, you agree your contributions are under the same license.
