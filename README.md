# FM SSTV Transmitter

This is a small **through-hole FM audio transmitter** designed at **Northern Labs PK** by Muhammad Uzzam Butt.  
When you input **SSTV audio**, it will transmit that over FM so a normal FM receiver + SSTV decoder can reconstruct images.

![schematic](https://github.com/uzzambutt/FM-Image-Transmitter-with-SSTV/blob/main/src/Schematic_New-Project_2025-04-30_PCBWay%20Community.png)

> ⚠️ **Warning**  
>  please don't do anything illegal  or modify the circuit to output higher power signal as FM without license is illegal.

---

## FM SSTV Transmitter

This board is basically an **analog FM circuit**:

![PCBF](https://github.com/uzzambutt/FM-Image-Transmitter-with-SSTV/blob/main/src/1533336674b4b2fec0f114ee6a4b1cce5372c8df10261.png)

![PCBB](https://github.com/uzzambutt/FM-Image-Transmitter-with-SSTV/blob/main/src/153402481240933be95a94050b96a42203242d4660238.png)



- You give it **audio** (SSTV tones from PC/phone).
- It transmits in the 88–108 MHz band.
- Any FM radio tuned to that frequency gets the audio back out.
- SSTV software on the receive side decodes that audio into an image.

designed for learning how SSTV actually behaves over a real RF link.

---

## Custom Features!

- Line-in **audio input** via 3.5 mm jack (for PC / phone SSTV audio)
- On-board **electret microphone** input for simple voice tests
- **3–6 V** DC input, low current (~15–20 mA typical)
- RF oscillator + buffer using **S9014/S9018** BJTs
- Output **ANT pad** for wire antenna or dummy load
- Tuneable LC tank for setting carrier in the FM band
- Designed specifically to carry **SSTV modes** like Martin M1/2, Robot36, etc.

---

## Typical SSTV System:

**TX side:**

1. Pick an image on your **PC or phone**.
2. Encode it as SSTV audio using:
   - PC: *MMSSTV*, *fldigi*, *QSSTV*, etc.  
   - Android: *Robot36 - my fav*, *SSTV Droid*, etc.
3. Play that audio out of the headphone jack → into the board’s **AUDIO IN** jack.
4. Board FM-modulates the signal and sends it out on the chosen frequency.

**RX side:**

1. Tune a regular **FM radio** (or SDR) to the transmitter’s frequency.
2. Take its audio output into a PC/phone running SSTV **decoder** software.
3. Watch the image rebuild line by line.

---

## PCB Design

The FM SSTV Transmitter is a single-board, mostly through-hole PCB laid out for:

- Short RF paths around the **oscillator and output network**
- Separate audio front-end area (mic + line-in + AF preamp)
- Clear **ANT** pad and coil footprints for easy coil-winding and tweaking
- 3–6 V power header with LED indicator and decoupling

You can open the project in EasyEDA (or your preferred viewer) to see:

- Full **schematic**
- Top / bottom copper
- Drill and mask layers
- 2D and 3D board previews
