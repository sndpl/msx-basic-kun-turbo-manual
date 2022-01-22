# ☆ ★ ☆ About "MSX-BASIC Kun Turbo" Part 3 (Application) ☆ ★ ☆


This manual explains points to note when using "MSX-BASIC Kun Turbo" and advanced usage. For the basic usage, please refer to "About" MSX-BASIC Kun Turbo Part 1 (Basic)". For how to use each command, please refer to "About MSX-BASIC Kun Turbo Part 2 (Command Edition)". For how to use the attached sample program, refer to "About the sample program".

MSX is a trademark of ASCII.
MSX-DOS is a registered trademark of Microsoft Corporation.




## [8] When it doesn't run

"MSX BASIC Kun Turbo" is made to be compatible with MSX-BASIC, but due to the nature of the compiler, there are parts with different specifications. When "It works on MSX-BASIC, but it doesn't work with" MSX BASIC KUN Turbo", or "No error, but it works differently from MSX-BASIC". Please check the following points.

- "Illegal function call" appears and stops  
    Are you using variables that have never been used or array variables that have not been declared as arguments? When passing data with "CALL TURBO ON / OFF", be sure to use the variable name that was used once. Also, when using an array variable, be sure to declare it.

- "String formula too complex" appears and stops  
    Have you written an expression for a long character variable? In MSX-BASIC, there are 10 string stacks (where you put the result in the middle of the expression), but there is only one in "MSX-BASIC Kun Turbo". If the character expression becomes complicated, divide it into several expressions.

- Even though the variable type is declared, the calculation result is different.
     1. Is it declared using DEFDBL?
        In "MSX-BASIC Kun Turbo", all double precision variable declarations are treated as single precision variable declarations.
     2. Is the variable type declared after the execution instruction?
        In "MSX-BASIC Kun Turbo", there must be a declaration command at the very beginning of the program that is converted to machine language.  


- The calculation result is completely different  
    Is the answer overflowing in the formula for integer variables? For example, in the case of the following calculation, the answer will be different from MSX-BASIC.

        10 A% = 200
        20 PRINT A% * A%

    This is because it exceeds the range of numerical values that can be handled as an integer type. In such a case, MSX-BASIC automatically displays the answer as a real number type, but in "MSX-BASIC Kun Turbo", the integer type expression is always an integer type except for special cases. I'm calculating it as an answer. Therefore, the value will be different from the actual answer. To avoid this, take the following measures.

        10 A% = 200
        20 PRINT 1! * A% * A%

    If you do this, the expression itself will be treated as a real expression. (Make sure that the real number is assigned by the operator with the highest priority.) Also, the special case is when the answer can be a real number. For example, division and power multiplication.

- Runaway when moved  
    1. Are you using # N-?
        If the FOR-NEXT parameters overflow (exceeding & H8000) while using # N-, you will not be able to get out of the process. If you are out of control due to such a cause, please specify # N + only for that part.
        
        Note that such a program does not work on MSX-BASIC, so it is recommended to review the program.
    2. Are you using USR instructions?
        If the USR instruction has a structure that uses the inside of MSX-BASIC, its operation cannot be guaranteed. This is because the memory map is different from when MSX-BASIC is executed. Please note that "MSX Besshi-kun Tabo" appears on page 1 (4000H-7FFFH) when running on "MSX Bess-kun Tabo".

- It works, but it doesn't get faster  
Are you using interrupt instructions? Using interrupt instructions increases execution time.

- Behaves strangely  
Are you using the instructions of the higher model? If you use the MSX2 + or MSX turbo R instructions on MSX2, or the MSX turbo R instructions on MSX2 +, the operation is not guaranteed.

- It works on other MSX, but it doesn't work.  
If the system configuration is different, such as when there are many connected floppy disks, compilation may not be possible due to the free area of memory.

- Does not work at all  
Have you started MSX-DOS once after installing "MSX-BASIC Kun Turbo"? "MSX Bess Kimitabo" is built into the memory used by MSX-DOS.
For this reason, once you start MSX-DOS, "MSX Bess Kimitabo" will disappear from the memory.



## [9] Aiming for a faster program
"MSX-BASIC Kun Turbo" converts BASIC programs into machine language as they are. Therefore, depending on how you write the formula and the type of the variable, you may not be able to fully demonstrate its capabilities.
Here, we will explain the programming techniques to fully demonstrate the abilities of "MSX-BASIC Kun Turbo".

