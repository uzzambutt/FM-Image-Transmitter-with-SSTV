# FM SSTV Transmitter

This project is a **low-power FM transmitter PCB** designed to send **SSTV (Slow Scan Television)** images using an ordinary FM receiver as the front-end.  
You feed the board with SSTV **audio tones**, it FM-modulates them and a receiver + decoder reconstructs the image.

> ⚠️ **Legal warning**  
> Transmitting on the public FM band (88–108 MHz) without a license is illegal in many countries.  
> Use a dummy load, shielding, or direct-cable tests where possible and keep RF tests short and low-power.

---

## 1. How It Works

1. An image is converted to **SSTV audio** (a sequence of tones).
2. That audio is fed into the **audio input** (MIC or line-in jack J1).
3. The audio modulates the RF oscillator around an FM carrier (≈ 88–108 MHz).
4. A nearby **FM receiver** tuned to the same frequency recovers the audio.
5. **SSTV decoding software** on a PC/phone listens to the receiver audio and reconstructs the image.

The transmitter itself doesn’t “know” about images. It just transmits audio.  
The SSTV encoder/decoder on the PC/phone does the heavy lifting.

---

## 2. Hardware Overview

Key sections (see schematic):

- **Power Input**
  - `JP1` — DC input, **3–6 V**, approx **15–20 mA**.
  - `C15` and other decoupling capacitors stabilize the supply.

- **Audio Input / Pre-Amp**
  - `U1` — Electret microphone.
  - `J1` — Audio jack (line-in from PC/phone).
  - `T1 (S9014)` — AF pre-amplifier.
  - `R1`, `R2`, `R4`, `R7`, `R10`, `C3`, `C5` — biasing and coupling network.

- **RF Oscillator & Modulator**
  - `T2 (S9018)` — RF oscillator stage.
  - `L4` — **Tuneable inductor** (VFO tank).
  - `C8`, `C12`, `C14`, `C11` — RF tank & feedback capacitors.

- **RF Buffer / Power Amp**
  - `T3 (S9018)` — RF buffer / driver to antenna.
  - `L1`, `L2`, `L3`, `C1`, `C4`, `C6`, `C7`, `C9`, `C10` — matching & filtering network.
  - `ANT` pad — antenna / dummy load connection.

- **Indicators**
  - `LED1` + `R11` — power indicator.

The top copper contains all components; the bottom is mostly routing with ground returns.

---

## 3. Assembly Notes

1. **Check transistor pinouts** (`S9014`, `S9018`) vs footprints before soldering.
2. Solder **small passives first**, then transistors, then connectors, then the tuneable inductor.
3. Keep solder joints clean, especially around:
   - RF tank (`L4`, `C8`, `C12`, `C14`, `C11`)
   - Output network and antenna area.
4. Use short, direct ground connections and make sure all decoupling capacitors are actually connected to ground pours.
5. Use a **non-metallic screwdriver** to adjust `L4` during tuning.

---

## 4. Power & Audio Setup

### Power

- Connect **3–6 V** DC to `JP1`:
  - `+` to VCC
  - `–` to GND
- Verify polarity before powering up.
- The LED should light when powered.

### Audio Input

Two options:

1. **Line-in (recommended for SSTV)**
   - PC / phone headphone jack → `J1`.
   - Use a **shielded cable**.
   - Start at low volume and increase slowly while watching the SSTV decoder.

2. **On-board microphone**
   - For voice tests, not ideal for SSTV (worse SNR, more noise).

---

## 5. Generating SSTV Audio

You need software that converts an image → SSTV audio.

### Desktop

- **MMSSTV** (Windows)
- **fldigi** (Win / Linux / macOS)
- **QSSTV** (Linux)

### Android / Mobile

- Apps like **Robot36**, **SSTV Droid** etc. (search for “SSTV”).

### Typical Workflow

1. Open your SSTV app.
2. Load an image.
3. Choose a mode:
   - `Martin M1` (good quality, moderate length)  
   - or `Robot36` (faster).
4. Hit **Transmit / Play** — this outputs SSTV tones via your sound card / phone jack.
5. Send that audio into `J1` on the transmitter.

---

## 6. Tuning the Transmitter

