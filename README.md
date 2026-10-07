# Industrial Power Conversion and Motor Drive Analysis Using MATLAB/Simulink

An academic study of **single-phase rectification, chopper-based DC motor speed control, and voltage-source inversion** for variable-load industrial applications. Prepared by **Puja Roy Dipa** for **ECE 3205: Industrial Electronics** at **Rajshahi University of Engineering & Technology (RUET)**.

The project combines converter models, electrical waveform analysis, and motor-drive responses to examine the relationship between power conversion, ripple, efficiency, and speed regulation.

## Project overview

| Stage | Focus | Included source |
| --- | --- | --- |
| Single-phase full-wave rectifier | Input/output voltage, ripple factor, approximate THD, and power calculations | MATLAB post-processing script |
| Step-down DC-DC chopper | PWM armature control and PI-based DC motor speed regulation | Simulink model |
| Single-phase voltage-source inverter (VSI) | PWM switching and a single-phase asynchronous motor drive | Simulink model |

The report compares light/full-load operating points, explains speed and torque transients, and discusses filtering and converter design choices for industrial drives.

## Repository contents

| File | Contents |
| --- | --- |
| [`matlab_simulink_files.zip`](matlab_simulink_files.zip) | One archive containing all three original MATLAB/Simulink source files. |
| [`figures.pdf`](figures.pdf) | Figures 1-6 from the report, collected into one three-page PDF with their original captions. |
| [`report.pdf`](report.pdf) | Complete, unmodified 12-page report: *Industrial Power Conversion Analysis for Variable Load Applications*. |
| `README.md` | Overview, requirements, usage instructions, and reproducibility notes. |

The source archive contains:

```text
matlab_simulink_files.zip
├── vsi.slx
├── speed_control_of_DC_motor_using_chopper.slx
└── rectifier.m
```

Two files are Simulink models (`.slx`); `rectifier.m` is an analysis script (`.m`). All three are preserved exactly as supplied.

## Requirements

- **MATLAB and Simulink**. Both supplied models were saved in **R2021a**, according to their embedded metadata.
- **Simscape Electrical**, including the **Specialized Power Systems** libraries referenced by the models (IGBTs, electrical machines, sources, measurements, PWM generators, and `powergui`).
- The `rms` function used by the rectifier script must be available in the installed MATLAB environment; MATLAB R2021a provides it through Signal Processing Toolbox.

R2021a is the reference release for the archived models. Compatibility with other releases and full model execution have not been verified for this repository.

## Getting started

Clone or download this repository, then extract the source archive. From MATLAB, with the repository as the current folder:

```matlab
unzip('matlab_simulink_files.zip', 'matlab_simulink');
cd('matlab_simulink');
```

### DC motor speed control

Open the chopper model:

```matlab
open_system('speed_control_of_DC_motor_using_chopper.slx');
```

Review the source voltage, PWM settings, motor parameters, reference input, PI controller, and simulation stop time. Click **Run** and inspect the model's scopes to observe speed and electromagnetic torque.

### Voltage-source inverter

Open the VSI model:

```matlab
open_system('vsi.slx');
```

Review the DC-link source, PWM generator, machine parameters, and simulation settings. Click **Run** and inspect the scopes and workspace logging blocks for the inverter and machine responses.

### Rectifier analysis

`rectifier.m` processes an existing simulation output named `out`. It does not build or run a rectifier model. The required signals use the structure-with-time format:

| Required field | Signal |
| --- | --- |
| `out.vs_rect.signals.values` | Source voltage |
| `out.v_rect.signals.values` | Rectifier output voltage |
| `out.i_rect.signals.values` | Rectifier output current |
| `out.is_rect.signals.values` | Source current |

Run a compatible rectifier model or load saved simulation data containing those fields, then execute:

```matlab
run('rectifier.m');
```

The script prints input RMS/peak voltage, theoretical and measured average output voltage, ripple factor, a no-FFT THD estimate, and an efficiency estimate.

