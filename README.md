# Infrared-data-transmission-reception-module-
    Device development from idea to finished product with all processes and complexities. Processes such as block diagram design, schematic diagram, code design, block diagram code presentation, program code writing and debugging, microcontroller compilation and firmware, circuit diagram development and design, testing of the finished prototype for possible errors during operation will be considered. The idea of ​​the device is to receive an IR signal and use a transmitter to read and redirect to another source of IR beam.

# Functional diagram of the device
![1](https://user-images.githubusercontent.com/64357748/85791713-f7b45c00-b73a-11ea-8cd8-08fbfcc4a59e.jpg)

# Electrical functional diagram of the device
![2](https://user-images.githubusercontent.com/64357748/85793436-ad80aa00-b73d-11ea-94f2-7b37e860d175.gif)

# Device development environment
    Arduino is a hardware computing platform, the main components of which are the I / O board and the development environment in the Processing / Wiring language. The Arduino platform is used to create electronic devices with the ability to receive signals from various digital and analog sensors that can be connected to it, and control various devices. Arduino can be used both to create interactive automation objects and to connect to software on a computer via standard wired and wireless interfaces (for example: Adobe Flash, Processing, Max / MSP, Pure Data, SuperCollider). The Arduino board mainly consists of microcontrollers of the Atmel AVR family: ATmega328, ATmega168, ATmega2560, ATmega32U4, ATTiny85, as well as elements for programming and integration with other devices. Many boards have a linear voltage regulator + 5V or + 3.3V. Clocking is performed at a frequency of 16 or 8 MHz quartz resonator. The microcontroller has a bootloader, so no external programmer is required. As part of third-party collaboration, the Arduino IDE included support for some Intel x86 hardware. Intel Galileo and Intel Edison - Arduino-compatible boards on Intel x86 architecture. The boards are mechanically and electrically compatible with Arduino peripheral boards. The boards operate under their own Linux operating system, on top of which runs an application that allows you to download and run sketches Arduino. The boards are programmed via USB, which is possible thanks to the USB-to-Serial FT232R converter chip. Arduino boards are programmed through its own software shell (IDE), available free of charge on the Arduino website. This shell contains a text editor, project manager, preprocessor, compiler and tools for downloading the program. Using free software in the learning process Arduino nano 3.0 Arduino UNO R3 Arduino Leonardo microcontroller. The shell is written in Java based on the Processing project, runs on Windows, Mac OS X and Linux. Arduino programs are written in the C or C ++ programming language. The Arduino development environment comes with a software library called Wiring. Users need to define only two functions in order to create a program that will work on the principle of cyclic execution: setup (): the function is executed only once at program start and allows you to set the initial parameters; loop (): the function is executed periodically until the board is turned off .

# The text of the program and explanations to it
## 1) Sketch of the board to which the receiver and LCD are connected
```C++
#include "Wire.h" //library for interaction with I2C (бібліотека для взаємодії з I2C)
#include "LiquidCrystal_I2C.h" // I2C library for LCD (бібліотека I2C для LCD)
#include <IRremote.h> // library for working with the IR receiver (бібліотека для роботи з ІЧ приймачем)
LiquidCrystal_I2C lcd(0x27,16,2); //set the address for the LCD (встановлюємо адресу для LCD)
int receiver = 11; // connect the signal contact of the receiver to 11 (підключаємо сигнальний контакт ресивера до 11)
IRrecv irrecv(receiver); // create a receiver object using a name of your choice (створюємо об’єкт ресивера, використовуючи ім’я за власним вибором)
decode_results results; //the results are returned by the decoder (результати повернуті декодером) 
void setup()
{
  lcd.init();          // LCD initialization (ініціалізація LCD)
  lcd.backlight(); // включаємо підсвітку LCD дисплея (turn on the backlight of the LCD display)
  irrecv.enableIRIn(); // запускаємо приймач (start the receiver)
}
void translateIR() // робить певні операції на основі отриманого коду (performs certain operations based on the received code)
// відображення ІК-кодів на модулі LCD (display of IR codes on the LCD module)
{ 
  switch(results.value)// конструкція switch отримує результат (the switch construct gets the result)
  {
    case 0xFF58A7: lcd.println(" OK      "); break;
    case 0xFF5AA5: lcd.println(" Power   ")     ; break;
    case 0xFF728D: lcd.println(" Mute            "); break;
    case 0xFFCA35:  lcd.println(" One             "); break;
    case 0xFF0AF5:  lcd.println(" Two           "); break;
    case 0xFF08F7:  lcd.println(" Three          "); break;
    case 0xFFEA15:  lcd.println(" Four           "); break;
    case 0xFF2AD5:  lcd.println(" Five           "); break;
    case 0xFF28D7:  lcd.println(" Six            "); break;
    case 0xFFF20D:  lcd.println(" Seven          "); break;
    case 0xFF32CD:  lcd.println(" Eight          "); break;
    case 0xFF30CF:  lcd.println(" Nine           "); break;
    case 0xFFF00F:  lcd.println(" Zero           "); break;
    case 0xFFD827:  lcd.println(" Volume up      "); break;
    case 0xFFDA25:  lcd.println(" Volume down    "); break;
    case 0xFF609F:   lcd.println(" Channel up     "); break;
    case 0xFF6897:  lcd.println(" Channel down   "); break;
// use break to forcibly exit the loop (використовуємо break для примусового виходу з циклу)
    default: lcd.println(" Other button   ");
  }
  delay(500);
  lcd.clear(); // clear the display of data (очищаємо дисплей від данних)
}
void loop()
{
  if (irrecv.decode(&results)) // Did the data come? (Дані прийшли?)
  {
    translateIR();
    for (int z=0; z<2; z++) // ignore the second and third re-signal (ігнорувати другий і третій повторний сигнал)
    {
      irrecv.resume(); // accept the following command (приймаємо наступну команду)
    }
  }
}
```

## 2) Sketch of the board to which the transmitter is connected.
