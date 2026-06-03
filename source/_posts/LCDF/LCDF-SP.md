---
title: Verilog
date: 2026-05-25 15:24:31
mathjax: true
categories: Logic and Computer Design Fundamentals
---

# Verilog Syntax

整数数值表示方法
数字声明时，合法的基数格式有 4 中，包括：十进制('d 或 'D)，十六进制('h 或 'H)，二进制（'b 或 'B），八进制（'o 或 'O）。数值可指明位宽，也可不指明位宽。

```verilog
// BINARY representations
3'b010; // size=3 bits, binary, value=2 (010 in binary)
8'b1111_0000; // size=8 bits, binary, value=240 (underscore for readability)

// DECIMAL representations
3'd2; // size=3 bits, decimal, value=2
8'd234; // size=8 bits, decimal, value=234
8'D234; // Legal to use uppercase 'D'

// HEXADECIMAL representations
8'h70; // size=8 bits, hex, value=0x70 (decimal 112)
9'h1FA; // size=9 bits, hex, value=0x1FA (decimal 506)
32'hFACE_47B2; // size=32 bits, underscore separates for readability

// OCTAL representations
4'o12; // size=4 bits, octal, value=12 (octal) = 10 (decimal)

// All represent the same value (decimal 10):
4'hA = 4'd10 = 4'b1010 = 4'o12
```

Numbers without a base_format specification are decimal numbers by default. Numbers without a size specification have a default number of bits depending on the type of simulator and machine (typically 32 bits)
```verilog
integer a = 5423; // No base format -> treated as decimal 5423
integer b = 'h1AD7; // No size -> defaults to machine width (usually 32 bits)
 // Value stored: 32'h0000_1AD7

reg [15:0] c = 16; // Unsized decimal 16 assigned to 16-bit register
 // Stored as: 16'h0010
```
负数表示

通常在表示位宽的数字前面加一个减号来表示负数。例如：
```verilog
-6'd15  
-15
```
-15 在 5 位二进制中的形式为 5'b10001, 在 6 位二进制中的形式为 6'b11_0001。

## Sections of Verilog Code
All behavior code should be described within the keywords module and endmodule. Rest of the design code would mostly follow the given template.

Verilog sections template

1. Module definition and port list declaration
2. List of input and output ports
3. Declaration of other signals using allowed Verilog data types
4. Design may depend on other Verilog modules and hence their instances are created by module instantiations
5. The actual Verilog design for this module that describes its behavior

例如D触发器
```verilog
// "dff" is the name of this module

module dff ( 	input 	d, 			// Inputs to the design should start with "input"
						rstn,
						clk,
				output	q); 		// Outputs of the design should start with "output"

	reg q; 							// Declare a variable to store output values

	always @ (posedge clk) begin 	// This block is executed at the positive edge of clk 0->1
		if (!rstn) 				// At the posedge, if rstn is 0 then q should get 0
			q <= 0;
		else
			q <= d; 				// At the posedge, if rstn is 1 then q should get d
	end
endmodule 							// End of module
```

## Testbench Code
Testbench code
The testbench is the Verilog container module that allows us to drive the design with different inputs and monitor its outputs for expected behavior. In the example shown below, we have instantiated the flop design illustrated above and connected it with testbench signals denoted by tb_*. These testbench signals are then assigned certain values and are eventually driven as inputs to the design.

```verilog
module tb;

	// 1. Declare input/output variables to drive to the design
	reg 	tb_clk;
	reg 	tb_d;
	reg 	tb_rstn;
	wire 	tb_q;

	// 2. Create an instance of the design
	// This is called design instantiation
	dff 	dff0 ( 	.clk 	(tb_clk), 		// Connect clock input with TB signal
					.d 		(tb_d), 		// Connect data input with TB signal
					.rstn 	(tb_rstn), 		// Connect reset input with TB signal
					.q 		(tb_q)); 		// Connect output q with TB signal

	// 3. The following is an example of a stimulus
	// Here we drive the signals tb_* with certain values
	// Since these tb_* signals are connected to the design inputs,
	// the design will be driven with the values in tb_*
	initial begin
		tb_rstn 	<= 	1'b0; // Start with reset asserted (active-low)
		tb_clk 		<= 	1'b0; // Initialize clock to 0
		tb_d 		<= 	1'b0; // Initialize data input to 0
	end
endmodule
```

