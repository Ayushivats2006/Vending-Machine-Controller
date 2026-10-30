module tb;
  reg clk,reset;
  reg [4:0] in;
  wire out;
  wire [3:0] change;
  
  vending_machine uut(clk,reset,in,out,change);
  
  initial
    begin
      clk=0;
      forever #5 clk=~clk;
    end
  
  always@(posedge clk)
    begin
      $display("Time = %0t || Current State = %0d Next State = %0d || Input Amount = %d || Output = %b Change = %d",$time,uut.current_state,uut.next_state,in,out,change);
    end
  
  initial
    begin
      $dumpfile("dump.vcd");
      $dumpvars(0,tb); 
      
      reset=1;
      in=0;
      
      #10 reset=0;
      
      //Case 1
      $display("\n========== CASE 1 ==========");
      @(negedge clk) in=20;
      @(negedge clk) in=0;
      @(negedge clk) in=10;
      @(negedge clk) in=0;
      @(negedge clk) in=10;
      @(negedge clk) in=0;
      @(negedge clk) in=5;
      @(negedge clk) in=0;
      #20 reset=0;
      
      //Case 2
      $display("\n========== CASE 2 ==========");
      @(negedge clk) in=20;
      @(negedge clk) in=0;
      @(negedge clk) in=10;
      @(negedge clk) in=0;
      @(negedge clk) in=10;
      @(negedge clk) in=0;
      @(negedge clk) in=10;
      @(negedge clk) in=0;
      #20 reset=0;
      
      //Case 3
      $display("\n========== CASE 3 ==========");
      repeat(9)
        begin
          @(negedge clk) in =5;
          @(negedge clk) in=0;
        end
      
      #20 $finish;
    end
endmodule
