# prog2.s
*----------------------------------------------------------------------
* Programmer: Liliann Andrade
* Assignment or Title: Program #2
* Filename: prog2.s
* Date completed: March 14, 2018
*----------------------------------------------------------------------
* Problem statement: This program will calculate the monthly payment using the apr, length of loan, and amount of loan. 
* Input: Amount of loan, APR, and length of loan
* Output: Monthly payment 
* Error conditions tested: none
* Included files: none
* Method and/or pseudocode: To calculate the monthly payment, first I converted the APR given into a monthly interest rate by dividing it by 1200. Then I solved for the numerator first, then the denominator, and multiplied the quotient by the principal. 
* References: none
*----------------------------------------------------------------------
*
        ORG     $0
        DC.L    $3000           * Stack pointer value after a reset
        DC.L    start           * Program counter value after a reset
        ORG     $3000           * Start at location 3000 Hex
*
*----------------------------------------------------------------------
*
#minclude /home/cs/faculty/riggins/bsvc/macros/iomacs.s
#minclude /home/cs/faculty/riggins/bsvc/macros/evtmacs.s
*
*----------------------------------------------------------------------
*
* Register use
*
*----------------------------------------------------------------------
*
start:  initIO                  * Initialize (required for I/O)
	setEVT			* Error handling routines
	initF			* For floating point macros only	
	lineout		title		*title of assignment
	lineout		newline		*space in b/w lines
	lineout		prompt1		*prints out statement to get APR
	floatin		buffer		*get input from user for APR
	cvtaf		buffer,D1	*turns amount of loan into float
	lineout		prompt2		*prints out 'enter annual percentage'
	floatin		rate		*gets apr inputted
	cvtaf		rate,D2		*turns rate into float
	lineout		prompt3		*prints out loan in months
	floatin		length		*gets loan in months
	cvtaf		length,D3	*turns loan in months to float
	itof		#1,D4		*turns int 1 to float
	itof		#1200,D5	*turns int 1200 to float
	fdiv		D5,D2		*divides APR/1200 to get monthly rate
	move.l		D2,D6
	
	fadd		D4,D2		*adds 1 + monthly rate
	fpow		D2,D3		*takes power of (1+r)^length of loan, and stores it in D0
	move.l		D0,D7		*makes copy of power
	fmul		D0,D6		*multiplies (1+r)^length by monthly rate
	
	fsub		D4,D7		*subtracts (1+r)^length by 1 for denominator
	fdiv		D7,D6		*divides numerator by denominator
	fmul		D6,D1		*multiplies quotient by amount of loan
	
	move.l		D1,D0		*moves answer from D1 to D0
	cvtfa		payment,#2	*allows xx.xx and converts float to askii
	lineout 	newline
	lineout		answer		*prints out answer for monthly payment			

        break                   * Terminate execution
*
*----------------------------------------------------------------------
*       Storage declarations
newline: dc.b	0
title: 	dc.b	'Program #2, Lily Andrade',0
prompt1: dc.b	'Enter the amount of the loan:',0
buffer: ds.b	80
prompt2: dc.b	'Enter the annual percentage rate:',0
rate: 	ds.b	80
prompt3: dc.b	'Enter the length of the loan in months:',0
length: ds.b	80
answer: dc.b	'Your monthly payment will be $'
payment: ds.b	20					
        end
