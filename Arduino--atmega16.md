Programming ATMEGA16 with arduino
=================================

boards.txt
----------
Add to the end of `[arduino folder]\hardware\arduino\avr\boards.txt`

    ##############################################################
    atmega16-8.name=Atmega16 (internal 8MHz clock)
    
    atmega16-8.upload.tool=avrdude
    atmega16-8.upload.protocol=avrispmkii
    atmega16-8.upload.maximum_size=14336
    atmega16-8.upload.speed=19200
    
    atmega16-8.bootloader.low_fuses=0x84
    atmega16-8.bootloader.high_fuses=0x99
    
    atmega16-8.build.mcu=atmega16
    atmega16-8.build.f_cpu=8000000L
    atmega16-8.build.core=arduino:arduino
    atmega16-8.build.variant=mega16
    atmega16-8.build.board=AVR_ATMEGA16-8
    
And this but it isn't verify.
    
    ##############################################################
    ATmega16-8.name=ATmega16-External 8Mhz
    
    ATmega16-8.upload.tool=avrdude
    ATmega16-8.build.mcu=atmega16
    ATmega16-8.build.f_cpu=8000000L
    ATmega16-8.build.core=arduino:arduino
    ATmega16-8.build.variant=ATmega16
    ATmega16-8.upload.maximum_size=16000
     
    ##############################################################
    ATmega16-16.name=ATmega16-External 16Mhz
    ATmega16-16.upload.tool=avrdude
    ATmega16-16.build.mcu=atmega16
    ATmega16-16.build.f_cpu=16000000L
    ATmega16-16.build.core=arduino:arduino
    ATmega16-16.build.variant=ATmega16
    ATmega16-16.upload.maximum_size=16000
     
    ##############################################################

