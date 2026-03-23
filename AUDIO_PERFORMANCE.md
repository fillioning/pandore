# Audio Performance

## Signal Chain

```
Mic/Line In (Neutrik XLR+TRS combo)
    → THS4521 Diff Amp → OPA1656 Conditioning → Input Buffer
        → CS4272 ADC (24-bit, up to 192 kHz)
            → I2S → ISO7762 Galvanic Isolation → I2S
                → Teensy 4.1 (I2S ↔ USB Audio Bridge)
                    → USB → LattePanda Mu
                        → USB → Teensy 4.1
                            → I2S → ISO7762 → I2S
                                → CS4272 DAC
                                    → Output Buffer → Line Out (Neutrik TRS)
                                    → Monitor Amp (volume knob) → Headphones (3.5mm TRS)
```

## Component Specs

| Component | Role | Key Specs |
|---|---|---|
| **CS4272** (Cirrus Logic) | Stereo codec | 24-bit, 4–192 kHz, 114 dB dynamic range (A-wtd), -100 dB THD+N |
| **THS4521** (TI, ×4) | Fully-differential amp | 4.6 nV/√Hz noise, -112 dB THD+N @ 1 kHz, 145 MHz BW, 490 V/µs, 102 dB CMRR |
| **OPA1656** (TI, ×4) | Dual op-amp | 2.9 nV/√Hz noise, -131 dB THD @ 1 kHz, 53 MHz GBW, 24 V/µs |
| **ISO7762** (TI) | I2S digital isolator | 6-ch (4F/2R), 100 Mbps, 11 ns delay, 5000 VRMS isolation |
| **H11L1** (Isocom) | MIDI optoisolator | Schmitt-trigger output |
| **Teensy 4.1** (PJRC) | Audio/MIDI bridge | Cortex-M7 @ 600 MHz, 2× I2S, USB Audio class-compliant |

## System-Level Estimates

The analog front-end (OPA1656 at -131 dB THD, THS4521 at -112 dB) significantly exceeds the codec's performance floor — the **CS4272 is the limiting factor**, not the preamp stage.

| Parameter | Expected Value |
|---|---|
| **Dynamic range** | 112–114 dB (A-weighted) |
| **THD+N (system)** | -98 to -100 dB (codec-limited) |
| **Noise floor** | ~-114 dBFS (A-weighted) |
| **Max sample rate** | 192 kHz / 24-bit |
| **USB audio latency** | ~5–10 ms round-trip (buffer-dependent) |
| **Frequency response** | 20 Hz – 90 kHz (at 192 kHz) |
| **Phantom power** | 48V / 10 mA |
| **Channel separation** | >100 dB |
| **Galvanic isolation** | 5000 VRMS (digital ↔ audio domain) |

## Physical I/O

| Connector | Part | Type |
|---|---|---|
| Mic/Line In (×2) | Neutrik NCJ9FI-H-0 | Combo XLR + 6.35mm TRS + 3-way switch |
| Line Out (×1) | Neutrik NSJ12HF-1 | 6.35mm TRS |
| Monitor | Same Sky SJ3-35083B-TR | 3.5mm TRS |
| MIDI In/Out | Same Sky SDS-50J (×2) | 5-pin DIN |
| Teensy USB | Same Sky CPG-23-SMT-TR (×2) | USB-C |

## Performance Class

Comparable to prosumer interfaces (Focusrite Scarlett, MOTU M4). The 114 dB dynamic range is well above the noise floor of any live performance or instrument prototyping scenario. The analog front-end is essentially transparent — the OPA1656's -131 dB THD provides substantial headroom over the codec.
