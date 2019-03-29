# Countdown-Timer--8051-Microcontroller
It basically counts down from a specified set time value
//Code:
ORG 0000H // initial starting address





MOV P0,#00000000B // clears port 1

MOV R0,#00D

MOV P1,#0H
HERE1:
INC R0
ACALL SEC
JB P1.0,CONTINUE
SJMP HERE1


CONTINUE:
MOV R6,#0D//stores "1"
MOV R7,#0D// stores "0"
MOV B,#1H
MOV R4,#50


MOV P2,#00000000B // clears port 3


MOV DPTR,#LABEL1 // loads the adress of line 29 to DPTR

SETB P1.0

MAIN:


MOV A,R6 // "1" is moved to accumulator

SETB P3.0 // activates 1st display
ACALL DISPLAY // calls the display sub routine for getting the pattern for "1"
MOV P0,A // moves the pattern for "1" into port 1
ACALL DELAY // calls the 1ms delay
// deactivates the 1st display

MOV A,R7 // "2" is moved to accumulator

SETB P3.1 // activates 2nd display
ACALL DISPLAY // calls the display sub routine for getting the pattern for "2"
MOV P2,A // moves the pattern for "2" into port 1
ACALL DELAY // calls the 1ms delay
// deactivates the 2nd display


ACALL SEC
//DEC R0
MOV A,R0
ADD A,B
DA A
MOV R0,A

 



MOV A,R0

;extract right nibble
MOV R1,#4D
LNIB:
RLC A
CLR C
DJNZ R1,LNIB
MOV R1,#4D
FLNIB:
RRC A
DJNZ R1,FLNIB
;display right nibble in R7
MOV R7,A

MOV A,R0

;extract left nibble
MOV R1,#4D
RNIB:
RRC A
CLR C
DJNZ R1,RNIB
;display left nibble in R6
MOV R6,A






SJMP MAIN // jumps back to main and cycle is repeated





DELAY: MOV R3,#02H
DEL1: MOV R2,#0FAH
DEL2: DJNZ R2,DEL2
DJNZ R3,DEL1
RET


SEC:MOV TMOD,#01H
MOV R5, #14
LOOP:MOV TH0,#0FFH
MOV TL0,#0FFH
CLR TF0
SETB TR0
HERE:JNB TF0,HERE
DJNZ R5, LOOP
RET






DISPLAY: MOVC A,@A+DPTR // adds the byte in A to the address in DPTR and loads A with data present in the resultant address
RET




LABEL1:DB 0C0H
DB 0F9H
DB 24H
DB 0B0H
DB 99H
DB 92H
DB 82H
DB 0F8H
DB 80H
DB 90H

END
