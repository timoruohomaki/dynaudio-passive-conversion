# Dynaudio BM6A mkII — passive conversion build notes

Document date: 2026-08-30, revised 2026-09-20.
Status: speaker 1 built and measured, crossover verified. Remaining work is listed in section 10.

## 1. Background and goal

The pair of Dynaudio BM6A mkII active monitors developed an audible background hiss that survived repair attempts on the active electronics. Rather than fund another repair round, the speakers were converted to passive operation: all active circuitry removed and replaced with a passive crossover, with final voicing done by measurement and listening in the actual room.

The design basis is the passive Dynaudio BM6, which used the same driver family. Its crossover was recovered from forum photographs and a traced schematic. Component-for-component the trace matches the photographed board, so it is treated as the factory reference topology. The traced schematic shows Visaton driver symbols (W 170 S, SC 10 N) — these are placeholders from the Boxsim simulator used for tracing, not the actual drivers.

Bench measurements showed that the BM6A mkII drivers are **different impedance variants** than the passive BM6's, so the reference network was rescaled rather than copied:

- Woofer: Re = 6,1 Ω → 8 Ω-class winding (passive BM6 used the 4 Ω twin). Woofer low-pass branch scaled ×2.
- Tweeter: Re ≈ 4,7 Ω → lower-impedance variant (likely nominal ~6 Ω), more sensitive per volt. With the quieter 8 Ω woofer, the tweeter was expected to run roughly 4–6 dB hot against the reference network, so attenuation was re-centered and split between series and shunt resistors. Measurement confirmed the estimate landed close (section 8).

## 2. Driver measurements

| Driver | Measurement | Value | Interpretation |
|---|---|---|---|
| Woofer | DC resistance (DMM, leads subtracted: 8,40 − 2,30 Ω) | 6,10 Ω | 8 Ω-class voice coil |
| Woofer | LCR 1 kHz: Rs / Xs | 9,178 Ω / +4,11 Ω | \|Z\| ≈ 10,1 Ω; apparent Le ≈ 0,65 mH; Rs above Re due to eddy losses |
| Tweeter | DC resistance (6,96 Ω incl. 2,30 Ω leads) | ≈ 4,66 Ω | Lower-impedance variant, ~6 Ω nominal |
| Tweeter | LCR 1 kHz: Rs / Xs | 11,57 Ω / −0,12 Ω | At resonance; Fs just below 1 kHz; peak of only ~11,6 Ω over ~4,7 Ω Re = heavy ferrofluid damping → no resonance trap needed |

## 3. Crossover as built (per speaker)

Both branches connect in parallel to the input binding posts. Crossover ≈ 2200 Hz, BM6 topology.

### Woofer low-pass (reference values scaled ×2 for the 8 Ω winding)

- L1 = 2,0 mH air core in series
- Damped shunt to ground: 2,2 Ω in series with 6,8 µF (design target 7,5 µF; the damped shunt is tolerant and measurement shows no need to change it)

### Tweeter high-pass

- Series: 8,2 Ω (built as 15 Ω ∥ 18 Ω = 8,18 Ω for thermal load sharing) → C2 = 6,8 µF
- Shunt: L2 = 0,20 mH to ground
- Level pad, switched (ON-OFF-ON SPDT, center-off): fixed 15 Ω always in circuit; one throw parallels 18 Ω, the other 8,2 Ω. Resistance always stays in the shunt, which preserves filter damping; a mid-travel open contact only means momentarily brighter — fail-safe by design. Never put switching in series with a driver.
- Top-octave contour: 10 Ω + 1,0 µF in series, across the tweeter terminals.

### Pad positions (switch labelled A–B–C, identified by measurement on speaker 1)

| Position | Shunt | Tweeter level vs B | Character |
|---|---|---|---|
| A | 15 Ω ∥ 8,2 Ω (≈ 5,3 Ω) | ≈ −2,4 dB | Flattest through the presence region |
| B (center-off) | 15 Ω alone | 0 dB (reference) | Brightest, most air |
| C | 15 Ω ∥ 18 Ω (≈ 8,2 Ω) | ≈ −1,4 dB | In between |

Steps match the design prediction (about 1 dB per step, about 2,4 dB total). All three positions are usable, so no resistor changes were needed. Mark the switch plate A / B / C, or by character.

The effect of PAD positions is illustrated below.

