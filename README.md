# Description

The OpenCores aoOCS SoC is a Wishbone compatible implementation of most of the Amiga Original Chip Set (OCS) and computer functionality.
aoOCS is not related in any way with Minimig - it is a new and independent Amiga OCS implementation.

## Features

* The aoOCS SoC contains the following Amiga/OCS components:
    - blitter
    - copper
    - system control (interrupts)
    - video: bitplains, sprites, collision detection
    - audio: 4 channels, low-pass filter
    - user input: PS/2 mouse, PS/2 keyboard and joystick (keyboard arrow keys)
    - floppy: read and write ADF files directly from a SD card. Only the internal floppy drive is implemented
    - 8520 CIA
    - ao68000 OpenCores IP core is used as the aoOCS processor
* All of the above components are WISHBONE revision B.3 compatible
* The aoOCS contains the following additional components:
** SD card controller written in HDL with DMA. Supports SDHC cards only.
** 10/100 Mbit Ethernet controller written in HDL to send the current VGA frames (frame grabber)
** HDL drivers for SSRAM, PS/2 keyboard, PS/2 mouse, audio codec, VGA DAC
* aoOCS uses only one external memory: a SSRAM with 36-bit words and pipelined access. A video buffer with about 250KB is located SSRAM. Another 256KB are used by the ROM. All the rest memory can be used as Chip RAM.
* The On-Screen-Display is implemented in HDL as a finite state machine. No additional controller/processor with firmware required to handle the SoC.
* The following options are available on the On-Screen-Display:
** select ROM file to load (only Amiga Kickstart v1.2 was tested)
** enable or disable Joystick (keyboard arrow keys)
** enable or disable floppy write protection
** insert a floppy - select one from a list
** eject an inserted floppy
** reset the system
* The On-Screen-Display is independent of the running Amiga software. It is enabled and disabled by the Home key and controled by the keyboard arrow keys and the right CTRL key.
* Only PAL timings are implemented.
* The video output is VGA compatible: 640x480 at 70 Hz. A rather simple method is used to extend the 256 PAL horizontal lines to 480 VGA lines: all lines are doubled except for every 8th one.
* The system uses generally a single clock: 30 MHz. There are two more clocks: 12 MHz, 25 MHz generated to interface with external hardware (Audio codec, Ethernet controller). A single altpll is used to generate all three clocks from one 50 MHz external clock. More information about clocks is available at Clocks.
* A VGA frame grabber is implemented that sends captured frames by 100 Mbit Ethernet in IP/UDP packets.
* The system uses about 26.400 LE on Altera Cyclone II and about 267.000 bits of on-chip RAM.
* The blitter functionality was tested against the E-UAE Amiga software emulator.
* Tested only on a Terasic DE2-70 board (www.terasic.com.tw).
* Documentation generated by Doxygen (www.doxygen.org) with doxverilog patch (http://developer.berlios.de/projects/doxverilog/). The specification is automatically extracted from the Doxygen HTML output.

## WISHBONE compatibility

* Version: WISHBONE specification Revision B.3,
* General description: 32-bit WISHBONE interface,
* WISHBONE data port size: 32-bit,
* Data port granularity: 8-bits,
* Data port maximum operand size: 32-bits,
* Data transfer ordering: BIG ENDIAN,
* Data transfer sequencing: UNDEFINED,
* Constraints on CLK_I signal: described in Clocks.

## Similar projects
Other Open-Source Amiga implementations include:

* Minimig (http://code.google.com/p/minimig/) - FPGA-based re-implementation of the original Amiga 500 hardware. Runs on the Minimig PCB and also on Terasic DE1,2 boards.

## Limitations

* No filesystem support on the SD card. Data is read from fixed positions. The contents of the SD card is generated by the aoOCS_tool described at Operation.
* No video external synchronize, lace mode, lightpen, genlock audio enable, color composite (BPLCON0)
* All bitplain data is fetched at once in a burst memory read at the begining of each line. No changes to the bitplain data done after the beginning of a line are visible.
* Currently aoOCS requires an 36-bit word SSRAM to store the video buffer. This way 3 pixels 12-bits each can be stored in one word.
* Serial port not implemented.
* Parallel port not implemented.
* Low-pass filter disable/enable by CIA-A port A bit 1 not implemented.
* Proportional controller and light pen not implemented.
* Some rarely used OCS registers are not implemented: strobe video sync, write beam position, coprocessor instruction fetch identify. For a complete list of not implemented registers look at Registers.
* Only some of the Amiga software was tested and works on the aoOCS. A list of aoOCS software compatability is located at Operation.

## TODO

* Fix some of the above limitations.
* Optimize the design.
* Run WISHBONE verification models.
* More documentation of Verilog sources.
* Describe testing and changes done in E-UAE sources.
* Prepare scripts for VATS: run_sim -r -> regresion test.
* Port the aoOCS SoC to a Xilinx FPGA.

## Status

* Amiga Workbench v1.2 runs with some minor graphic problems: bottom of screen not displayed correctly.
* Prince of Persia runs perfectly.
* Wings of Fury runs correctly. Some sound glitches in intro.
* Lotus 2 runs correctly. Some sound problems in intro.
* Warzone runs poor. Some major graphic problems.
* More information about aoOCS software compatability is available at Operation.

## Requirements

* Altera Quartus II synthesis tool (http://www.altera.com) is required to synthesise the aoOCS System-on-Chip.
* Java SDK (http://java.sun.com) is required to compile the aoOCS_tool (The tool is described in Operation).
* A FPGA board. Currently only the Terasic DE2-70 board was tested.
* Icarus Verilog simulator (http://www.icarus.com/eda/verilog/) is required to compile the and run some tests.
* Access to Altera Quartus II directory (directory eda/sim_lib/) is required to compile and run some tests.
* GCC (http://gcc.gnu.org) is required to compile some testes based on E-UAE sources.

## Structure diagram
![Structure diagram](http://github.com/alfikpl/aoOCS/raw/master/doc/img/structure.png)

## Screenshots
### Amiga Kickstart v1.2 bootstrap screen with aoOCS On-Screen-Display
![Amiga Kickstart v1.2 bootstrap screen with aoOCS On-Screen-Display](http://github.com/alfikpl/aoOCS/raw/master/doc/img/vga_menu_no_floppy.png)
### Amiga Workbench v1.2 screen
![Amiga Workbench v1.2 screen](http://github.com/alfikpl/aoOCS/raw/master/doc/img/vga_wb12.png)
### Wings of Fury
![Wings of Fury](http://github.com/alfikpl/aoOCS/raw/master/doc/img/vga_wings_of_fury.png)
