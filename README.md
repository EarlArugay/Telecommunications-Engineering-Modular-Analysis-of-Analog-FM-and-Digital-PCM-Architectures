# Telecommunications-Engineering-Modular-Analysis-of-Analog-FM-and-Digital-PCM-Architectures
A comprehensive hardware-based analysis of frequency modulation and digital pulse-code systems. This repository documents the end-to-end implementation of analog FM and 4-bit PCM architectures on the Emona ETT-101, featuring signal reconstruction, Nyquist sampling verification, and digital codec performance testing, and concluding with 1-bit Delta Modulation (DM) architectures.

## TABLE OF CONTENTS
* [Part 1: Analog Frequency Modulation & Demodulation](#part-1-analog-frequency-modulation--demodulation)
* [Part 2: Sampling and Reconstruction](#part-2-sampling-and-reconstruction)
* [Part 3: PCM Encoding & Decoding](#part-3-pcm-encoding--decoding)
* [Part 4: Bandwidth Limiting & Restoring Digital Signals](#part-4-Bandwidth-Limiting-&-Restoring-Digital-Signals)
* [Results & Data Analysis](#results--data-analysis)

## Part 1: Analog Frequency Modulation & Demodulation
This section explores the complete lifecycle of an analog Frequency Modulated (FM) signal, covering both its synthesis and subsequent recovery. Unlike Amplitude Modulation (AM), which embeds information in the signal's power level, FM varies the carrier's instantaneous frequency in proportion to the message signal's amplitude. This characteristic provides FM with superior immunity to electrical noise, as most interference affects signal amplitude rather than frequency.

### 1.1 Signal Synthesis (Modulation)
The FM signal is generated using a Voltage Controlled Oscillator (VCO). The process involves:
- Carrier Calibration: Setting the VCO to a stable "rest frequency" (e.g., $10\text{kHz}$), which serves as the center frequency when no message is applied.
- Frequency Deviation: Applying a message signal—such as a $2\text{kHz}$ square wave or real-time audio from a speech module—to the VCO input. This causes the carrier to shift above and below the rest frequency.
- Proportionality: Observing that the amount of frequency shift (deviation) is directly controlled by the amplitude of the message signal.

#### **1.2 Signal Synthesis (Modulation) Experimental Diagrams**
<details>
<summary>View Part 1.2 Diagrams</summary>

![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4314.jpg)
*Figure 1.2.1: FM Modulation Setup.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4319.jpg)
*Figure 1.2.2: FM Modulation Diagram.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4319.jpg)
*Figure 1.2.3: Considering the Spectral Composition of FM Setup.*

</details>

#### **1.3 Signal Synthesis (Modulation) Experimental Results**
<details>
<summary>View Part 1.1 Documentation</summary>

![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/9.2.jpg)
*Figure 1.3.1: Carrier and Modulating Signal*
![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/9.1.jpg)
*Figure 1.3.2: Frequency Modulated Signal*
![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/9.3.jpg)
*Figure 1.3.3: Frequency Modulated Signal with minimum VCO*
![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/9.4.jpg)
*Figure 1.3.3: Frequency Modulated Signal with middle VCO*
![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/9.5.jpg)
*Figure 1.3.3: Frequency Modulated Signal with max VCO*
![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/9.11.jpg)
*Figure 1.3.3: Spectral Composition of FM Signal.*

</details>

### 1.4 Signal Recovery (Demodulation)
To reconstruct the original message from the frequency-shifted carrier, a Zero-Crossing Detector (ZCD) architecture is employed:
- Limiting: The received signal passes through a comparator to remove amplitude fluctuations and produce a clean square wave.
- Pulse Conversion: The system generates fixed-width pulses at every zero-crossing of the squared signal. This creates a pulse train where the duty cycle changes in synchronization with the FM signal's frequency.
- Reconstruction: A Baseband Low-Pass Filter (LPF) extracts the average DC component of the pulse train, smoothing the variations back into the original analog message waveform.

#### **1.5 Signal Recovery (Demodulation) Experimental Diagrams**
<details>
<summary>View Part 1.2 Diagrams</summary>

![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4328.jpg)
*Figure 1.5.1: Zero Crossing Detector Setup.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4329.jpg)
*Figure 1.5.1: Zero Crossing Detector Diagram.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4330.jpg)
*Figure 1.5.2: Operation of Zero Crossing Setup.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4331.jpg)
*Figure 1.5.2: Operation of Zero Crossing Diagram.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4336.jpg)
*Figure 1.5.3: Transmitting and Receiving sinewave using FM Setup.*
![Calibration Waveform](Diagrams/FM_Modulation_Demodulation/IMG_4337.jpg)
*Figure 1.5.3: Transmitting and Receiving sinewave using FM Diagram.*

</details>

#### **1.6 Signal Recovery (Demodulation) Experimental Results**
<details>
<summary>View Part 1.2 Documentation</summary>

![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/10-Part-B-1-FM-TP.png)