![SPL diagram of three PAD modes](https://github.com/timoruohomaki/dynaudio-passive-conversion/blob/main/PAD%20AtoC.png)

### Polarity

Both drivers in normal (marked) polarity. Confirmed by measurement: the summed response runs continuously through the 2200 Hz crossover region with no dip in any pad position. A reversed tweeter would show a deep null there, so no polarity-flip test was needed.

## 4. Parts list and verification results

| Position | Part | Nominal | Measured | Verdict |
|---|---|---|---|---|
| L1 woofer series | Jantzen 000-0027 air core, 14 AWG (1,6 mm) | 2,0 mH ±3%, DCR 0,363 Ω | 2,027 mH; ESR 0,341 Ω at 100 Hz (short-calibrated) | Accepted; within ±3% and under the 0,4 Ω DCR budget |
| L2 tweeter shunt | Jantzen 000-1528 air core, 20 AWG | 0,25 mH | 0,2034 mH ±3%, DCR 0,3 Ω | Trimmed by removing 13 rounds |
| C1, C2 (6,8 µF) | Audyn MKP, older unused stock | 6,8 µF ±2%, 800 VDC | 6,841 µF; D = 0,000; ESR at meter floor (θ = −90,0°) | Accepted; film caps don't age on the shelf |
| RC contour cap | Jantzen Standard Z-Cap | 1,0 µF, 400 VDC | Not logged | Least cap-sensitive position in the design |
| Resistors | 10 W sand-cast/MOX: 2,2 / 8,2 / 10 / 15 / 18 Ω | — | Not logged | Series 8,2 Ω built as 15 ∥ 18 pair |
| Switch | ON-OFF-ON SPDT toggle, 3–6 A | — | — | Seal bushing with silicone (bass-reflex cabinet) |

For speaker 2: pair the 6,8 µF caps deliberately. Put the closest-matched pair in the tweeter series positions (they set the highpass corner, so match L/R), the rest in the woofer shunts.

## 5. Component verification procedure (LCR discipline)

1. **Check the frequency indicator before logging anything.** The meter's test frequency can change between sessions, and every displayed value belongs to that frequency. Take all values from one screen at one time. (This meter's high test frequency is 7,8 kHz, not 10 kHz.)
2. **Short-circuit calibrate before any low-resistance measurement.** Lead and contact resistance is several tenths of an ohm and wanders from shot to shot.
3. **Coil DCR: measure at 100 Hz** after short-cal, coil in free air on a wooden surface, away from metal. At 100 Hz the ESR reading is essentially the DC resistance. Air-core inductance doesn't depend on frequency, so any test frequency gives a valid L.
4. **High-frequency coil readings look alarming and usually aren't.** Thick-wire multilayer air coils show large AC resistance at high test frequencies (proximity effect; e.g. 14 Ω at 7,8 kHz on a coil with 0,34 Ω DCR). Normal physics, mostly in the stopband, and captured by the acoustic measurement anyway.
5. **Sanity-check every reading set:** Q = X/ESR, D = 1/Q, θ = arctan(Q). θ positive = inductive, negative = capacitive, near 0° = resistive (a driver at resonance). Inconsistent numbers mean a settings or contact problem, not a bad part. D = 9,999 or Q = 999 means the display ran out of digits.
6. **A standard DMM can't measure sub-ohm resistance** through its own leads (2,3 Ω leads vs a 0,34 Ω coil = noise). If ever needed, force a known current from a bench supply and read millivolts across the part (poor-man's Kelvin). Otherwise trust the short-calibrated LCR.
7. Measure driver Re at DC or 100/120 Hz, never at 1 kHz, where eddy losses inflate the reading.

## 6. Mechanical build

