# Custom ESP32 Audio Processing Shield
## 1.) Project Overview
### 1.1) Description
This project aims to design, fabricate, and validate a high-fidelity, mixed-signal hardware-in-the-loop (HIL) audio processing board utilizing the ESP32-S3 microcontroller. The system will serve as a high-performance, real-time digital signal processing (DSP) platform capable of executing custom 3-band parametric equalization via optimized, native firmware architectures on an custom 4-layer printed circuit board (PCB).

### 1.2) Objectives
* Mathematically model and Simulate a 3-band parametric equalizer using Direct Form I IIR biquad filters in Python, a 512-sample Radix-4 FFT pipeline simulation and a 2nd-order Sallen-Key low-pass anti-aliasing filter.
  
* Design and fabrication of an EMI-optimized 4-layer PCB utilizing an integrated power tree and an explicit single-point star ground layout.
  
* A stable ESP-IDF codebase featuring register-level I2C configuration drivers for the ES8311 codec, a 4-buffer ping-pong DMA engine running at 44.1 kHz/16-bit, and strict FreeRTOS task isolation (DSP math pinned to CPU Core 1; system operations isolated to CPU Core 0).
  
* A fully operational Python-based pyserial host validation engine that executes swept-frequency test sequences, captures physical output data, and automatically renders empirical Bode plots to verify design targets.

### 1.3) Key Features
* Dynamic Audio Path Routing: Features a dual-pathway input topology leveraging a physical SPDT slide switch or a CD4051 analog multiplexer to route low-voltage audio interchangeables between an external ES8311 I2S Codec and the internal 12-bit SAR ADC of the ESP32.

* Non-Intrusive Hardware Debugging: Includes inline 2-pin current shunt headers on both sub-power networks for real-time power draw profiling, alongside an unpopulated 6-pin diagnostic header array breaking out critical I2S and I2C lines.

* Maximum Signal Integrity: Achieves optimal electrical isolation by physically splitting the PCB layout into a high-frequency digital left quadrant (MCU, USB-UART, Buck switcher) and a low-noise analog right quadrant (Codec, active op-amps, audio I/O).

* Strict Return-Path Routing: Restricts high-speed digital audio lines directly over a continuous, unbroken Layer 2 solid ground plane while strictly prohibiting any analog traces from crossing digital return paths.

## 2.) Requirements
### 2.1) Hardware
* Microcontroller: ESP32-S3

* Low Power Mono Audio CODEC: ES8311

* Analog Mutiplexer: CD4051

* USB-TO-UART Bridge: CH343C

* Synchronous Buck Regulator: AP63203

* Low Dropout Regulator (LDO): AP2112K

* Bipolar Junction Transistors: S8050

* Operational Amplifiers

* Resistors & Capacitors

* Inline Pins

* 3.5 mm Audio Jacks

* Multimeter

* Oscilloscope

* Logic Analyzer

* Function Generator

### 2.2) Software & Tools
* Simulations: MATLAB, LTSpice
* Development: KiCAD, Native ESP-IDF (VSCode)
* Programming: C, Python, MATLAB
* RTOS: FreeRTOS

### 2.3) Constraints & Challenges
* Finite Word-Length Effects & Quantization Noise
* Statistical Sensitivity vs. Component Tolerances
* Mixed-Signal Isolation on a High-Density PCB
* Real-time audio processing requires balancing latency against processing safety.
* CPU Core Inter-Dependency and Priority Inversion

## 3.) Development Log (Ongoing Update)

Rough Work:

26/05/26 7:50 PM- I studied how to work with matplotlib and scipy.signal, will read more on how to use it for butterworth and IIR filters, to implement the final IIR filter.

27/05/26 2:02 AM- I studied more about how to work with scipy.signal amd learned about signal resampling, spectral analysis using FFTs, Savitzky-Golay Filtering, and Wavelet Analysis. Spectral Analysis should particularly help us in understanding leakages, and perhaps I could work with resampling to figure out an improvement in audio processing accuracy, but that may not help also. I also read a bit about IIR Filter and Real-Time Processing implementation in the IIR Filter article, and a bit about Butterworth response. I will do more work, and I shall have a complete design of a 3-band parametric equalizer by the end of this week, alongside the Radix-4 FFT Pipeline and Anti-Aliasing Filter implemented in LTSpice (or other relevant software).

28/05/26 1:50 PM - Begun finalizing paramteric coefficients for IIR Biquad Filter, will make the final IIR filter very soon.

### 3.1) Resources:

 Date        | Links           | Notes  |
| ------------- |:-------------:| -----:|
|    26/05/26         | https://pythonguides.com/scipy-signal/ | Learnt the usage and basics of scipy.signal |
|   26/05/26  |  https://pythonguides.com/python-scipy-iir-filter/   | Learnt the implementation of IIR filters in scipy.signal   |
| 26/05/26 |  https://pythonguides.com/python-scipy-butterworth-filter/     |  Learnt the implementation of Buterworth Response   |
| 26/05/26  | https://webaudio.github.io/Audio-EQ-Cookbook/audio-eq-cookbook.html      | IIR Biquad Filter Design for Equalizer   |



### 3.2) Key Decisions & Changes:

### 3.3) Issues & Solutions:

For the 3-Band Paramteric Equalizer code in python, the code does not run the graph plotting required. The code taken from the website does not have plt.show(), which I have added to get the final plot. 

## 4.) Development Process

### Week 1: ALGORITHM MODELING & SIMULATION

Algorithm modeling and simulations are very important to ensure that our design decisions are feasible and will give us our desired results. For this project, we must first model a 3-Band Parametric Equalizer using Direct Form I  IIR Biquad Filters.

IIR Filters are a type of 