pins_arduino.h
--------------
Create folder `Atmega16` in `[arduino folder]\hardware\arduino\avr\variants` and create file `pins_arduino.h` with :

    /**
      pins_arduino.h - Pin definition functions for Arduino for ATmega 16
    
      Based on the code from the following places
      - Alternate core - http://www.avr-developers.com/corefiles/index.html
      - Arduino - http://www.arduino.cc/
    
      @author: Sudar <http://hardwarefun.com>
    
    */
    
    #ifndef Pins_Arduino_h
    #define Pins_Arduino_h
    
    #include <avr/pgmspace.h>
    
    #define NUM_DIGITAL_PINS            32
    #define NUM_ANALOG_INPUTS           8
    #define analogInputToDigitalPin(p)  ((p < 8) ? (p) + 24 : -1)
    #define digitalPinHasPWM(p)         ((p) == 4 || (p) == 5 || (p) == 7 || (p) == 11)
    
    static const uint8_t SS   = 12;
    static const uint8_t MOSI = 13;
    static const uint8_t MISO = 14;
    static const uint8_t SCK  = 15;
    
    static const uint8_t SDA = 17;
    static const uint8_t SCL = 16;
    static const uint8_t LED_BUILTIN = 13; // We don't have a built-in LED, but still ..
    
    static const uint8_t A0 = 24;
    static const uint8_t A1 = 25;
    static const uint8_t A2 = 26;
    static const uint8_t A3 = 27;
    static const uint8_t A4 = 28;
    static const uint8_t A5 = 29;
    static const uint8_t A6 = 30;
    static const uint8_t A7 = 31;
    
    //TODO: these are not assigned properly. Need to test more
    #define digitalPinToPCICR(p)    (((p) >= 0 && (p) <= 32) ? (&PCICR) : ((uint8_t *)0))
    #define digitalPinToPCICRbit(p) (((p) <= 7) ? 2 : (((p) <= 13) ? 0 : 1))
    #define digitalPinToPCMSK(p)    (((p) <= 7) ? (&PCMSK2) : (((p) <= 13) ? (&PCMSK0) : (((p) <= 21) ? (&PCMSK1) : ((uint8_t *)0))))
    #define digitalPinToPCMSKbit(p) (((p) <= 7) ? (p) : (((p) <= 13) ? ((p) - 8) : ((p) - 14)))
    
    #ifdef ARDUINO_MAIN
    
    // digital pins are also used for the analog output (software PWM).
    // Analog input pins are a separate set.
    
    // ATMEL ATMEGA16 & 16A
    //
    //                   +-\/-+
    //      (D  8) PB0  1|    |40  PA0 (AI 0)
    //      (D  9) PB1  2|    |39  PA1 (AI 1)
    //      (D 10) PB2  3|    |38  PA2 (AI 2)
    //  PWM (D 11) PB3  4|    |37  PA3 (AI 3)
    //      (D 12) PB4  5|    |36  PA4 (AI 4)
    //      (D 13) PB5  6|    |35  PA5 (AI 5)
    //      (D 14) PB6  7|    |34  PA6 (AI 6)
    //      (D 15) PB7  8|    |33  PA7 (AI 7) 
    //           RESET  9|    |32  AREF
    //             VCC 10|    |31  GND
    //             GND 11|    |30  AVCC
    //           XTAL2 12|    |29  PB7 (D 23)
    //           XTAL1 13|    |28  PC6 (D 22)
    //       (D 0) PD0 14|    |27  PC5 (D 21)
    //       (D 1) PD1 15|    |26  PC4 (D 20)
    //       (D 2) PD2 16|    |25  PC3 (D 19) 
    //       (D 3) PD3 17|    |24  PC2 (D 18)
    //   PWM (D 4) PD4 18|    |23  PC1 (D 17)
    //   PWM (D 5) PD5 19|    |22  PC0 (D 16) 
    //       (D 6) PD6 20|    |21  PD7 (D 7) PWM
    //                   +----+
    
    // these arrays map port names (e.g. port B) to the
    // appropriate addresses for various functions (e.g. reading
    // and writing)
    const uint16_t PROGMEM port_to_mode_PGM[] = {
    	NOT_A_PORT,
    	(uint16_t) &DDRA,
    	(uint16_t) &DDRB,
    	(uint16_t) &DDRC,
    	(uint16_t) &DDRD,
    };
    
    const uint16_t PROGMEM port_to_output_PGM[] = {
    	NOT_A_PORT,
    	(uint16_t) &PORTA,
    	(uint16_t) &PORTB,
    	(uint16_t) &PORTC,
    	(uint16_t) &PORTD,
    };
    
    const uint16_t PROGMEM port_to_input_PGM[] = {
    	NOT_A_PIN,
    	(uint16_t) &PINA,
    	(uint16_t) &PINB,
    	(uint16_t) &PINC,
    	(uint16_t) &PIND,
    };
    
    const uint8_t PROGMEM digital_pin_to_port_PGM[] = {
    	PD, PD, PD, PD, PD, PD, PD, PD, /*  0 - 7  */
    	PB, PB, PB, PB, PB, PB, PB, PB, /*  8 - 15 */
    	PC, PC, PC, PC, PC, PC, PC, PC, /* 16 - 23 */
    	PA, PA, PA, PA, PA, PA, PA, PA, /* 24 - 31 */
    };
    
    const uint8_t PROGMEM digital_pin_to_bit_mask_PGM[] = {
    	_BV(0), _BV(1), _BV(2), _BV(3), _BV(4), _BV(5), _BV(6), _BV(7), /*  0 - 7 : port D */
    	_BV(0), _BV(1), _BV(2), _BV(3), _BV(4), _BV(5), _BV(6), _BV(7), /*  8 - 15 : port B */
    	_BV(0), _BV(1), _BV(2), _BV(3), _BV(4), _BV(5), _BV(6), _BV(7), /* 16 - 23 : port C */
    	_BV(0), _BV(1), _BV(2), _BV(3), _BV(4), _BV(5), _BV(6), _BV(7), /* 24 - 31 : port A */
    };
    
    const uint8_t PROGMEM digital_pin_to_timer_PGM[] = {
    	NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, TIMER1B, TIMER1A, NOT_ON_TIMER, TIMER2, /*  0 - 7 : port D */
    	NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, TIMER0A, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, /*  8 - 15 : port B */
    	NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER,NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER,NOT_ON_TIMER, /* 16 - 23 : port C */
    	NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, NOT_ON_TIMER, /* 24 - 31 : port A */
    	/* TODO: Need to verify TIMER0A */
    };
    
    #endif
    
    #endif

UART
----
For Serial library to work properly must be made following changes to the file HardwareSerial.cpp
In `[arduino_path]\hardware\arduino\avr\cores\arduino\HardwareSerial.cpp`

we will replace:

    #if defined(__AVR_ATmega8__)
      config |= 0x80; // select UCSRC register (shared with UBRRH)
    #endif

with

    #if defined(__AVR_ATmega8__) || defined(__AVR_ATmega32__) || defined(__AVR_ATmega16__)
      config |= 0x80; // select UCSRC register (shared with UBRRH)
    #endif

Useful link
-----------
[openhardware.ro](http://openhardware.ro/using-atmega16-with-arduino-ide/) : inspired tutorial

[instructurables.com](http://www.instructables.com/id/Programming-ATmega16A-using-arduino-IDE/)

[Arduino IDE 1.5 3rd party Hardware specification](https://github.com/arduino/Arduino/wiki/Arduino-IDE-1.5-3rd-party-Hardware-specification)