The RF frequency is set by the **LC tank** around `L4`, `C8`, `C12`, `C14`.

### Finding the Carrier

1. Connect a **dummy load** or short antenna to `ANT`.
2. Power up the board.
3. Use a portable **FM radio** or SDR:
   - Sweep across 88–108 MHz until you hear the carrier (quiet “dead” spot).
4. Adjust `L4` carefully until you land on a **clear frequency** with minimal interference.

> ⚠️ Try to pick a frequency that is unused in your area.

### Deviation & Audio Level

- SSTV needs **clean, undistorted audio**.
- If the SSTV image looks noisy or skewed:
  - Lower the audio volume on the PC/phone.
  - Check the pre-amp and coupling caps (no bad joints or wrong values).

---

## 7. Receiving & Decoding SSTV

You have two basic options:

### Option A — **Fully RF (over-the-air)**

> Only if legal in your country.

1. Attach a small antenna to `ANT`.
2. Place a nearby **FM receiver** (radio, SDR, etc.).
3. Tune the receiver to the transmitter frequency.
4. Take the **audio output** of the receiver:
   - Line-out / headphone → PC sound card / phone mic input.
5. Run **SSTV decoder** software (MMSSTV, fldigi, QSSTV, Robot36, etc.).
6. Start transmitting an SSTV image from the encoder.
7. The decoder should:
   - Detect the SSTV header & sync.
   - Gradually draw the image line by line.

### Option B — **Zero-RF Bench Test (recommended & legal)**

This does not radiate RF at all:

1. Skip the RF section altogether and just test your **SSTV chain**:
   - Image → SSTV encoder → audio → decoder → image.
2. Connect PC headphone → PC line-in and loopback in software, or use a virtual audio cable.
3. Once you know the encode/decode chain works, you can add the transmitter in between and keep power extremely low.

---

## 8. Example Test Procedure

A concrete test you can follow:

1. **Assemble the board** and visually inspect all solder joints.
2. Connect **5 V** to `JP1`.
3. Connect PC headphone out → `J1` (line-in) with a TRS cable.
4. On PC:
   - Open **MMSSTV** or **fldigi**.
   - Load a small test image.
   - Select **Martin M1**.
5. On the receiver:
   - Tune to your adjusted carrier frequency.
   - Feed receiver audio into the **decoder PC** (either the same PC or another).
6. Hit **Transmit** (play the SSTV audio).
7. Watch the decoder:
   - It should detect the VIS code and begin drawing the image.
   - If the image is tilted, noisy, or colors are off, tweak:
     - Audio drive level.
     - RF tuning (`L4`).
     - Receiver volume.

---

## 9. Common Problems & Fixes

| Symptom                            | Possible Cause / Fix                                                                 |
|-----------------------------------|--------------------------------------------------------------------------------------|
| No RF / no carrier                | Check power, transistor orientation, `L4` value, continuity in LC tank and antenna. |
| Receiver hears nothing            | Wrong frequency; mis-tuned `L4`; antenna / dummy load not connected.                |
| Decoder never syncs               | Audio too loud / clipping, or too low; check levels on both transmitter & receiver. |
| Image noisy / streaked            | RF interference, poor SNR, bad grounding; improve shielding, use shorter leads.     |
| Strong hum or warble in tones     | Power supply noise; improve decoupling (`C15` etc.), use a clean DC source.         |

---

## 10. Recommended Settings

- **Sample rate**: 44.1 kHz or 48 kHz.
- **Audio**: Mono, no heavy EQ or compression.
- **SSTV modes**:
  - `Martin M1` — generally good starting point.
  - `Robot36` — quicker tests.
- **Transmitter supply**: 3–6 V, higher voltage gives a bit more range but also more risk of spurs.

---

## 11. Notes & Extensions

- This board acts as a **generic FM audio transmitter**. SSTV is just one application.
- You can also:
  - Send AFSK, RTTY, or other digital modes within the audio bandwidth.
  - Use an SDR instead of a simple FM radio on the receiver side for better control and analysis.

If you build this and your images look cursed, capture a screenshot of the decoded frame and your test setup and we can debug your gain staging, tuning and layout together.

Happy (legal) experimenting with SSTV over FM 🚀
