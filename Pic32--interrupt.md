Gestion des interruptions extérieurs sur PIC32
==============================================

    #include <plib.h>
    #define SYS_FREQ (80000000L)
    
    void setup() {
      SYSTEMConfig(SYS_FREQ, SYS_CFG_WAIT_STATES | SYS_CFG_PCACHE);
      INTEnableSystemMultiVectoredInt();
    
      /* initialisation interruption extern 0 */
      IEC0CLR = 0x00000008; // disable INT0
      INTCONSET = 0x00000001; // set the bit for rising edge trigger
      IFS0CLR = 0x00000008; // clear the interrupt flag
      IPC0CLR = 0x1F000000; // clear the priority
      IPC0SET = 0x09000000; // set the priority to 2 and sub-priority to 1
      IEC0SET = 0x00000008; // enable INT0;
    
      // temporaire
      pinMode(23,OUTPUT);
      LATCCLR=0x0008;
      Serial.begin(115200);
      // fin temporaire
    }
    
    void loop(){
    }
    
    extern "C" { 
      void __ISR(_EXTERNAL_0_VECTOR,ipl2) Int0Handler(void){
        IFS0CLR = 0x00000008;
        LATCINV = 0x0008;
      }
    }