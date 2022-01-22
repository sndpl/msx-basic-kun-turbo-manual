# ☆ ★ ☆ About "MSX-BASIC Kun Turbo" Part 1 (Basic) ☆ ★ ☆

This manual explains the basic usage of "MSX-BASIC Kun Turbo". For how to use each command, please refer to "About MSX-BASIC Kun Turbo Part 2 (Command Edition)". Please refer to "About MSX-BASIC Kun Turbo Part 3 (Application)" for precautions and advanced usage.

MSX is a trademark of ASCII.
MSX-DOS is a registered trademark of Microsoft Corporation

## 1. Introduction
"MSX-BASIC Kun Turbo" is software for making BASIC programs into machine language. The software that converts a program into machine language in this way is called a "compiler".

In the case of "MSX-BASIC Kun Turbo", it will be up to 15 to 20 times faster than MSX-BASIC, although it depends on the model and program. With this, you can also make games with MSX-BASIC. Of course, real numbers and graphic-related instructions can also be used, so even computer graphics that require a large amount of complicated calculations can be processed at high speed in a short time.

However, "MSX-BASIC Kun Turbo" has the following points that are different from MSX-BASIC and should be noted, although it can perform high-speed processing.

- Cannot use double precision variables.
- Uses more memory for character variables than MSX-BASIC.
- Instructions for file input / output such as cassettes and disks cannot be used.
- The Kanji mode and ANK (alphanumerical) mode cannot be switched during the program.
- The program converted to machine language cannot be saved.
- Processing cannot be interrupted by pressing [CTRL] + [STOP].
- There are instructions that are used differently.
- A program that is too large cannot be changed to machine language.

There are some points to be aware of in this way, but you can also leave the processing of the parts that cannot be done with "MSX-BASIC Kun Turbo" to BASIC.
Please read this manual carefully and make full use of the functions of "MSX-BASIC Kun Turbo".

## 2. Differences between "MSX-BASIC Kun", "MSX-BASIC Kun Plus" and "MSX-BASIC Kun Turbo"

Until now, ASCII has released BASIC compilers called "MSX-BASIC Kun" and "MSX-BASIC Kun Plus". The differences between these software and "MSX-BASIC Kun Turbo" are as follows.

- MSX-BASIC Kun → MSX-BASIC Kun Plus
    - Supports the new screen mode (SCREEN 10-12) of MSX2 +.
    - Supports VDP functions for registers added in V9958.
    - Supports scroll command (SET SCROLL).
    - A sample disk is included.
- MSX-BASIC Kun Plus → MSX-BASIC Kun Turbo
    - Supports multiplication instructions for the new CPU R800.
    - Released as a disk instead of the ROM cartridge up to "MSX Base-kun Plus".

> ### Note
> Previously, this "MSX-BASIC Kun Turbo" was listed in "MSX Magazine May 1992 Program Service" (ASCII Publishing Bureau). The BASIC compiler itself included in this program service is the same as the BASIC compiler itself of the newly released "MSX-BASIC Kun Turbo".

## 3. How to use "MSX-BASIC Kun Turbo"

First, insert the "MSX-BASIC Kun Turbo" system disk into the MSX disk drive, and then turn on or reset the MSX. When MSX-BASIC starts up, "MSX-BASIC Kun Turbo" is automatically read from the disc. When the reading is completed, the command of "MSX-BASIC Kun Turbo" is ready to use.

There are three instructions for speeding up "MSX-BASIC Kun Turbo". I will explain in order.

### 3.1 CALL RUN

"CALL RUN" is an instruction to execute after converting all the programs written in BASIC into machine language. Corresponds to the BASIC "RUN" instruction. Please enter the following example.

    10 SCREEN 8
    20 LINE -(RND(1)*256,RND(1)*212),255
    30 IF INKEY$="" THEN 20
    40 END

Compare this program with "RUN" and "CALL RUN". You can see that "CALL RUN" is faster.

### 3.2 CALL TURBO ON

As I wrote earlier, "MSX-BASIC Kun Turbo" has instructions that cannot be converted into machine language. In such a case, "CALL RUN" cannot be used, so use "CALL TURBO ON" which can be executed with "MSX-BASIC Kun Turbo" only where you want to make it faster. Try entering the following program.

    10 SCREEN 8
    20 COLOR 255
    30 OPEN "GRP:" FOR OUTPUT AS #1
    40 PRESET (0,0):PRINT #1,"ﾅﾆｶ ﾎﾞﾀﾝｦ ｵｼﾃｸﾀﾞｻｲ"
    60 IF INKEY$="" THEN 60
    70 LINE -(RND(1)*256,RND(1)*212),255
    80 IF INKEY$="" THEN 70
    90 END

