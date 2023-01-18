# BADTRAN - bad, but hopefully usable

BADTRAN is a language with some aspects inspired by FORTRAN, but designed to be bad, while still being usable.
At present there is no implementation of the language, as I (rchipp42) lack the skills to write one. If you would like to, you may write an implementation of BADTRAN, and for the first one, an interpreter in a portable compiled language would be nice.

__Read this document carefully!__ Informantion can be in __odd places__, so read carefully and in full.

## A First Programme
The most basic programme possible is as follows:
```
        PROGRAMME DEMONSTRATION
        END PROGRAMME
```
The ```PROGRAMME X``` statement, where X stands for your chosen name, names and begins a programme. <br/>
The ```END PROGRAMME``` statement ends the most-recently-begun programme. <br/>
There can only be one programme per source file. <br/>
Upper case is mandatory, and the only allowed characters (outside strings) are alphanumerics, and punctuation. <br/>
The suffix for source files is ```.bts``` for ***B***AD***T***RAN ***S***OURCE

## Output
A programme using output is as follows:
```
        PROGRAMME OUTPUT-DEMONSTRATION
        S20-MESSAGE IS "HELLORLD!"
        READ OUT S20-MESSAGE
        END PROGRAMME
```
The ```X IS Y``` statement (where X stands for a variable name, and y for an expression, literal or other variable) assigns a value to a variable, and if undeclared, declares it. <br/>
Variables cannot be declared without an assignment. <br/>

Hungarian notation is enforced. The type prefixes are as follows: <br/>

```SX-VARIABLE-NAME``` denotes a string of length ```X``` <br/>
```IX-VARIABLE-NAME``` denotes an integer of width ```X``` (an "X-Bit integer") <br/>
```FX-VARIABLE-NAME``` denotes a fixed-point number with ```X``` bits before the point, and the same amount after it <br/>

The ```READ OUT X``` statement displays the value of ```X```. ```X``` __cannot__ be an expression or literal, and __must__ be an __existing__ variable. <br/>

The ```WRITE OUT X``` statement is the same as the ```READ OUT X``` statement, except in that it does not add a newline.

## Input
A programme using input is as follows:
```
        PROGRAMME INPUT-DEMONSTRATION
        I8-NUMBER IS 0
        READ IN I8-NUMBER
        S20-MESSAGE IS "YOU TYPED "
        WRITE OUT S20-MESSAGE
        READ OUT I8-NUMBER
        END PROGRAMME
```
The ```READ IN X``` statement is identical to the ```READ OUT X``` statement, except that instead of displaying the value of ```X```, it displays a prompt (```? ```) and after the ```Enter``` key is pressed, the typed value is assigned to ```X```. <br/>
The ```WRITE IN X``` statement is identical to the ```READ IN X``` statement, except that instead of doing what the latter statement does, if there is a key depressed in the instant the statement is executed, it assigns the value of that key to ```X```. <br/>
This only works with length-1 strings (imitation chars), and __does not__ wait for a key to be pressed. <br/>

## Flow Control
A  programme using flow control is as follows:
```
        PROGRAMME FLOW-CONTROL-DEMONSTRATION
        I8-COUNTER IS 1  
        COME FROM 10 UNLESS I8-COUNTER .GT. 10
        READ OUT I8-COUNTER
        I8-COUNTER IS I8-COUNTER AND 1
C       THE NEXT LINE IS A LABEL
 10     TWIDDLE THUMBS
        END PROGRAMME
```
For the first time, we now have stuff *before* the start of normal statements. <br/>
Normal statements start in the eighth column. <br/>
The first column is reserved for the comment marker ```C```. If ```C``` is found in the  first column, the rest of the line is ignored. This is useful for writing comments and remarks. Comments *ought* to start in the eighth column anyway. <br/>
The second through seventh columns are reserved for numerical labels. <br/>
The ```COME FROM X``` statement resumes execution at its own location when execution has reached a line with the numerical label ```X```. <br/>
The ```COME FROM X UNLESS Y``` statement is the same as the ```COME FROM X``` statement, except that it will only perform the jump __if__ the condition ```Y``` is __not__ true. <br/>
The ```TWIDDLE THUMBS``` statement does absolutely nothing, and is useful for lines with numerical labels. <br/>
The ```A .GT. B``` operator returns true if ```A``` is greater than ```B```. Both ```A``` and ```B``` can be expressions, literals, or variables. <br/>
The ```A AND B``` operator __is not__ a bitwise ```AND```, and rather, simply means "plus". It is used for addition. Apart from the difference in purpose, it behaves like the ```A .GT. B```  operator.

