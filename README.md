# FSM-Based Vending Machine Controller

A Verilog-based implementation of a coin-operated vending machine using a **Finite State Machine (FSM)**. The controller accepts ₹5, ₹10, and ₹20 coins, dispenses a product priced at ₹45, and automatically calculates and returns the excess amount whenever the customer inserts more money than the product price.

## Project Overview

The vending machine operates using a nine-state FSM (S0–S8), where each state represents the accumulated amount in multiples of ₹5, ranging from ₹0 to ₹40.

Whenever a coin is inserted, the controller calculates the updated amount based on the current state and input denomination. If the total reaches or exceeds ₹45, the product dispensing signal (`out`) is activated, and the difference between the deposited amount and product price is returned through the `change` output. After completing the transaction, the FSM transitions back to the initial state S0.

## Specifications

| Parameter | Value |
|---|---|
| Product Price | ₹45 |
| Accepted Coins | ₹5, ₹10, ₹20 |
| Input Bus Width | 5 bits |
| Dispensing Output | 1-bit `out` |
| Change Output Width | 4 bits |
| FSM Architecture | Mealy |
| Clock Operation | Positive-edge triggered |
| Reset Type | Asynchronous, active high |

## State Encoding

The controller consists of nine states, each assigned a unique 4-bit binary representation corresponding to the amount deposited by the customer.

| State | Encoding | Accumulated Amount |
|---|---|---|
| S0 | `0000` | ₹0 (Idle) |
| S1 | `0001` | ₹5 |
| S2 | `0010` | ₹10 |
| S3 | `0011` | ₹15 |
| S4 | `0100` | ₹20 |
| S5 | `0101` | ₹25 |
| S6 | `0110` | ₹30 |
| S7 | `0111` | ₹35 |
| S8 | `1000` | ₹40 |

The FSM determines its next state according to the current accumulated amount and the incoming coin value. If the resulting amount reaches ₹45 or exceeds it, the machine dispenses the product, calculates the remaining balance as change, and moves back to S0.

## Change Calculation

The change is determined by subtracting the product price from the total amount inserted by the customer.

| Total Inserted | Change Returned |
|---|---|
| ₹45 | ₹0 |
| ₹50 | ₹5 |
| ₹55 | ₹10 |
| ₹60 | ₹15 |

## State Transition Table

The following table illustrates all 27 valid state transitions for the nine states and three supported coin denominations. Inputs other than ₹5, ₹10, and ₹20, including zero, keep the FSM in its existing state.

| Current State | Amount | Coin | Next State | Dispense | Change | Total |
|---|---:|---:|---|---:|---:|---:|
| S0 | ₹0 | ₹5 | S1 | 0 | ₹0 | ₹5 |
| S0 | ₹0 | ₹10 | S2 | 0 | ₹0 | ₹10 |
| S0 | ₹0 | ₹20 | S4 | 0 | ₹0 | ₹20 |
| S1 | ₹5 | ₹5 | S2 | 0 | ₹0 | ₹10 |
| S1 | ₹5 | ₹10 | S3 | 0 | ₹0 | ₹15 |
| S1 | ₹5 | ₹20 | S5 | 0 | ₹0 | ₹25 |
| S2 | ₹10 | ₹5 | S3 | 0 | ₹0 | ₹15 |
| S2 | ₹10 | ₹10 | S4 | 0 | ₹0 | ₹20 |
| S2 | ₹10 | ₹20 | S6 | 0 | ₹0 | ₹30 |
| S3 | ₹15 | ₹5 | S4 | 0 | ₹0 | ₹20 |
| S3 | ₹15 | ₹10 | S5 | 0 | ₹0 | ₹25 |
| S3 | ₹15 | ₹20 | S7 | 0 | ₹0 | ₹35 |
| S4 | ₹20 | ₹5 | S5 | 0 | ₹0 | ₹25 |
| S4 | ₹20 | ₹10 | S6 | 0 | ₹0 | ₹30 |
| S4 | ₹20 | ₹20 | S8 | 0 | ₹0 | ₹40 |
| S5 | ₹25 | ₹5 | S6 | 0 | ₹0 | ₹30 |
| S5 | ₹25 | ₹10 | S7 | 0 | ₹0 | ₹35 |
| S5 | ₹25 | ₹20 | S0 | 1 | ₹0 | ₹45 |
| S6 | ₹30 | ₹5 | S7 | 0 | ₹0 | ₹35 |
| S6 | ₹30 | ₹10 | S8 | 0 | ₹0 | ₹40 |
| S6 | ₹30 | ₹20 | S0 | 1 | ₹5 | ₹50 |
| S7 | ₹35 | ₹5 | S8 | 0 | ₹0 | ₹40 |
| S7 | ₹35 | ₹10 | S0 | 1 | ₹0 | ₹45 |
| S7 | ₹35 | ₹20 | S0 | 1 | ₹10 | ₹55 |
| S8 | ₹40 | ₹5 | S0 | 1 | ₹0 | ₹45 |
| S8 | ₹40 | ₹10 | S0 | 1 | ₹5 | ₹50 |
| S8 | ₹40 | ₹20 | S0 | 1 | ₹15 | ₹60 |