**The rectifier Simulink model and its saved simulation output were not supplied.** Its circuit and waveforms are available in `figures.pdf` and `report.pdf`; the rectifier results cannot be reproduced from the three source files alone.

## Figures

The figures document three converter stages:

1. Single-phase full-wave rectifier model with an RL load.
2. Source/load waveforms and input/output power traces.
3. Buck chopper supplying a DC machine with PI speed control.
4. DC motor speed and electrical torque responses.
5. Single-phase VSI supplying a split-phase motor.
6. Inverter voltage, current, and PWM-related waveform traces.

These are archived report figures, not newly generated simulation results.

## Reported operating-point results

The following values are transcribed from the report's comparative summary. They combine reported measurements and analytical estimates and have not been independently reproduced.

| Converter / operating point | Reported voltage | Ripple / current ripple | Reported THD | Reported efficiency |
| --- | --- | --- | --- | --- |
| Rectifier: light load | 153.87 V average | 87.92% | 77.85% | 88.17% |
| Rectifier: full load | 153.64 V average | 88.28% | 77.87% | 86.62% |
| Chopper: light load | 88.7 V average | 2.52 A peak-to-peak | Not reported | 98.56% |
| Chopper: full load | 238.2 V average | 1.92 A peak-to-peak | Not reported | 99.25% |
| VSI: AC drive | 84.85 V fundamental RMS | Not reported | Approximately 4.8% | Approximately 96% |

The report uses a 300 V source for the chopper calculations, a 5 kHz switching frequency, and a 5 mH inductor. Its VSI analytical example uses a 300 V DC link, 50 Hz fundamental frequency, modulation index 0.8, and an equivalent 60-ohm / 0.1 H load. These report assumptions should be checked against the actual model settings before comparison.

## Reproducibility and interpretation

- The provided source files and full report are unchanged. The figures PDF displays the six original figures and captions from physical PDF pages 5, 7, and 9.
- The two `.slx` archives passed structural integrity checks. This confirms readable archive contents, not successful Simulink simulation.
- The rectifier script's THD expression uses a peak-based fundamental estimate rather than harmonic extraction. Its efficiency calculation uses output average instantaneous power divided by input RMS apparent power. Interpret these as the supplied script's estimates rather than validated harmonic distortion or real-power conversion efficiency.
- The report's rectifier waveform figure displays an efficiency of approximately 99.08%, while its operating-point tables report 88.17% and 86.62%. The operating conditions or calculation methods need reconciliation before treating these values as a common benchmark.
- Existing model metadata is retained, including the chopper model's original creator entry, `Deepak`. The academic report is credited to Puja Roy Dipa; preservation of the model does not imply authorship of every original component.

## Further development

Useful extensions include adding the missing rectifier model and logged data, measuring harmonics with FFT-based analysis, computing efficiency from synchronized real input/output power, and automating steady-state comparisons across converter operating points.

## Academic context

- **Prepared by:** [Puja Roy Dipa](https://github.com/puja-roy-dipa)
- **Course:** Industrial Electronics (ECE 3205)
- **Institution:** Rajshahi University of Engineering & Technology
- **Supervisor:** Md. Faysal Ahamed, Lecturer, ECE, RUET
- **Report date:** 28 October 2025

## References

As listed in the original report:

1. N. Mohan, T. M. Undeland, and W. P. Robbins, *Power Electronics: Converters, Applications, and Design*, 3rd edition, John Wiley & Sons, 2003.
2. R. W. Erickson and D. Maksimovic, *Fundamentals of Power Electronics*, 2nd edition, Springer, 2001.
3. *IEEE Recommended Practice and Requirements for Harmonic Control in Electric Power Systems*, IEEE Std 519-2014.
4. D. G. Holmes and T. A. Lipo, *Pulse Width Modulation for Power Converters: Principles and Practice*, IEEE Press / Wiley, 2003.