## Verilog Hello World
```verilog
// Single line comments start with double forward slash "//"
// Verilog code is always written inside modules, and each module represents a digital block with some functionality
module tb;

  // Initial block is another construct typically used to initialize signal nets and variables for simulation
	initial
		// Verilog supports displaying signal values to the screen so that designers can debug whats wrong with their circuit
		// For our purposes, we'll simply display "Hello World"
		$display ("Hello World !");
endmodule
```

A module called tb with no input-output ports act as the top module for the simulation. The initial block starts and executes the first statement at time 0 units. $display is a Verilog system task used to display a formatted string to the console and cannot be synthesized into hardware. Its primarily used to help with testbench and design debug. In this case, the text message displayed onto the screen is "Hello World !".

## Operator
```verilog
// Unary operator: operates on one operand
x = ~y; // ~ is a unary operator (bitwise NOT), y is the operand

// Binary operator: operates on two operands
x = y | z; // | is a binary operator (bitwise OR), y and z are operands

// Ternary operator: conditional expression with 3 operands
x = (y > 5) ? w : z; // ?: is ternary operator
 // Operand 1: (y > 5) - condition
 // Operand 2: w - value if true
 // Operand 3: z - value if false
```

## Strings
```verilog
"Hello World!" // String with 12 characters -> requires 12 bytes (96 bits)
"x + z" // String with 5 characters (spaces count!)

// String storage in register
reg [8*12:1] message = "Hello World!"; // 12 characters x 8 bits = 96 bits

// ILLEGAL - string cannot span multiple lines
"How are you
feeling today ?" // Syntax error: newline not allowed in string
```

# Verilog Module
## Syntax
{% callout success %}
A ``module`` is a block of Verilog code that implements a certain functionality. Modules can be embedded(嵌入) within other modules and a higher level module can communicate with its lower level modules using their input and output ports. They are the fundamental building blocks of digital hardware design, enabling modular, reusable, and hierarchical design methodologies
{% endcallout %}

```verilog
// Basic module syntax with port list
module <name> ([port_list]);
	// Variable declarations (wires, regs, integers, etc.)
	// Dataflow statements (assign, always, initial blocks)
	// Function and task definitions
	// Sub-module instantiations
endmodule

// A module can have an empty portlist (typically for testbenches)
module testbench_top;
	// Testbench logic without external ports
endmodule
```

依旧D触发器的例子
```verilog
// Module called "dff" has 3 inputs and 1 output port
module dff ( 	input 			d,       // Data input
							input 			clk,     // Clock signal
							input 			rstn,    // Active-low asynchronous reset
							output reg	q);      // Registered output (must be 'reg' for sequential logic)

	// Sequential logic block: executes on every positive clock edge
	always @ (posedge clk) begin
		if (!rstn)             // Reset condition (active-low: rstn=0 triggers reset)
			q <= 0;              // Non-blocking assignment: reset output to 0
		else
			q <= d;              // Non-blocking assignment: capture input data
	end
endmodule
```
![](img/LCDF/sp/sp1-1.png)

{% callout info::What is the purpose of a module ? %} 
A module represents a design unit that implements certain behavioral characteristics and will get converted into a digital circuit during synthesis. Any combination of inputs can be given to the module and it will provide a corresponding output. This allows the same module to be reused to form bigger modules that implement more complex hardware.
{% endcallout %}

