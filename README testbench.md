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
    
    for(i=0;i<256;i=i+1) begin
      d = i;
      exp_y = 3'b000;
      for(j=7;j>=0;j=j-1)begin
        if(d[j])begin
          exp_y = j;
          j = -1;
        end
      end
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
        $display("[CHECK] error");
        err= err+1;
      end else begin
        $display("[CHECK] matching");
      end
    end
  endtask
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule
                
  
