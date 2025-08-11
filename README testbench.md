```verilog
`timescale 1ps/1ps
module tb_priority_encoder_8_3;
  
  reg [7:0]d;
  reg [2:0]exp_y;
  wire [2:0]y;
  integer i,j,err=0;
  
  priority_encoder_8_3 DUT (d,y);
  
  initial begin
    $monitor("time=%0t, d=%b, y=%b | exp_y=%b",$time,d,y,exp_y);
    
    for(i=7;i>=0;i=i-1) begin
      d = 8'b1 << i ;
      exp_y = i;
      #5;
      check(y,exp_y);
    end
    
    if(err==0)begin
      $display ("---------");
      $display ("Test pass");
      $display ("----------");
    end else begin
      $display ("---------");
      $display ("Test fail with %d error",err);
      $display ("----------");
    end
    $finish;
  end
  
  task check(input [2:0]y, input [2:0]exp_y);
    begin
      if(y!==exp_y)begin
        $display("[CHECK] Error");
        err= err+1;
      end else begin
        $display("[CHECK] Matching");
      end
    end
  endtask
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
                
  
