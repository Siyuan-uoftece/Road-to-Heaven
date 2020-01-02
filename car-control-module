module car_control(
	input resetn,restart,game_over,clock,game_start,go,
	input done_plot_background,done_divide,done_plot_car,done_plot_person,done_clearScreen,
	output reg go_drawBackground,go_drawCar,go_drawPerson,go_divide,go_clearScreen,go_updateCar,go_updatePerson,go_draw_gameOver);

parameter			reset = 0,
				drawBackground = 1,
				drawCar = 2,
				drawPerson = 3,
				waitDraw = 4,
				clearScreen = 5,
				updateCar = 6,
				updatePerson = 7,
				gameOver = 8;
				
			
				
reg [3:0]current_state,next_state;
				
always@(*)
begin : state_table
	case (current_state)
			reset : next_state = (game_start||restart) ? drawBackground : reset;
			drawBackground : next_state = done_plot_background ? drawCar : drawBackground;	
			drawCar : next_state = done_plot_car ? drawPerson : drawCar;
			drawPerson : next_state = done_plot_person ? waitDraw : drawPerson;
			waitDraw : next_state = done_divide ? clearScreen : waitDraw;
			clearScreen : next_state = done_plot_background ? updateCar : clearScreen;
			updateCar : begin
					if(go) next_state = updatePerson;
					else next_state = drawCar;
end
			updatePerson : next_state = drawCar;
			gameOver : next_state = restart ? drawBackground : gameOver;
				
	endcase
end	
 
always@(*)
begin : enable_signals	
				
	case (current_state)
	
		reset : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 0;
					go_draw_gameOver = 0;
				  end	
		
		drawBackground : begin
					go_drawBackground = 1;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 0;
					go_draw_gameOver = 0;
				  end
		
		
		drawCar : begin
					go_drawBackground = 0;
					go_drawCar = 1;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 0;
					
				  end
				  
		
		drawPerson : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 1;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 0;
					
				  end
				  
		
		waitDraw : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 1;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 0;
					
				  end
				 			 		  
		
		clearScreen : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 1;
					go_updateCar = 0;
					go_updatePerson = 0;
					
				  end
					
		updateCar : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 1;
					go_updatePerson = 0;
					go_draw_gameOver = 0;
				  end
				 			  
		updatePerson : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 1;
					go_draw_gameOver = 0;
				  end
				  
		gameOver : begin
					go_drawBackground = 0;
					go_drawCar = 0;
					go_drawPerson = 0;
					go_divide = 0;
					go_clearScreen = 0;
					go_updateCar = 0;
					go_updatePerson = 0;
					go_draw_gameOver = 1;
				  end
								
	endcase
end
				

always@ (posedge clock)
	begin : state_FFs
		if (resetn)
			current_state<=reset;	
		else if(game_over)
			current_state<=gameOver;
		else 
			current_state<=next_state;			
	end
endmodule 
