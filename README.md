# Telecommunications-Engineering-Modular-Analysis-of-Analog-FM-and-Digital-PCM-Architectures
A comprehensive hardware-based analysis of frequency modulation and digital pulse-code systems. This repository documents the end-to-end implementation of analog FM and 4-bit PCM architectures on the Emona ETT-101, featuring signal reconstruction, Nyquist sampling verification, and digital codec performance testing, and concluding with 1-bit Delta Modulation (DM) architectures.

## TABLE OF CONTENTS
* [Part 1: Analog Frequency Modulation & Demodulation](#part-1-analog-frequency-modulation--demodulation)
* [Part 2: Sampling and Reconstruction](#part-2-sampling-and-reconstruction)
* [Part 3: PCM Encoding & Decoding](#part-3-pcm-encoding--decoding)
* [Part 4: 1-bit Delta Modulation](#part-4-1-bit-delta-modulation)
* [Results & Data Analysis](#results--data-analysis)

## Part 1: Analog Frequency Modulation & Demodulation
This section explores the complete lifecycle of an analog Frequency Modulated (FM) signal, covering both its synthesis and subsequent recovery. Unlike Amplitude Modulation (AM), which embeds information in the signal's power level, FM varies the carrier's instantaneous frequency in proportion to the message signal's amplitude. This characteristic provides FM with superior immunity to electrical noise, as most interference affects signal amplitude rather than frequency.

### 1.1 Signal Synthesis (Modulation)
The FM signal is generated using a Voltage Controlled Oscillator (VCO). The process involves:
- Carrier Calibration: Setting the VCO to a stable "rest frequency" (e.g., $10\text{kHz}$), which serves as the center frequency when no message is applied.
- Frequency Deviation: Applying a message signal—such as a $2\text{kHz}$ square wave or real-time audio from a speech module—to the VCO input. This causes the carrier to shift above and below the rest frequency.
- Proportionality: Observing that the amount of frequency shift (deviation) is directly controlled by the amplitude of the message signal.

#### **1.1 Signal Synthesis (Modulation) Experimental Results**
<details>
<summary>View Part 1 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 1.1.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 1.1.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 1.1.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

### 1.2 Signal Recovery (Demodulation)
To reconstruct the original message from the frequency-shifted carrier, a Zero-Crossing Detector (ZCD) architecture is employed:
- Limiting: The received signal passes through a comparator to remove amplitude fluctuations and produce a clean square wave.
- Pulse Conversion: The system generates fixed-width pulses at every zero-crossing of the squared signal. This creates a pulse train where the duty cycle changes in synchronization with the FM signal's frequency.
- Reconstruction: A Baseband Low-Pass Filter (LPF) extracts the average DC component of the pulse train, smoothing the variations back into the original analog message waveform.

#### **1.2 Signal Recovery (Demodulation) Experimental Results**
<details>
<summary>View Part 1 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 1.1.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 1.1.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 1.1.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

## Part 2: Sampling and Reconstruction




## Part 3: PCM Encoding & Decoding




## Part 4: 1-bit Delta Modulation




## Results & Data Analysis




## Project Resources