*Figure 1.6.1: Zero Crossing Detector Result.*

![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/10-Part-C-1-Fig10.png)

*Figure 1.6.2: Operation of Zero Crossing.*

![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/10-Part-C-2-Fig12.png)

*Figure 1.6.3: Operation of Zero Crossing.*

![Calibration Waveform](Waveform_Captures/FM-Modulation-Demodulation/10-Part-D-1-Fig16.png)

*Figure 1.6.4: Transmitting and Receiving sinewave using FM Result.*

</details>

## Part 2: Sampling and Reconstruction
This section marks the transition from purely analog signals to discrete-time processing. We explore the Nyquist-Shannon Sampling Theorem, which defines the mathematical bridge between continuous waveforms and their digital representations. By using a switching gate to "sample" an analog signal, we demonstrate that a signal can be perfectly reconstructed provided the sampling frequency is sufficiently high.

### 2.1. The Sampling Process
The continuous message signal is converted into a series of pulses using a Dual Analog Switch (acting as a sampling gate):
- Sampling Clock: A high-frequency pulse train (Sampling Signal) is used to open and close the switch at regular intervals.
- Sampled Output: The output consists of "slices" of the original message. We observe how the pulse amplitude follows the original waveform’s shape, creating a Pulse Amplitude Modulated (PAM) signal.

#### **2.1 The Sampling Process Experimental Diagrams**
<details>
<summary>View Part 2.1 Diagrams</summary>

![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4343.jpg)
*Figure 2.2.1: Sampling a simple message Setup.*
![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4344.jpg)
*Figure 2.2.2: Sampling a simple message Diagram.*
![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4349.jpg)
*Figure 2.2.3: Sampling a voice Setup.*


</details>

#### **2.2 The Sampling Process Experimental Results**
<details>
<summary>View Part 2.2 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.1.jpg)
*Figure 2.2.1: Sampled simple message without VDC.*
![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.2.jpg)
*Figure 2.2.2: Sampled simple message without VDC.*
![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.7.jpg)
*Figure 2.2.3: Sampled voice without direct voice.*
![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.6.jpg)
*Figure 2.2.4: Sampled voice without direct voice.*

</details>

### 2.3. Signal Reconstruction
To return the discrete samples back to their original analog form:
- Filtering: The sampled signal is passed through a Tuneable Low-Pass Filter.
- Recovery: The filter removes the high-frequency "switching" components, leaving behind the smooth, original baseband message.

#### **2.3 Signal Reconstruction Experimental Diagrams**
<details>
<summary>View Part 2.3 Diagrams</summary>

![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4349.jpg)
*Figure 2.3.1: Reconstructing a simple message Setup*
![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4350.jpg)
*Figure 2.3.2: Reconstructing a simple message Diagram.*
![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4352.jpg)
*Figure 2.3.3: Aliasing Setup.*
![Calibration Waveform](Diagrams/Sampling_and_Reconstruction/IMG_4352_(1).jpg)
*Figure 2.3.4: Aliasing Diagram.*

</details>

#### **2.4 Signal Reconstruction Experimental Results**
<details>
<summary>View Part 2.4 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.4.jpg)
*Figure 2.4.1: Reconstructed Signal.*
![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.5.jpg)
*Figure 2.4.2: Reconstructed Signal.*
![Calibration Waveform](Waveform_Captures/Sampling-and-Reconstruction/11.9.jpg)
*Figure 2.4.3: Aliasing Result.*

</details>

## Part 3: PCM Encoding & Decoding
This section details the implementation of Pulse Code Modulation (PCM), the standard method for digitally representing analog signals. We move beyond simple sampling into Quantization and Encoding, where continuous voltage levels are mapped to discrete binary values. This process is the foundation of modern digital telephony and audio recording.

### 3.1. Encoding (Analog to Digital)
The conversion process involves three distinct steps implemented through the PCM Encoder module:
- Sampling: The analog input is sampled at a rate determined by the system clock.
- Quantization: The continuous range of the sample is divided into discrete levels. In this setup, we utilize a 4-bit system, providing $2^4 = 16$ possible quantization levels.
- Encoding: Each quantized level is converted into a serial string of binary digits (bits), creating a high-speed digital bitstream.

#### **3.1 Encoding (Analog to Digital) Experimental Diagrams**
<details>
<summary>View Part 3.1 Diagrams</summary>

![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4451.jpg)
*Figure 3.1.1: PCM Encoding using stating DC voltage Setup 2.*
![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4452.jpg)
*Figure 3.1.2: PCM Encoding using stating DC voltage Diagram 2.*
![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4454.jpg)
*Figure 3.1.3: PCM Encoding using stating DC voltage Setup 2.*
![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4454_(1).jpg)
*Figure 3.1.4: PCM Encoding using stating DC voltage Diagram 2.*
![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4457.jpg)
*Figure 3.1.5: PCM Encoding using variable DC voltage Setup.*
![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4458.jpg)
*Figure 3.1.6: PCM Encoding using variable DC voltage Diagram.*
![Calibration Waveform](Diagrams/PCM_Encoding_Decoding/IMG_4463.jpg)
*Figure 3.1.7: PCM Encoding with continuous changing voltage Setup.*


