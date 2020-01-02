module car
	(
		CLOCK_50,	          //	On Board 50 MHz
		// Your inputs and outputs here
		KEY,	
		SW,
		GPIO_0,
		HEX0,
		HEX1, 
		LEDR,// On Board Keys
		
    // The ports below are for the VGA output.  Do not change.
		VGA_CLK,   		//	VGA Clock
		VGA_HS,	          //	VGA H_SYNC
		VGA_VS,	          //	VGA V_SYNC
		VGA_BLANK_N,      //	VGA BLANK
		VGA_SYNC_N,        //	VGA SYNC
		VGA_R,   		//	VGA Red[9:0]
		VGA_G,	 	//	VGA Green[9:0]
		VGA_B   		//	VGA Blue[9:0]
	);
  
	input [2:0]SW;
	input [0:0]GPIO_0;
	input	CLOCK_50;	           //	50 MHz
	input	[3:0]	KEY;		
	output [6:0]HEX0,HEX1;
	output [8:0]LEDR;
	// Declare your inputs and outputs here
	// Do not change the following outputs
	output	VGA_CLK;   		//	VGA Clock
	output	VGA_HS;		//	VGA H_SYNC
	output	VGA_VS;		//	VGA V_SYNC
	output	VGA_BLANK_N;	//	VGA BLANK
	output	VGA_SYNC_N;	//	VGA SYNC
	output	[7:0]	VGA_R;   	//	VGA Red[7:0] Changed from 10 to 8-bit DAC
	output	[7:0]	VGA_G;	 //	VGA Green[7:0]
	output	[7:0]	VGA_B;         //	VGA Blue[7:0]
	
	wire resetn;
	assign resetn = ~KEY[3];
	wire game_start;
	assign game_start = ~KEY[0];
	wire jump;
	assign jump = GPIO_0[0];
	wire restart;
	assign restart = ~KEY[2];
	wire [2:0]difficulty;
	assign difficulty[2:0] = SW[2:0];
	
	// Create the colour, x, y and writeEn wires that are inputs to the controller.
	reg [2:0]colour;
	wire [2:0]colourBackground,colourCar,colourPerson,colourGameOver;
	reg [7:0]x;
	reg [6:0]y;
	wire [7:0]xCar,xPerson,xBackground,xShift,xGameOver;
	wire [6:0]yCar,yPerson,yBackground,yShift,yGameOver;
	wire [2:0]carType;
	wire go;
	wire done_plot_background,done_divide,done_plot_car,done_plot_person,done_clearScreen,
		go_drawBackground,go_drawCar,go_drawPerson,go_divide,go_clearScreen,go_updateCar,go_updatePerson,go_draw_gameOver;
	reg gameOver;
	initial gameOver = 0;
	
	car_control(resetn,restart,gameOver,CLOCK_50,game_start,go,done_plot_background,done_divide,done_plot_car,done_plot_person,done_clearScreen,
					go_drawBackground,go_drawCar,go_drawPerson,go_divide,go_clearScreen,go_updateCar,go_updatePerson,go_draw_gameOver);
	
	drawBackground drawB(restart,gameOver,CLOCK_50,go_drawBackground,go_clearScreen,xBackground,yBackground,colourBackground,done_plot_background);
	
	drawCar drawC(restart,gameOver,CLOCK_50,go_drawCar,carType,xShift,xCar,yCar,colourCar,done_plot_car);
	
	drawPerson drawP(restart,gameOver,CLOCK_50,go_drawPerson,yShift,xPerson,yPerson,colourPerson,done_plot_person);
	
	waitDraw w(CLOCK_50,go_divide,go,difficulty,done_divide);
	
	updatePerson p(restart,gameOver,CLOCK_50,go_updatePerson,jump,yShift,go);
	
	updateCar c(restart,gameOver,CLOCK_50,go_updateCar,xShift,carType);
	
	always@(*) begin
	
		if(xCar == xPerson && yCar == yPerson) gameOver <= 1;
		else gameOver <= 0;

	end
	
	drawGameOver(restart,gameOver,CLOCK_50,go_draw_gameOver,xGameOver,yGameOver,colourGameOver);
 	
	reg [7:0]score;
	
	always@(negedge go or posedge restart) begin
		if(restart) score <= 0;
		else begin
		
			If (difficulty == 3'b000) score <= score + 1;
			else if	(difficulty == 3'b001) score <= score + 2;
			else if	(difficulty == 3'b010) score <= score + 3;
			else if	(difficulty == 3'b011) score <= score + 4;
			else if	(difficulty == 3'b100) score <= score + 5;
			else if	(difficulty == 3'b101) score <= score + 6;
			else if	(difficulty == 3'b110) score <= score + 7;
			else if	(difficulty == 3'b111) score <= score + 8;
		end		
	end
	
	hex_decoder upper(score[7:4],HEX1);
	hex_decoder lower(score[3:0],HEX0);
	
	wire enable;
	
	assign enable = go_drawBackground | go_drawCar | go_drawPerson | go_clearScreen | go_draw_gameOver;
	
	always@(*) begin
	
		if(go_drawBackground || go_clearScreen ) begin		
				colour <= colourBackground;
				x <= xBackground;
				y <= yBackground;
			
		end
		
		else if(go_drawCar) begin
			if(colourCar == 3'b111) begin
				colour <= colourBackground;
				x <= xBackground;
				y <= yBackground;
			 end
			else begin
				colour <= colourCar;
				x <= xCar;
				y <= yCar;
			end
		end
		
		else if(go_drawPerson) begin
			if(colourPerson == 3'b111) begin
				colour <= colourBackground;
				x <= xBackground;
				y <= yBackground;
			end
			else begin
				colour <= colourPerson;
				x <= xPerson;
				y <= yPerson;
			end
		end
		
		else if(go_draw_gameOver) begin
			colour <= colourGameOver;
			x <= xGameOver;
			y <= yGameOver;
		end	 
	end
		
	// Create an Instance of a VGA controller - there can be only one!
	// Define the number of colours as well as the initial background
	// image file (.MIF) for the controller.
	vga_adapter VGA(
			.resetn(!resetn),
			.clock(CLOCK_50),
			.colour(colour),
			.x(x),
			.y(y),
			.plot(enable),
			/* Signals for the DAC to drive the monitor. */
			.VGA_R(VGA_R),
			.VGA_G(VGA_G),
			.VGA_B(VGA_B),
			.VGA_HS(VGA_HS),
			.VGA_VS(VGA_VS),
			.VGA_BLANK(VGA_BLANK_N),
			.VGA_SYNC(VGA_SYNC_N),
			.VGA_CLK(VGA_CLK));
		defparam VGA.RESOLUTION = "160x120";
		defparam VGA.MONOCHROME = "FALSE";
		defparam VGA.BITS_PER_COLOUR_CHANNEL = 1;
		defparam VGA.BACKGROUND_IMAGE = "cover.mif";
			
endmodule
