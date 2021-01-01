***UNICODE

**Timeline:**

**ASCII** - a character set which can be stored in 7 bits (0 - 127) (decimals) 
Codes 0 - 32 used for control characters. (inprintable, for example 7 made computer beep etc)  
Codes 32 - 127 used for characters. (for example character A was code 65, 1000001)  
Since computers use 8 bit bytes, 1 bit was left for imagination (codes 128 - 255).
Welcome **OEM** character sets - everyone had their own interpretation for codes 128 - 255.
It was a problem, since everyone had their idea what 128-255 should be, so for example when you sent  
email from Russia then in Israel the characters in this space show up incorrectly.

**ANSII**
OEM gets codified in the ANSII standard, meaning everyone agree what to do below 128 which is pretty much same
as ASCII but after 128 a code pages would be used depending on where you lived.

**Unicode**
An effort to create a single character set to include all the writing systems on the planet.  
In Unicode, characters map to a code points, for example U+0041 maps to character 'A'.  
**U+** means unicode and numbers are **hexademical**.  

**UTF-8** is a system of storing Unicode code points in memory using 8 bit bytes.  
Every code point from 0-127 is stored in single byte.
Codes 128 and above are stored using 2, 3 and up to 6 bytes.  
This means English text looks same in UTF-8 as in ASCII/ANSI/OEM. For example U+0041 will be stored as 41(hex).
