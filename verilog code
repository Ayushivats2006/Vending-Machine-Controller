module vending_machine(input clk,reset, input [4:0] in, output reg out,output reg [3:0] change);
  
  parameter S0 = 4'b0000, S1 = 4'b0001,S2 = 4'b0010, S3 = 4'b0011,S4 = 4'b0100, S5 = 4'b0101,S6 = 4'b0110, S7 = 4'b0111,S8 = 4'b1000;
  
  reg [3:0] current_state,next_state;
  
  always@(posedge clk or posedge reset)
    begin
      if(reset)
        current_state<=S0;
      else
        current_state<=next_state;
    end
  
  always@(*)
    begin
      next_state=current_state;
      out=0;
      change=4'd0;
      case(current_state)
        S0:
          if(in==5'd5)
            begin
              next_state=S1;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S2;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S4;
            out=0;
            change=4'd0;
          end
        S1:
          if(in==5'd5)
            begin
              next_state=S2;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S3;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S5;
            out=0;
            change=4'd0;
          end
        S2:
          if(in==5'd5)
            begin
              next_state=S3;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S4;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S6;
            out=0;
            change=4'd0;
          end
        S3:
          if(in==5'd5)
            begin
              next_state=S4;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S5;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S7;
            out=0;
            change=4'd0;
          end
        S4:
          if(in==5'd5)
            begin
              next_state=S5;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S6;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S8;
            out=0;
            change=4'd0;
          end
        S5:
          if(in==5'd5)
            begin
              next_state=S6;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S7;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S0;
            out=1;
            change=4'd0;
          end
        S6:
          if(in==5'd5)
            begin
              next_state=S7;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S8;
            out=0;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S0;
            out=1;
            change=4'd5;
          end
        S7:
          if(in==5'd5)
            begin
              next_state=S8;
              out=0;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S0;
            out=1;
            change=4'd0;
          end
        else if(in==5'd20)
          begin
            next_state=S0;
            out=1;
            change=4'd10;
          end
        S8:
          if(in==5'd5)
            begin
              next_state=S0;
              out=1;
              change=4'd0;
            end
        else if(in==5'd10)
          begin
            next_state=S0;
            out=1;
            change=4'd5;
          end
        else if(in==5'd20)
          begin
            next_state=S0;
            out=1;
            change=4'd15;
          end
        default:
          begin
            next_state=S0;
            out=0;
            change=4'd0;
          end
      endcase
    end
endmodule

        
          