This is a message added to the previous program. Now, try running this with "CALL RUN". It stops with the message "Syntax error in 30". But the program is not wrong. This means that you cannot use the "OPEN" command in "MSX-BASIC Kun Turbo". That's where the "CALL TURBO ON" mentioned earlier comes into play. Let's add this instruction to the current program.

     50 CALL TURBO ON

 What you do is a normal "RUN". This time it should work without any errors. Since "MSX-BASIC Kun Turbo" has corrected only the programs after "CALL TURBO ON" into machine language, no error occurs.

 ### 3.3 CALL TURBO OFF

 What you can do with "CALL TURBO ON" is not that. In the previous example, it happened that there was an instruction that could not be used in "MSX-BASIC Kun Turbo", but when actually creating a program, it is often tempting to use the instruction that cannot be used in the middle. .. In such a case, please use "CALL TURBO OFF". This will also be explained using an example. Please add the following lines to the previous program and use it.

    90 CLS
    110 PRESET (0,0):PRINT #1,"ﾓｳ ｲﾁﾄﾞ ﾔﾘﾏｽｶ? (Y/N)"
    130 I$=INKEY$
    140 IF I$="Y" THEN 170
    150 IF I$="N" THEN END
    160 GOTO 130
    170 LINE -(RND(1)*256,RND(1)*212),255
    180 IF INKEY$="" THEN 170

If you try to run this with "RUN", you should get the error "Syntax error in 110". This time, the "PRINT #" instruction cannot be used. If you want to use an instruction that cannot be used, you can use "CALL TURBO OFF", so let's add the following two.

    100 CALL TURBO OFF
    120 CALL TURBO ON

The reason for executing "CALL TURBO ON" again on line 120 is to run fast except for unusable instructions. If you get an instruction that cannot be used, you can insert it between "OFF" and "ON" in this way, and the speed will be the same as BASIC for that part, but it will not be impossible to process.

> ### Note: Installation on hard disk
>
>The "MSX-BASIC Kun Turbo" at the time of purchase is designed to boot from a floppy disk, but it can also be installed in a hard disk. In this case, copy the following files from the "MSX-BASIC Kun Turbo" disk to the directory where you want to install "MSX-BASIC Kun Turbo" on the hard disk.
>
>        XBASICTR.BIN
>
>If you install it in this way, execute the following command from MSX-BASIC before loading the program that uses "MSX BASIC".
>
>        BLOAD"XBASICTR.BIN",R
>
>If you include it in a subdirectory, don't forget the directory path before the filename.

  ## 4. important point
Please note the following when using "MSX-BASIC Kun Turbo".

### 4.1 ON / OFF cannot be used with multi-statements
"CALL TURBO ON / OFF" cannot be used in multi-statements.
So don't write other instructions on the same line as "CALL TURBO ON / OFF".

### 4.2. Different variables with the same name
When running with "CALL TURBO ON" and running with "CALL TURBO OFF", even if the variable or array name is the same, it will be a different variable inside MSX. This is because the variable area of BASIC (the memory area where MSX stores variables) and the variable area of "MSX-BASIC Kun Turbo" are different. However, integer type variables (variables with % in the name) and integer type arrays can pass data. When you want to receive the data of the integer type variable A% and the integer type array variable E%

    CALL TURBO ON(A%, E%())

If you first declare an integer variable with the DEFINT instruction, you can omit % as follows.

    CALL TURBO ON(A, E())

### 4.3 Scope of real variables

In "MSX-BASIC Kun Turbo", the variables of the real type are slightly different from MSX-BASIC. The type is only a single precision type represented by "!", And there is no double precision type real number. The precision of this single precision real number is about 4.5 digits, and the range is positive and negative 2.939E-39 to 1.701E + 38. Floating point numbers over 10000 are displayed in "E format". Since all real type functions and operations conform to this type, the operation results will be slightly different from those of MSX-BASIC.

### 4.4 Small array variable

In MSX-BASIC, array variables with subscripts (numbers in parentheses) of 10 or less can be used without declaring them, but in "MSX-BASIC Kun Turbo", they must be declared. Also, when using an array variable, declare it immediately after the first "CALL TURBO ON".

### 4.5 Infinite loop break

An infinite loop is an endless process. For example:

    10 GOTO 10

There is no end to the process. In such a case, you can interrupt the process by pressing *CTRL* + *STOP* in BASIC, but you cannot interrupt it in "MSX-BASIC Kun Turbo". This is because we are running a program that has been converted to machine language.
If you want to be able to interrupt the process, do the following:

    10 CALL TURBO ON
    20 ON KEY GOSUB 100
    30 KEY(1) ON
    40 GOTO 40
    100 END


This uses a function called function key interrupt. If you put the same process after each "CALL TURBO ON", you can interrupt the process at any time. If you are not familiar with "MSX-BASIC Kun Turbo", please try to include such processing as much as possible. Also, running a program on "MSX-BASIC Kun Turbo" is the same as running a machine language program, so it is important to save the program before running it. Please do not forget it even if it is troublesome.


> ### Note
> In MSX-BASIC, there is almost no runaway even if there is a programming mistake, but since "MSX-BASIC Kun Turbo" is a compiler, runaway due to a programming mistake is inevitable. Therefore, be sure to save the program to a floppy disk before executing. For the SAVE method, refer to your MSX BASIC manual.



### 4.6 Correspondence of FOR-NEXT

In MSX-BASIC, it is not necessary to specify the parameters of the NEXT statement (variables used in FOR to NEXT), but be sure to specify them in "MSX-BASIC Kun Turbo". If it is not specified, it cannot be converted to machine language. For example:

    10 FOR I = 0 TO 10
    20 NEXT

will not work, but if you write it as:

    10 FOR I = 0 TO 10
    20 NEXT I

it will.