例如，D触发器可以构成移位寄存器
```verilog
// 4-bit shift register built from four DFF module instances
module shift_reg ( 	input 	d,       // Serial data input
										input 	clk,     // Clock signal shared by all DFFs
										input 	rstn,    // Reset signal shared by all DFFs
										output 	q);      // Serial data output (final stage)

	// Internal wires connecting adjacent DFF stages
	wire [2:0] q_net;  // q_net[0] = u0 output, q_net[1] = u1 output, q_net[2] = u2 output

	// Module instantiation using named port mapping
	dff u0 (.d(d),          .clk(clk), .rstn(rstn), .q(q_net[0]));  // Stage 0: input -> q_net[0]
	dff u1 (.d(q_net[0]),   .clk(clk), .rstn(rstn), .q(q_net[1]));  // Stage 1: q_net[0] -> q_net[1]
	dff u2 (.d(q_net[1]),   .clk(clk), .rstn(rstn), .q(q_net[2]));  // Stage 2: q_net[1] -> q_net[2]
	dff u3 (.d(q_net[2]),   .clk(clk), .rstn(rstn), .q(q));         // Stage 3: q_net[2] -> output

endmodule
```

## Top-Level Modules
A top-level module is one which contains all other modules. A top-level module is not instantiated within any other module.

### Design Top Level
The design code shown below has a top-level module called ``design``. This is because it contains all other sub-modules requried to make the design complete. The submodules can have more nested sub-modules like mod3 inside mod1 and mod4 inside ``mod2``. Anyhow, all these are included into the top level module when ``mod1`` and ``mod2`` are instantiated. So this makes the design complete and is the top-level module for the design.

```verilog
//---------------------------------
// Leaf-level modules (lowest in hierarchy)
//---------------------------------
module mod3 ( [port_list] );
	reg c;                     // Internal register storage
	// Low-level functional logic
endmodule

module mod4 ( [port_list] );
	wire a;                    // Internal wire connection
	// Low-level functional logic
endmodule

//---------------------------------
// Mid-level modules (contain leaf modules)
//---------------------------------
module mod1 ( [port_list] );    // Mid-level module containing mod3 instances
	wire y;                       // Internal signal within mod1

	// Instantiate two mod3 modules with unique instance names
	mod3 mod_inst1 ( ... );       // First mod3 instance
	mod3 mod_inst2 ( ... );       // Second mod3 instance (reusing mod3 module)
endmodule

module mod2 ( [port_list] );    // Mid-level module containing mod4 instances
	// Instantiate two mod4 modules with unique instance names
	mod4 mod_inst1 ( ... );       // First mod4 instance
	mod4 mod_inst2 ( ... );       // Second mod4 instance (reusing mod4 module)
endmodule

//---------------------------------
// Top-level module (root of hierarchy)
//---------------------------------
module design ( [port_list] );  // TOP-LEVEL: contains all design sub-modules
	wire _net;                    // Internal signal for inter-module communication

	// Instantiate mid-level modules (which contain mod3/mod4 instances)
	mod1 mod_inst1 ( ... );       // mod1 instance (brings in 2x mod3 instances)
	mod2 mod_inst2 ( ... );       // mod2 instance (brings in 2x mod4 instances)
endmodule
// Design hierarchy: design -> {mod1, mod2} -> {mod3, mod3, mod4, mod4}
```

### Testbench Top Level
The testbench module contains stimulus to check functionality of the design and is primarily used for functional verification using simulation tools. Hence the design is instantiated and called ``d0`` inside the testbench module. From a simulator perspective, ``testbench`` is the top level module.

```verilog
//-----------------------------------------------------------
// Testbench code - TOP-LEVEL MODULE for simulation
// From simulation perspective, this is the root of the hierarchy
// because 'design' module is instantiated within this module
//-----------------------------------------------------------
module testbench;                             // No ports (testbench is self-contained)

	// Instantiate the design-under-test (DUT)
	design d0 ( [port_list_connections] );     // Connect testbench signals to design ports

	// Testbench components:
	// - Clock generation (initial/always blocks)
	// - Stimulus generation (initial blocks with test vectors)
	// - Response checking (always blocks with assertions)
	// - Waveform dumping ($dumpfile, $dumpvars)
endmodule
// Simulation hierarchy: testbench -> design -> {mod1, mod2} -> {mod3, mod3, mod4, mod4}
```

