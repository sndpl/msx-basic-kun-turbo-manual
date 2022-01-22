# ☆ ★ ☆ About "MSX-BASIC Kun Turbo" Part 2 (Command) ☆ ★ ☆

This manual explains the instructions for "MSX-BASIC Kun Turbo". For the basic usage, please refer to "[About MSX-BASIC Kun Turbo Part 1 (Basic)](MSX-BASIC_Kun_Part_1.md)". Please refer to "[About MSX-BASIC Kun Turbo Part 3 (Application)](MSX-BASIC_Kun_Part_3.md)" for precautions and advanced usage.
For how to use the included sample programs, please refer to "[About the sample programs](Sample_Programs.md)".

MSX is a trademark of ASCII.  
MSX-DOS is a registered trademark of Microsoft Corporation.  


## 5. Instructions with a different grammar

"MSX BASIC Kun Turbo" is almost the same as MSX-BASIC, but there are some restrictions to make the program run faster. The following is an explanation comparing the grammars of MSX-BASIC and "MSX BASIC Kun Turbo".
 
### 1. CIRCLE  
It is an instruction to write a circle. In "MSX-BASIC Kun Turbo", you cannot specify the start angle and end angle of the arc, and you cannot specify the aspect ratio.

___MSX-BASIC___  
CIRCLE (X coordinate, Y coordinate), radius, color code, start angle, end angle, ratio  

___MSX-BASIC Kun Turbo___  
CIRCLE (X coordinate, Y coordinate), radius, color code  

### 2. COPY  
It is an instruction to copy data to various places. You can only copy on the screen with "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
There are many ways to use the COPY command, so please refer to your MSX manual.  

___MSX-BASIC Kun Turbo___  
COPY (X coordinate 1, Y coordinate 1)-(X coordinate 2, Y coordinate 2), source page TO (X coordinate 3, Y coordinate 3), destination page, logical operator  

### 3. DEFDBL  
It is an instruction to declare a double precision variable. Since double precision variables cannot be used in "MSX-BASIC Kun Turbo", even if you declare double precision, it is treated as single precision (DEFSNG).

___MSX-BASIC___  
DEFDBL variables, variables,…  
    or  
DEFDBL Variables-Variables  

___MSX-BASIC Kun Turbo___  
The format is the same as MSX-BASIC, but declared as single precision.  

### 4. DIM  
It is an instruction to declare an array variable. Please note that the place where this command can be executed is limited in "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
You can write it anywhere before using array variables.  

