---
layout: home
title: FPGA VGA Driver Project
tags: fpga vga verilog
categories: demo
---
Using Xilinx Vivado and a Basys 3 FPGA, this project implements a 640×480 VGA driver from a baseline colour-cycle template before adapting the pixel-generation logic to render concentric geometric arcs in real time.

## **Template VGA Design**
### **Project Set-Up**
The project is set up using Xilinx Vivado targeting the Artix-7 FPGA architecture on a Basys 3 board (xc7a35tcpg236-1), utilizing Verilog as the hardware description language and a mixed-language simulator. The design flow follows a standard FPGA development cycle: writing modular hardware description code for timing synchronization and pixel generation, simulating the waveform behavior to verify clock dividers and coordinate counters, synthesizing and implementing the netlist onto the programmable fabric, and finally programming the board to drive a live monitor via its physical VGA port.

### **Template Code**
The template project is structured around a top-level module (VGATop) that coordinates two primary subsystems: a timing generator and a pixel-generation circuit. The VGA timing generator acts as a state machine driven by a partitioned pixel clock to produce accurate horizontal and vertical synchronization pulses (hsync and vsync), alongside active video region flags that determine when the beam is actively drawing on screen. This reflects standard VGA interface protocols—such as the 640x480 @ 60Hz standard—which require precise porch intervals and sync widths to properly lock a monitor's raster scan.

Within this architecture, the template pixel generator maps the current row and column coordinates (x and y) to RGB color outputs. In the base VGColourCycle project, this logic uses coordinate counters or frame clocks to dynamically cycle pixel colors across the screen. By separating the timing control from the visual pattern logic, the design ensures clean adherence to hardware refresh rates while allowing flexible graphical generation.
### **Simulation**
The simulation process validates the timing generator and pixel pipeline prior to physical implementation by testing testbench waveforms against expected clock cycles. By inspecting the simulation waveform, you can verify that the pixel clock divider correctly scales the system clock, and that the horizontal and vertical counters reset at the proper scanline boundaries (e.g., reaching the end of a line and advancing the row). This step ensures that synchronization pulses maintain correct polarity and duration before deploying code to the hardware.
### **Synthesis**
During synthesis and implementation, Vivado translates the Verilog source files into a gate-level netlist, maps the logic onto the Basys 3's Configurable Logic Blocks (CLBs), and routes the signals while meeting timing constraints. The implementation output generates a bitstream (.bit) file, detailing resource utilization such as flip-flops, look-up tables (LUTs), and I/O buffers required to drive the external VGA pins.### **Demonstration**


## **My VGA Design Edit**
Building upon the baseline template, the custom design idea shifts from a simple color-cycling pattern to procedurally rendering a series of concentric geometric arcs—resembling a multi-layered rainbow arc. Generating shapes dynamically on an FPGA without a dedicated frame buffer requires evaluating mathematical conditions on the fly during every active pixel clock cycle. By leveraging coordinate geometry (such as the equation of a circle, $r^2 = (x - x_c)^2 + (y - y_c)^2$), this approach is well within the logic capacity of the Artix-7 FPGA, provided arithmetic operations are kept efficient to meet timing closure. For further background on procedural shape generation and rasterization techniques, check out resources like FPGA Graphics Generation Concepts.
### **Code Adaptation**
To implement this, the pixel-generation block was modified to replace the original color-cycle logic with conditional checks based on the current horizontal (x) and vertical (y) scan coordinates. Instead of cycling colors across horizontal stripes, the code calculates the squared radial distance from a designated center point on the screen. By defining distinct inner and outer radius thresholds, different color values are assigned to the RGB output buses depending on which radial band the current pixel falls into, successfully creating distinct concentric arches on the display.
### **Simulation**
Simulating the custom design involved running the testbench to verify that the coordinate counters correctly feed into the arithmetic distance equations without introducing combinatorial timing violations. Inspecting the simulation waveforms confirms that the RGB signals toggle at the precise x and y boundary conditions corresponding to the arc's geometry.(Insert simulation screenshot here)
### **Synthesis**
During synthesis and implementation, the addition of arithmetic operations—specifically squaring coordinates and comparing radial thresholds—results in a noticeably higher utilization of Lookup Tables (LUTs) and arithmetic components compared to the lightweight template design. Because the logic evaluates pixel math combinationally every clock cycle, Vivado maps these operations across the Basys 3's logic fabric while maintaining the strict timing constraints required to keep the 25 MHz pixel clock stable.
## **More Markdown Basics**
This is a paragraph. Add an empty line to start a new paragraph.

Font can be emphasised as *Italic* or **Bold**.

Code can be highlighted by using `backticks`.

Hyperlinks look like this: [GitHub Help](https://help.github.com/).

A bullet list can be rendered as follows:
- vectors
- algorithms
- iterators

Images can be added by uploading them to the repository in a /docs/assets/images folder, and then rendering using HTML via githubusercontent.com as shown in the example below.

<img src="https://raw.githubusercontent.com/melgineer/fpga-vga-verilog/main/docs/assets/images/VGAPrjSrcs.png">

## Week 1
I changed the code to test if I can generate a new image.
I learned how the new code works.
Tested the output on my display via VGA port.
<img width="4000" height="3000" alt="myWork" src="https://github.com/user-attachments/assets/12a98705-565a-4ae8-9272-a841487cb36b" />
<img width="1681" height="731" alt="Screenshot 2026-11-02 165613" src="https://github.com/user-attachments/assets/563f2966-e05d-465d-ab34-c7e275c713dc" />


## Week 2
I have come up with a new idea. I want to print out circles with rainbow colours.
The distance formula from a center point(x, y) to compute circles is: r^2 = (x-xc)^2 + (y-yc)^2.
My new idea for my project is to printout rainbow. Here we can see my attempts:
<img width="1920" height="1440" alt="35578283-97eb-4b0b-99c9-c70254fb1f89" src="https://github.com/user-attachments/assets/0d86e682-3dfa-4cdc-ab2a-ea49d487ed53" />
<img width="1920" height="1440" alt="dd8db7f5-cad7-493b-a6ab-37442b4a13dc" src="https://github.com/user-attachments/assets/c0814c44-2cd7-4895-a5d5-ec17c21a3ed4" />
<img width="1920" height="1440" alt="623890b6-547f-4050-aecd-ef4200a06a77" src="https://github.com/user-attachments/assets/2734820b-c512-4272-9fc6-8f2fcfb69d51" />
<img width="1079" height="679" alt="image" src="https://github.com/user-attachments/assets/c0f82f17-b92c-43ea-85f4-1e568922ded3" />

## Week 3
Working on my rainbow. I managed to write a code to print out arc image but haven't managed to make it in rainbow colours.
<img width="1920" height="1440" alt="d5ee75ce-edef-4474-9125-15b064577aed" src="https://github.com/user-attachments/assets/2c79f2e1-f94e-427b-ad90-88a1aee3c967" />
My attempt to print out rainbow colours 
<img width="4000" height="3000" alt="1000012885" src="https://github.com/user-attachments/assets/a18cb10f-6f21-4b9f-8dff-2b0a3074fe0a" />




