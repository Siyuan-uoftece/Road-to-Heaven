
module racecarSprite(x, y, mem_address);

	parameter RESOLUTION = "40x10";
	input [5:0] x; 
	input [4:0] y;	
	output [10:0] mem_address;
	
	assign mem_address =({1'b0, y, 5'd0} + {1'b0, y, 3'd0} + {1'b0, x});
	
endmodule 