___MSX-BASIC Kun Turbo___  
Do not execute any instruction other than the declarative instruction before DIM.  
The instructions that can be used before DIM are as follows.  
DEFINT DEFSNG DEFDBL DATA DIM REM (including')  

### 5. INPUT  
This is the command used when entering data. With MSX-BASIC, you can enter many data at a time, but with "MSX BASIC", you can only enter one at a time.

___MSX-BASIC___  
    INPUT <"character string";> variable, variable ... (<> means optional)  

___MSX-BASIC Kun Turbo___  
    INPUT <"string";> variable  

### 6. KEY  
There are many commands with the word KEY, but some of them cannot be used with "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
KEY key number, "string"  
KEY LIST  
KEY ON / OFF  
KEY (key number) ON / OFF / STOP  
ON KEY GOSUB  

___MSX-BASIC Kun Turbo___  
KEY (key number) ON / OFF / STOP  
ON KEY GOSUB  

### 7. LOCATE  
It is a command to determine the coordinate position of the character. Be sure to specify both X and Y in "MSX-BASIC Kun Turbo". Also, the cursor switch cannot be specified.

___MSX-BASIC___  
LOCATE \<X coordinate>, \<Y coordinate>, \<cursor switch>  

___MSX-BASIC Kun Turbo___  
LOCATE X and Y coordinates  

### 8. NEXT  
It is an instruction used as a pair with the FOR instruction. Parameter variables cannot be omitted in "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
NEXT \<variable>  

___MSX-BASIC Kun Turbo___  
NEXT variable  

### 9. PRINT  
It is a screen display command. When displaying data using a comma (,), the blank space is different between MSX-BASIC and "MSX BASIC".

___MSX-BASIC___  
Display example of PRINT 10,20  
10 20  

___MSX-BASIC Kun Turbo___  
Display example of PRINT 10,20  
10 20 (Display blanks on tab (CHR$(9)))  

### 10. RUN  
It is an instruction to execute the program. Variables are not initialized in "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
All variables are initialized.  

___MSX-BASIC Kun Turbo___  
The content of the variable is undefined. At the beginning of the program, you need to put a routine to initialize the variables.  

### 11. SCREEN  
It is a command to switch the screen mode and set other functions. In "MSX-BASIC Kun Turbo", you can only set the screen mode and sprite size.  

___MSX-BASIC___  
SCREEN \<screen mode>, \<sprite size>, \<key click>, \<baud rate>,
\<Printer Options>, \<Interlace>  

___MSX-BASIC Kun Turbo___  
SCREEN \<screen mode>, \<sprite size>  

### 12. SET  
There are many commands with the word SET, but only two can be used with "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
    
    SET ADJUST SET BEEP SET DATE SET PAGE SET PASSWORD  
    SET SCREEN SET TIME SET TITLE SET VIDEO SET SCROLL  

___MSX-BASIC Kun Turbo___  
    
    SET PAGE SET SCROLL  

### 13. STOP  
This is an instruction to suspend the program. In "MSX-BASIC Kun Turbo", it is interpreted in the same way as the END command.

___MSX-BASIC___  
After interruption, it is possible to continue with the CONT instruction.  

___MSX-BASIC Kun Turbo___  
Since it has the same meaning as the END instruction, it cannot be continued.  

### 14. USR  
It is an instruction to execute a machine language routine. Arguments other than integers cannot be used in "MSX-BASIC Kun Turbo".

___MSX-BASIC___  
USRn (A) (n is an integer from 0 to 9, A is a variable or number of each form)  

___MSX-BASIC Kun Turbo___  
USRn (A) (n is an integer from 0 to 9, A is an integer variable or number)  

### 15. VARPTR  
It is an instruction to find the memory address containing the value of the variable or the start address of the file control block. In "MSX-BASIC Kun Turbo", it is only possible to find the memory address of the variable.

___MSX-BASIC___  
VARPTR (variable name)  
VARPTR (#file number)  

___MSX-BASIC Kun Turbo___  
VARPTR (variable name)  

## 6. Instructions that cannot be used in "MSX-BASIC Kun Turbo"

The commands that "MSX-BASIC Kun Turbo" cannot convert into machine language are as follows:  

___MSX-BASIC___  

        AUTO            BASE            BLOAD           BSAVE  
        CALL            CDBL            CINT            CLEAR  
        CLOAD           CLOAD?          CLOSE           CONT  
        CSAVE           CSNG            DEFFN           DELETE  
        DRAW            EOF             ERASE           ERL  
        ERR             ERROR           FRE             GET  
        INPUT#          KEY LIST        LINE INPUT#     LIST  
        LLIST           LOAD            LPRINT USING    MAXFILES  
        MERGE           MOTOR           NEW             ON EROR GOTO  
        ON STOP GOSUB   OPEN            PLAY            PRINT#  
        PRINT# USING    PRINT USING     PUT KANJI       RENUM  
        RESUME          SAVE            SPC             TAB  
        TRON            TROFF           WIDTH  

___MSX-Disk BASIC___

        CVD             CVI             CVS             DSKF  
        FIELD           FILES           FPOS            KILL  
        LFILES          LOC             LOF             LSET  
        MKD$            MKI$            MKS$            NAME  
        RSET  
___Logical operator___  

        EQV             IMP  


## 7. Commands only for "MSX-BASIC Kun Turbo"

"CALL RUN / TURBO ON / TURBO OFF" introduced at the very beginning are also instructions that can be executed only by "MSX-BASIC Kun Turbo", but other instructions are explained here.

> Note  
> The commands explained here are all commands written in comments ("REM" and "'").

### 1. #I  
   
Abbreviation for the word INLINE, you can write a machine language program in "MSX-BASIC Kun Turbo".
The machine language can be registered in the program by writing the machine language code like the BASIC DATA instruction after "#I". In addition to the machine language code, you can also write integer variable names. At this time, the address (2 bytes) of the variable is entered in the place where the variable name is written.

___Example: Substitute 100 from the integer variable A%___

    10 CALL TURBO ON
    20 A% = 0
    30 '#I &H21,100,0
    40 '#I &H22,A%
    50 PRINT A%
    60 END

The machine language source program used in this example is as follows.

    line 30: LD HL, 100
    line 40: LD (A%), HL

> ### Caution  
> When using a variable in #I, please use the variable once in the program of "MSX-BASIC Kun Turbo" before using it.

In addition to the variable address in the machine language code, you can also write the line number.
In this case, the address (2 bytes) of the line number will be entered where the line number is written. If you add an @ mark to the beginning of a number, that number indicates the line number.

___Example:___

    10 CALL TURBO ON
    20 A% = 0
    30'#I &H21,100,0
    40'#I &H22,A%
    50'#I &HCD,@100
    60 END
    100 PRINT A%
    110 RETURN

The source program of line 50 is as follows.

    CALL @100

Please note that if you write the line number with @, it will not be renumbered with the RENUM command of MSX-BASIC.

### 2. #C  

Abbreviation for the word CLIP, which changes the upper limit of the Y coordinate on the graphic screen.
In MSX, the Y coordinate of the graphic screen is from 0 to 211, but if you use this command, you can draw a line with the Y coordinate from 0 to 255. (However, the displayed area does not change.)

- #C+  
The Y coordinate is treated in the same way as MSX-BASIC. In this case, all numbers above 212 numbers are treated as 211.
- #C-  
You can also specify coordinates for numbers above 212.

___Example: Difference between #C- and #C+___
    
    10 CALL TURBO ON
    20 SCREEN 8
    30 '#C+
    40 LINE (0,0)-(255,255), 255
    50 IF INKEY $ = "" THEN 50
    60 '#C-
    70 LINE (0,0)-(255,255), 255
    80 IF INKEY $ = "" THEN 80
    90 END

> ### Note
> For screen modes 0 to 3, PAINT instruction, and CIRCLE instruction, the #C specification is invalid (same as #C+).
> If you specify a value of 212 or more for the Y coordinate, the sprite area will be destroyed, and in some cases dust may appear on the screen. Please note that this program is a sample program for confirming the function of #C.


### 3. #N  

Abbreviation for NEXT. Set whether to include the function to check the overflow of parameter variables when converting NEXT of FOR to NEXT to machine language.

___Example:___

    10 FOR I% = 0 TO &H7FFF
    20 NEXT I%
    30 END

An error will occur if this program is executed on MSX-BASIC. This is because MSX-BASIC checks that "&H7FFF + 1" overflows the integer range at the end of the loop.
By using "#N+" in "MSX-BASIC Kun Turbo", overflow check will be performed in the same way. 

However, it does not stop with an error, it just forces you out of the loop. If you use "#N-", overflow will not be checked, but you will be able to move at a higher speed. If the parameter overflows when "#N-" is specified, an infinite loop will occur.
If nothing is set, the "#N-" state will be selected, so use "#N+" only when parameter overflow is expected.
