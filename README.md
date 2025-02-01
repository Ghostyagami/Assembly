# AVR Assembly Simulation Guide with SimAVR and AVR-GDB

## Introduction
This guide explains how to assemble, link, and simulate AVR assembly code using **SimAVR** and **AVR-GDB** on macOS (or Linux). These tools allow you to debug and test AVR programs without requiring a physical microcontroller.

---

## Prerequisites
Ensure you have the necessary tools installed:

### **Install Required Packages (macOS/Linux)**
```bash
brew install avr-gcc avr-binutils avr-gdb simavr
```
If using Linux:
```bash
sudo apt install gcc-avr binutils-avr gdb-avr simavr
```

---

## **1. Writing AVR Assembly Code**
Create an AVR assembly file named `program.asm`.

### **Example: Simple AVR Assembly Code**
```assembly
; Simple AVR Program

.org 0x00       ; Reset vector (execution starts here)
rjmp start      ; Jump to the main program

start:
    ldi r16, 10  ; Load 10 into register r16
    ldi r17, 5   ; Load 5 into register r17
    add r18, r16 ; r18 = r16 + r17 (10 + 5)
    rjmp start   ; Infinite loop
```

---

## **2. Assembling and Linking the Code**
To assemble and link the AVR code into an ELF executable:

### **Assemble the Code**
```bash
avr-as -mmcu=atmega328p -o program.o program.asm
```

### **Link the Object File**
```bash
avr-ld -o program.elf program.o
```

Alternatively, using **AVR-GCC**:
```bash
avr-gcc -mmcu=atmega328p -nostartfiles -o program.elf program.asm
```

---

## **3. Simulating the AVR Code using SimAVR**
Run the ELF file in **SimAVR**:
```bash
simavr -m atmega328p program.elf
```

or with **GDB Debugging enabled**:
```bash
simavr -m atmega328p -g program.elf
```

This starts a simulation and listens for AVR-GDB on port **1234**.

---

## **4. Debugging with AVR-GDB**
To debug the ELF file with **AVR-GDB**:

### **Start AVR-GDB**
```bash
avr-gdb program.elf
```

### **Connect to SimAVR**
Inside GDB, run:
```gdb
target remote :1234   # Connects to SimAVR
```

### **Debugging Commands in GDB**
| Command | Description |
|---------|-------------|
| `break start` | Set a breakpoint at `start` |
| `run` | Start the program |
| `stepi` | Step through instructions one by one |
| `info registers` | View register values |
| `layout asm` | View assembly code |
| `continue` | Continue execution |
| `quit` | Exit GDB |

---

## **5. Using AVR-GDB and SimAVR Together**

### **Step 1: Start SimAVR with Debugging**
```bash
simavr -m atmega328p -g program.elf
```
This starts the simulator and opens a debugging port (default: `1234`).

### **Step 2: Launch AVR-GDB and Connect to SimAVR**
```bash
avr-gdb program.elf
```
Inside GDB, run:
```gdb
target remote :1234  # Connect GDB to SimAVR
```

### **Step 3: Set Breakpoints and Run Debugging Commands**
```gdb
break start  # Set a breakpoint at the start of the program
run          # Run the program
stepi        # Step through instructions one by one
info registers  # View register values
layout asm   # Show assembly instructions
continue     # Resume execution until the next breakpoint
quit         # Exit GDB
```

---

## **6. Summary of Workflow**
1. **Write AVR assembly code** (`program.asm`).
2. **Assemble and link** into an ELF file.
3. **Run in SimAVR** (`simavr -m atmega328p program.elf`).
4. **Enable GDB debugging** (`simavr -m atmega328p -g program.elf`).
5. **Attach AVR-GDB** (`target remote :1234`).
6. **Use GDB commands to debug and inspect execution**.

---

## **Conclusion**
With **SimAVR** and **AVR-GDB**, you can develop and debug AVR assembly code **without needing a physical microcontroller**. This setup is perfect for testing AVR code before flashing it onto an actual device.

Would you like an example with **I/O operations (LED toggling)** next? 🚀

