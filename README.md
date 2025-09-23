# Flipflop-using-Blocking-and-Non-blocking-Assignments
Exp 3 -Write and simulate D, SR, JK, T Flipflops using Blocking and Non blocking Assignments
   Aim: To design and simulate D, SR, JK, T Flipflops using Blocking and Non blocking Assignments in Verilog HDL and verify its functionality through a testbench using the Vivado 2023.1 simulation environment.
   
  Apparatus Required:
  Vivado 2023.1
Procedure:

Launch Vivado Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu. 
Create a New Project Click on "Create Project" from the Vivado Quick Start window. In the New Project Wizard: Project Name: Enter a name for the project (e.g., Mux4_to_1). 
Project Location: Select the folder where the project will be saved. Click Next. 
Project Type: Select RTL Project, then click Next. Add Sources: Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.). 
Make sure to check the box "Copy sources into project" to avoid any external file dependencies. 
Click Next.
Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation). 
Default Part Selection: You can choose a part based on the FPGA board you are using (if any). 
If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7). 
Click Next, then Finish.
Add Verilog Source Files In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier.
Add the Verilog files and the testbench. 
Check Syntax Expand the "Flow Navigator" on the left side of the Vivado interface. 
Simulate the Design In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation". 
Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench. 
View and Analyze Simulation Results Adjust Simulation Time To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings". Under "Simulation", modify the Run Time (e.g., set to 1000ns).
Generate Simulation Report Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results".
Save the report for reference in your lab records. Save and Document Results Save your project by clicking File → Save Project.
Take screenshots of the waveform window and include them in your lab report to document your results. 
You can include the timing diagram from the simulation window showing the correct functionality of the Seven Segment across different select inputs and data inputs. 
Close the Simulation Once done, by going to Simulation → "Close Simulation

Input/Output Signal Diagram:

D FF

![WhatsApp Image 2025-09-16 at 20 50 10_3d8860d6](https://github.com/user-attachments/assets/6952d974-6763-40a8-a2c8-527670b8f621)


SR FF

![Types-of-flip-flop-and-their-working](https://github.com/user-attachments/assets/311ac779-1eec-435b-a33e-90652f05147d)


JK FF

![Types-of-flip-flop-and-their-working 2](https://github.com/user-attachments/assets/483e799a-70c3-4f83-9894-6d61ecc01617)


T FF

![Types-of-flip-flop-and-their-working 3](https://github.com/user-attachments/assets/72b21f85-26be-408d-9b1d-6106d7bfdc95)


RTL Code:
D SS:
```
module dff_block(clk,rst,d,dout);
    input clk,rst,d;
    output reg dout;
    always@ (posedge clk)
    begin
        if(rst)
            dout = 1'b0;
        else
            dout = d;
     end
endmodule
```
SR FF:
```
module sr_ff (
    input clk,
    input s,
    input r,
    output reg q
);
    initial q = 0;  

    always @(posedge clk) begin
        case ({s, r})
            2'b00: q <= q;      
            2'b01: q <= 0;     
            2'b10: q <= 1;      
            2'b11: q <= 1'bx;  
        endcase
    end
endmodule
```
JK FF:
```
module jk_ff (
    input clk,
    input j,
    input k,
    output reg q
);
    initial q = 0;  

    always @(posedge clk) begin
        case ({j, k})
            2'b00: q <= q;      
            2'b01: q <= 0;     
            2'b10: q <= 1;     
            2'b11: q <= ~q;     
        endcase
    end
endmodule
```
T FF:
```
module jk_ff (
    input clk,
    input j,
    input k,
    output reg q
);
    initial q = 0;  

    always @(posedge clk) begin
        case ({j, k})
            2'b00: q <= q;      
            2'b01: q <= 0;     
            2'b10: q <= 1;     
            2'b11: q <= ~q;     
        endcase
    end
endmodule
```
TestBench:
D SS:
```
module dff_block_tb;
    reg clk_t,rst_t,d_t;
    wire dout_t;
    dff_block dut(.clk(clk_t),.rst(rst_t),.d(d_t),.dout(dout_t));
    initial
      begin
        clk_t = 1'b0;
        rst_t = 1'b1;
     #20
        rst_t = 1'b0;
        d_t   = 1'b0;
     #20
        d_t = 1'b1;
     end
     always 
        #10 clk_t = ~clk_t;
endmodule
```
SR FF:
```
module tb_sr_ff;
    reg clk, s, r;
    wire q;

    sr_ff uut (.clk(clk), .s(s), .r(r), .q(q));
     initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

 initial begin
        $monitor("time=%0t S=%b R=%b Q=%b", $time, s, r, q);

        s=0; r=0; #10;  
        s=1; r=0; #10; 
        s=0; r=1; #10; 
        s=1; r=1; #10; 
        s=0; r=0; #10;  
        $stop;
    end
endmodule
```
JK FF:
```
module tb_jk_ff;
    reg clk, j, k;
    wire q;

    jk_ff uut (.clk(clk), .j(j), .k(k), .q(q));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        $monitor("Time=%0t  J=%b K=%b Q=%b", $time, j, k, q);

        j=0; k=0; #10;
        j=0; k=1; #10;
        j=1; k=0; #10;
        j=1; k=1; #10;
        j=1; k=1; #10;
        $stop;
    end
endmodule
```
T FF:
```
module tb_jk_ff;
    reg clk, j, k;
    wire q;

    jk_ff uut (.clk(clk), .j(j), .k(k), .q(q));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        $monitor("Time=%0t  J=%b K=%b Q=%b", $time, j, k, q);

        j=0; k=0; #10;
        j=0; k=1; #10;
        j=1; k=0; #10;
        j=1; k=1; #10;
        j=1; k=1; #10;
        $stop;
    end
endmodule
```
Output waveform:
D FF
 <img width="1919" height="1199" alt="Screenshot 2025-09-16 162702" src="https://github.com/user-attachments/assets/d71bdc48-51d3-48bb-a264-d9d450ec72b9" />
SR FF
<img width="1919" height="1199" alt="Screenshot 2025-09-16 210620" src="https://github.com/user-attachments/assets/1840b4ae-7dfd-44da-979c-8153f2c76697" />
JK FF
<img width="1919" height="1199" alt="Screenshot 2025-09-16 210808" src="https://github.com/user-attachments/assets/1520b1bb-0649-4472-aeae-c5a0e8c4cf1b" />
T FF
<img width="1919" height="1199" alt="Screenshot 2025-09-16 162953" src="https://github.com/user-attachments/assets/5a5feda0-708e-43ab-8af4-a738784ff53a" />

Conclusion:
Flip-flops are fundamental sequential logic elements used to store one bit of data in digital circuits. The SR flip-flop sets or resets the output based on its inputs, while the JK flip-flop eliminates the invalid state of SR and also supports toggling of the output. The T flip-flop is a simplified version of the JK flip-flop that toggles its output whenever the T input is high. The D flip-flop ensures predictable output by directly transferring the D input to the output on the clock edge. All these flip-flops operate on clock edges to provide synchronized data storage and transfer. They play a vital role in designing registers, counters, and memory units. Understanding their operation is essential for building reliable and efficient digital systems. Overall, these flip-flops form the basic building blocks of sequential logic circuits.

