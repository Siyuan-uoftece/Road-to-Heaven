//============================================================
module drawBackground(
	input reset,gameOver,clock,go_drawBackground,go_clearScreen,
	output reg [7:0]x,
	output reg [6:0]y,
	output [2:0]colour,
	output reg done_drawBackground
);

	initial x = 0;
	initial y = 0;
				

parameter width = 160,
			 height = 120;
			 
wire [14:0]address;

lowVGA(x,y,address);
background back(address,clock,colour);

always@(posedge clock) begin
	if(reset||gameOver) begin
		x = 0;
		y = 0;
	end
	if(!go_drawBackground) done_drawBackground = 0;
	
	if(go_drawBackground || go_clearScreen) begin
		x = x+1;
		if(x == width) begin
			x = 0;
			y = y+1;
		end
		
		
		if(y == height) begin
			y = 0;
			done_drawBackground = 1;
		end
	end
end

endmodule

//============================================================
module drawCar(
	input reset,gameOver,clock,go_drawCar,
	input [2:0]carType,
	input [7:0]xShift,
	output signed [7:0]x,
	output [6:0]y,
	output reg [2:0]colour,
	output reg done_drawCar
);

reg[5:0] width = 40;
reg[5:0] height;
reg[6:0]yShift;

			 
wire [10:0]address;
wire [2:0]colourRaceCar,colourPolice,colourTruck,colourAnotherRace,colourTank,colourJiao,colourJail,colourDig;
reg [5:0]xAddress;
reg [4:0]yAddress;
racecarSprite(xAddress,yAddress,address);
racecar (address,clock,colourRaceCar);
policecar (address,clock,colourPolice);
truck (address,clock,colourTruck);
another (address,clock,colourAnotherRace);
tank (address,clock,colourTank);
jiao (address,clock,colourJiao);
jail (address,clock,colourJail);
dig (address,clock,colourDig);



