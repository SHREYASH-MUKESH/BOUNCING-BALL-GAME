#include <windows.h>
#include <stdio.h>
#include <stdbool.h>
#include<conio.h>



void gotoxy(int x, int y)
{
	COORD coord;
	coord.X = x;
	coord.Y = y;
	HANDLE consoleHandle = GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleCursorPosition(consoleHandle, coord);
}
int main()
{
		int racket_size = 10;
		int racket_color = 178;
		int ball_color = 'O'; 
		int erase_color = ' ';
		int max_x = 79;
		int min_x = 0;
		int max_y = 25;
		int min_y = 0;

		int ball_velocity_x = 1;
		int ball_velocity_y = 1;

		int ball_coord_x = max_x / 2;
		int ball_coord_y = max_y;

		int racket_length = 0;

		int Life = 2;

		bool is_game_time = true;
		while (is_game_time)
		{
			////draw Life////////
			gotoxy( (max_x/ 2)-2, max_y +2);
			printf("Life: %d", Life);

			////my name//////
			gotoxy((max_x / 2)-4, max_y + 4);
			printf("Game by Shreyash");

			///drawing of racket/////
			
			for (int x = racket_length; x < racket_length+ racket_size; x++)
			{
				gotoxy(x, max_y);
				printf("%c", racket_color);
			}

			///////draw the ball/////////////
			gotoxy(ball_coord_x, ball_coord_y);
			printf("%c", ball_color);

			///////dealy between drawings/////////////

			Sleep(100);

			/////erase the ball and update location////////
			gotoxy(ball_coord_x, ball_coord_y);
			printf("%c", erase_color);

			if ((ball_coord_y == max_y)||(ball_coord_y==min_y))
			{
				ball_velocity_y = -1 * ball_velocity_y;
			}
			if ((ball_coord_x == max_x) || (ball_coord_x == min_x))
			{
				ball_velocity_x = -1 * ball_velocity_x;
			}

			ball_coord_x += ball_velocity_x;
			ball_coord_y += ball_velocity_y;

			
			////check for bypassing of rackets////////
			bool is_ball_at_bottom = (ball_coord_y == max_y);
			if (is_ball_at_bottom)
			{
				bool is_at_bottom = (ball_coord_y == max_y);
				int relevant_racket_height = is_at_bottom ? racket_length : racket_length;
				bool did_hit_the_racket = (relevant_racket_height <= ball_coord_x) && ((relevant_racket_height + racket_size) >= ball_coord_x);
				if (!did_hit_the_racket)
				{
					//update the Life//

					if (is_at_bottom)

						Life--;
					
					Sleep(2000);
				}
				
			}
			///////erase the rackets://///

			for (int x = racket_length; x < racket_length + racket_size; x++)
			{
				gotoxy(x, max_y);
				printf("%c", erase_color);
			}
			
			/////////when lost exit game//////////////
			if (Life<0)
			{
			
				gotoxy((max_x / 2) - 2, max_y/2);
				printf("YOU LOST THE GAME\n");
				gotoxy((max_x / 2) - 3, (max_y / 2)+2);
                printf("Thanks For Playing\n");

				is_game_time = false;
			}


			///handle keyboard
			if (_kbhit())
			{
				int key = _getch();
				switch (key)
				{
					////////////handle the racket/////////
				case'z':
					//////////move left;///////////////
					racket_length = racket_length == min_x ? min_x : racket_length - 3;
					break;
				case'm':
					/////////move the left racket down;///////////////////
					racket_length = (racket_length + racket_size) > max_x ? racket_length : racket_length + 3;
					break;
					
			

					/////////////quit game//////
				case'Q':
					is_game_time = false;

				}

			}

		}
		return 0;
			
}
