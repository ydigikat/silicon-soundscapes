---
title: Introduction to FPGA Audio
parent: Foundations
nav_order: 1
---

# Introduction to FPGA Audio

Envisage yourself working at a modern hardware synthesiser.  You change a control and milliseconds later the instrument responds with no perceptible delay between gesture and sound, just instantaneous music expression.  This is the magic of hardware-based synthesis, and with an FPGA we can create this same magic from the ground up.

While software synthesisers running on the very powerful general purpose processors found on laptops today can equal this kind of performance, many of us who perform live are cautious about using them, an unexpected update or crash in the middle of a performance is a jarring experience for both artist and audience!

Field Programmable Gate Arrays (FPGAs) represent a sweet spot in digital audio processing that many makers overlook. 

![alt text](artix7.png)

As programmable sillicon, they offer some of the flexibility of software with the performance of dedicated hardware, making them ideal for building custom audio instruments. 

While microcontrollers and CPUs process audio samples one at a time, FPGAs can handle multiple audio streams simultaneously, with sample accurate,  precise, deterministic timing that musicians can rely on.

In this section, we'll begin our journey toward building Tullia, a fully-featured FM synthesizer implemented entirely in digital hardware. But before we dive into frequency modulation and oscillator matrices, we need to understand the fundamental concepts of audio processing on FPGAs. Why does parallel hardware matter for audio? How do we represent sound in digital logic? What architectural patterns help us build complex audio systems?

