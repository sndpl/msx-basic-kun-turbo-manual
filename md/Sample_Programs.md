# ☆ ★ ☆ About the sample programs ☆ ★ ☆


This manual explains how to use the sample program that comes with "MSX-BASIC Kun Turbo". For the basic usage, please refer to "[About MSX-BASIC Kun Turbo Part 1 (Basic](MSX-BASIC_Kun_Part_1.md)". For information on how to use the instructions, please refer to "[About MSX-BASIC Kun Turbo Part 2 (Orders)](MSX-BASIC_Kun_Part_2.md)".In addition, please refer to "[About MSX-BASIC Kun Turbo Part 3 (Application)](MSX-BASIC_Kun_Part_3.md)" for precautions when using "MSX-BASIC Kun Turbo" and advanced usage.

MSX is a trademark of ASCII.  
MSX-DOS is a registered trademark of Microsoft Corporation.


## Introduction

Sample programs are included in the "MSX-BASIC Kun Turbo" disc. Please use it to test the power of "MSX-BASIC Kun Turbo" and to use it as a reference for programming.  
It is recommended to back up the disk before using these sample programs.

> Caution
>
> 1. The sample programs included with "MSX-BASIC Kun Turbo" are for individuals.
You may copy or modify it as long as you can use it. However, copying,
Distribution of modified sample programs is prohibited.
> 2. When executing the following sample program, garbage may be displayed on the screen. This is a sprite. If you see this garbage, please execute the command.  
    
        VDP (9) = VDP (9) OR 2  

>This instruction prohibits sprite display. In addition, sprite display permission / non-permission cannot be set in SCREEN statements.
To display the sprite again, execute.

        VDP (9) = VDP (9) AND &HFD  



## Sample A: Maze  
  
*Program name*: MAZE.BAS  
*Compatible models*: MSX2 + / MSX turbo R  
### Commentary  
Execute with the "RUN" command. First, you will be asked how to make a maze, so please answer by number. When the maze is complete, press the spacebar. Next, I will ask you how to solve it. Please enter a number for this as well. If you select "Solve by yourself (3D)", use the cursor keys to move from the start point to the goal. Press the spacebar to scroll horizontally to reveal the 2D maze. Your position is represented by a red square. Press the spacebar again to return to the original 3D maze.
The cursor keys are  
[↑]… Advance  
[→] ... Go to the right  
[←]… Go to the left  
Is assigned to. On the 2D screen, the up direction is north, the down direction is south, the right direction is east, and the left direction is west.

### Algorithm  
- High-speed type
Consider one rectangle. Then, draw a line from the center to either the top, bottom, left, or right. (Figure 1)
This is the basis of this algorithm. Now try doing the same thing by arranging two of these squares side by side (Figure 2). One thing to keep in mind here is that the two walls must not overlap. When the walls overlap, a so-called "island" wall that does not connect to the outer frame is created. Now spread it further vertically. (Fig. 3).


        + - - - +                   + - - - + - - - +
        |       |                   |               |
        |  * →  |                   |   * - - → *   |
        |       |                   |           ↓   |
        + - - - +                   + - - - + - - - +
        Figure 1                    Figure 2


        + - - - + - - - +           + - - - + - - - +
        |               |           |               |
        |   * - - → *   |           |   * - - → *   |
        |           |   |           |   ↑       |   |
        +           |   +           +   |       |   +
        |           ↓   |           |   |       ↓   |           
        | ← *       * → |           |   * ← - - *   |
        |               |           |               |
        + - - - + - - - +           + - - - + - - - +
        Figure 3                    Figure 4


    Another point to note is the case shown in Fig. 4. In order not to create such an "island", we have a restriction that "the line must not be extended upward after the second stage".
    The above is this algorithm. In the actual program, line 350 is "Which way do you want to extend?", Lines 370 to 430 are "Is the wall overlapping?", And line 370 "Y = 8 + K" is "Top". Is it a stage? "

- Wall extension type
The algorithm is simpler than the high-speed type, but the program is a little complicated.
As before, this algorithm also sets a reference point in the square first (Fig. 5). Then stretch the line as much as you can from point to point (Fig. 6). When this line hits the wall leading to the outer frame, it begins to extend from the next origin (Fig. 7). If you are surrounded by the line you have drawn (Fig. 8), return to the first point and start drawing the line again.


        + - - - - - - - +           + - - - - - - - +
        |               |           |               |
        |   *   *   *   |           |   * → *   * → |
        |               |           |       ↓   ↑   |
        |   *   *   *   |           |   *   * → *   |
        |               |           |               |
        |   *   *   *   |           |   *   *   *   |
        |               |           |               |
        + - - - - - - - +           + - - - - - - - +
        Figure 5                    Figure 6


        + - - - - - - - +           + - - - - - - - - -
        |               |           |               
        |   * - +   + - |           |   * → * → * → * 
        |       |   |   |           |   |            
        |   *   + - +   |           |   + - + - +   * 
        |   ↓           |           |           |    
        |   * → * → *   |           |   + → *   +   *
        |           ↓   |           |   |       |     
        + - - - - - - - +           |   + - + - +   *
        Figure 7                    Figure 8
                

    Including the case of (Fig. 7), if the stretched line hits the wall connected to the outer frame, make the line you just drawn a "wall connected to the outer frame" and perform the same process from the next base point.
    In the actual program, the line that is currently being stretched is displayed in red, and the wall connected to the outer frame is displayed in white.

### How to solve  
We are using a modified version of the so-called "left method". While the left method always turns to the left, this program turns randomly. A line is drawn with color code 1 (set to black) on the dead end road. Therefore, the same road will never be taken again.

