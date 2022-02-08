# Lab2RomanLeone
module Lab1(I,clock,reset,F,S);
  input clock, reset, I;
    output reg F;
    output reg [2:0] S;

    parameter
        S0=3'b000,
        S1=3'b001,
        S2=3'b010,
        S3=3'b011,
        S4=3'b100;


    reg [2:0] CS,NS;

    always @(posedge clock)
    begin
        if(reset==1)
            CS <= S0;// when reset=1, reset the CS of the FSM to "S0" CS
        else
            CS <= NS; // otherwise, next CS
    end

    always @(CS,I)
    begin
        case(CS)
        S0:begin
        NS = (I==1)? S1:S0;
        end

        S1:begin
        NS = (I==1)? S1:S2;
        end

        S2:begin
        NS = (I==1)? S1:S3;
        end

        S3:begin
        NS = (I==1)? S4:S0;
        end

        S4:begin
        NS = (I==1)? S1:S2;
        end

        default:NS = S0;
        endcase
    F = (CS==S4)? 1:0;
    S = CS;
    end


endmodule


//test bench code 
module Lab1_testbench();

reg clock, reset, I;
    wire F;
    wire [2:0] S;

    Lab1 uut (.I(I), .clock(clock), .reset(reset), .F(F), .S(S));

    initial
        clock = 1'b0;
    always begin
        #5 clock = (clock == 0)? 1:0;
    end

    initial begin
         reset = 1'b0;
        I = 1'b0;
        #14 I = 1'b1;
          #10 I = 1'b0;
        #10 I = 1'b0;
        #10 I = 1'b1;
        #10 I = 1'b0;
        #10 I = 1'b0;
        #10 I = 1'b1;
        #10 I = 1'b0;
        #10 I = 1'b0;
        #10 I = 1'b1;
        #10 I = 1'b0;
        #10 I = 1'b0;
        #10 I = 1'b0;
        #10 I = 1'b1;
        #10 I = 1'b0;

          
    end

    initial begin
        #1000 $stop;
    end

endmodule