</details>

#### **3.2 Encoding (Analog to Digital) Experimental Results**
<details>
<summary>View Part 3.2 Documentation</summary>

![Calibration Waveform](Waveform_Captures/PCM-Encoding-Decoding/Screenshot-2026-03-07-223946.png)

*Figure 3.2.1: PCM Encoding using stating DC voltage Result.*

![Calibration Waveform](Waveform_Captures/PCM-Encoding-Decoding/Screenshot-2026-03-07-224006.png)

*Figure 3.2.2: PCM Encoding using stating DC voltage Result.*

![Calibration Waveform](Waveform_Captures/PCM-Encoding-Decoding/Screenshot-2026-03-07-224028.png)

*Figure 3.2.3: PCM Encoding using variable DC voltage Result.*

![Calibration Waveform](Waveform_Captures/PCM-Encoding-Decoding/Screenshot-2026-03-07-224043.png)

*Figure 3.2.4: PCM Encoding using variable DC voltage Result.*

![Calibration Waveform](Waveform_Captures/PCM-Encoding-Decoding/Screenshot-2026-03-07-224059.png)

*Figure 3.2.5: PCM Encoding using variable DC voltage Result.*

![Calibration Waveform](Waveform_Captures/PCM-Encoding-Decoding/Screenshot-2026-03-07-224114.png)

*Figure 3.2.6: PCM Encoding with continuous changing voltage Result.*

</details>

### 3.3. Decoding (Digital to Analog)
To recover the signal, the PCM Decoder performs the inverse operation:
- Digital Reception: The decoder identifies the start of each 4-bit word using a "Frame Synchronization" signal.
- D/A Conversion: The binary words are converted back into discrete voltage levels, creating a "staircase" approximation of the original signal.
- Smoothing: The output is passed through a Low-Pass Filter to remove the sharp edges of the staircase and restore the smooth analog waveform.

#### **3.4 Decoding (Digital to Analog) Experimental Diagrams**
<details>
<summary>View Part 3.4 diagrams</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 3.4.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 3.4.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 3.4.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

#### **3.5 Decoding (Digital to Analog) Experimental Results**
<details>
<summary>View Part 3.5 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 3.5.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 3.5.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 3.5.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>


## Part 4: Bandwidth Limiting & Restoring Digital Signals

In an ideal lab environment, digital signals are perfect square waves. However, in real-world telecommunications, the physical medium (cables, air, or fiber) has a limited bandwidth. This acts as a Low-Pass Filter, removing high-frequency components and "smearing" the pulses. This section demonstrates how to manage this bandwidth limitation and use a Decision Circuit to restore the digital bitstream to its original state.

### 4.1. Simulating the Channel (Bandwidth Limiting)
A digital bitstream (from the PCM or Delta Modulator) was passed through a Tuneable Low-Pass Filter:
- Rounding: As the filter's cut-off frequency was lowered, the sharp edges of the pulses began to round off.
- Intersymbol Interference (ISI): At very low bandwidths, the pulses began to "smear" into each other, making it difficult for a receiver to distinguish between a '1' and a '0'.

#### **4.2 Simulating the Channel (Bandwidth Limiting) Experimental Diagrams**
<details>
<summary>View Part 4.2 Diagrams</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 4.2.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 4.2.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 4.2.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

#### **4.3 Simulating the Channel (Bandwidth Limiting) Experimental Results**
<details>
<summary>View Part 4.3 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 4.3.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 4.3.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 4.3.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

### 4.4. Signal Restoration (The Decision Circuit)
To recover the distorted data, a Decision Circuit (often a Comparator with a Variable DC Threshold) was used:
- Thresholding: The "smeared" signal was compared against a fixed voltage level.
- Regeneration: If the incoming voltage was above the threshold, the circuit snapped to a high state; if below, it snapped to a low state. This effectively "squared up" the signal and removed the noise and rounding caused by the bandwidth-limited channel.

#### **4.5 Signal Restoration (The Decision Circuit) Experimental Diagrams**
<details>
<summary>View Part 4.5 Diagrams</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 4.5.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 4.5.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 4.5.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

#### **4.6 Signal Restoration (The Decision Circuit) Experimental Results**
<details>
<summary>View Part 4.6 Documentation</summary>

![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig4.jpeg)
*Figure 4.6.1: Internal CAL signal showing the verified 1Vp-p square wave.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig5.jpeg)
*Figure 4.6.2: Internal CAL signal showing the square wave with 1khz frequency and 1ms period.*
![Calibration Waveform](Waveform_Captures/Part1_Results/Part1_resultfig6.jpeg)
*Figure 4.6.3: Internal CAL signal showing the square wave with 1khz frequency and 1ms period zoomed in for manual computation.*

</details>

## Results & Data Analysis




## Project Resources