### 9.1 Use integer variables as much as possible  
The CPU Z80 / R800, which is the brain of MSX, does not have a floating point arithmetic function. When calculating a real number type variable, the real number calculation program prepared inside "MSX-BASIC Kun Turbo" will operate. Also, even if the value of the real type variable that appears in the expression is an integer, the execution speed does not change because the internal representation is a real number (for example, 5 is expressed as 5.0).
If you know in advance that the numerical value handled by the variable is an integer, using an integer type variable will dramatically improve the calculation speed. Also, if you don't use real numbers in the whole program, you can write "DEFINT A-Z" at the beginning of the program and all variables will be treated as integer type.

> ### Note
>   If you write "DEFINT A-Z", you can omit the "%" in the variable name. If you want to use a real variable at this time, add "!". If it is a character type variable, add "$".

### 2. Use "\" rather than "/" for division  
There are "/" and "\" in the division sign of MSX. "/" Is the division of a real number and "\" is the division of an integer. Since the Z80 / R800 cannot handle real numbers directly, using "\" will improve the calculation speed.

> ### Note  
> If you use "\", the calculation result after the decimal point will be truncated.

### 3. Substitute "^" with "*" as much as possible  
"^" Is an operator for finding the Nth root. For example, in the case of the formula "A = A ^ 4", it is executed after being corrected to the form "A = A * A * A * A" inside "MSX-BASIC Kun Turbo".
Therefore, if you write the formula "A * A * A * A" in advance, the execution speed will be faster because there is no conversion of the formula.

### 4. Divide an integer variable by 2 to the Nth root as much as possible.  
Z80 / R800 does not have a division instruction. However, if you divide by 2 to the Nth root, you can calculate at high speed by using a function called shift instruction. Therefore, dividing by 2 to the Nth root will improve the calculation speed.

### 5. In MSX2 / 2 +, there is no multiplication instruction in Z80 that multiplies integer variables as much as possible on N of 2  
 
However, as with division, when multiplying by 2 to the Nth root, it is possible to perform high-speed calculation by using the shift instruction.
Also, unlike the case of division, high-speed processing is possible when multiplying by the next numerical value other than 2 to the Nth root.
        
    3, 5, 6, 7, 9, 10, 20, 25, 40, 50, 80, 100, 200, 257

### 6. The size of the array should be 2 to the Nth root as much as possible.  
Array variables are secured in a contiguous area of memory. So when using the specified array variable, multiplication is used. Therefore, when defining with DIM, using the numerical value -1 explained in 5. will speed up the process. If you cannot use the desired number, declare that the number should be written in the rightmost subscript.

### 7. Do not repeat "CALL TURBO ON / OFF" too much  
If you use "CALL TURBO OFF", it will be executed by MSX-BASIC, so it will be slower, of course, but in addition to that, compilation (creating a machine language program) time is required. If you use "CALL TURBO ON", it will be compiled each time, so please try not to use ON / OFF as much as possible for programs that you want to have high speed.

## [10] About memory limit

When using "MSX Bashikun Tabo", the BASIC program (source program) on the main RAM and the program converted into machine language (object program) exist at the same time. .. Therefore, even if you try to compile a large program, you may not be able to compile it because there is no room for the object program. Although it depends on the contents of the program, as a rough guide, source programs up to about 10KB can be compiled and executed.
However, the restrictions are even more stringent in the following cases.

### 1. In the case of advanced system configuration  
When adding disk drives, etc., a work area (work area) is required for each peripheral device to operate, so the memory (free area) that can be freely used is reduced.

### 2. When the CLEAR and MAXFILES instructions are used  
The CLEAR command is a command to set the place where machine language programs other than "MSX-BASIC Kun Turbo" are entered. Naturally, the free area will be reduced, so please do not use it except when using a machine language program other than "MSX-BASIC Kun Turbo".
The MAXFILES instruction sets the number of files used by BASIC. This also reserves a work area for working with files, reducing free area.

### 3. In the case of a program that makes heavy use of multi-statements  
In the case of a source program that does not use any comments and makes effective use of memory to the limit by multi-statement, it may not be possible to compile even if it is within the range of 10KB.

### 4. When using an interrupt instruction  
If you use timer interrupts, function key interrupts, and sprite interrupts, you have to register a large machine language program, so the area that can be programmed is greatly reduced.

### 5. When using many character variables or large array variables  
"MSX-BASIC Kun Turbo" consumes 256 bytes of memory for each character variable for high-speed processing. Also, since the memory area is reserved when the array variable is declared, if it is too large, it will affect the program area.

## [11] How to increase the free area

There are two main ways to increase the free area. One is to make the program smaller and the other is to make the work area smaller.

