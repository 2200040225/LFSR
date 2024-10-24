# LFSR

Design code :

module lfsr(
input clk,            
input reset,          
output reg [3:0] q   
);
wire feedback;           
assign feedback = q[3] ^ q[2];
always @(posedge clk or posedge reset) begin
    if (reset) begin
        q <= 4'b0001;
    end else begin
        q <= {q[2:0], feedback}; 
    end
end
endmodule

Test bench:

module tb_lfsr;
reg clk;
reg reset;
wire [3:0] q;
lfsr uut (
    .clk(clk),
    .reset(reset),
    .q(q)
);
always begin
    #10 clk = ~clk;  
end
initial begin
    clk = 0;
    reset = 1;
    #20;
    reset = 0; 
    #200;
    reset = 1;
    #20;
    reset = 0;
    #100;
    $stop;
end
initial begin
    $monitor("Time: %0t | Reset: %b | LFSR output (q): %b", $time, reset, q);
end

endmodule

