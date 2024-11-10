---
title: MicroBlaze System Design
parent: Foundations
nav_order: 3
---

# Hybrid Hardware Architecture: When MCUs Meet FPGAs

Ever found yourself torn between using a microcontroller or an FPGA for your project? Well, who says you can't have both? Let's dive into how we're architecting our audio processing platform to leverage the best of both worlds.

### The Plot Twist
If you've been following along, you might be thinking: "Hold up – weren't we supposed to be using an FPGA for this project?"

You're right, but here's where it gets interesting: we're actually going to use both an FPGA and a MicroBlaze MCU. Before you raise an eyebrow, let me explain why this hybrid approach makes perfect sense.

### Why MicroBlaze?

The MicroBlaze (our chosen soft-core processor) shares many characteristics with traditional MCUs, making it perfect for handling control-heavy tasks. Think of it as the project manager of our system – great at making decisions and coordinating various components.

MicroBlaze was created by AMD specifically for use with their FPGAs and is highly optimal for this purpose.

The primary functions that our MicroBlaze will handle are:

- MIDI Parsing: Processing incoming MIDI messages, interpreting control changes, and managing timing
- User Interface Processing: Handling button presses, encoder inputs, and display updates
- Voice Management: Allocating and deallocating voices, managing priorities
- Memory Management: Coordinating access to shared resources and managing sample storage

These tasks have something in common: they involve complex decision trees and sequential operations that aren't well-suited for parallel processing. For example, MIDI parsing requires interpreting a stream of bytes according to the MIDI specification – a task that's much more straightforward in sequential code than in hardware logic.

### Enter the FPGA

While the MicroBlaze handles the control plane, our FPGA fabric will be doing the heavy lifting for audio processing. This is where parallelism really shines. We'll implement functions like:

- Digital filters
- Envelope generators
- Oscillators
- Effects processing

Each of these blocks can run independently and simultaneously, taking full advantage of the FPGA's parallel nature.

### The Bridge: AXI Interfaces
To make this hybrid architecture work, we need a robust communication mechanism between our MCU and FPGA logic. This is where AXI (Advanced eXtensible Interface) comes in. AXI is part of the ARM AMBA family of microcontroller buses but is widely used in FPGA designs.

### Hardware Acceleration: The Best of Both Worlds

This pattern of enhancing an MCU with custom hardware is known as hardware acceleration, and it's more common than you might think. You'll find similar architectures in:

Modern smartphones (main processor + neural processing unit)
Graphics cards (CPU + GPU)
Network interface cards (host CPU + network processor)

In our case, we're using this pattern to create an audio processing platform that combines the ease of programming an MCU with the processing power of custom FPGA logic.

### The Big Picture

Our architecture demonstrates a clear separation of concerns between three main subsystems:

![alt text](big_picture.drawio.png)


#### MicroBlaze MCU Domain

- Handles control-flow heavy tasks like MIDI parsing and UI management
- Uses local memory (BRAM) for caching data and instructions.
- Uses external memory for storing program and data.
- Runs at 100Mhz.

#### Hardware Accelerator Domain

- Implements parallel audio processing blocks
= Each block (filters, envelopes, oscillators) operates independently
- Can process multiple audio streams simultaneously
- Runs at 200MHz
- Optimized for specific DSP operations

#### AXI Bus Infrastructure

Acts as the communication backbone of the system.  AXI interconnect allows components in each of the domains to communicate directly with each other via AXI ports.


#### Memory Architecture

The system's memory architecture is designed to balance performance with cost-effectiveness. 

The Microcontroller (MCU) primarily relies on external memory, which offers larger capacity at a lower cost. While this external memory is slower, requiring dozens of cycles per access through the AXI interface, the MCU compensates by using a small 4kB block RAM cache to store frequently accessed instructions and data, thereby improving overall performance.

In contrast, the Audio Processing Accelerators exclusively use block RAM (BRAM). Though BRAM has limited capacity, it provides very fast access speeds (1-2 clock cycles), making it ideal for time-critical operations like lookup tables and delay lines. 

This dual approach—using external memory for bulk storage and BRAM for speed-critical components—creates an efficient balance between performance, cost, and functionality.


# Creating the MicroBlaze Hardware Platform