### Hierarchical Name
A hierarchical structure is formed when modules can be instantiated inside one another, and hence the top level module is called the root. **Since each lower module instantiation within a given module is required to have different identifier names**, there will not be any ambiguity in accessing signals. A hierarchical name is constructed by a list of these identifiers separated by dots ``.`` for each level of the hierarchy. Any signal can be accessed within any module using the hierarchical path to that particular signal.

```verilog
// Hierarchical path examples using the design structure above

// Access module instances (intermediate nodes in hierarchy tree)
design.mod_inst1                    // Access mod1 instance within design module
design.mod_inst2.mod_inst1          // Access first mod4 instance within mod2

// Access signals through hierarchical paths (leaf nodes)
design.mod_inst1.y                  // Access wire "y" inside mod1 instance
design.mod_inst2.mod_inst2.a        // Access wire "a" within second mod4 instance

// Access design signals from testbench scope
testbench.d0._net                   // Access wire "_net" within design module from TB
testbench.d0.mod_inst1.mod_inst2.c  // Access reg "c" in second mod3 instance from TB

// Hierarchical paths enable cross-module signal probing in:
// - Assertions (checking internal design state)
// - Waveform dumps (selective signal dumping)
// - Forced signals for debug ($force, force/release)
```

{% callout warning %} 
Module definitions (the structural template) cannot be nested inside other modules. Verilog only supports module instantiation (creating instances of already-defined modules). Attempting to define a module inside another causes "illegal nested module definition" errors.

Best Practice: Define all modules separately at the top level of your file. Inside parent modules, only instantiate pre-defined modules: ``dff u0 (.d(data), .clk(clk), .q(output));``
{% endcallout %}

# Verilog Ports
>Ports are a set of signals that act as inputs and outputs to a particular module and are the primary way of communicating with it. Think of a module as a fabricated chip placed on a PCB and it becomes quite obvious that the only way to communicate with the chip is through its pins. Ports are like pins and are used by the design to send and receive signals from the outside world.


|Port Type|Description Use|Cases|
|:-:|:-:|:-:|
|input|The design module can only receive values from outside using its input ports|Clock signals, data inputs, control signals, reset|
|output|The design module can only send values to the outside using its output ports|Computed results, status flags, data outputs|
|inout|The design module can either send or receive values using its inout ports (bidirectional)|Memory data buses, I2C/SPI data lines, tri-state buses|

Ports are by default considered as nets of type ```wire```. If you need an output port to be driven by a register (from an ```always``` block), you must explicitly declare it as ```output reg```.

## Syntax
```verilog
module my_design (
  input wire        clk,        // Clock input (explicit wire type)
  input             en,         // Enable input (implicit(暗示的) wire type)
  input             rw,         // Read/Write control (implicit wire)
  inout [15:0]      data,       // 16-bit bidirectional data bus
  output            int         // Interrupt output (implicit wire)
);

  // Design behavior as Verilog code

endmodule
```
>Each port must have a unique identifier within a module.

## Signed Ports
Use signed ports when performing arithmetic that involves negative numbers, such as digital signal processing (DSP), coordinate calculations, or signed comparisons. For example, a 2's complement signed 4-bit value can represent -8 to +7, while unsigned represents 0 to 15

If either the net/reg declaration or the port declaration has a ```signed``` attribute, then the signal shall be considered signed.

signed就意味着所有数都用补码形式表示
其中正数的补码就是原码，例如5的补码表示就是0101
用补码形式表示-5是1011
求法是正数表示取反+1(0101->1010, 1010+1=1011)
或者$2^N-X$(2^4-5=11=1011)

## Complete & Incomplete Declarations
If a port declaration includes a net or variable type (``wire``, ``reg``), then that port is considered to be completely declared. It is illegal to redeclare the same port in a net or variable type declaration

```verilog
module test (
  input  [7:0] a,                      // No type specified - incomplete declaration
  output [7:0] e                       // No type specified - incomplete declaration
);

  reg [7:0] e;                         // OKAY - net_type was not declared before

  // Rest of the design code
endmodule
```