always@(posedge clock) begin
	if(carType == 3'b000) begin
		height <= 10;
		colour <= colourRaceCar;
		yShift <= 61;
	end
	else if(carType == 3'b001) begin
		height <= 20;
		colour <= colourPolice;
		yShift <= 52;
	end
	else if(carType == 3'b010) begin
		height <= 30;
		colour <= colourDig;
		yShift <= 42;
		
	end
	else if(carType == 3'b011) begin
		height <= 30;
		colour <= colourTank;
		yShift <= 42;
	end
	else if(carType == 3'b100) begin
		height <= 15;
		colour <= colourAnotherRace;
		yShift <= 58;
	end
	else if(carType == 3'b101) begin
		height <= 30;
		colour <= colourJiao;
		yShift <= 45;
	end
	else if(carType == 3'b110) begin
		height <= 30;
		colour <= colourJail;
		yShift <= 45;
	end
	else if(carType == 3'b111) begin
		height <= 20;
		colour <= colourTruck;
		yShift <= 52;
	end
		
end
		

	
always@(posedge clock) begin
	if(reset||gameOver) begin
			xAddress = 0;
			yAddress = 0;
		end
		
	else begin	
		if(!go_drawCar) done_drawCar = 0;
		
		if(go_drawCar) begin
			xAddress = xAddress+1;
			if(xAddress == width) begin
				xAddress = 0;
				yAddress = yAddress+1;
			end
		
			if(yAddress == height) begin
				yAddress = 0;
				done_drawCar = 1;
			end
		end
	end
end

assign x = xShift -40 + xAddress;
assign y = yShift + yAddress;


endmodule	
//============================================================

module drawPerson(
	input reset,gameOver,clock,go_drawPerson,
	input [6:0]yShift,
	output [7:0]x,
	output [6:0]y,
	output [2:0]colour,
	output reg done_drawPerson
);

parameter width = 24,
			 height = 24;
			 
wire [10:0]address;
reg [4:0]xAddress;
reg [4:0]yAddress;
wire [2:0]colourStand;
wire [2:0]colourJump;

standSprite(xAddress,yAddress,address);
jump tiao(address,clock,colourJump);
stand zhan(address,clock,colourStand);

assign colour = (yShift >= 45 && yShift <=50) ? colourStand : colourJump;

always@(posedge clock) begin
	if(reset||gameOver) begin
			xAddress = 0;
			yAddress = 0;
		end
	if(!go_drawPerson) done_drawPerson = 0;
	
	if(go_drawPerson) begin
		xAddress = xAddress+1;
		if(xAddress == width) begin
			xAddress = 0;
			yAddress = yAddress+1;
		end
		
		if(yAddress == height) begin
			yAddress = 0;
			done_drawPerson = 1;
		end
	end
end

assign x = 120 + xAddress;
assign y = yShift + yAddress;

endmodule
//============================================================

module waitDraw(
	input clock,go_divide,jump,
	input [2:0]difficulty,
	output reg done_divide
	);
	reg [26:0] limit;
	reg [26:0] Q;
	
	always@(*) begin
		case (difficulty) 
			3'b000: limit <= 808888;
			3'b001: limit <= 708888;
			3'b010: limit <= 608888;
			3'b011: limit <= 508888;
			3'b100: limit <= 408888;
			3'b101: limit <= 308888;
			3'b110: limit <= 208888;
			3'b111: limit <= 148888;
		endcase
	end

	always@(posedge clock) begin		
		Q <= limit;
		 if(go_divide) begin
			if(Q<=0) begin
					done_divide <= 1;
					Q<=limit;
			end
			else begin
				done_divide <= 0;
				Q<=Q-1;
			end
		end
	end
	
endmodule
//============================================================

module updatePerson(
	input reset,gameOver,clock,go_update_person,jump,
	output reg [6:0]y,
	output reg enable
);
	parameter  yMin=1,
				  yMax=50;
	reg up = 0, down = 1;
	initial y=yMax;
	
		
	
	always@(posedge clock) begin
			if(reset||gameOver) begin
				y = yMax;
				enable = 0;
			end
			
			if(jump) enable = 1;
			
			if(go_update_person) begin
				if(down && y>yMin) y=y-1;
				if(y==yMin && down) begin
					down=0;
					up=1;
					y=y-1;				
				end
				
				if(y<yMax && up) y=y+1;
				if(y==yMax) begin
					down=1;
					up=0;
					enable = 0;
				end
			end
		end
					
	
endmodule

//============================================================	
	
module updateCar(
	input reset,gameOver,clock,go_update_car,
	output reg  [7:0]x,
	output reg  [2:0]carType
);

	parameter  xMin=0,
				  xMax=199;
				  
	
	
	always@(posedge clock) begin
			if(reset||gameOver) begin
				x <= xMin;
				carType = 3'b000;
			end
			
			if(go_update_car) begin
				if(x<xMax) x<=x+1;
				if(x==xMax) begin 
					x<=xMin;
					carType <= carType + 1;
				end
			end
	end
	
endmodule
//============================================================	

module hex_decoder(hex_digit, segments);
    input [3:0] hex_digit;
    output reg [6:0] segments;
   
    always @(*)
        case (hex_digit)
            4'h0: segments = 7'b100_0000;
            4'h1: segments = 7'b111_1001;
            4'h2: segments = 7'b010_0100;
            4'h3: segments = 7'b011_0000;
            4'h4: segments = 7'b001_1001;
            4'h5: segments = 7'b001_0010;
            4'h6: segments = 7'b000_0010;
            4'h7: segments = 7'b111_1000;
            4'h8: segments = 7'b000_0000;
            4'h9: segments = 7'b001_1000;
            4'hA: segments = 7'b000_1000;
            4'hB: segments = 7'b000_0011;
            4'hC: segments = 7'b100_0110;
            4'hD: segments = 7'b010_0001;
            4'hE: segments = 7'b000_0110;
            4'hF: segments = 7'b000_1110;   
            default: segments = 7'h7f;
        endcase
endmodule
//=============================================================
module drawGameOver(
	input reset,gameOver,clock,go_draw_gameOver,
	output reg [7:0]x,
	output reg [6:0]y,
	output [2:0]colour
);

	initial x = 0;
	initial y = 0;
				

parameter width = 160,
			 height = 120;
			 
wire [14:0]address;

lowVGA(x,y,address);
gameover over(address,clock,colour);

always@(posedge clock) begin
	if(reset||gameOver) begin
		x = 0;
		y = 0;
	end
	
	if(go_draw_gameOver) begin
		x = x+1;
		if(x == width) begin
			x = 0;
			y = y+1;
		end
		
		if(y == height) begin
			y = 0;
		end
	end
end

endmodule