- How to make the program smaller
    1. Heavy use of subroutines  
       Collect similar processing as a subroutine as much as possible.  
    2. Use multi-statements  
       Programs that can be put together in one line should be put together as much as possible.  
    3. Eliminate comments  
       If it is okay to take the comment itself, take it. If you really don't want to take it, do the following.  
       (a) Reduce the number of characters in the comment.  
       (b) Change "'" (3 bytes internally) to "REM" (1 byte internally).  
    4. Eliminate extra space  
        Eliminate the space between each command. The program is hard to see, but the free area is wide.  


- How to make the work area smaller
    1. If you have added more disks or have RS-232C, if you have one disk drive to remove, start MSX while holding down the [CTRL] key to disconnect the 2-drive simulator function. can do. At this time, the B drive cannot be used, but about 3.5KB of memory can be saved.
    2. Reduce the parameters of MAXFILES
      MAXFILES is only needed when using the OPEN instruction. This number is set to 1 when the power is turned on, so set "MAXFILES = 0" if you do not want to open any files. Note that MAXFILES consumes 267 bytes of memory for each increment of 1.
    3. Make the character variable area of MSX-BASIC smaller
        If the program run by "CALL RUN" or the character variable is not used at the time of "CALL TURBO OFF", delete the character variable area as "CLEAR 0". The character variable area is set to 200 bytes when the power is turned on.
    4. Make the machine language area smaller (Part 1)
        If you are running another program before the program you are trying to run, you may have secured the machine language area by using the CLEAR instruction in that program. In such a case, turn off the power once or reset it before executing the program. This will give you the largest free area for each MSX.
    5. Make the machine language area smaller (Part 2)
        The method introduced here is a very dangerous method, so please use it only if you have a thorough understanding of the MSX system or if you have the knowledge to program with the assembler.
        By executing "CLEAR character variable area, & HF380", the upper limit of the free area can be brought to the limit of the system area. However, this is tricking the MSX system into increasing the free area, so there is no guarantee if you are using a disk or installing a machine language program. Also, if the hook area of address &HFD9F (H.TIMI, 1/60 second interval timer interrupt hook) is not &HC9, it means that you are reading the program under address &HF380 by timer interrupt, so be sure to use this hook as &HC9. Please rewrite to and then execute CLEAR.    


## [12] Internal format of variables

Here, for those who want to use it more advanced, we will explain how to express the inside of variables.
I will reveal.

・ Integer type
It is expressed in 2 bytes. The expression method is the same as the 16-bit data of Z80 / R800, but the MSB (most significant bit) is the sign flag.
・ Real number type
It is expressed in 3 bytes. The expression method is exponential part, mantissa part Low, mantissa part Hi from the youngest address. The relationship between the exponential part and the mantissa part is as follows.

    Mantissa x 2 ^ Exponent

In the index part, 80H becomes 0, and when it is minus 1st power, it becomes 7FH.

    Index:… -2 -1 0 +1 +2…
    Internal representation:… 7E 7F 80 81 82…

The mantissa is represented by a number greater than or equal to 0.5 and less than 1. In other words, MSB is 0.5, then 0.25, and so on. Expressed this way, the MSB will always be 1.
Then, this bit becomes meaningless, so I decided to use it as a sign flag. So, to actually refer to memory and find out the value of that variable

    (Mantissa part +0.5) × 2 ^ Exponent part

or

    (Mantissa part-0.5) × 2 ^ Exponent part

Will be calculated.
The contents of the mantissa are as follows.

    MSB LSB
    Bits: 15 14 13 12 11…
    Expression value: Code 2 ^ -2 2 ^ -3 2 ^ -4 2 ^ -5…

" example "
The number 1 is expressed as follows.
First, start with the expression "1 × 2 ^ 0". First, 1 is a number that does not fit in the mantissa, so increase the exponent by one to make the mantissa smaller. Since the exponent part is expressed as 2 to the Nth root, raising the exponent part by 1 means dividing the mantissa part by 2. in short,
    (1 ÷ 2) × 2 ^ (0 + 1) = 0.5 × 2 ^ 1
is. In addition, 0.5 is automatically added to the mantissa, so in the internal representation
    (0.5-0.5) × 2 ^ 1 = 0 × 2 ^ 1
Will be. If this is expressed in hexadecimal according to the internal format mentioned earlier,
    81H, 00H, 00H
Will be. If it is -1, the sign of the mantissa is inverted, so
    81H, 00H, 80H
is.
With this representation method, "0" cannot be represented by the mantissa. Therefore, when the exponent part is "0", it is processed so that it is a real number "0". The mantissa at this time has no meaning regardless of the value.

・ Character type
Character variables are represented by 256 bytes. The first byte contains the length of the string. The actual character data is stored in the 2nd to 256th bytes. The address indicated by VARPTR indicates the part containing the length of the character string.