# Verilog Module Instantiations
Bigger and complex designs are built by integrating multiple modules in a hierarchical manner. Modules can be ***instantiated*** within other modules and ports of these ***instances*** can be connected with other signals inside the parent module.

These port connections can be done via an ordered list or by name.

## Syntax
One method of making the connection between the port expressions listed in a module instantiation with the signals inside the parent module is by the ***ordered list***.

``mydesign`` is a ``module`` instantiated with the name ``d0`` in another module called ``tb_top``. Ports are connected in a certain order which is determined by the position of that port in the port list of the module declaration. For example, ``b`` in the testbench is connected to ``y``of the design simply because both are at the second position in the list of ports.

```verilog
module mydesign ( input x, y, z, // x is at position 1, y at 2, x at 3 and
 output o); // o is at position 4

endmodule

module tb_top;
	wire [1:0] a;
	wire b, c;

	mydesign d0 (a[0], b, a[1], c); // a[0] is at position 1 so it is automatically connected to x
	 // b is at position 2 so it is automatically connected to y
	 // a[1] is at position 3 so it is connected to z
	 // c is at position 4, and hence connection is with o
endmodule
```

A better way to connect ports is by explicitly linking ports on both the sides using their ***port name***.

The dot ``.`` indicates that the port name following the dot belongs to the design. The signal name to which the design port has to be connected is given next within parentheses ``( )``.

```verilog
module design_top;
	wire [1:0] a;
	wire b, c;

	mydesign d0 ( .x (a[0]), // signal "x" in mydesign should be connected to "a[0]" in this module (design_top)
	 .y (b), // signal "y" in mydesign should be connected to "b" in this module (design_top)
	 .z (a[1]),
	 .o (c));
endmodule
```

## Unconnected/Floating Ports
Ports that are not connected to any wire in the instantiating module will have a value of high-impedance(阻抗).

```verilog
module shift_reg ( input d,
 input clk,
 input rstn,
 output q);

 wire [2:0] q_net;

 dff u0 (.d(d), .clk(clk), .rstn(rstn), .q(q_net[0]));
 dff u1 (.d(q_net[0]), .clk(clk), .rstn(rstn), .q()); 						// Output q is left floating
 dff u2 (.d(q_net[1]), .clk(clk), .rstn(rstn), .q()); 						// Output q is left floating
 dff u3 (.d(q_net[2]), .clk(clk), .rstn(rstn), .q(q));

endmodule
```
仿真中，未连接的输入/输出端口会被赋予高阻态Z，综合后，未连接的输出会被悬空；未连接的输入可能会被接地（拉低）或保持高阻，取决于工具

All port declarations are implicitly declared as ``wire`` and hence the port direction is sufficient in that case. However ``output`` ports that need to store values should be declared as ``reg`` data type and can be used in a procedural block like ``always`` and ``initial`` only.

Ports of type input or inout cannot be declared as ``reg`` because they are being driven from outside continuously and should not store values, rather reflect the changes in the external signals as soon as possible. It is perfectly legal to connect two ports with varying vector sizes, but the one with lower vector size will prevail and the remaining bits of the other port with a higher width will be ignored.

```verilog
// Case #1 : Inputs are by default implicitly declared as type "wire"
module des0_1	(input wire clk ...); 		// wire need not be specified here
module des0_2 	(input clk, ...); 			// By default clk is of type wire

// Case #2 : Inputs cannot be of type reg
module des1 (input reg clk, ...); 		// Illegal: inputs cannot be of type reg

// Case #3: Take two modules here with varying port widths
module des2 (output [3:0] data, ...);	// A module declaration with 4-bit vector as output
module des3 (input [7:0] data, ...); 	// A module declaration with 8-bit vector as input

module top ( ... );
	wire [7:0] net;
	des2 u0 ( .data(net) ... ); 		// Upper 4-bits of net are undriven
	des3 u1 ( .data(net) ... );
endmodule

// Case #4 : Outputs cannot be connected to reg in parent module
module top_0 ( ... );
	reg [3:0] data_reg;

	des2 ( .data(data) ...); 	// Illegal: data output port is connected to a reg type signal "data_reg"
endmodule
```