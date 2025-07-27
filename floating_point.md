**Why does not 0.1+0.2-0.3 equal to 0.0.?**
This has to do with floating point accuracy and computer's abilities to represent numbers in memory. For a full breakdown, check out: https://github.com/BlackSmith09/techdocs.git
![alt text](image.png)

**Floating point Arithmetic: Issues and Limitations**
Floating-point numbers are represented in computer hardware as base 2 (binary) fractions. For example, the decimal fraction.
0.125
has value 1/10 + 2/100 + 5/1000, and in the same way the binary fraction
0.001
has value of 0/2 + 0/4 + 1/8. These two fractions have identical values, the only real difference being that the first is written in base 10 fractional notation, and the second in base 2.
              
Unfortunately, most decimal fractions cannot be represented exactly as binary fractions. A consequence is that, in general, the decimal floating-point numbers you enter are only approximated by the binary floating-point numbers actually stored in the machine.
              
The problem is easier to understand at first in base 10. Consider the fraction 1/3. You can approximate that as a base 10 fraction:
              
0.3
or, better,
0.33
or, better,
0.333
and so on. No matter how many digits you are willing to write down, the result will never be exactly 1/3, but will be an increasingly better approximation of 1/3.
              
In the same way, no matter how many base 2 digits you are willing to write down, the result will never be exactly 1/3, but will be an increasingly better approximation of 1/3.
In the same way, no matter how many base 2 digits you are willing to use, the decimal value 0.1 cannot be represented exactly as a base 2 fraction. In base 2, 1/10 is the infinitely repeating fraction.
              
0.0001100110011001100110011001100110011001100110011…
              
Stop at any finite number of bits, and you get an approximation.
On a typical machine running Python, there are 53 bits of precision available for a Python float, so the value stored internally when you enter the decimal number 0.1 is the binary fraction
              
0.00011001100110011001100110011001100110011001100110011010
              
which is close to, but not exactly equal to, 1/10.
It’s easy to forget that the stored value is an approximation to the original decimal fraction, because of the way that floats are displayed at the interpreter prompt. Python only prints a decimal approximation to the true decimal value of the binary approximation stored by the machine. If Python were to print the true decimal value of the binary approximation stored for 0.1, it would have to display
              
>>> 0.1
0.1000000000000000055511151231257827021181583404541015625
              
That is more digits than most people find useful, so Python keeps the number of digits manageable by displaying a rounded value instead
              
>>> 0.1
0.1
              
It’s important to realize that this is, in a real sense, an illusion: the value in the machine is not exactly 1/10, you’re simply rounding the display of the true machine value. This fact becomes apparent as soon as you try to do arithmetic with these values
              
>>> 0.1 + 0.2
0.30000000000000004
              
Note that this is in the very nature of binary floating-point: this is not a bug in Python, and it is not a bug in your code either. You’ll see the same kind of thing in all languages that support your hardware’s floating-point arithmetic (although some languages may not display the difference by default, or in all output modes).
              
Other surprises follow from this one. For example, if you try to round the value 2.675 to two decimal places, you get this
              
>>> round(2.675, 2)
2.67
              
The documentation for the built-in round() function says that it rounds to the nearest value, rounding ties away from zero. Since the decimal fraction 2.675 is exactly halfway between 2.67 and 2.68, you might expect the result here to be (a binary approximation to) 2.68. It’s not, because when the decimal string 2.675 is converted to a binary floating-point number, it’s again replaced with a binary approximation, whose exact value is
              
2.67499999999999982236431605997495353221893310546875
              
Since this approximation is slightly closer to 2.67 than to 2.68, it’s rounded down.