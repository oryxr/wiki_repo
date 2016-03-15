Code for MPLAB X
================

Fichier de base
---------------
    /* File:   main.c
     * Author: pierre
     * Fichier pour Microchip PIC32MX795F512L 
     * Compilateur MPLAB C32 Lite
     */
    #include <plib.h>
    
    // Configuration Bit settings
    #pragma config FPLLMUL = MUL_20, FPLLIDIV = DIV_2, FPLLODIV = DIV_1, FWDTEN = OFF
    #pragma config POSCMOD = HS, FNOSC = PRIPLL, FPBDIV = DIV_8
    
    // Definition de la frequence du systeme
    #define SYS_FREQ (80000000L)
    
    int main(int argc, char** argv) {
    /*Configure cache, wait states and peripheral bus clock
     Configure the device for maximum performance but do not change the PBDIV
     Given the options, this function will change the flash wait states, RAM
     wait state and enable prefetch cache but will not change the PBDIV.
     The PBDIV value is already set via the pragma FPBDIV option above.. */
        SYSTEMConfig(SYS_FREQ, SYS_CFG_WAIT_STATES | SYS_CFG_PCACHE);
        
        while(1){
        }
        return (EXIT_SUCCESS);
    }

Interruption Timer 2
--------------------
interruption timer toutes les 1 micro secondes
    #include <plib.h>
    
    // Configuration Bit settings
    #pragma config FPLLMUL = MUL_20, FPLLIDIV = DIV_2, FPLLODIV = DIV_1, FWDTEN = OFF
    #pragma config POSCMOD = HS, FNOSC = PRIPLL, FPBDIV = DIV_2
    
    // Definition de la frequence du systeme
    #define SYS_FREQ (80000000L)
    
    int main(int argc, char** argv) {
        SYSTEMConfig(SYS_FREQ, SYS_CFG_WAIT_STATES | SYS_CFG_PCACHE);
        INTEnableSystemMultiVectoredInt();
        mPORTDSetPinsDigitalOut(BIT_1);
    
        // Timer 2
          T2CON = 0x0; // Stop timer and clear control register
                                   // set prescaler at 1:1, internal clock source
          TMR2 = 0x0;  // Clear timer register
          PR2 = 40; //= 0x0028;//  Load period register : incrementation ttes les 25ns
                           //  40 incrementations avant interruptions = 1 interruption toutes les microsecondes;
          IPC2SET = 0x0000000C; // Set priority level = 3;
          IPC2SET = 0x00000001; // Set priority level = 1;
          IFS0CLR = 0x00000100; // Clear timer interrupt status flag
          IEC0SET = 0x00000100; // Enable timer interrupts
          T2CONSET = 0x8000; // Start timer
        
        while(1){
        }
        return (EXIT_SUCCESS);
    }
    
    void __ISR(_TIMER_2_VECTOR,ipl3)Timer2Handler(void){
        mPORTDToggleBits(BIT_1);
        IFS0CLR = 0x00000100; // Clear timer interrupt status flag
    }