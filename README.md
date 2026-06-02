# EMG Real-Time Digital Filtering (STM32 / Arduino)

This project implements a real-time digital filtering pipeline for EMG (Electromyography) signals using embedded C++.

It is designed for STM32 / Arduino microcontrollers and focuses on biomedical signal conditioning in real time.

---

## Features

- Real-time EMG processing
- Embedded C++ implementation (no external DSP library required)
- Configurable filter chain
- Lightweight and suitable for microcontrollers
- Biquad IIR filtering implementation

---

## Filter Configuration

Typical EMG parameters:

- Sample rate: 1000 Hz  
- High-pass: 20 Hz (removes motion artifacts and DC drift)  
- Notch: 50 Hz (power line interference removal)  
- Low-pass: 150 Hz (limits EMG bandwidth)  
- Envelope: 5 Hz (muscle activation smoothing)

---

## Signal Processing Chain

Raw EMG signal is processed using the following stages:

High-pass filter → Notch filter → Low-pass filter → Rectification → Envelope low-pass filter

---

## Algorithm

Each filter stage is implemented as a second-order IIR biquad:

y[n] = b0*x[n] + b1*x[n-1] + b2*x[n-2] - a1*y[n-1] - a2*y[n-2]

The notch filter is implemented using two cascaded biquads around the target frequency to improve attenuation.

---

## Files

EMG_filtering_STM32.h  
EMG_filtering_STM32.cpp  

---

## Usage Example

#include "EMG_filtering_STM32.h"

EMGFilter emg;

void setup() {
    emg.init(1000, 50, 150, 20, 5);
}

void loop() {
    float raw = analogRead(A0);
    float filtered = emg.update(raw);
}

---

## Performance

- Designed for real-time embedded execution
- Floating-point implementation
- No external dependencies
- Can be optimized using CMSIS-DSP (ARM Cortex-M)

---

## License (MIT)

MIT License

Copyright (c) 2026

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
