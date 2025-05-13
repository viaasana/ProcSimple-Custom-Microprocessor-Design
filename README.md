# CE118 – FINAL LAB: Simple Custom Microprocessor Design

## 👨‍💻 Author
- **Name**: Thach Via Sa Na  
- **Student ID**: 23520966

## 📚 Project Overview

This lab involves building a simplified processor architecture, including core components such as the Program Counter (PC), Instruction Register (IR), ALU, Register File, Control Unit, and supporting logic. The processor executes a custom instruction set and simulates instruction execution via waveform verification.

---

## 🧠 Theoretical Notes

Although only an 8x16 register file is required, a 16x16 version was reused from a previous lab. Input wiring was modified to use only 8 registers for this lab.

---

## 🏗 System Architecture Overview

1. **Program Counter (PC)**: Points to the current instruction address. It increments or branches based on control signals.
2. **Instruction Register (IR)**: Holds the current instruction; triggered by rising/falling edge detectors when `Get_code` changes.
3. **Control Unit**: Decodes instructions into signals to control other components.
4. **Instruction Types**:
   - **RRR** (Register-Register-Register)
   - **RRI** (Register-Register-Immediate)
   - **RI** (Register-Immediate)

5. **Register File**: 16 registers of 16-bit width, with 3-bit addressing for reading and writing.

---

## 🧾 Instruction Decoding

### 🔢 Instruction Type Detection
- `opcode[2:0]` is used to determine instruction type:
  - `000` → RRR
  - `010` → RRI
  - `100`, `101` → RI

### 🧪 Register Usage
- **WA (Write Address)**:
  - RRR → `instruction[6..4]`
  - RRI → `instruction[12..10]`
- **RB (Second Operand)**:
  - RRR → `instruction[9..7]`
  - RI → same as RA

### ⚙️ ALU Control
- RRR → `func[3..0]`
- RRI → `0001` (e.g. increment)
- RI → `0100` (e.g. AND)

---

## 🧮 ALU Operations

Performs operations like:
- `add`, `sub`, `inc`, `dec`
- Logical: `and`, `or`, `xor`, `nand`

Supported via two subunits:
- **AU (Arithmetic Unit)**
- **LU (Logic Unit)**

Also includes:
- **Zero Detector**: Signals when output is zero
- **Sign Extension**: Extends 7-bit immediates to 16-bit

---

## 🔁 PC Counter

- If `Branch = 1`: `PC = IMM`
- Else: `PC = PC + 1`

---

## 🔄 Instruction Register Control

Uses:
- **Rising Detect**: Triggers on `Get_code` rising edge
- **Falling Detect**: Triggers on falling edge
- Ensures PC resets when instruction input starts

---

## 📦 Register File (16x16-bit)

- **Inputs**:
  - `WA[2:0]`: write address
  - `RAA[2:0]`, `RAB[2:0]`: read addresses
  - `I[15:0]`: data input
- **Outputs**:
  - `OA[15:0]`, `OB[15:0]`: data outputs

---

## 🧪 Assembly Program – Example

```assembly
Linp $0        # Input 110 to $0
Linp $1        # Input 210 to $1
Linp $2        # Input 310 to $2
Add $0, $1, $4 # R[$4] = R[$0] + R[$1]
Wout $4        # Output the result in $4
---
Binary
1000000000000000
1000010000000000
1000100000000000
0000000011000000
1011000000000000
```
### 📈 Waveform Output

![Waveform Simulation](https://github.com/user-attachments/assets/2be9a139-1d70-4685-8c6d-b38ddda1ada0)


