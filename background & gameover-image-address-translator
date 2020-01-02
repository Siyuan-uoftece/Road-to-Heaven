module lowVGA(x,y,mem_address);

	parameter RESOLUTION = "160x120";
	input [7:0] x; 
	input [6:0] y;	
	output [14:0] mem_address;
	
	assign mem_address = ({1'b0, y, 7'd0} + {1'b0, y, 5'd0} + {1'b0, x});
	
endmodule 
