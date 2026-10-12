# Digital Electronics and Computer Organization: Complete Study Notes

# UNIT 1: Combinational Logic Modules and Register Transfer

## 1.1 Combinational vs Sequential

| Combinational | Sequential |
|---|---|
| Output depends only on the **present inputs** | Output depends on present inputs and **past state** |
| No memory or feedback | Has memory (flip-flops) and a clock |
| Examples: adder, MUX, decoder | Examples: counter, register |

```
         n inputs                          m outputs
  X ──────────────►┌────────────────────┐──────────────► Z
                   │  Combinational     │
                   │  Logic Circuit     │
                   └────────────────────┘
```



## 1.2 Decoder

A decoder converts an **n-bit code into one of 2ⁿ lines**. Exactly one output is active at a time.

### 2-to-4 Decoder (with Enable)

| E | A | B | D0 | D1 | D2 | D3 |
|---|---|---|----|----|----|----|
| 0 | X | X | 0 | 0 | 0 | 0 |
| 1 | 0 | 0 | 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 0 | 1 | 0 | 0 |
| 1 | 1 | 0 | 0 | 0 | 1 | 0 |
| 1 | 1 | 1 | 0 | 0 | 0 | 1 |

Each output is a minterm: D0 = E·A'·B', D1 = E·A'·B, D2 = E·A·B', D3 = E·A·B

```
        ┌───────────┐
  A ───►│           ├──► D0
  B ───►│  2 : 4    ├──► D1
        │  DECODER  ├──► D2
  E ───►│           ├──► D3
        └───────────┘
```

### Applications
- **Implementing Boolean functions.** A decoder gives all minterms, so OR the needed outputs. A function in n variables needs an n-to-2ⁿ decoder plus an OR gate.
- Memory address decoding (chip select).
- Instruction decoding in the CPU (3-to-8 decoder for the opcode).
- BCD-to-7-segment display driving.
- Demultiplexer (decoder with Enable as data input).

**Example:** F(A,B,C) = Σm(1,2,4,7) is a 3-to-8 decoder with outputs D1, D2, D4, D7 fed to a 4-input OR.

### Decoder Expansion (Tree Construction)
A 4-to-16 decoder can be built from two 3-to-8 decoders:

```
  A (MSB) ─────┬───────► E of Dec-1 (via NOT, active when A=0) → outputs D0-D7
               └───────► E of Dec-2 (active when A=1)          → outputs D8-D15
  B C D ──────────────► inputs of both decoders
```

Alternatively, a 4-to-16 decoder can be built from five 2-to-4 decoders (1 + 4).



## 1.3 Encoder

An encoder is the **inverse of a decoder**. It has 2ⁿ inputs and an n-bit output code.

### Octal-to-Binary (8-to-3) Encoder
- A2 = D4 + D5 + D6 + D7
- A1 = D2 + D3 + D6 + D7
- A0 = D1 + D3 + D5 + D7

**Problems with a simple encoder:**
1. Only one input may be active at a time.
2. D0 = 1 and "no input active" both produce 000, which is ambiguous.

### Priority Encoder (4-to-2)
It resolves multiple active inputs by encoding the **highest-priority** one. It also has a **valid bit V** for the "no input" case.

| D3 | D2 | D1 | D0 | x | y | V |
|----|----|----|----|---|---|---|
| 0 | 0 | 0 | 0 | X | X | 0 |
| 0 | 0 | 0 | 1 | 0 | 0 | 1 |
| 0 | 0 | 1 | X | 0 | 1 | 1 |
| 0 | 1 | X | X | 1 | 0 | 1 |
| 1 | X | X | X | 1 | 1 | 1 |

x = D2 + D3; y = D3 + D1·D2'; V = D0 + D1 + D2 + D3

**Applications:** interrupt priority resolution, keyboard encoding, A/D converters.



## 1.4 Multiplexer (MUX)

A MUX **selects one of 2ⁿ data inputs** using n select lines and routes it to a single output (a "data selector").

### 4-to-1 MUX

| S1 | S0 | Y |
|----|----|----|
| 0 | 0 | I0 |
| 0 | 1 | I1 |
| 1 | 0 | I2 |
| 1 | 1 | I3 |

**Y = S1'S0'·I0 + S1'S0·I1 + S1S0'·I2 + S1S0·I3**

```
   I0 ──►┐
   I1 ──►│  4 : 1
   I2 ──►│  MUX    ├──► Y
   I3 ──►│
         └─▲──▲──┘
          S1  S0
```

### Applications
1. **Implementing Boolean functions.** An n-variable function can use a 2ⁿ⁻¹ : 1 MUX. Use n−1 variables as selects and the last variable (or 0, 1, or its complement) as data inputs.
2. Data routing (bus selection, register-file read ports).
3. Parallel-to-serial conversion.
4. Waveform and sequence generation.
5. Selecting the ALU operand source.

**Example:** F(A,B,C) = Σm(1,2,6,7) on a 4:1 MUX with selects A,B:

| | C=0 | C=1 | Input |
|---|---|---|---|
| AB=00 | m0 | **m1** | I0 = C |
| AB=01 | **m2**→ wait, use B as select (A,B) so 01 → m2,m3: m2 ✓, m3 ✗ | | I1 = C' |
| AB=10 | m4 ✗, m5 ✗ | | I2 = 0 |
| AB=11 | m6 ✓, m7 ✓ | | I3 = 1 |

