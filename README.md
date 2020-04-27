# LCD1602A in C/C++
LCD1602A for Raspberry in C
This code is not mine , just uploaded because didnt find it anywhere else , the true dev is on the below link

http://www.bristolwatch.com/rpi/i2clcd.htm

by Lewis Loflin

See YouTube video Interface I2C LCD to Raspberry Pi in C

In this project I'll use WiringPi I2C to interface an I2C LCD display module. This is an interface board with a small microcontroller that controls a HD44780 type liquid crystal display.

There are libraries for operating this device with Arduino but getting the actual operating codes to avoid the libraries was useless. I found a program in Python and ported that over to WiringPi and used C directly.

Above illustrates the type of connection in the module. (This is not the actual schematic, but can be programmed to do the same thing if one wanted to build their own.) One must write each command or data byte as two half bytes or nibbles. This is done using two small functions lcd_byte(int bits, int mode) and lcd_toggle_enable(int bits).

Here we can pass display control data or ASCII character data.


// added by Lewis
void typeInt(int i);
void typeFloat(float myFloat);
void lcdLoc(int line); //move cursor
void ClrLcd(void); // clr LCD return home
void typeln(const char *s);
void typeChar(char val);

The above commands were used with Arduino C and Microchip PIC C and ported over to Raspberry Pi C. TypeFloat() and typeInt() functions use the sprintf() function to convert the integers and floats to as string. Then both are sent to typeln() function which needs a little explanation.

In fact we can send a simple text string director to the typeln() function: typeln("Hello world!"). lcdLoc() sets the cursor location for the next character even is programmed to display no blinking cursor. Ox80 (LINE1) is the address for ROW 0 COL 0 while 0xC0 (LINE2) is ROW 1 COL 0.

Assuming a 2-line display and I wanted send a "F" three places to the right on the first line would be lcdLoc(LINE1 + 3).


// this allows use of any size string
void typeln(const char *s)   {
  while ( *s ) lcd_byte(*(s++), LCD_CHR);
}

Recall that a text string is an array of characters numbered from zero through n-1 terminated with a null character I believe zero. We pass with the "*" the address pointer to the beginning memory address of the string not the string itself.

The function typeln() will output to the display via lcd_byte() function one character at a time incrementing the address pointer "s" until the null character is reached terminating the "while" loop.

For full code see i2clcd.txt
