# RAM Verification using Class-Based Environment and UVM

**GitHub Repository:** [manarabdoali/RAM-Verification-using-class-based-env-and-UVM](https://github.com/manarabdoali/RAM-Verification-using-class-based-env-and-UVM)

![SystemVerilog](https://img.shields.io/badge/SystemVerilog-Verification-blue)
![UVM](https://img.shields.io/badge/UVM-1.1d-green)
![License](https://img.shields.io/badge/License-Educational-yellow)

## Table of Contents
- [Project Overview](#project-overview)
- [Project Structure](#project-structure)
- [Architecture](#architecture)
  - [Class-Based Environment](#class-based-environment)
  - [UVM Environment](#uvm-environment)
- [Components](#components)
- [Design Under Test (DUT)](#design-under-test-dut)
- [Verification Plan](#verification-plan)
- [Test Cases](#test-cases)
- [Coverage](#coverage)
- [Getting Started](#getting-started)
- [Running Simulations](#running-simulations)
- [Results](#results)
- [Key Features](#key-features)
- [Learning Objectives](#learning-objectives)
- [Contributing](#contributing)
- [Contact](#contact)

## Project Overview

**Repository:** [https://github.com/manarabdoali/RAM-Verification-using-class-based-env-and-UVM](https://github.com/manarabdoali/RAM-Verification-using-class-based-env-and-UVM)

This project focuses on the comprehensive verification of a Single Port RAM module using two different verification methodologies:

1. **Class-Based Verification Environment** using SystemVerilog
2. **Universal Verification Methodology (UVM)** Environment

The primary objective is to ensure the functional correctness of the RAM design by generating and validating various test scenarios through structured verification environments.

### Key Highlights

- ✅ **Dual Verification Approaches** - Compare class-based and UVM methodologies
- ✅ **Modular Architecture** - Reusable and scalable components
- ✅ **Comprehensive Testing** - Multiple test scenarios covering all functionality
- ✅ **Self-Checking** - Automated verification with scoreboard
- ✅ **Coverage-Driven** - Functional and code coverage tracking
- ✅ **Industry-Standard** - Follows UVM best practices

## Project Structure

```
RAM-Verification-using-class-based-env-and-UVM/
├── Class_Based_Environment/
│   ├── ram.v                    # RAM RTL design
│   ├── interface.sv             # Interface definition
│   ├── transaction.sv           # Transaction class
│   ├── generator.sv             # Stimulus generator
│   ├── driver.sv                # Driver class
│   ├── monitor.sv               # Monitor class
│   ├── scoreboard.sv            # Scoreboard class
│   ├── subscriber.sv            # Coverage subscriber
│   ├── environment.sv           # Environment class
│   └── test.sv                  # Top-level test
│
├── UVM_Environment/
│   ├── ram.v                    # RAM RTL design
│   ├── ram_interface.sv         # UVM interface
│   ├── ram_seq_item.sv          # Sequence item
│   ├── ram_sequence.sv          # Sequence
│   ├── ram_sequencer.sv         # Sequencer
│   ├── ram_driver.sv            # UVM driver
│   ├── ram_monitor.sv           # UVM monitor
│   ├── ram_agent.sv             # UVM agent
│   ├── ram_scoreboard.sv        # UVM scoreboard
│   ├── ram_subscriber.sv        # UVM subscriber
│   ├── ram_env.sv               # UVM environment
│   ├── ram_test.sv              # UVM test
│   └── top.sv                   # Top module
│
└── README.md                    # This file
```

## Architecture

### Class-Based Environment

The class-based verification environment consists of the following components organized in a modular hierarchy:

```
┌─────────────────────────────────────────────────────────┐
│                       Top Module                         │
│  ┌───────────┐  ┌──────────┐  ┌──────────────────────┐ │
│  │   Clock   │  │  Reset   │  │     Interface        │ │
│  └───────────┘  └──────────┘  └──────────────────────┘ │
└─────────────────────────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│                    Environment                           │
│  ┌──────────────────────────────────────────────────┐  │
│  │               Virtual Interface                   │  │
│  └──────────────────────────────────────────────────┘  │
│                         │                                │
│  ┌──────────┐  ┌───────┴────────┐  ┌───────────────┐  │
│  │Generator │→ │     Driver     │→ │      DUT      │  │
│  └──────────┘  └────────────────┘  └───────┬───────┘  │
│        │                                    │           │
│        │        ┌───────────────────────────┘           │
│        │        │                                       │
│        │        ▼                                       │
│        │  ┌──────────┐                                 │
│        │  │ Monitor  │                                 │
│        │  └────┬─────┘                                 │
│        │       │                                       │
│        │       ├──────────┐                            │
│        │       │          │                            │
│        │       ▼          ▼                            │
│        │  ┌──────────┐  ┌────────────┐                │
│        └→ │Scoreboard│  │ Subscriber │                │
│           └──────────┘  └────────────┘                │
│                             (Coverage)                  │
└─────────────────────────────────────────────────────────┘
```

### UVM Environment

The UVM-based verification environment follows the standard UVM testbench architecture:

```
┌─────────────────────────────────────────────────────────┐
│                       UVM Test                           │
│                    (Top Level)                           │
└────────────────────┬─────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────┐
│                  UVM Environment                         │
│  ┌────────────────────────────────────────────────┐    │
│  │                 UVM Agent                       │    │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐    │    │
│  │  │Sequencer │→ │  Driver  │→ │ Monitor  │    │    │
│  │  └──────────┘  └──────────┘  └────┬─────┘    │    │
│  │       ▲                            │          │    │
│  │       │                            │          │    │
│  │  ┌────┴─────┐                     │          │    │
│  │  │ Sequence │                     │          │    │
│  │  └──────────┘                     │          │    │
│  └────────────────────────────────────┼──────────┘    │
│                                       │                │
│  ┌────────────────┐                  │                │
│  │  Scoreboard    │←─────────────────┤                │
│  └────────────────┘                  │                │
│                                       │                │
│  ┌────────────────┐                  │                │
│  │  Subscriber    │←─────────────────┘                │
│  │  (Coverage)    │                                    │
│  └────────────────┘                                    │
└─────────────────────────────────────────────────────────┘
```

## Components

### 1. Interface
Encapsulates all input and output signals for the DUT:
- **Data Input** (`data_in`)
- **Address** (`addr`)
- **Write Enable** (`wr_en`)
- **Read Enable** (`rd_en`)
- **Reset** (`rst`)
- **Data Output** (`data_out`)

### 2. Transaction Class
Defines the data packet structure containing:
- Address
- Data (input/output)
- Control signals (read/write enable, reset)
- Randomization constraints

### 3. Generator/Sequencer
- Generates random stimulus transactions
- Manages transaction flow
- Implements constrained randomization for various test scenarios

### 4. Driver
- Receives transactions from generator/sequencer
- Drives signals to the DUT through the virtual interface
- Converts transaction-level data to pin-level signals

### 5. Monitor
- Observes DUT signals
- Captures transactions from the interface
- Broadcasts transactions to scoreboard and subscriber

### 6. Scoreboard
- Receives transactions from the monitor
- Implements reference model for expected behavior
- Compares actual outputs with expected outputs
- Reports pass/fail status

### 7. Subscriber
- Collects functional coverage data
- Implements covergroups and coverpoints
- Tracks coverage metrics

### 8. Environment
- Instantiates and connects all verification components
- Manages the overall testbench infrastructure
- Provides configuration and control

### 9. Test
- Top-level component that starts verification
- Configures the environment
- Initiates stimulus sequences
- Controls simulation flow

## Design Under Test (DUT)

### Single Port RAM Specifications
- **Memory Size**: Configurable (typically 16 locations)
- **Data Width**: 16 bits
- **Address Width**: 4 bits (for 16 locations)
- **Operations**: 
  - Synchronous write
  - Synchronous read
  - Synchronous reset

### Port Description

| Signal | Direction | Width | Description |
|--------|-----------|-------|-------------|
| clk | Input | 1 | Clock signal |
| rst | Input | 1 | Synchronous reset (active high) |
| wr_en | Input | 1 | Write enable |
| rd_en | Input | 1 | Read enable |
| addr | Input | 4 | Address bus |
| data_in | Input | 16 | Data input |
| data_out | Output | 16 | Data output |

## Verification Plan

### Objectives
1. Verify basic read/write functionality
2. Test boundary conditions
3. Validate reset behavior
4. Check simultaneous read/write operations
5. Verify data retention
6. Test all memory locations
7. Validate error conditions

### Verification Strategy
- Random stimulus generation with constraints
- Directed tests for corner cases
- Functional coverage tracking
- Code coverage analysis
- Assertion-based verification

## Test Cases

### Test Case 1: Reset Verification
**Objective**: Verify that all memory locations are cleared after reset

**Procedure**:
1. Assert reset signal
2. Read from all memory locations
3. Verify all locations contain 0x0000

**Expected Result**: All locations should read as zero

---

### Test Case 2: Write-Read All Locations
**Objective**: Verify write and read operations for all addresses

**Procedure**:
1. Write value 0xFAF5 to all memory locations
2. Read from all memory locations
3. Compare read data with written data

**Expected Result**: All read values should match written value (0xFAF5)

---

### Test Case 3: Boundary Address Test (Address 0)
**Objective**: Test boundary condition at lowest address

**Procedure**:
1. Write specific value to address 0
2. Read from address 0
3. Verify data integrity

**Expected Result**: Read data should match written data

---

### Test Case 4: Boundary Address Test (Maximum Address)
**Objective**: Test boundary condition at highest address

**Procedure**:
1. Write specific value to maximum address (e.g., 15 for 4-bit address)
2. Read from maximum address
3. Verify data integrity

**Expected Result**: Read data should match written data

---

### Test Case 5: Random Access Pattern
**Objective**: Verify random read/write operations

**Procedure**:
1. Generate random addresses and data
2. Perform random write operations
3. Perform random read operations
4. Verify data consistency

**Expected Result**: All read data should match previously written data

---

### Test Case 6: Data Retention
**Objective**: Verify data is retained after write operation

**Procedure**:
1. Write value to a specific address
2. Wait for multiple clock cycles
3. Read from the same address
4. Verify data hasn't changed

**Expected Result**: Data should remain unchanged

## Coverage

### Functional Coverage Points

1. **Address Coverage**
   - All valid addresses accessed
   - Boundary addresses (0 and max)
   - Sequential address patterns
   - Random address patterns

2. **Data Coverage**
   - All zeros (0x0000)
   - All ones (0xFFFF)
   - Random data patterns
   - Specific test patterns

3. **Operation Coverage**
   - Write-only operations
   - Read-only operations
   - Back-to-back writes
   - Back-to-back reads
   - Write followed by read
   - Reset operation

4. **Cross Coverage**
   - Address × Operation type
   - Data patterns × Address
   - Operation sequences

### Code Coverage
- **Line Coverage**: Track execution of all code lines
- **Branch Coverage**: Cover all conditional branches
- **Toggle Coverage**: Ensure all signals toggle
- **FSM Coverage**: Cover all state transitions (if applicable)

## Getting Started

### Prerequisites
- **Simulator**: QuestaSim/ModelSim or VCS
- **SystemVerilog Support**: Required for class-based environment
- **UVM Library**: Version 1.1d or later
- **Make**: For running compilation scripts (optional)

### Installation

1. Clone the repository:
```bash
git clone https://github.com/manarabdoali/RAM-Verification-using-class-based-env-and-UVM.git
cd RAM-Verification-using-class-based-env-and-UVM
```

2. Set up UVM library path (if using UVM environment):
```bash
export UVM_HOME=/path/to/uvm-1.1d
```

## Running Simulations

### Class-Based Environment

#### Compilation
```bash
vlog +incdir+./Class_Based_Environment \
     ./Class_Based_Environment/ram.v \
     ./Class_Based_Environment/*.sv
```

#### Simulation
```bash
vsim -voptargs=+acc work.test -do "run -all"
```

#### With GUI
```bash
vsim -gui work.test
# In GUI: run -all
```

---

### UVM Environment

#### Compilation
```bash
vlog -coveropt 3 +cover \
     +incdir+$UVM_HOME/src \
     +incdir+./UVM_Environment \
     $UVM_HOME/src/uvm.sv \
     ./UVM_Environment/ram.v \
     ./UVM_Environment/*.sv
```

#### Simulation
```bash
vsim -coverage -voptargs=+acc work.top \
     +UVM_TESTNAME=ram_test \
     +UVM_VERBOSITY=UVM_MEDIUM \
     -do "run -all"
```

#### With Different Verbosity Levels
```bash
# Low verbosity
vsim -coverage work.top +UVM_TESTNAME=ram_test +UVM_VERBOSITY=UVM_LOW -do "run -all"

# High verbosity (for debugging)
vsim -coverage work.top +UVM_TESTNAME=ram_test +UVM_VERBOSITY=UVM_HIGH -do "run -all"
```

---

### Makefile Usage

If a Makefile is provided:

```bash
# For Class-Based Environment
make class_based

# For UVM Environment
make uvm

# Run with coverage
make coverage

# Clean build files
make clean

# View waveforms
make wave
```

## Results

### Expected Outputs

#### Successful Simulation
```
# ========================================
# RAM VERIFICATION TEST RESULTS
# ========================================
# 
# Test Case 1: PASSED - Reset verification successful
# Test Case 2: PASSED - All locations write/read successful
# Test Case 3: PASSED - Boundary address 0 test passed
# Test Case 4: PASSED - Boundary address 15 test passed
# Test Case 5: PASSED - Random access pattern verified
# Test Case 6: PASSED - Data retention verified
# 
# ========================================
# COVERAGE SUMMARY
# ========================================
# Functional Coverage: 95.5%
# Code Coverage: 98.2%
# - Line Coverage: 100%
# - Branch Coverage: 96.4%
# - Toggle Coverage: 98.2%
# 
# ========================================
# SIMULATION PASSED
# All tests completed successfully!
# ========================================
```

### Coverage Reports

Coverage reports are generated in:
- **HTML Format**: `coverage_report/index.html` - For easy viewing in web browser
- **Text Format**: `coverage.txt` - For quick command-line review
- **UCDB Format**: `coverage.ucdb` - For detailed analysis in QuestaSim

To view HTML coverage report:
```bash
firefox coverage_report/index.html
# or
google-chrome coverage_report/index.html
```

### Waveform Analysis

View signal waveforms for debugging:
```bash
# If waveform was saved during simulation
vsim -view vsim.wdb
```

To save waveforms during simulation:
```bash
vsim work.test -do "log -r /*; run -all"
```

## Key Features

| Feature | Class-Based | UVM |
|---------|-------------|-----|
| Modular Components | ✅ | ✅ |
| Randomization | ✅ | ✅ |
| Functional Coverage | ✅ | ✅ |
| Scoreboard | ✅ | ✅ |
| Reusability | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Industry Standard | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| Learning Curve | Easy | Moderate |
| Configuration | Manual | UVM Config DB |
| Reporting | Custom | UVM Reporter |

### Advantages

**Class-Based Environment:**
- ✅ Simpler to understand for beginners
- ✅ Direct control over all components
- ✅ Faster to set up for small projects
- ✅ Good for learning OOP concepts

**UVM Environment:**
- ✅ Industry-standard methodology
- ✅ Highly reusable and scalable
- ✅ Built-in reporting and messaging
- ✅ Configuration database for flexibility
- ✅ Better for complex designs
- ✅ Recognized by industry professionals

## Learning Objectives

This project demonstrates:

### SystemVerilog Concepts
- Object-Oriented Programming (classes, inheritance, polymorphism)
- Interfaces and virtual interfaces
- Randomization and constraints
- Mailboxes and inter-process communication
- Events and synchronization
- Covergroups and functional coverage

### UVM Methodology
- UVM component hierarchy
- Transaction-level modeling (TLM)
- Sequence and sequencer architecture
- UVM factory and configuration database
- UVM reporting mechanism
- UVM phases (build, connect, run, etc.)
- Agent-based testbench architecture

### Verification Techniques
- Test planning and test case development
- Self-checking testbenches
- Reference model implementation
- Coverage-driven verification
- Assertion-based verification
- Debugging and result analysis

## Project Evolution

This project can be extended with:

1. **Additional Features**
   - Dual-port RAM verification
   - Cache memory verification
   - Error injection and checking
   - Protocol checkers

2. **Advanced Techniques**
   - Constrained random testing with complex constraints
   - Layered sequences
   - Virtual sequences for complex scenarios
   - Register abstraction layer (RAL)

3. **Automation**
   - Regression testing scripts
   - Automated coverage analysis
   - CI/CD integration
   - Automated report generation

## Troubleshooting

### Common Issues

**Issue**: Compilation errors related to UVM
```
Solution: Ensure UVM_HOME is set correctly and UVM library version is compatible
export UVM_HOME=/path/to/uvm-1.1d
```

**Issue**: Interface connection errors
```
Solution: Check that virtual interface is properly set using config_db or assignment
```

**Issue**: Coverage not collected
```
Solution: Ensure compilation includes coverage flags: -coveropt 3 +cover
```

**Issue**: Simulation hangs
```
Solution: Check for missing clocking or event triggers, add timeout mechanism
```

## Contributing

Contributions are welcome! Please follow these guidelines:

1. **Fork the repository**
2. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Commit your changes**
   ```bash
   git commit -m "Add: Brief description of your changes"
   ```
4. **Push to the branch**
   ```bash
   git push origin feature/your-feature-name
   ```
5. **Submit a pull request**

### Contribution Ideas
- Add more test scenarios
- Implement dual-port RAM verification
- Add SystemVerilog assertions
- Improve coverage models
- Add regression scripts
- Enhance documentation

## License

This project is provided for **educational purposes**. Feel free to use it for learning and academic projects.

## Acknowledgments

This project follows industry-standard verification practices and UVM methodology guidelines. Special thanks to the verification community for established best practices and methodologies.

### References
- UVM 1.1d User Guide
- IEEE 1800-2017 SystemVerilog Standard
- Verification Methodology Manual (VMM)
- Industry verification best practices

## Contact

**Author:** Manar Abdoali  
**GitHub:** [@manarabdoali](https://github.com/manarabdoali)  
**Repository:** [RAM-Verification-using-class-based-env-and-UVM](https://github.com/manarabdoali/RAM-Verification-using-class-based-env-and-UVM)

For questions, feedback, or discussions:
- Open an issue in the repository
- Submit a pull request
- Connect on GitHub

---

⭐ **If you find this project helpful, please consider giving it a star!** ⭐

---

**Last Updated:** December 2024  
**Version:** 1.0  
**Status:** Active Development

---

*Built with ❤️ for the verification community*
