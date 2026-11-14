<div align="center" style="padding-bottom: 80px;">
<img align="right" alt="dimap logo" height="60" src="docs/assets/dimap.png">
<img align="left" alt="ufrn logo" height="60" src="docs/assets/ufrn-logo.png">
</div>

<div align="center">

# 🏢 Elevator Control System in VHDL

</div>

## 📝 About the project

A complete system to control **three elevators** in a **32-floor building (floors 0 to 31)**, implemented in **VHDL**. The project uses a two-level hierarchical architecture with Finite State Machines (FSMs) for robust and efficient management of multiple elevators.

Developed as the final project for the **Digital Circuits** course (UFRN), demonstrating advanced concepts such as:
- Multi-level hierarchical architecture
- Finite State Machines (FSM)
- Scheduling algorithms
- Real-time control systems
- Modular design in VHDL
- Simulation and validation with testbenches

**Professor:** Márcio Eduardo Kreutz  
**Date:** October 2025

> 📄 For more details, see the [full report](docs/report/Trabalho_Circuitos.pdf) (in Portuguese).

## 👥 Contributors

- [Luisa Ferreira](https://github.com/luisaferreirass)
- [Kauã do Vale Ferreira](https://github.com/DevlTz)
- [Cícero Paulino de Oliveira Filho](https://github.com/ciceropaulino)
- [Ryan David dos Santos Silvestre](https://github.com/RyanSilv257)

## 🎯 Goals

The main goal is to develop a robust and efficient system to control the operation of multiple elevators. The architecture is strictly divided to ensure modularity and clarity:

1. **Level 1 (Local Controller - 3x):** Manages the physical operations of each individual elevator (motor, doors, floor register)
2. **Level 2 (Scheduler/Supervisor - 1x):** Manages all external calls, decides which of the three elevators should serve each request and dispatches commands to the local controllers

The whole project is validated through simulation with VHDL testbenches.

## 🏗️ Overall Architecture

The architecture follows a two-level hierarchical control model:

![Block diagram of the three-elevator control system](docs/diagramas_projeto/diagrama2710.png)

### Main Modules

| Module | Role | Main Logic |
|--------|--------|------------------|
| **Scheduler (Supervisor)** | Manages external calls, applies the scheduling algorithm and dispatches requests to the elevators | Selection logic and high-level FSM |
| **Local Controller (3x)** | Coordinates the operation of each elevator (motor and door). Receives internal calls and requests from the Scheduler | Main FSM with states: `IDLE`, `MOVENDO` (moving), `ABRINDO_PORTA` (opening door), `PORTA_ABERTA` (door open), `FECHANDO_PORTA` (closing door) |
| **Components (Motor, Door)** | "Executor" components that follow the Local Controllers' commands | FSM for the Motor (`PARADO` stopped, `SUBINDO` going up, `DESCENDO` going down, `FREANDO` braking) and sequential logic for the Door |

## 🔌 Signal Interface (Top-Level)

The top-level module is the connection point for all blocks in the system.

### Global Inputs (from the Testbench)

- `clk`, `rst`: Global clock and reset
- `chamadas_externas_subir [0..30]`: Up call buttons on each floor
- `chamadas_externas_descer [1..31]`: Down call buttons on each floor
- `chamadas_internas_E1, E2, E3 [0..31]`: Internal panels of each elevator
- `sensor_andar_E1, E2, E3 [0..31]`: Position sensors (simulated)

### Global Outputs (to Displays/Testbench)

- `display_andar_E1, E2, E3`: Current floor indicator
- `display_porta_E1, E2, E3`: Door indicator (open or closed)
- `display_direcao_E1, E2, E3`: Movement indicator (going up or down)

## 📊 Scheduling Strategy

The project primarily uses the **Scan (elevator sweep)** method.

### Scan Method

The elevator travels through all floors in a single direction (up or down), serving every request along the way. The direction is only reversed when it reaches the building limits (ground floor or top floor) or the last pending request in that direction.

**Advantages:**
- Simple and clear implementation
- Predictable behavior
- Well suited to a structural VHDL implementation

### Alternatives Considered

The following approaches were evaluated:

- **Basic Scan** (chosen): Simple to implement
- **Proximity and Precedence**: Although more efficient in terms of waiting time, it was considered too complex for the project's structural VHDL implementation

## ⚙️ System Parameters

| Parameter | Value | Notes |
|-----------|-------|-------|
| **Number of Floors** | 32 (0 to 31) | Ground floor to top floor |
| **Number of Elevators** | 3 | Simultaneous operation |
| **Door Timer** | 250,000,000 cycles | Equivalent to 5 seconds with a 50MHz clock |
| **System Clock** | 50 MHz | Default frequency |
| **Operation Times** | Variable | Defined in the Motor and Door modules |

## 🎮 State Machines

### Local Controller (Elevator)
```
IDLE → MOVENDO → FREANDO → ABRINDO_PORTA → PORTA_ABERTA → FECHANDO_PORTA → IDLE
```

### Motor
```
PARADO ↔ SUBINDO
       ↔ DESCENDO
       ↔ FREANDO
```

## 🧪 Simulation and Testing

The project includes complete testbenches to validate every module and operating scenario.

### How to Run the Simulations
```bash
# Clone the repository
git clone https://github.com/DevlTz/Logic_Circuits_Elevator.git
cd Logic_Circuits_Elevator

# Make the script executable
chmod u+x testbench.sh

# Run the simulation script
./testbench.sh
```

### Test Scenarios

- ✅ Simultaneous external calls
- ✅ Internal calls from multiple elevators
- ✅ Edge cases (ground floor and top floor)
- ✅ Timed door operation
- ✅ Scheduling with multiple requests
- ✅ System reset and initialization


## 🚀 Implemented Features

- ✅ **Control of 3 independent elevators**
- ✅ **32 floors (0-31)**
- ✅ **Scan scheduling algorithm**
- ✅ **External calls (up/down)**
- ✅ **Internal calls (panels)**
- ✅ **Automatic door control**
- ✅ **Floor sensors**
- ✅ **Status displays (floor, door, direction)**
- ✅ **Modular hierarchical architecture**
- ✅ **Full validation via testbench**

## 🛠️ Tools Used

- **VHDL** - Hardware description language
- **ModelSim/GHDL** - VHDL simulator
- **GTKWave** - Waveform viewer (optional)

## 📚 Concepts Applied

- Finite State Machines (FSM)
- Hierarchical architecture
- Process synchronization
- Scheduling algorithms
- Real-time control
- Modular design
- Validation through simulation

## 🤝 Contributing

This is an academic project. For suggestions or improvements, please contact the contributors.

## 📄 License

This project was developed for educational purposes as part of the Digital Circuits course (UFRN).

## 👨‍💻 Authors

Developed by [Luisa Ferreira](https://github.com/luisaferreirass), [Kauã do Vale](https://github.com/DevlTz), [Cícero Paulino](https://github.com/ciceropaulino) and [Ryan Silvestre](https://github.com/RyanSilv257)
