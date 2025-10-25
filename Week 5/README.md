# Placement and Floorplanning using OpenROAD

## Overview
OpenROAD is an open-source EDA tool that enables automated digital design implementation, including placement, floorplanning, and routing. It is widely used for VLSI design flow automation with minimal manual intervention.

## Floorplanning
Floorplanning is the process of defining the chip area and arranging the major blocks/modules within the chip. It determines the chip's **die size, aspect ratio, core area, and placement regions** for standard cells.  
Key steps in floorplanning using OpenROAD:
1. **Define die area**: Set the chip boundaries using the `set_die_area` command.
2. **Define core area**: Specify the area where standard cells will be placed.
3. **Macro placement**: Place large macros and I/O pads according to design constraints.
4. **Aspect ratio and utilization**: Set utilization target and optimize the core area for better performance and routability.

## Placement
Placement assigns the exact locations of standard cells inside the core area. Good placement improves **timing, congestion, and power**.  
In OpenROAD:
- **Global placement**: Initially places cells to optimize wirelength and reduce congestion.
- **Legalization**: Adjusts cells to legal locations without overlaps.
- **Detailed placement**: Final fine-tuning to improve timing and routability.