**Backplane:** original aluminium amp plate replaced with MDF (no heat sink needed at passive dissipation levels; also removes eddy-current concerns near the coils). The panel is part of a bass-reflex enclosure, so it must be stiff and airtight: 16–19 mm or braced; closed-cell foam gasket or silicone bead around the perimeter; screws every 8–10 cm; binding-post and switch penetrations sealed (toggle bushings aren't airtight). Keep the original damping material, positioned so it can't touch the resistors — wool-wrapped resistors lose their free-air rating. The new MDF boards are cut to 140 x 244 mm. The board is 9 mm but is structurally enhanced by glueing a 35 x 35 mm wooden block in the center.

**Boards:** two turret boards per speaker, 45 × 145 mm, 2 rows × 15 positions. Board A = woofer branch, Board B = tweeter branch. Top row = signal bus, bottom row = ground bus, vertical parts = shunt legs, flying leads to off-board parts. The isolated junction turrets (woofer R–C midpoint, RC-contour midpoint, and the tops of the two switched pad resistors) are the only positions where a wiring slip changes the circuit — check them against the schematic before power-up.

![Turret board design](https://github.com/timoruohomaki/dynaudio-passive-conversion/blob/main/bm6a_turret_board_layout.png)

**Off-board on the MDF panel:** L1, L2, C1, C2, pad switch, binding posts. Coils with axes perpendicular and maximum separation, at least 10 cm from the woofer magnet; nylon or brass bolt through any coil centre (steel turns an air core into a cored inductor). Heavy parts get restraint beyond their leads — cabinet vibration will eventually crack lead-only joints. Wrap, then solder, on every turret.

**Wiring:** length doesn't matter electrically here (about 0,012 Ω and 1 µH per metre). Run out/return pairs together, cut to length, use ≥1,5 mm² wire, and put the care into joint quality.

## 7. Measurement setup (REW)

> [!NOTE]
> These measurements were taken in acoustically non-treated room which is less than ideal.

Hardware: Focusrite Scarlett Solo, calibrated measurement mic, Mac, amplifier driving one speaker.

- **Mic on the Solo's XLR input** with 48 V phantom. On this unit that is input 2 — only the XLR input has phantom. Air mode off, direct monitor off.
- **Timing reference:** Output R → the jack input (line mode, INST off, gain near minimum). Output L → amplifier. In REW: output Left, timing reference output Right, timing reference input = the jack input. A line-to-line loop is already hot, so don't try to match input level to output level; a clean, unclipped signal is all it needs.
- **Soundcard calibration: none.** It's optional on a Focusrite (the correction is a fraction of a dB), and a wrong cal file is worse than none. It must be run as a cable loop — through amp, speaker and mic it measures the whole acoustic path, and REW warns with "Excessive variation in measurement".
- **Sweep:** 20 Hz – 20 kHz (not 0 Hz), 256k, −12 dBFS output, about 80–85 dB at the mic. Tick "Abort above SPL limit" (100 dB). Keep gain and amp level fixed through a session.
- **Far-field geometry:** mic on the tweeter axis at 0,5 m, speaker raised on a stand. Lifting the speaker moved the first reflection from about 3 ms to about 4 ms.
- **Gate (IR Windows dialog):** left width 1 ms, right width 3,5 ms, Tukey 0,25, ref time at the impulse peak. Valid from about 300 Hz upward. Every time the speaker or mic moves, re-check the first reflection in the impulse view (zoom to −5…+20 ms) before setting the window.
- **Smoothing:** Graph menu → 1/6 octave for overall shape, 1/12 for crossover detail. Unsmoothed, ungated curves are only for diagnostics.
- **Nearfield:** remove the gate (right width 300–500 ms) and drop the output 15–20 dB. Woofer: capsule 5–10 mm from the dustcap centre. Port: capsule in the plane of the port mouth, not inside it. Nearfield data is valid only below roughly 500 Hz.
- **Comparisons are valid only with identical gate, calibration and geometry.** Name every measurement clearly and save the .mdat before changing anything.

## 8. Measured results — speaker 1

**Far-field, gated, 0,5 m on tweeter axis, 1/6 octave:**

- 300 Hz – 10 kHz within about ±2,5 dB (pad A). Crossover region clean, no dip or discontinuity at 2200 Hz in any pad position.
- Pad steps as in section 3: B → C −1,4 dB, B → A −2,4 dB above 5 kHz. Below 3 kHz all three curves are identical, which confirms nothing drifted between sweeps.
- Broad, shallow dip around 4–5 kHz, about 4 dB. Most likely cabinet diffraction. No change planned.
- Peak at 13–15 kHz, about +9 dB, present in all pad positions with the same shape. Most likely the tweeter's own top-octave resonance, above the reach of the 10 Ω + 1,0 µF contour. Leave it unless it bothers you in listening; if it does, try raising the contour cap to 1,5 µF to move the shelf lower.
- The large 12–16 kHz peak and the dense ripple in the first, ungated sweep were room artifacts, not the speaker.

**Nearfield (box tuning):**

- Woofer nearfield minimum at about 100 Hz; port output peaks at about 90–95 Hz. The two agree, so **Fb ≈ 90–100 Hz**.
- This is roughly an octave higher than the 45–55 Hz first estimated. The BM6A's quoted 41 Hz extension came from bass EQ in the active electronics, not from the enclosure. Removing the electronics removed that extension.
- Consequence: the passive speaker is usable down to about 90–100 Hz and rolls off below. For full-range use, add a subwoofer crossed at about 90–100 Hz, which should integrate naturally with this tuning.
- The panel wasn't fully sealed during the measurement. A leak raises and softens the apparent tuning, but typically by a few Hz, not an octave. Re-measure after sealing to confirm.

**Absolute level:** SPL calibration was inconsistent during the session (one gated sweep read about 10 dB, the others about 90 dB at the same geometry). All comparisons in this section use measurements with matched calibration, but the absolute sensitivity (estimated 84–85 dB/2,83 V) has **not** been verified.

## 9. Expectations

- Load: 4–6 Ω class, estimated sensitivity 84–85 dB/2,83 V (unverified) — plan amplification accordingly.
- Bass: flat to about 90–100 Hz, rolling off below. A subwoofer is needed for full-range listening.
- Voicing: final pad choice by ear in the listening room. The target is "comfortable in this room", not a textbook curve. Below a few hundred Hz, in-room response is dominated by the room, so don't chase it with crossover changes.

## 10. Remaining work

1. Build and measure speaker 2 with identical settings; overlay with speaker 1 to check pair matching (this is the measurement that would reveal a component tolerance problem between builds).
2. Seal the MDF panel permanently (gasket, screws, silicone at switch and terminals); re-run the port nearfield to confirm Fb.
3. Optional: one gated sweep at 1 m to check crossover summation at a more realistic listening distance.
4. Listening tests; pick a pad position and record it here with the date.
5. Optional: log the 1,0 µF cap and resistor values from the LCR for completeness.
6. Decide on a subwoofer and crossover point (about 90–100 Hz) if full-range playback is wanted.