(Method: for each select pair, write the two minterms (data variable = 0 and 1), then see whether the data input is 0, 1, C, or C'.)

**MUX expansion:** a 16:1 MUX is built from four 4:1 MUXes plus one 4:1 MUX as the final stage.



## 1.5 Demultiplexer (DEMUX)

A DEMUX routes **one input to one of 2ⁿ outputs** selected by n select lines. It is the reverse of a MUX.

### 1-to-4 DEMUX
- Y0 = D·S1'S0'
- Y1 = D·S1'S0
- Y2 = D·S1S0'
- Y3 = D·S1S0

```
          ┌─────────┐──► Y0
   D ────►│ 1 : 4   ├──► Y1
          │ DEMUX   ├──► Y2
          └─▲────▲──┘──► Y3
           S1   S0
```

A DEMUX and a decoder with enable are the **same circuit**; the enable input acts as the data input. 

**Applications:** serial-to-parallel conversion, routing data to one of several registers, a communication multiplexer/demultiplexer pair.



## 1.6 Parity Circuits

Parity is a simple **error-detecting code** that adds one bit so the total number of 1s is even (even parity) or odd (odd parity).

**Parity generator (3-bit message ABC, even parity):**
P = A ⊕ B ⊕ C

| A B C | P (even) |
|-------|---|
| 000 | 0 |
| 001 | 1 |
| 011 | 0 |
| 111 | 1 |

**Parity checker (4-bit received ABCP, even):**
PEC = A ⊕ B ⊕ C ⊕ P → 0 = no error, 1 = error

```
 Transmitter                           Receiver
 A B C ──►[ Parity Gen ]──► A B C P ──►[ Parity Checker ]──► Error?
                              (channel)
```

- It detects **all odd-numbered bit errors** and cannot detect even-numbered ones.
- It cannot correct errors (Hamming code does that).
- The IC is the 74180 (9-bit parity generator/checker).



## 1.7 Magnitude Comparators

A comparator compares two binary numbers A and B and gives A>B, A=B, A<B.

### 1-bit
- A>B = A·B'
- A<B = A'·B
- A=B = (A ⊕ B)' = XNOR

### 2-bit (A1A0 vs B1B0)
- Let xᵢ = AᵢBᵢ + Aᵢ'Bᵢ' (XNOR)
- **(A=B) = x1 · x0**
- **(A>B) = A1B1' + x1·A0B0'**
- **(A<B) = A1'B1 + x1·A0'B0**

### n-bit rule
Compare from the **MSB**. The first unequal bit decides the result (the higher number has 1 there).

```
   A3A2A1A0 ──►┌──────────┐──► A>B
               │ 4-bit    ├──► A=B
   B3B2B1B0 ──►│ (7485)   ├──► A<B
   cascade in ►└──────────┘
```

The 74LS85 can be cascaded for 8-bit, 12-bit, and wider comparisons.



## 1.8 Arithmetic Modules

### Half Adder (HA)
| A | B | Sum | Carry |
|---|---|-----|-------|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

**S = A ⊕ B, C = A·B** (1 XOR + 1 AND)

### Full Adder (FA)
Adds A, B, and carry-in Cin.
- **S = A ⊕ B ⊕ Cin**
- **Cout = AB + Cin(A ⊕ B)** = AB + BCin + ACin

```
 A ─►┬────────►┌────┐   S1       ┌────┐ 
 B ─►┘ HA-1    │ XOR│──────────► │ HA-2│──► Sum
          C1   └────┘   Cin ───► │     │──► C2
 Full adder = 2 half adders + OR gate:  Cout = C1 + C2
```

### Ripple Carry Adder (n-bit)

```
  A3 B3     A2 B2     A1 B1     A0 B0
   │  │      │  │      │  │      │  │
 ┌─▼──▼─┐  ┌─▼──▼─┐  ┌─▼──▼─┐  ┌─▼──▼─┐
 │  FA  │◄─│  FA  │◄─│  FA  │◄─│  FA  │◄── Cin
 └─┬──┬─┘C3└─┬──┬─┘C2└─┬──┬─┘C1└─┬──┬─┘
 Cout S3     S2          S1         S0
```
**Drawback:** delay = n × (FA carry delay), so it is slow.

### Carry Look-Ahead Adder (CLA)
Define for each bit:
- **Generate** Gᵢ = AᵢBᵢ
- **Propagate** Pᵢ = Aᵢ ⊕ Bᵢ
- Sum Sᵢ = Pᵢ ⊕ Cᵢ, Carry Cᵢ₊₁ = Gᵢ + PᵢCᵢ

Expanded:
- C1 = G0 + P0C0
- C2 = G1 + P1G0 + P1P0C0
- C3 = G2 + P2G1 + P2P1G0 + P2P1P0C0
- C4 = G3 + P3G2 + P3P2G1 + P3P2P1G0 + P3P2P1P0C0

All carries are computed **in parallel** (constant delay). The cost is more hardware (74182 look-ahead carry generator).

### BCD Adder
Add two BCD digits with a 4-bit binary adder. **If the sum > 9 or Cout = 1, add 0110 (6)** to correct it.
Correction condition: **K = Cout + S3·S2 + S3·S1**

```
 A(BCD) ─►┌──────────┐ Z8 Z4 Z2 Z1      ┌──────────┐
 B(BCD) ─►│ 4-bit    ├──────────────────►│ 4-bit    ├──► BCD Sum
          │ Adder 1  │                   │ Adder 2  │
 Cin ────►└────┬─────┘      0 K K 0 ────►│ (adds 6) │
               │ Cout ──►[K logic]──────►└────┬─────┘
                                              └──► Carry out
```

### Subtractors
**Half subtractor:** D = A ⊕ B, Borrow = A'B
**Full subtractor:** D = A ⊕ B ⊕ Bin, Bout = A'B + A'Bin + B·Bin

### Binary Adder-Subtractor (using 2's complement)

```
 B3..B0 ──►[XOR with M]──► FA chain ◄── A3..A0
 M = 0 → ADD  (B ⊕ 0 = B,  Cin = 0)         → A + B
 M = 1 → SUB  (B ⊕ 1 = B', Cin = 1)         → A + B' + 1 = A − B
```
**Overflow** V = C_n ⊕ C_(n−1) (carry into MSB XOR carry out of MSB). It is valid for signed numbers.

## 1.9 Register Transfer Language (RTL)

**Register transfer notation**
- `R2 ← R1` : copy R1 to R2
- `P: R2 ← R1` : transfer only if control condition P = 1
- `R1 ← R2, R2 ← R1` : swap, done simultaneously on the same clock
- `R[7:0]` or `R(0-7)` : bit selection (subfield)
- `M[AR]` : memory word addressed by AR
- A comma separates simultaneous microoperations; `:` separates the condition from the action.

```
     Load (P)              
       │                  
   ┌───▼────┐   clock     ┌────────┐
   │  R1    ├────────────►│   R2   │
   └────────┘             └────────┘
 Hardware: transfer occurs on the clock edge only when load = 1
```

### Bus Transfers
A **common bus** connects several registers using **multiplexers** (or three-state buffers).

**Bus using MUX (4 registers, 4-bit each):**

```
 R0 ─────►┐
 R1 ─────►│ 4 × (4:1 MUX)      ┌───────────────┐
 R2 ─────►│ select S1 S0       │ BUS (4 lines) ├──► to all register inputs
 R3 ─────►┘                    └───────────────┘
```

| S1 S0 | Register placed on bus |
|-------|------|
| 00 | R0 |
| 01 | R1 |
| 10 | R2 |
| 11 | R3 |

Statement: `R2 ← R1` means select R1 onto the bus (S1S0 = 01) and enable load on R2.

**Three-state buffer bus:** each register output goes through a tri-state buffer. A decoder enables **only one** buffer at a time. The other outputs are in the high-impedance (Hi-Z) state. This approach is cheaper for large buses.

### Memory Transfers
- **Read:** `DR ← M[AR]` (AR holds the address, data goes to DR)
- **Write:** `M[AR] ← R1`

```
        ┌─────────┐ address  ┌──────────────┐
        │   AR    ├─────────►│  MEMORY      │
        └─────────┘          │  UNIT        │
        ┌─────────┐◄─ Read ──┤  (2^k words) │
        │   DR    │── Write─►│              │
        └─────────┘ data     └──────────────┘
```

## 1.10 Arithmetic Microoperations

| Symbolic | Description |
|----------|-------------|
| R3 ← R1 + R2 | Add |
| R3 ← R1 − R2 | Subtract (R1 + R2' + 1) |
| R2 ← R2' | Complement (1's) |
| R2 ← R2' + 1 | Negate (2's complement) |
| R1 ← R1 + 1 | Increment |
| R1 ← R1 − 1 | Decrement |
| R3 ← R1 + R2' + 1 | Subtraction |

### Arithmetic Circuit (4-bit, with select S1 S0 and Cin)
Each stage has a FA whose Y input comes from a 4:1 MUX choosing among **B, B', 0, 1**.

| S1 S0 | Cin | Y | Output D = A + Y + Cin | Operation |
|-------|-----|---|---------|-----------|
| 00 | 0 | B | A + B | Add |
| 00 | 1 | B | A + B + 1 | Add with carry |
| 01 | 0 | B' | A + B' | Sub with borrow |
| 01 | 1 | B' | A + B' + 1 | Subtract |
| 10 | 0 | 0 | A | Transfer A |
| 10 | 1 | 0 | A + 1 | Increment A |
| 11 | 0 | all 1s | A − 1 | Decrement A |
| 11 | 1 | all 1s | A | Transfer A |

```
   Aᵢ ───────────────►┌────┐
   Bᵢ ──► MUX(S1,S0) ─►│ FA │──► Dᵢ
   Bᵢ'──► (Y: B,B',0,1)│    │──► Cᵢ₊₁
                  Cᵢ ─►└────┘
```

**Incrementer:** a chain of half adders (one input fixed at 1).



## 1.11 Logic Microoperations

Bitwise operations on register contents (each bit treated independently).