## File Structure

```text
FSM-Based-Vending-Machine-Controller/
├── verilog code
├── Verilog Testbench
├── Netlist.svg
├── Output 1.png
├── Output 2.png
├── Output Waveform.png
└── README.md
```
## Module Interface

The vending machine is implemented as a Verilog module named `vending_machine`, with the following interface:

```verilog
module vending_machine(
    input        clk,
    input        reset,
    input  [4:0] in,        // Coin input: 5, 10, or 20
    output reg   out,       // Product dispensing signal
    output reg [3:0] change // Returned change
);
```

## Simulation

Functional testing was carried out on [EDA Playground](https://www.edaplayground.com/) using the Icarus Verilog simulator (`iverilog/vvp`). The testbench was used to validate FSM operation, coin processing, product dispensing, and change calculation under different input conditions.

### Simulator Configuration

- **Platform:** EDA Playground
- **Simulator:** Icarus Verilog
- **Compilation Flags:** `-Wall -g2012`
- **Execution Command:** `vvp a.out`

### Test Cases

**Case 1 – Exact Payment (₹45)**

Input sequence: `₹20 + ₹10 + ₹10 + ₹5`

The machine receives a total of ₹45, exactly matching the product price. It activates the dispensing signal, returns zero change, and resets to the initial state.

**Case 2 – Payment Exceeding Product Price (₹50)**

Input sequence: `₹20 + ₹10 + ₹10 + ₹10`

The total deposited amount becomes ₹50. The controller dispenses the product and returns ₹5 as change before transitioning back to the idle state.

**Case 3 – Repeated Insertion of ₹5 Coins**

Input sequence: `₹5 × 9 = ₹45`

This test verifies the controller's ability to process multiple small-denomination coins consecutively. After receiving nine ₹5 coins, the accumulated amount reaches ₹45, triggering product dispensing without any change.

### Simulation Output

The following selected simulation log entries demonstrate the successful execution of all three test scenarios.

```text
========== CASE 1 ==========
Time = 75 || Current State = 8 Next State = 0 ||
Input Amount = 5 || Output = 1 Change = 0

========== CASE 2 ==========
Time = 165 || Current State = 8 Next State = 0 ||
Input Amount = 10 || Output = 1 Change = 5

========== CASE 3 ==========
Time = 355 || Current State = 8 Next State = 0 ||
Input Amount = 5 || Output = 1 Change = 0
```

The simulation results verify that the controller correctly identifies the required payment amount, generates the dispensing signal, calculates the appropriate change, and returns to its initial state after completing each transaction.

## Synthesis and Netlist Generation

RTL synthesis was performed using **Yosys** to convert the Verilog design into a hardware netlist. The generated JSON netlist was subsequently converted into an SVG diagram using **netlistsvg** to obtain a graphical representation of the synthesized circuit.

### Synthesis Steps

First, navigate to the directory containing the project files and launch Yosys.

```bash
cd ~/Desktop
ls
yosys
```

Execute the following commands within the Yosys environment:

```bash
read_verilog vending_machine.v
prep -top vending_machine
write_json vending_machine.json
exit
```

Once the JSON netlist is generated, use the following commands to create and open the circuit diagram.

```bash
netlistsvg vending_machine.json -o vending_machine.svg
open vending_machine.svg
```

The resulting SVG file displays the synthesized circuit structure and provides a visual representation of the hardware generated from the RTL implementation.

## Tools Used

| Tool | Purpose |
|---|---|
| [EDA Playground](https://www.edaplayground.com/) | Online platform for HDL development and simulation |
| Icarus Verilog | Compilation and execution of Verilog testbenches |
| Yosys | RTL synthesis and hardware netlist generation |
| netlistsvg | Graphical visualization of synthesized netlists |

## Design Notes

- The RTL implementation is divided into three `always` blocks, separating sequential state storage, next-state determination, and combinational output logic.
- The sequential block updates the current FSM state at every positive clock edge and supports asynchronous active-high reset.
- The combinational block evaluates the current state and coin input to determine the next state and corresponding output signals.
- Default assignments are provided for `out` and `change` before evaluating the different conditions. This ensures that all outputs are assigned and avoids unintended latch inference.
- The input bus is maintained at zero between successive coin insertions in the testbench to represent the absence of a coin at the input.
- The 4-bit `change` bus is sufficient to represent all possible change values generated by the controller: ₹0, ₹5, ₹10, and ₹15.
- Invalid state encodings are handled using the `default` branch, which directs the FSM back to the initial state S0.

---

**Note:** For SystemVerilog source files (`.sv`), use `read_verilog -sv design.sv` in Yosys instead of `read_verilog vending_machine.v`, if that is the actual filename.
