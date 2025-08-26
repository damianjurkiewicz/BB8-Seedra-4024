# BB8-Seedra-4024 - TwinCAT Automation Project

## Project Overview

This is a TwinCAT 3 automation project for a 3-axis CNC-style gantry machine named "Seedra". The system is designed for automated cutting operations with integrated safety systems and motion control.

## System Architecture

### Hardware Configuration
- **Platform**: Beckhoff TwinCAT 3 (Version 3.1.4024.65)
- **Communication**: EtherCAT fieldbus
- **Motion Control**: 3-axis servo system (X, Y, Z)
- **Safety**: TwinSAFE integrated safety system

### Key Components

#### Motion Control System
- **3x EL7221-9014**: Servo motor output stages (50V, 8A RMS) with STO safety
- **Motors**: AM8122-0J10-0000 servo motors with Hiperface DSL feedback
- **Encoders**: Sick EKS36-0KF0A0S16 absolute encoders
- **Axes Configuration**:
  - **Axis X**: Horizontal movement (gantry width)
  - **Axis Y**: Horizontal movement (gantry length) 
  - **Axis Z**: Vertical movement (cutting depth)

#### I/O System
- **EL1008**: 8-channel digital input terminal
- **EL2008**: 8-channel digital output terminal  
- **EL2622**: 2-channel relay output terminal (230V AC / 30V DC)
- **EL9011**: End terminal

#### Safety System
- **EL6900**: TwinSAFE PLC logic controller
- **EL1904**: 4-channel TwinSAFE digital inputs (emergency stops)
- **EL2904**: 4-channel TwinSAFE digital outputs (safety valves)

### Software Architecture

#### Main Program Structure
```
MAIN (Program)
├── FB_Machine (PackML State Machine)
│   └── FB_GantryXYZ (Gantry Control)
│       ├── AxisX (FB_Component_BasicAxis)
│       ├── AxisY (FB_Component_BasicAxis) 
│       ├── AxisZ (FB_Component_BasicAxis)
│       └── GantryCylinder (FB_GantryXYCylinder)
└── Safety Task (TwinSAFE Logic)
```

#### Key Features

**1. PackML State Machine**
- Implements industry-standard PackML (Packaging Machine Language) states
- Supports Production, Manual, and Maintenance modes
- Comprehensive alarm management system

**2. Motion Control**
- 3-axis coordinated motion control
- Configurable cutting and rapid speeds for each axis
- Automatic homing sequences for all axes
- Position monitoring with proximity sensors

**3. Cutting Operations**
- Segment-based cutting patterns
- Support for complex cutting paths using ST_CutSegment data structures
- Automatic tool height compensation
- Spindle and cooling control integration

**4. Safety System**
- Emergency stop circuit with TwinSAFE
- Safety-rated piston monitoring for cutting mechanism
- Integrated safety permission logic
- Error acknowledgment and restart functionality

### I/O Mapping

#### Digital Inputs
- **ProximitySensorX/Y/Z**: Axis limit detection
- **PistonSensorCutX/Y**: Safety monitoring of cutting pistons
- **eStopButton**: Emergency stop input (safety-rated)

#### Digital Outputs  
- **SpindleEnable**: Spindle motor control
- **CoolingEnable**: Cutting fluid control
- **PistonValveCutX/Y**: Safety valve control for cutting pistons

### Safety Features

#### Emergency Stop System
- Hardware emergency stop button with TwinSAFE processing
- Software emergency stop via HMI
- Automatic machine abort on safety violation
- Safety permission monitoring

#### Cutting Safety
- Dual-channel piston position monitoring
- Safety-rated valve control
- Position verification before cutting operations

## Development Information

### Project Structure
```
BB8-Seedra-4024/
├── BB-Seedra-4024.tsproj          # Main TwinCAT project file
├── PLC/                           # PLC application
│   ├── POUs/                      # Program Organization Units
│   │   ├── MAIN.TcPOU            # Main program
│   │   ├── FB_Machine.TcPOU      # Machine control
│   │   ├── FB_GantryXYZ.TcPOU    # Gantry control
│   │   ├── Components/           # Component libraries
│   │   └── Segments/             # Cutting segment definitions
│   ├── GVLs/                     # Global Variable Lists
│   │   ├── IO.TcGVL             # I/O mapping
│   │   ├── GVL_Safety.TcGVL     # Safety variables
│   │   └── GVL_HMI.TcGVL        # HMI interface
│   └── DUTs/                     # Data Unit Types
├── SAFETY/                        # TwinSAFE safety application
└── BB8-DriveManager/             # Drive configuration files
```

### Key Programming Languages
- **Structured Text (ST)**: Main logic implementation
- **TwinSAFE**: Safety application programming

### Dependencies
- TwinCAT 3 Runtime
- Beckhoff Motion Control libraries
- TwinSAFE runtime system
- PackML framework libraries

## Usage

This automation system is designed for industrial cutting applications where:
- Precise 3-axis positioning is required
- Safety compliance is critical
- Integration with HMI/SCADA systems is needed
- Flexible cutting patterns must be supported

## Configuration

### Motion Parameters
Key parameters are stored in persistent variables within FB_GantryXYZ:
- Cutting speeds for each axis
- Rapid traverse speeds  
- Tool height and overpass settings
- Panel dimensions

### Safety Configuration
Safety parameters are configured within the TwinSAFE application and include:
- Emergency stop monitoring
- Safety interlock logic
- Piston position verification

## Version Information
- **TwinCAT Version**: 3.1.4024.65
- **PLC Version**: 3.1.4024.15
- **Project Created**: 2024
- **License**: MIT License

## Notes
This project represents a complete industrial automation solution incorporating modern safety standards, precise motion control, and flexible operation modes suitable for manufacturing environments.