| Control | Symbolic | Name | Boolean |
|---|----|---|---|
| `R ← 0` | | Clear | F = 0 |
| `R1 ← R1 ∧ R2` | ∧ | AND | AB |
| `R1 ← R1 ∨ R2` | ∨ | OR | A+B |
| `R1 ← R1 ⊕ R2` | ⊕ | XOR | A⊕B |
| `R1 ← R1'` | ' | Complement | A' |
| `R1 ← R2` | | Transfer | B |
| `R ← 1s` | | Set | F = 1 |
| | | NAND, NOR, XNOR | |

### 16 logic functions (4-bit select S3S2S1S0)
Hardware: each bit uses a **4:1 MUX** with selects Aᵢ, Bᵢ and data inputs S0–S3. This produces all 16 functions of two variables.

### Applications (very important)

| Operation | Rule | Example |
|---|---|---|
| **Selective-set** | A ← A ∨ B | A=1010, B=1100 → 1110 (bits set where B=1) |
| **Selective-complement** | A ← A ⊕ B | A=1010, B=1100 → 0110 |
| **Selective-clear** | A ← A ∧ B' | A=1010, B=1100 → 0010 |
| **Mask** | A ← A ∧ B | A=1010, B=1100 → 1000 |
| **Insert** | Mask then OR | Replace selected bits by new value |
| **Clear** | A ← A ⊕ A | Always 0 |

**Insert example:** replace the upper 4 bits of A=0110 1010 with 1001.
(1) Mask: A ∧ 0000 1111 = 0000 1010
(2) OR: 0000 1010 ∨ 1001 0000 = 1001 1010

## 1.12 Shift Microoperations

| Type | Symbol | Action |
|------|--------|--------|
| Logical shift left | `shl` | Zero enters LSB, MSB lost |
| Logical shift right | `shr` | Zero enters MSB, LSB lost |
| Circular shift left | `cil` | MSB → LSB (rotate) |
| Circular shift right | `cir` | LSB → MSB |
| Arithmetic shift right | `ashr` | **Sign bit preserved** and copied (÷2) |
| Arithmetic shift left | `ashl` | Zero enters LSB (×2); may overflow |

```
 Logical right:    0 ──► [ R7 R6 ... R0 ] ──► lost
 Logical left:   lost ◄── [ R7 R6 ... R0 ] ◄── 0
 Circular right:   ┌──► [ R7 ... R0 ] ──┐
                   └────────────────────┘
 Arithmetic right: R7 stays; R7 ──► R6 ──► ... ──► R0 ──► lost
```

**Arithmetic shift left overflow:** V = R_(n−1) ⊕ R_(n−2) (sign bit changes).
Example: ashr(1010) = 1101; shl(0011) = 0110.

**Hardware shifter:** a 4-bit register shifter uses **MUXes**; select H = 0 shifts right, H = 1 shifts left. A barrel shifter allows multi-bit shifts in one clock.

## 1.13 Arithmetic Logic Shift Unit (ALSU)

It combines arithmetic, logic, and shift circuits in one unit using a selection structure.

```
        ┌──────────────┐
 A ────►│  Arithmetic  ├────┐
 B ────►│   Circuit    │    │      ┌──────┐
        └──────────────┘    ├─────►│ 4:1  │
        ┌──────────────┐    │      │ MUX  ├──► F
 A ────►│    Logic     ├────┤      │(S3,S2)│
 B ────►│   Circuit    │    │      └──────┘
        └──────────────┘    │         ▲
   shr A ──────────────────►┤         │
   shl A ──────────────────►┘      S3 S2
```

| S3 S2 S1 S0 | Cin | Operation | Function |
|---|---|---|---|
| 0 0 0 0 | 0 | F = A | Transfer A |
| 0 0 0 0 | 1 | F = A + 1 | Increment |
| 0 0 0 1 | 0 | F = A + B | Add |
| 0 0 0 1 | 1 | F = A + B + 1 | Add with carry |
| 0 0 1 0 | 0 | F = A + B' | |
| 0 0 1 0 | 1 | F = A + B' + 1 | Subtract |
| 0 0 1 1 | 0 | F = A − 1 | Decrement |
| 0 1 0 0 | X | F = A ∧ B | AND |
| 0 1 0 1 | X | F = A ∨ B | OR |
| 0 1 1 0 | X | F = A ⊕ B | XOR |
| 0 1 1 1 | X | F = A' | Complement |
| 1 0 X X | X | F = shr A | Shift right |
| 1 1 X X | X | F = shl A | Shift left |

S3S2 select which circuit's result reaches the output via a final 4:1 MUX. S1S0 and Cin control the chosen circuit. Cout of the arithmetic circuit is the carry; the shifters give the end bit.


# UNIT 2: Basic Computer Organization and Design

## 2.1 Instruction Codes

An instruction code is a group of bits that tells the computer to perform a specific operation.

**Basic computer instruction format (16-bit):**

```
 15  14  13  12  11                    0
┌───┬───────────┬───────────────────────┐
│ I │  Opcode   │       Address         │
│   │ (3 bits)  │      (12 bits)        │
└───┴───────────┴───────────────────────┘
 I = 0 → direct address;  I = 1 → indirect address
```

- Memory: 4096 words × 16 bits (12-bit address).
- **Direct address:** the address field is the operand's location.
- **Indirect address:** the address field points to the location that **holds** the operand's address.
- Opcode = 111 means a register-reference (I=0) or I/O (I=1) instruction. No memory operand is needed.

```
 Direct:                    Indirect:
 instr: [0|opcode|ADDR]     instr: [1|opcode|ADDR]
          └─► Operand  M[ADDR]       M[ADDR] = EA ──► Operand at M[EA]
```


## 2.2 Computer Registers

| Register | Bits | Function |
|----------|------|----------|
| **DR** | 16 | Data Register (holds memory operand) |
| **AR** | 12 | Address Register |
| **AC** | 16 | Accumulator |
| **IR** | 16 | Instruction Register |
| **PC** | 12 | Program Counter |
| **TR** | 16 | Temporary Register |
| **INPR** | 8 | Input Register |
| **OUTR** | 8 | Output Register |
| **SC** | 4 | Sequence Counter |
| E | 1 | Extended-carry flip-flop |
| I | 1 | Indirect-mode bit |
| IEN | 1 | Interrupt Enable |
| R | 1 | Interrupt cycle flag |
| FGI / FGO | 1 | Input / Output flags |

### Common Bus System

```
   Memory unit ◄──► ┌─────────────────────────────────┐
   (4096 × 16)      │      16-bit COMMON BUS          │
                    └─────────────────────────────────┘
   Select S2 S1 S0:
   001 AR   010 PC   011 DR   100 AC   101 IR   110 TR   111 Memory
   AR,PC,DR,AC,IR,TR connected by LD / INR / CLR signals; ALU between DR and AC
```

Data path: AC ← AC op DR through the adder/logic circuit; output goes back to the AC (and through the bus to other registers).

## 2.3 Computer Instructions

Three instruction types are defined by the opcode bits (12–14) and I (bit 15):

```
 Memory-reference:  [I | 000–110 | Address]            (opcode ≠ 111)
 Register-ref:      [0 | 111 | B11 ... B0]   = 0111 xxxx xxxx xxxx  (7xxx)
 Input-output:      [1 | 111 | B11 ... B0]   = 1111 xxxx xxxx xxxx  (Fxxx)
```

### Memory-Reference Instructions

