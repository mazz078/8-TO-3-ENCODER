# 8-to-3 Encoder using Verilog

## 📌 Project Overview

This project implements an **8-to-3 Encoder** using Verilog HDL.

An encoder is a combinational digital circuit that converts one active input among multiple input lines into a binary-coded output.

An 8-to-3 encoder has:

* 8 input lines
* 3 output lines
* 1 valid output signal

For normal operation, only one input should be HIGH at a time.

---

## 🎯 Objective

The objective of this project is to design and verify an 8-to-3 Encoder using Verilog HDL.

The project demonstrates:

* Encoder operation
* Binary encoding
* Combinational logic
* Verilog `case` statements
* Input validation
* Testbench-based verification

---

## 🛠️ Technologies Used

* Verilog HDL
* VS Code
* Icarus Verilog
* ModelSim / QuestaSim
* GTKWave (optional)
* GitHub

---

## 📂 Project Structure

```text
8-to-3-Encoder/
│
├── README.md
├── src/
│   └── encoder_8to3.v
│
├── testbench/
│   └── tb_encoder_8to3.v
│
└── simulation/
    └── simulation_results.txt
```

---

## 🔢 Inputs and Outputs

| Signal | Width | Description                           |
| ------ | ----: | ------------------------------------- |
| D      | 8-bit | Encoder input                         |
| Y      | 3-bit | Encoded binary output                 |
| VALID  | 1-bit | Indicates a valid single active input |

---

## ⚙️ Truth Table

| Input D    | Output Y | VALID |
| ---------- | -------- | ----: |
| `00000001` | `000`    |     1 |
| `00000010` | `001`    |     1 |
| `00000100` | `010`    |     1 |
| `00001000` | `011`    |     1 |
| `00010000` | `100`    |     1 |
| `00100000` | `101`    |     1 |
| `01000000` | `110`    |     1 |
| `10000000` | `111`    |     1 |
| `00000000` | `000`    |     0 |

---

## 🧠 Working Principle

The encoder identifies which input line is HIGH and produces the corresponding binary value.

For example:

```text
D = 00100000
```

Here, `D5` is HIGH.

Therefore:

```text
Y = 101
```

So:

```text
D5 → 101
```

---

## 🚦 VALID Signal

The `VALID` output indicates whether exactly one valid input is active.

For example:

```text
D = 00010000
```

gives:

```text
Y     = 100
VALID = 1
```

But:

```text
D = 00000000
```

gives:

```text
Y     = 000
VALID = 0
```

Similarly, multiple active inputs such as:

```text
D = 00000011
```

are treated as invalid.

---

## 🧪 Testbench

The testbench verifies:

* D0 through D7
* No active input
* Multiple active inputs

A total of **10 test cases** are included.

---

## ▶️ Simulation Using Icarus Verilog

Open the terminal inside the project directory.

### Compile

```bash
iverilog -o encoder_sim src/encoder_8to3.v testbench/tb_encoder_8to3.v
```

### Run

```bash
vvp encoder_sim
```

---

## 📋 Expected Output

```text
================================================
             8-TO-3 ENCODER TEST
================================================
 INPUT       | OUTPUT | VALID
-----------------------------------------------
00000001     | 000    | 1
00000010     | 001    | 1
00000100     | 010    | 1
00001000     | 011    | 1
00010000     | 100    | 1
00100000     | 101    | 1
01000000     | 110    | 1
10000000     | 111    | 1
00000000     | 000    | 0
00000011     | 000    | 0
================================================
```

---

## 📚 Concepts Demonstrated

* Encoder
* Binary encoding
* Combinational circuits
* Truth tables
* `case` statement
* Validity checking
* Module design
* Testbench development
* Functional verification

---

## 🚀 Applications

Encoders are used in:

* Digital keyboards
* Interrupt controllers
* Data compression
* Priority systems
* Digital communication
* Processor control logic
* Multiplexed systems

---

## ⚠️ Limitation

A basic encoder assumes that only one input is active at a time.

If multiple inputs are active simultaneously, the input is considered invalid.

A **Priority Encoder** can be used when multiple inputs may be active.

---

## 🚀 Future Improvements

This project can be extended to:

* 4-to-2 Encoder
* 16-to-4 Encoder
* 8-to-3 Priority Encoder
* Parameterized Encoder
* FPGA implementation

---

## 👩‍💻 Author

**Harshu**

B.Tech - Electronics and Communication Engineering

---

## 📄 License

This project is created for educational and academic purposes.
```verilog
`timescale 1ns/1ps

module encoder_8to3 (
    input  wire [7:0] d,
    output reg  [2:0] y,
    output reg        valid
);

    always @(*) begin

        // Default values
        y     = 3'b000;
        valid = 1'b0;

        case (d)

            8'b00000001: begin
                y     = 3'b000;
                valid = 1'b1;
            end

            8'b00000010: begin
                y     = 3'b001;
                valid = 1'b1;
            end

            8'b00000100: begin
                y     = 3'b010;
                valid = 1'b1;
            end

            8'b00001000: begin
                y     = 3'b011;
                valid = 1'b1;
            end

            8'b00010000: begin
                y     = 3'b100;
                valid = 1'b1;
            end

            8'b00100000: begin
                y     = 3'b101;
                valid = 1'b1;
            end

            8'b01000000: begin
                y     = 3'b110;
                valid = 1'b1;
            end

            8'b10000000: begin
                y     = 3'b111;
                valid = 1'b1;
            end

            default: begin
                y     = 3'b000;
                valid = 1'b0;
            end

        endcase

    end

endmodule
```
```verilog
`timescale 1ns/1ps

module tb_encoder_8to3;

    reg  [7:0] d;
    wire [2:0] y;
    wire       valid;

    // Instantiate Design Under Test
    encoder_8to3 DUT (
        .d(d),
        .y(y),
        .valid(valid)
    );

    task test_input;
        input [7:0] test_d;

        begin
            d = test_d;
            #10;

            $display(
                "INPUT=%b | OUTPUT=%b | VALID=%b",
                d, y, valid
            );
        end
    endtask

    initial begin

        $display("================================================");
        $display("             8-TO-3 ENCODER TEST");
        $display("================================================");
        $display(" INPUT       | OUTPUT | VALID");
        $display("-----------------------------------------------");

        // D0 active
        test_input(8'b00000001);

        // D1 active
        test_input(8'b00000010);

        // D2 active
        test_input(8'b00000100);

        // D3 active
        test_input(8'b00001000);

        // D4 active
        test_input(8'b00010000);

        // D5 active
        test_input(8'b00100000);

        // D6 active
        test_input(8'b01000000);

        // D7 active
        test_input(8'b10000000);

        // No input active
        test_input(8'b00000000);

        // Multiple inputs active
        test_input(8'b00000011);

        $display("================================================");

        $finish;

    end

endmodule
```
# 8-TO-3 ENCODER SIMULATION RESULTS

## INPUT       | OUTPUT | VALID

00000001     | 000    | 1
00000010     | 001    | 1
00000100     | 010    | 1
00001000     | 011    | 1
00010000     | 100    | 1
00100000     | 101    | 1
01000000     | 110    | 1
10000000     | 111    | 1
00000000     | 000    | 0
00000011     | 000    | 0

================================================
Simulation completed successfully.
==================================