## Binary operators
### Arithmetic
```X AND Y``` - ```x + y``` <br/>
```Y FROM X``` - ```x - y``` <br/>
```X TIMES Y``` - ```x * y``` <br/>
```Y INTO X``` - ```x / y``` <br/>
```X .MD. Y``` - ```x % y``` <br/>
### Comparison 
```X .LT. Y``` - ```x < y``` <br/>
```X .GT. Y``` - ```x > y``` <br/>
```X .EQ. Y``` - ```x == y``` <br/>
### Logical
```X .AN. Y``` - ```x && y``` (where ```X``` and ```Y``` are simple or simple equating conditions [or their inverting counterparts]) <br/>
```X .OR. Y``` - ```x || y``` (where ```X``` and ```Y``` are simple or simple equating conditions [or their inverting counterparts]) <br/>
```.NT. X``` - ```! ( x )``` (where ```X``` is a simple or simple equating condition)

## Notes on Operators
An expression may only contain __one__ operator - for example ```X AND Y``` <br/>
A condition may only be of one of the five types listed below:  <br/>
Simple condition - for example ```X .LT. Y``` <br/>
Simple equating condition - for example ```X TIMES Y .EQ. Z``` <br/>
Inverted simple condition - for example ```.NT. X .GT. Y``` (equal to C ```not(x > y)``` <br/>
Inverted simple equating condition - for example ```.NT. Y FROM X .EQ. Z``` (equal to C ```not(x - y == z)``` <br/>
Gated condition - for example ```X .GT. Y .OR. X .EQ. Y``` (equal to C ```x > y || x == y``` <br/>

## Tapes
A programme using a tape is as follows:
```
       PROGRAMME TAPE-DEMONSTRATION
       TAPE 44 OF I8 CALLED TAPE-NAME
       I8-TAPE-NAME-HEAD IS 1
       I8-A IS 1
       
       COME FROM 10 UNLESS TAPE-NAME-HEAD .GT. 43
       TAPE PUT TAPE-NAME I8-A
       I8-A IS I8-A AND 1
 10    TWIDDLE THUMBS
       
       I8-A IS 1
       TAPE-NAME-HEAD IS 1
       
       COME FROM 20 UNLESS TAPE-NAME-HEAD .GT. 43
       TAPE GET TAPE-NAME I8-A
       READ OUT I8-A	   
 20    TWIDDLE THUMBS
       
       END PROGRAMME
```

A tape is a data structure, essentially a two-dimensional array. <br/>
The ```TAPE X OF Y CALLED Z``` statement declares a tape of length ```X``` (```X``` is an integer literal), with elements of type ```Y``` (```Y``` is a type prefix), and of name ```Z```. <br/>
 <br/>
 The tape *head* is an integer variable that contains the location on the tape that will be written to and read from. <br/>
 To avoid situations where one has a tape so long that the head can overflow, the programmer must declare the head and give it a type. <br/>
 To do this, one simply declares a variable as usual, with an integer type, a name the same as that of the tape, and the suffix ```-HEAD```. It is usual to initialise it as being equal to 1, which is the __first element__ in the tape, __not__ zero. For example, ```I24-TAPE-NAME-HEAD IS 1``` declares a 24-bit tape pointer for the tape ```TAPE-NAME``` and initialises it to 1. <br/>
 The ```TAPE PUT X Y``` statement stores  the value of ```Y``` (a variable) to the tape ```X``` (a tape) at the element pointed to its tape head. After using this statement the __head automatically increments__, but can be set to anything like an ordinary variable. <br/>
The ```TAPE GET X Y``` statment gets the value of the element of tape ```X``` pointed to in its head, and assigns its value to the variable ```Y```. After using this statement, the head automatically increments__, but can be set to anything like an ordinary variable.

## Subroutines
A programme that demonstrates a subroutine is as follows:
```
       PROGRAMME SUBROUTINE-DEMO
C      THIS IS A DEMONSTRATION OF A SUBROUTINE
C      THE NEXT LINE IS A LABEL
10     TWIDDLE THUMBS

C      NOW WE'VE COME BACK FROM THE SUBROUTINE, LET'S JUMP PAST IT TO THE END
C      THE NEXT LINE IS A LABEL
20     TWIDDLE THUMBS


       COME FROM 10
       S20-MESSAGE IS "HELLORLD FROM A SUBROUTINE!"
       READ OUT S20-MESSAGE
C      WE'VE DONE WHAT WE NEED TO, NOW LET'S GO BACK
       RETURN
	   
       COME FROM 20
       END PROGRAMME
```
The only new statement is the ```RETURN``` statement, which resumes execution at the line most recently came from in a ```COME FROM X``` statement. <br/>
It is important to, after the main part of your programme is finished running, jump to the end, so your subroutines will not be accidently run!