| Symbol | Opcode (I=0) | (I=1) | Operation |
|---|---|---|---|
| AND | 0xxx | 8xxx | AC ← AC ∧ M[AR] |
| ADD | 1xxx | 9xxx | AC ← AC + M[AR], E ← Cout |
| LDA | 2xxx | Axxx | AC ← M[AR] |
| STA | 3xxx | Bxxx | M[AR] ← AC |
| BUN | 4xxx | Cxxx | PC ← AR (branch unconditionally) |
| BSA | 5xxx | Dxxx | M[AR] ← PC, PC ← AR + 1 (subroutine call) |
| ISZ | 6xxx | Exxx | M[AR] ← M[AR] + 1; if zero then PC ← PC + 1 |

### Register-Reference Instructions (7xxx)

| Symbol | Hex | Operation |
|---|---|---|
| CLA | 7800 | Clear AC |
| CLE | 7400 | Clear E |
| CMA | 7200 | Complement AC |
| CME | 7100 | Complement E |
| CIR | 7080 | Circulate right (E, AC) |
| CIL | 7040 | Circulate left (E, AC) |
| INC | 7020 | Increment AC |
| SPA | 7010 | Skip next if AC positive |
| SNA | 7008 | Skip next if AC negative |
| SZA | 7004 | Skip next if AC = 0 |
| SZE | 7002 | Skip next if E = 0 |
| HLT | 7001 | Halt |

### Input-Output Instructions (Fxxx)

| Symbol | Hex | Operation |
|---|---|---|
| INP | F800 | AC(0-7) ← INPR, FGI ← 0 |
| OUT | F400 | OUTR ← AC(0-7), FGO ← 0 |
| SKI | F200 | Skip if FGI = 1 |
| SKO | F100 | Skip if FGO = 1 |
| ION | F080 | IEN ← 1 |
| IOF | F040 | IEN ← 0 |


## 2.4 Timing and Control

The basic computer uses a **hardwired control unit**.

```
         IR(12-14) ──►┌───────────┐ D0…D7
                      │ 3×8 Decoder│─────────┐
         IR(15)=I ──────────────────────────┤
                                            ▼
 ┌──────┐ 4 bits ┌───────────┐ T0…T15   ┌──────────────┐
 │  SC  ├───────►│ 4×16 Dec  ├─────────►│  CONTROL     ├──► control
 └──▲───┘        └───────────┘          │  LOGIC GATES │    signals
    │ INR/CLR                           └──────────────┘
    clock                    IR(0-11), AC, DR, E, FGI, FGO, R, IEN … ──►
```

- **Sequence counter SC** is incremented each clock pulse. Decoding gives T0…T15.
- SC ← 0 at the end of an instruction (control signal CLR) so the next instruction restarts at T0.
- D0–D7: decoded opcode (D7 means register or I/O type).

## 2.5 Instruction Cycle

Every instruction goes through: **Fetch → Decode → (Fetch operand if indirect) → Execute**.

```
          ┌─────────────────────┐
          │       START         │
          │      SC ← 0         │
          └──────────┬──────────┘
                     ▼
            ┌──────────────────┐
     ┌─ Yes─┤  R = 1 ?         │
     │      └────────┬─────────┘
     │               │ No
     │               ▼
     │     FETCH:  T0: AR ← PC
     │             T1: IR ← M[AR], PC ← PC + 1
     │             T2: D0..D7 ← Decode IR(12-14)
     │                 AR ← IR(0-11), I ← IR(15)
     │               ▼
     │        ┌─────────────┐   D7 = 1 ?
     │        │  DECODE     │───Yes──► (I = 0)? ──Yes──► REGISTER-REF  (T3: r)
     │        └──────┬──────┘                 └──No───► INPUT-OUTPUT    (T3: p)
     │               │ D7 = 0 (memory-ref)
     │               ▼
     │        I = 1 ? ─Yes─► T3: AR ← M[AR]   (indirect)
     │               │ No      (nothing)
     │               ▼
     │        T4,T5,T6 : EXECUTE memory-reference
     │               │
     │               ▼ SC ← 0, go back to START
     ▼
  INTERRUPT CYCLE:
     RT0: AR ← 0, TR ← PC
     RT1: M[AR] ← TR, PC ← 0
     RT2: PC ← PC + 1, IEN ← 0, R ← 0, SC ← 0
```

**Fetch and decode summary:**

```
 R'T0:   AR ← PC
 R'T1:   IR ← M[AR], PC ← PC + 1
 R'T2:   D0..D7 ← Decode IR(12-14), AR ← IR(0-11), I ← IR(15)
 D7'IT3: AR ← M[AR]                   (indirect, memory ref)
 D7'I'T3: nothing
 D7I'T3: Execute register-ref (r)
 D7IT3:  Execute I/O (p)
```


## 2.6 Memory-Reference Instructions (microoperations)

| Instr | Microoperations |
|---|---|
| **AND** | D0T4: DR ← M[AR]<br>D0T5: AC ← AC ∧ DR, SC ← 0 |
| **ADD** | D1T4: DR ← M[AR]<br>D1T5: AC ← AC + DR, E ← Cout, SC ← 0 |
| **LDA** | D2T4: DR ← M[AR]<br>D2T5: AC ← DR, SC ← 0 |
| **STA** | D3T4: M[AR] ← AC, SC ← 0 |
| **BUN** | D4T4: PC ← AR, SC ← 0 |
| **BSA** | D5T4: M[AR] ← PC, AR ← AR + 1<br>D5T5: PC ← AR, SC ← 0 |
| **ISZ** | D6T4: DR ← M[AR]<br>D6T5: DR ← DR + 1<br>D6T6: M[AR] ← DR, if (DR = 0) then PC ← PC + 1, SC ← 0 |

**BSA illustration:**

```
 Before:  PC = 21,  BSA 135 at addr 20
          M[135] ← 21 (return address),  PC ← 136 (subroutine starts)
 Return:  BUN 135 (indirect: BUN I 135) → PC ← M[135] = 21
```