### Tips for porting to MSX2
This program is for MSX2 + / MSX turbo R, so it will not work on MSX2 as it is. A particular problem is screen switching by side-scrolling on lines 2430 to 2570. This part can also be executed on MSX2 by using the SET PAGE instruction.  
You also need to change the Japanese characters. This is the case with the CALL KANJI instruction and the Kanji message in the PRINT statement. If you have Kanji BASIC (built into "Japanese Cartridge" (Sony), "Japanese MSX-DOS2" (ASCII), etc.), you do not need to change the Kanji part.

### Variable table  
AL maze creation algorithm selection  
D The direction you are heading now  
FLG3D Shows whether to solve by yourself or with MSX  
K road width  
K1 ～ K6 Condition of the wall around you  
Half the width of the KH road  
The rightmost X coordinate of the KX outer frame  
Y coordinate on the leftmost side of the KY outer frame  
I For various work (work)  
S Cursor key state  
SC Currently, the screen displaying the maze  
X Currently the reference X coordinate  
X Coordinates of the wall extending with respect to X X  
Y Currently the reference Y coordinate  
Y coordinate of the wall extending with respect to YY Y  

## Sample B: Quick line

*Program name* Q.BAS  
*Compatible models* MSX2 / MSX2 + / MSX turbo R  
### Commentary
This program is a program that draws a line. Execute with the "CALL RUN" instruction.
You can execute it with the "RUN" command as it is with MSX-BASIC, so please see how fast the execution speed of "MSX BASIC" is.

### Algorithm
Initialize the array variables to random positions for the number of lines in lines 140 to 160, and determine the direction in which the entire line moves in lines 170. C and O in line 180 are the amount of movement in the direction determined by line 170. Line 200 erases the previously written line, and lines 220-230 determine the starting point of the new line. If it is about to go off the screen, flip the direction vertically and horizontally independently. The end point uses the start point of the line written last time. This is 240 lines
I'm making sense. Then draw a line and memorize the newly calculated starting point.
After that, this process is repeated.

### Variable table
X () X coordinate of the starting point  
Y () Y coordinate of the starting point  
A () X coordinate of the end point  
B () Y coordinate of the end point  
The coordinates for 100 lines are stored in the above array variables.  
N Number of lines to move  
Direction in which the entire K line moves (X coordinate)  
Direction in which the entire L line moves (Y coordinate)  
Increase in CK  
Increase in OL  
P line color  


## Sample C: Graphic screen character output

*Program name*: GRPCHR.BAS  
*Compatible models*:  MSX / MSX2 / MSX2 + / MSX turbo R  
### Commentary
In "MSX-BASIC Kun Turbo", "OPEN" and "PRINT #" cannot be used, so it is a program that outputs characters on the graphic screen to supplement them. This program is executed by "CALL RUN".

    10 DEFINT A-Z
    20 SCREEN 5
    30 X = 100: Y = 100
    40 '#I & H2A, X, & H22, & HB7, & HFC, & H2A, Y, & H22, & HB9, & HFC
    50 FOR I = 65 TO 69
    60 '#I & H3A, I, & HCD, & H8D, & H00
    70 NEXT I
    80 IF LEN (INKEY $) THEN END
    90 GOTO 80

The output position is determined in 30 lines, and the value is set in the graphic accumulator (GRPACX, GRPACY) in 40 lines. Then, put the code of the character to be displayed in line 50 in I, and call GRPPRT (entry to display characters on the graphic screen) in the BIOS in line 60 to display the character.
Below is a 40-line and 60-line assembler list.

___Line 40___

        LD HL, (X)
        LD (GRPACX), HL
        LD HL, (Y)
        LD (GRPACY), HL

___Line 60___

        LD A, (I)
        CALL GRPPRT

> Caution
> 1. X and Y can be different variable names, but they must be integer types.
> 2. GRPACX (FCB7H) is a work area that shows the X coordinates on the graphic screen.
> 3. GRPACY (FCB9H) is a work area that shows the Y coordinate on the graphic screen.




## Sample D: Mandelbrot set

*Program name*: MANDEL.BAS  
*Compatible models*: MSX2 / MSX2 + / MSX turbo R  
### Commentary
This program calculates and displays the Mandelbrot set based on the fractal geometry pioneered by Mr. Mandelbrot of IBM Watson Research Institute in the United States.  
Generating the Mandelbrot set requires a daunting repetitive operation. Therefore, even if it is "MSX-BASIC Kun Turbo", it will take a considerable amount of time to finish drawing a single graphic. For example, for the Mandelbrot overview (magnification 1x, X = -2, Y = 1.25), which is the basis of this program, it takes about 2 hours and 30 minutes for MSX2 / MSX2 + and about 18 minutes for MSX turbo R.  

This program is executed by the "RUN" instruction, but once it starts executing, it cannot be stopped halfway. When you run it, you will first be asked for the filename to save the picture of the completed Mandelbrot set. Please enter an appropriate file name. If you don't need to save, press Return.  
Then enter the magnification and the target X and Y coordinates (not the MSX graphic coordinates). In this program

    Magnification: 1 to 256
    Range of X… -2 to +0.5
    Range of Y… -1.25 to +1.25

Since the range of is supported, please answer the question at the time of starting the program within this range.
This disc contains two sample data created by MANDEL.BAS.

| File name | Magnification | X     | Y |
| --------- |---------------|-------| ----- |
| MANDEL1.PIC | 1 | -2    | 1.25 |
| MANDEL2.PIC | 64            | -0.25 | 0.73 |

To see this sample data, please execute the following program.

    10 SCREEN 8: SET PAGE 1,1: CLS: VDP(9) = VDP(9) OR 2
    20 BLOAD"filename",S
    30 GOTO 30
