# BADTRAN - a bad FORTRAN

BADTRAN is a language derived from FORTRAN, but designed to be bad, while still being usable.
At present there is no implementation of the language, as I (rchipp42) lack the skills to write one. If you would like to, you may write an implementation of BADTRAN, and for the first one, an interpreter in a portable compiled language would be nice.

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