**Register-reference (r = D7I'T3, B_i = IR(i)):**

```
 r:     SC ← 0
 rB11:  AC ← 0                   (CLA)
 rB10:  E ← 0                    (CLE)
 rB9:   AC ← AC'                 (CMA)
 rB8:   E ← E'                   (CME)
 rB7:   AC ← shr AC, AC(15) ← E, E ← AC(0)    (CIR)
 rB6:   AC ← shl AC, AC(0) ← E,  E ← AC(15)   (CIL)
 rB5:   AC ← AC + 1              (INC)
 rB4:   if AC(15)=0 then PC ← PC+1   (SPA)
 rB3:   if AC(15)=1 then PC ← PC+1   (SNA)
 rB2:   if AC = 0   then PC ← PC+1   (SZA)
 rB1:   if E = 0    then PC ← PC+1   (SZE)
 rB0:   S ← 0                    (HLT)
```


## 2.7 Input-Output and Interrupt

### Input–Output Configuration

```
 Keyboard ──► INPR(8) ──►(FGI)──► AC(0-7)
 AC(0-7) ──► OUTR(8) ──►(FGO)──► Printer
 Serial communication interface; FGI/FGO are flip-flops (flags)
```

**Program-controlled I/O** (CPU polls the flag):
```
 Input:  LOOP: SKI           ; FGI = 1 ?
               BUN LOOP      ; wait
               INP           ; AC ← INPR, FGI ← 0
 Output: LOOP: SKO
               BUN LOOP
               OUT
```
The CPU is **busy-waiting** and speed is mismatched, so interrupt-driven I/O is more efficient.

### Interrupt-Initiated I/O
- Interrupt enable IEN is set by ION and cleared by IOF.
- R = 1 when (IEN = 1) and (FGI = 1 or FGO = 1), checked at T0'T1'T2'.
- During the interrupt cycle: return address saved in **memory location 0**; the CPU branches to **location 1** (the service routine / branch to ISR).

```
 R=1 at T2:                          Location 0: return address (PC)
  RT0: AR ← 0, TR ← PC                Location 1: BUN ISR (branch to service routine)
  RT1: M[AR] ← TR, PC ← 0
  RT2: PC ← PC + 1, IEN ← 0, R ← 0, SC ← 0
```

**ISR flow:** save AC and E → check which flag (FGI/FGO) → service → restore AC, E → ION → return through `BUN I 0`.


## 2.8 Design of Accumulator Logic

The AC receives data from: AND, ADD, DR, INPR, COM (complement), SHR, SHL, CLR and INC.

```
         AND(AC,DR) ──┐
         ADD(AC,DR) ──┤
           DR ────────┤
           INPR ──────┼──► [Adder & Logic circuit] ──► AC input ──► AC (with LD, INR, CLR)
           COM ───────┤
           SHR/SHL ───┤
                       └── control gates
```

Control signals (from instructions):

| Control | Condition |
|---|---|
| AND to AC | D0T5 |
| ADD to AC | D1T5 |
| DR to AC | D2T5 |
| INPR to AC | pB11 (INP) |
| Complement AC | rB9 |
| Shift right AC | rB7 |
| Shift left AC | rB6 |
| Clear AC | rB11 |
| Increment AC | rB5 |

**LD (AC) = D0T5 + D1T5 + D2T5 + rB9 + rB7 + rB6 + pB11**
**CLR (AC) = rB11; INR (AC) = rB5**


## 2.9 Hardwired vs Microprogrammed Control

| Feature | Hardwired | Microprogrammed |
|---|---|---|
| Implementation | Gates, decoders, flip-flops | Control memory (ROM) storing microprograms |
| Speed | **Faster** | Slower (memory access) |
| Flexibility | Hard to modify (rewiring) | Easy to modify (change microcode) |
| Cost / complexity | High for complex ISA | Lower design effort |
| Used in | RISC | CISC |



## 2.10 Microprogrammed Control

**Terms:**
- **Control word:** bits that select the microoperations in one clock cycle.
- **Microinstruction:** a control word in control memory.
- **Microprogram:** a sequence of microinstructions (it implements one machine instruction).
- **Control memory:** ROM storing microprograms, with a **control address register (CAR)** and **control data register (CDR)**.
- **Microoperation:** an elementary operation in a register.

```
                     External inputs / status / opcode
                              │
                              ▼
 ┌────────────────────┐   ┌──────────────┐   ┌─────────┐   ┌──────────┐
 │ Next-address       │◄──│ Control      │──►│ Control │──►│ Control  │──► to datapath
 │ generator          │   │ address reg  │   │ memory  │   │ data reg │    (control
 │ (sequencer)        │──►│    CAR       │   │ (ROM)   │   │ (CDR)    │     signals)
 └────────────────────┘   └──────────────┘   └─────────┘   └──────────┘
        ▲                                                        │
        └───────────── branch/next address field ◄───────────────┘
```

**Control sequence:**
1. CAR ← address of the first microinstruction (from the opcode mapping).
2. Read microinstruction from control memory into CDR.
3. Execute the microoperations; compute the next address.
4. Repeat until the instruction finishes (then fetch the next).

### Address Sequencing
The next microinstruction address (CAR) is determined by:
1. **Incrementing CAR** (CAR + 1) in normal sequence.
2. **Unconditional branch** (jump to an address in the AD field).
3. **Conditional branch** (based on status bits).
4. **Mapping** from the instruction's opcode bits to a control memory address.
5. **Subroutine call and return** (using a Subroutine Register SBR).

```
 Mapping (opcode → address)           Example mapping (Mano):
 Opcode  4 bits  ──► 0 xxxx 00        opcode ABCD → address 0 ABCD 00
 (gives 4 microinstruction slots per routine)
```

```
                    ┌─────────────────────────────────┐
 IR(11-14) ──►[Mapping logic]──────────►┐            │
 Subroutine reg SBR ───────────────────►│  Multiplexer ──► CAR
 CAR + 1 ──────────────────────────────►│  (selects next address)
 Branch address AD ────────────────────►┘      ▲
 Status bits ─►[Condition select MUX]─►[Branch logic]─┘  (MUX selection S1 S0)
```

### Microinstruction Format (20 bits, Mano)

```
  3     3     3    2    2     7
┌─────┬─────┬─────┬────┬────┬───────┐
│ F1  │ F2  │ F3  │ CD │ BR │  AD   │
└─────┴─────┴─────┴────┴────┴───────┘
 F1,F2,F3 : microoperation fields (3 bits each, up to 7 microops each)
 CD: Condition (00 U=1 always, 01 I, 10 S, 11 Z)
 BR: Branch (00 JMP, 01 CALL, 10 RET, 11 MAP)
 AD: Address field (7 bits → 128 words control memory)
```

**BR field:**

| BR | Symbol | Function |
|---|---|---|
| 00 | JMP | CAR ← AD if condition = 1, else CAR ← CAR + 1 |
| 01 | CALL | CAR ← AD, SBR ← CAR + 1 if condition = 1, else CAR + 1 |
| 10 | RET | CAR ← SBR (return) |
| 11 | MAP | CAR(2-5) ← DR(11-14), CAR(0,1,6) ← 0 |

**Microoperation fields:**

| F1 | Symbol | F2 | Symbol | F3 | Symbol |
|---|---|---|---|---|---|
| 000 | NOP | 000 | NOP | 000 | NOP |
| 001 | ADD (AC←AC+DR) | 001 | SUB | 001 | XOR |
| 010 | CLRAC | 010 | OR | 010 | COM |
| 011 | INCAC | 011 | AND | 011 | SHL |
| 100 | DRTAC | 100 | READ | 100 | SHR |
| 101 | DRTAR | 101 | ACTDR | 101 | INCPC |
| 110 | PCTAR | 110 | INCDR | 110 | ARTPC |
| 111 | WRITE | 111 | PCTDR | 111 | — |

**Fetch routine in microcode** (control memory starts at address 64):

```
 FETCH:  PCTAR              U  JMP  NEXT
         READ, INCPC        U  JMP  NEXT
         DRTAR              U  MAP          ; map opcode → routine
```

**Instruction routine (symbolic) example, ADD:**
```
 ADD:   NOP    I  CALL  INDRCT   ; if I=1 call indirect routine
        READ   U  JMP   NEXT     ; DR ← M[AR]
        ADD    U  JMP   FETCH    ; AC ← AC + DR, go to fetch
```

### Design of Control Unit (microprogram sequencer)

The microprogram sequencer contains:
- **Multiplexer** selecting the next CAR source (CAR+1, AD, SBR, mapped address).
- **Branch logic** (inputs: I, S, Z, and BR field → output MUX select and a load signal for SBR).
- **Incrementer** for CAR + 1.
- **Subroutine register SBR.**

```
 BR field, CD field ──► [Branch Logic] ──► MUX select S1 S0, SBR load
   Status I,S,Z   ───►     

 MUX input:  0: CAR+1 | 1: AD | 2: SBR | 3: mapping
```

Decoders on F1, F2, F3 convert the fields into individual microoperation control signals (e.g., F1 = 101 asserts DRTAR).

**Branch logic truth table (S1 S0 MUX select):**

| BR | Input condition | S1 S0 | L (SBR load) |
|---|---|---|---|
| 00 | 0 | 00 | 0 |
| 00 | 1 | 01 | 0 |
| 01 | 0 | 00 | 0 |
| 01 | 1 | 01 | 1 |
| 10 | X | 10 | 0 |
| 11 | X | 11 | 0 |



# UNIT 3: Central Processing Unit

## 3.1 Introduction

The CPU contains the **ALU**, **register set**, and **control unit**.

```
          ┌──────────────────────────────────┐
          │              CPU                 │
 Memory◄─►│  Registers ◄──► ALU ◄── Control  │◄─► I/O
          │  (PC, IR, AC, GPR) (arith/logic) │
          └──────────────────────────────────┘
```

Functions: fetch instructions, decode, fetch operands, execute, store results, and handle interrupts.



## 3.2 General Register Organization

Seven registers (R1–R7) plus input data, connected through two MUXes to the ALU.

```
 Input ──────────────────────────────────┐
                                         ▼
      ┌────────────────────────────┐  ┌────────┐
 R1 ─►│                            │  │        │
 R2 ─►│  MUX A ──────► A bus ─────►│  │        │
 ...  │                            ├─►│  ALU   ├──► Output (result)
 R7 ─►│  MUX B ──────► B bus ─────►│  │        │       │
      └────────────────────────────┘  └───▲────┘       │
              ▲ SELA   ▲ SELB           OPR             │
              │        │                                ▼
      [3×8 Decoder: SELD]───► load enable of destination register
      result is fed back to all register inputs
```

**Control word (14 bits):**

```
  3      3      3       5
┌──────┬──────┬──────┬────────┐
│ SELA │ SELB │ SELD │  OPR   │
└──────┴──────┴──────┴────────┘
 SELA: source register for A bus (000 = external input)
 SELB: source register for B bus
 SELD: destination register selected by decoder
 OPR: ALU operation
```

**Example:** `R1 ← R2 − R3` → SELA = 010 (R2), SELB = 011 (R3), SELD = 001 (R1), OPR = 00101 (SUB) → Control word = 010 011 001 00101.

The register organization processes one microoperation per clock cycle with no memory access.



## 3.3 Stack Organization

A **stack** is a LIFO structure using a **stack pointer (SP)** that holds the address of the top of stack.

### Register Stack

```
  Stack pointer SP (6 bits)       64 words
      ┌───────┐              ┌────────────────┐ ← Address 63
      │  SP   │──────────►   │      ...       │
      └───────┘              │  top of stack  │ ← SP points here
  FULL, EMPTY flags          │      ...       │
                             └────────────────┘ ← Address 0
 DR = data register
```

**PUSH:**
```
 SP ← SP + 1
 M[SP] ← DR
 If (SP = 0) then (FULL ← 1)
 EMPTY ← 0
```

**POP:**
```
 DR ← M[SP]
 SP ← SP − 1
 If (SP = 0) then (EMPTY ← 1)
 FULL ← 0
```

### Memory Stack (stack in RAM)
Stack grows toward **lower addresses**: SP decrements on push.
```
 PUSH: SP ← SP − 1;   M[SP] ← DR
 POP:  DR ← M[SP];    SP ← SP + 1
```
Memory is divided into a program area, a data area, and a stack area. Stack overflow and underflow checks use limit registers.

```
  Address
  1000  ┌──────────────┐
        │ Program (PC) │
  2000  ├──────────────┤
        │ Data (AR)    │
  3000  ├──────────────┤
        │ Stack (SP) ↓ │  grows toward lower address
  4000  └──────────────┘
```

### Reverse Polish Notation (RPN) / Postfix
| Notation | Example |
|---|---|
| Infix | A + B |
| Prefix (Polish) | + A B |
| **Postfix (RPN)** | A B + |

**Conversion:** (A + B) × (C + D) → AB+ CD+ ×
**Evaluation (using stack):** scan left to right; operand → push; operator → pop two, apply, push result.

```
 Expr: 3 4 + 5 ×   → push 3, push 4, '+' → push 7, push 5, '×' → push 35
```

RPN needs no parentheses, so stack computers (0-address) evaluate expressions efficiently.



## 3.4 Instruction Formats and Types

### Instruction Representation

```
┌─────────────┬──────────────────┬──────────────────┐
│   OPCODE    │  Mode / Register │   Address / Op   │
└─────────────┴──────────────────┴──────────────────┘
```
Fields: **opcode** (operation), **address field(s)** (operands), **mode field** (how to interpret the address).

### Instruction Formats by Number of Addresses
Evaluate **X = (A + B) × (C + D)**:

**Three-address**
```
 ADD R1, A, B       ; R1 ← M[A] + M[B]
 ADD R2, C, D       ; R2 ← M[C] + M[D]
 MUL X, R1, R2      ; M[X] ← R1 × R2
```

**Two-address**
```
 MOV R1, A          ; R1 ← M[A]
 ADD R1, B          ; R1 ← R1 + M[B]
 MOV R2, C
 ADD R2, D
 MUL R1, R2         ; R1 ← R1 × R2
 MOV X, R1
```

**One-address (accumulator)**
```
 LOAD A      ; AC ← M[A]
 ADD B       ; AC ← AC + M[B]
 STORE T     ; M[T] ← AC
 LOAD C
 ADD D
 MUL T       ; AC ← AC × M[T]
 STORE X
```

**Zero-address (stack)**
```
 PUSH A
 PUSH B
 ADD
 PUSH C
 PUSH D
 ADD
 MUL
 POP X
```

| Type | Instructions needed | Features |
|---|---|---|
| 3-address | Fewest (3) | Long instructions, more bits per instruction |
| 2-address | 6 | Common in commercial machines |
| 1-address | 7 | AC implied |
| 0-address | 8 | Stack-based, short instructions |

### Instruction Types (by function)
1. **Data transfer** (LOAD, STORE, MOV)
2. **Data manipulation** (arithmetic, logical, shift)
3. **Program control** (branch, call, return)



## 3.5 Addressing Modes

Addressing modes define how the **effective address (EA)** is obtained from the address field.

| Mode | Syntax | Effective Address / Operand | Notes |
|---|---|---|---|
| **Implied** | `CMA` | Operand is implied (AC, stack top) | No address field |
| **Immediate** | `#5` | **Operand = address field** | No memory access; fast; constant is fixed |
| **Register** | `R1` | Operand = R1 | Short address field; fast |
| **Register indirect** | `(R1)` | EA = R1 | Register holds the address |
| **Autoincrement / Autodecrement** | `(R1)+` / `−(R1)` | EA = R1, then R1 ± 1 | Array/stack processing |
| **Direct** | `ADR` | EA = address field | One memory access |
| **Indirect** | `@ADR` | EA = M[address field] | Two memory accesses; pointers |
| **Relative** | PC + disp | EA = PC + address field | Position-independent code, branches |
| **Indexed** | X(R) | EA = address field + index register XR | Arrays; loops |
| **Base register** | B + disp | EA = base register + address field | Relocation; segments |

**Numerical example:** PC = 200, R1 = 400, XR = 100, address field = 500; memory: M[399] = 450, M[400] = 700, M[500] = 800, M[600] = 900, M[702] = 325, M[800] = 300.

| Mode | EA | Operand |
|---|---|---|
| Direct | 500 | 800 |
| Immediate | 201 (PC points to the operand word) | 500 |
| Indirect | 800 | 300 |
| Relative | 202 + 500 = 702 (PC already incremented) | 325 |
| Indexed | 500 + 100 = 600 | 900 |
| Register | R1 | 400 |
| Register indirect | 400 | 700 |
| Autoincrement | 400 (then R1 = 401) | 700 |
| Autodecrement | 399 (R1 first reduced) | 450 |

```
 Direct:            Indirect:           Indexed:
 [op|ADDR]──►M[ADDR]=operand   [op|ADDR]─►M[ADDR]=EA─►M[EA]=operand   ADDR + XR = EA ─► M[EA]
```



## 3.6 Data Transfer and Manipulation

### Data Transfer Instructions

| Name | Mnemonic |
|---|---|
| Load | LD |
| Store | ST |
| Move | MOV |
| Exchange | XCH |
| Input | IN |
| Output | OUT |
| Push | PUSH |
| Pop | POP |

### Data Manipulation Instructions

**Arithmetic:** INC, DEC, ADD, SUB, MUL, DIV, ADDC (add with carry), SUBB (subtract with borrow), NEG.
**Logical and bit manipulation:** CLR, COM, AND, OR, XOR, CLRC, SETC, COMC, and the bit-level instructions.
**Shift:** SHR, SHL, SHRA, SHLA, ROR, ROL, RORC, ROLC.



## 3.7 Program Control

### Status Register (Flags)

```
 ┌───┬───┬───┬───┐
 │ C │ S │ Z │ V │   Status bit conditions (from ALU)
 └───┴───┴───┴───┘
 C = Carry out;  S = Sign (MSB);  Z = Zero;  V = Overflow
```

### Branch and Jump Instructions

| Mnemonic | Condition |
|---|---|
| BR | Unconditional |
| BZ / BNZ | Z = 1 / Z = 0 |
| BC / BNC | C = 1 / C = 0 |
| BP / BM | S = 0 / S = 1 |
| BV / BNV | V = 1 / V = 0 |
| BHI, BLO, BE ... | Unsigned comparisons |
| BGT, BLT, BGE ... | Signed comparisons |

**CMP** subtracts to set flags without storing the result. **TST** ANDs to set flags.

### Subroutine Call and Return

```
 CALL: SP ← SP − 1; M[SP] ← PC; PC ← EA
 RET:  PC ← M[SP]; SP ← SP + 1
```

### Program Interrupt
| Types | Examples |
|---|---|
| **External** | I/O device, timer, power failure |
| **Internal (traps)** | Overflow, divide by zero, invalid opcode, protection violation |
| **Software** | SVC / INT instruction (system call) |

Steps: save the return address and the **program status word (PSW)** → load ISR address from the interrupt vector → execute ISR → restore PSW and PC (RTI).



## 3.8 RISC vs CISC

| Feature | **RISC** | **CISC** |
|---|---|---|
| Instruction set | Small, simple (~50–100) | Large, complex (200+) |
| Instruction length | Fixed (e.g., 32 bit) | Variable |
| Addressing modes | Few (3–5) | Many (12+) |
| Memory access | **Only LOAD/STORE** (load/store architecture) | Many instructions access memory directly |
| Registers | Large register file | Fewer registers |
| Execution | **1 instruction per clock cycle** (pipelined) | Multiple cycles |
| Control unit | **Hardwired** | Microprogrammed |
| Pipelining | Efficient | Difficult |
| Code size | Larger programs | Smaller programs |
| Compiler role | More work on the compiler | Hardware does more work |
| Examples | ARM, MIPS, SPARC, RISC-V, PowerPC | Intel x86, VAX, Motorola 68000 |

**RISC characteristics:**
1. Relatively few instructions and addressing modes.
2. Memory access limited to load and store.
3. All operations are register to register.
4. Fixed-length, easily decoded instruction format.
5. Single-cycle instruction execution.
6. Hardwired control.
7. Many registers, often with **overlapped register windows**.
8. Efficient instruction pipeline; delayed load and delayed branch.
9. Compiler support for optimization.

**Overlapped register windows:** registers are divided into global, local, and overlapping (input/output) windows. The output registers of the caller become the input registers of the callee, so parameter passing needs no memory copying.

```
 Procedure A:  [Local A][ Out A ]
                          ║ same registers
 Procedure B:        [ In B ][Local B][ Out B ]
```
Number of registers per window = L + 2C + G (L local, C common/overlap, G global).

**RISC pipeline (3-stage):** I: fetch, A: ALU, E: execute/memory.

```
 Clock:     1    2    3    4    5
 Instr 1:   I    A    E
 Instr 2:        I    A    E
 Instr 3:             I    A    E
```
**Hazards:** data hazard (solved by forwarding/stall), control hazard (**delayed branch**: insert NOP or useful instruction after a branch).



## 3.9 Computer Arithmetic

### Representations
- **Signed magnitude:** MSB = sign, remaining = magnitude.
- **1's complement, 2's complement:** negative number = complement of the positive.
- 2's complement range for n bits: −2ⁿ⁻¹ to +2ⁿ⁻¹ − 1.

### Addition and Subtraction (signed magnitude)

| Operation | Add magnitudes | Subtract magnitudes |
|---|---|---|
| (±A) + (±B) | If signs equal | If signs differ |
| (±A) − (±B) | If signs differ | If signs equal |

```
              ┌────────────────┐
              │  Add or Sub?   │
              └───────┬────────┘
       ┌── Add ───────┴────── Sub ──┐
 Signs equal? ─Yes: A + B       Signs different? ─Yes: A + B
       │No                            │No
   Compare A and B                  Compare A and B
  A > B: A − B, sign = A         A > B: A − B, sign = A
  A < B: B − A, sign = B'        A < B: B − A, sign = (A's complement)
  A = B: A − B = 0, sign +       A = B: result 0, sign +
```
**Hardware:** register A (AC), B (BR), adder with complementer controlled by mode M, sign flip-flops As, Bs, overflow flip-flop AVF, and E.

**2's complement addition:** add the numbers including sign bits; ignore the carry out; **overflow** occurs when the carry into the sign bit differs from the carry out of it.
Subtraction: A − B = A + (2's complement of B).

### Multiplication Algorithms

**Signed magnitude (add and shift):**
Registers: BR (multiplicand), QR (multiplier), AC (partial product), E, SC (counter).

```
   START
   Multiplicand in BR, Multiplier in QR
   As ← Qs ⊕ Bs ; Qs ← 0 ; Bs ← 0   (sign handled separately)
   AC ← 0, E ← 0, SC ← n − 1
     │
     ▼
   ┌─── QR0 = 1 ? ───┐
   │Yes              │No
   EA ← AC + BR      │
   AC ← 0, E ← 0     │
   └────► shr EAQ ◄──┘          (shift E,AC,QR right)
   SC ← SC − 1
   SC = 0 ? ─No─► loop
     │Yes
   END   Product in AC and QR
```

### Booth's Multiplication Algorithm (signed 2's complement)

Looks at multiplier bit pairs **(Qₙ, Qₙ₊₁)**; skips runs of 1s.

| Qₙ Qₙ₊₁ | Operation |
|---|---|
| 1 0 | Beginning of a run of 1s → **AC ← AC − BR** |
| 0 1 | End of a run of 1s → **AC ← AC + BR** |
| 0 0 or 1 1 | Middle of a run: no operation |
Then arithmetic shift right **(ashr)** on AC, QR, Qₙ₊₁.

```
   START
   AC ← 0, Qn+1 ← 0, BR ← multiplicand, QR ← multiplier, SC ← n
     │
     ▼
   (Qn, Qn+1) = ?
     10 ─► AC ← AC + BR' + 1  (subtract)
     01 ─► AC ← AC + BR
     00/11 ─► nothing
     │
   ashr (AC, QR, Qn+1)
   SC ← SC − 1
   SC = 0 ? ─No─► loop ;  Yes ─► END
```

**Example: (+7) × (−3), n = 4**
BR = 0111, −BR = 1001, QR = 1101

| Step | Qn Qn+1 | Operation | AC | QR | Qn+1 | SC |
|---|---|---|---|---|---|---|
| Init | | | 0000 | 1101 | 0 | 4 |
| 1 | 10 | AC − BR | 1001 | 1101 | 0 | |
| | | ashr | 1100 | 1110 | 1 | 3 |
| 2 | 01 | AC + BR | 0011 | 1110 | 1 | |
| | | ashr | 0001 | 1111 | 0 | 2 |
| 3 | 10 | AC − BR | 1010 | 1111 | 0 | |
| | | ashr | 1101 | 0111 | 1 | 1 |
| 4 | 11 | no op, ashr | 1110 | 1011 | 1 | 0 |

Result = AC:QR = 1110 1011 = **−21** ✓

**Why Booth is useful:** it handles signed numbers directly and reduces additions when the multiplier has long strings of 1s.

### Array Multiplier
A combinational multiplier using AND gates (partial products) and full adders arranged in an array. For j-bit × k-bit: **j × k AND gates, (j − 1) × k full adders**, and the product has j + k bits. It is fast (one pass) but needs a large amount of hardware.

```
 2-bit × 2-bit:  a1a0 × b1b0
 C0 = a0b0
 C1,C2 from HA of (a1b0, a0b1)
 C3 ... from a1b1 + carry
```

### Division Algorithms

**Unsigned (restoring) division hardware:** Dividend in AC:QR (2n bits), divisor in BR, SC counts n bits.

```
   START
   Dividend in AC, QR; Divisor in BR; SC ← n
   Check overflow: if AC ≥ BR then overflow (DVF ← 1)
     │
     ▼
   shl EAQ  (shift dividend left)
   EA ← AC + BR' + 1       (trial subtraction)
   E = 1 ? (A ≥ B)
     Yes: Q0 ← 1
     No : Q0 ← 0, AC ← AC + BR   (restore)
   SC ← SC − 1
   SC = 0 ? ─No─► loop
     │Yes
   END:  Quotient in QR, Remainder in AC
```

**Restoring vs Non-restoring**

| Restoring | Non-restoring |
|---|---|
| If the trial subtraction is negative, **add the divisor back** (restore) | **No restore.** If the remainder is negative: shift, then **add** divisor in the next step; if positive: shift, then **subtract** |
| 2 operations in worst case per bit | Exactly 1 operation per bit (faster) |
| Final remainder is always correct | Final remainder may need a correction (add the divisor if negative) |

**Non-restoring:**
```
 For each of n steps:
   if remainder sign = 0 (≥ 0):  shift left; subtract divisor; 
   else:                         shift left; add divisor
   quotient bit = 1 if the new remainder ≥ 0, else 0
 At the end: if the remainder < 0 then add divisor (correction)
```

Signed division: perform the division on magnitudes; sign of quotient = Qs = As ⊕ Bs; the remainder takes the sign of the dividend.

### Floating-Point Representation

**Number = ± M × Rᴱ** (mantissa M, exponent E, radix R usually 2).

```
 ┌──────┬──────────────┬───────────────────────┐
 │ Sign │ Exponent     │ Mantissa (fraction)   │
 └──────┴──────────────┴───────────────────────┘
```

**IEEE 754**

| Format | Sign | Exponent | Mantissa | Bias | Total |
|---|---|---|---|---|---|
| Single | 1 | 8 | 23 | 127 | 32 bit |
| Double | 1 | 11 | 52 | 1023 | 64 bit |

Value = (−1)ˢ × **1.M** × 2^(E − bias) (normalized, hidden leading 1).

**Special values (single):**
- E = 0, M = 0 → ±0
- E = 255, M = 0 → ±∞
- E = 255, M ≠ 0 → NaN
- E = 0, M ≠ 0 → denormalized number

**Example:** −6.5 = −110.1₂ = −1.101 × 2² → S = 1, E = 2 + 127 = 129 = 10000001, M = 101 0000… → `1 10000001 10100000000000000000000` = C0D00000₁₆

**Normalization:** the mantissa's leading bit is nonzero (1.xxx). It improves precision. Floating-point **overflow/underflow** occurs when the exponent exceeds or falls below its representable range.

### Floating-Point Arithmetic Operations

**Add / Subtract:** (F₁ = m₁ × 2^e₁, F₂ = m₂ × 2^e₂)

```
   START
   Check for zeros (if an operand = 0, result = the other)
     │
     ▼
   Align mantissas: shift the mantissa of the smaller exponent right,
                    incrementing its exponent until exponents are equal
     │
     ▼
   Add / subtract mantissas (sign-magnitude / 2's complement)
     │
     ▼
   Mantissa overflow? ─Yes─► shift right 1, increment exponent
     │No
   Result zero? ─Yes─► set result to zero, END
     │No
   Normalize: shift left until the leading bit is 1, decrement exponent
     │ (exponent underflow? → result = 0 / flag)
     ▼
   Round (if bits are lost) → END
```

**Multiplication:**
1. Check for zeros.
2. **Add the exponents** (subtract the bias once).
3. **Multiply the mantissas** (fixed-point algorithm).
4. Result sign = S₁ ⊕ S₂.
5. Normalize and round.

**Division:**
1. Check for zeros (divisor zero → error/∞; dividend zero → 0).
2. **Subtract the exponents** (add the bias back).
3. **Divide the mantissas** (fixed-point division).
4. Sign = S₁ ⊕ S₂.
5. Normalize and round.

```
 FP multiplication:                     FP division:
 e = e1 + e2 − bias                     e = e1 − e2 + bias
 m = m1 × m2                            m = m1 / m2
 normalize, round                       normalize, round
```

**Rounding modes (IEEE):** round to nearest even, toward 0, toward +∞, toward −∞. **Guard, round, and sticky bits** are kept to preserve accuracy.



# Quick Revision and Exam Tips

### Must-know formulas
| Item | Formula |
|---|---|
| Full adder | S = A⊕B⊕C, Cout = AB + C(A⊕B) |
| CLA | Gᵢ = AᵢBᵢ, Pᵢ = Aᵢ⊕Bᵢ, Cᵢ₊₁ = Gᵢ + PᵢCᵢ |
| BCD adder correction | K = Cout + S3S2 + S3S1 |
| Overflow (2's complement) | V = C_n ⊕ C_(n−1) |
| 2ⁿ:1 MUX for n+1 var function | n select lines |
| Decoder count | n-to-2ⁿ, each output a minterm |
| Basic computer | 16-bit word, 4K memory, 12-bit address, 3-bit opcode |
| Fetch | T0: AR←PC; T1: IR←M[AR], PC←PC+1; T2: decode |
| Interrupt cycle | RT0: AR←0, TR←PC; RT1: M[AR]←TR, PC←0; RT2: PC←PC+1, IEN←0, R←0 |
| Microinstruction | F1(3) F2(3) F3(3) CD(2) BR(2) AD(7) = 20 bits |
| IEEE single | 1, 8, 23; bias 127 |
| IEEE double | 1, 11, 52; bias 1023 |

### Frequently asked questions
1. Implement a function using a MUX or decoder.
2. Design a full adder, BCD adder, or look-ahead adder.
3. Explain the instruction cycle with a flowchart (fetch, decode, execute, interrupt).
4. Explain the common bus system, memory-reference instructions, and their microoperations.
5. Hardwired vs microprogrammed control; the microinstruction format; address sequencing.
6. Differentiate RISC and CISC.
7. Addressing mode numerical problems (compute the EA and the operand).
8. Convert infix to postfix and evaluate with a stack.
9. Booth's multiplication, a worked trace, and restoring/non-restoring division.
10. Floating-point add/sub flowchart and IEEE 754 conversion.
11. Write assembly (zero/one/two/three-address) for an expression.
12. Logic microoperations (selective set, clear, complement, mask, insert).

### Common mistakes to avoid
- In Booth's algorithm, always use **arithmetic** shift right, and don't forget the Qₙ₊₁ bit.
- Relative addressing uses the **updated PC** (PC after fetch).
- Autodecrement decrements **before** access; autoincrement increments **after**.
- The BSA saves the return address at M[AR] and starts at **AR + 1**.
- ISZ skips the next instruction when the incremented value equals 0.
- Interrupt return address is stored in **location 0**; the first ISR instruction is at **location 1**.
- Overflow is **not** the same as carry for signed numbers.
