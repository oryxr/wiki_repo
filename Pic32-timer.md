Gestion des timers sur Pic32
============================
Calcul de la valeur du registre PRx
-----------------------------------
A chaque fois que le compteur TMRx arrive à la valeur de PRx il se réinitialise et lance une interruption.

Pour le calculer on a :

`Période_INT = (PRx - 1)*période_horloge*prédiviseur`

donc : `PRx = (Période_INT / période_horloge / prédiviseur) - 1`

d'où : `PRx = (Fréq_horloge / Fréq_INT / prédiviseur) - 1`

Exemple
-------

    #include <plib.h>
    #define SYS_FREQ (80000000L)
    
    void setup() {
      SYSTEMConfig(SYS_FREQ, SYS_CFG_WAIT_STATES | SYS_CFG_PCACHE);
      INTEnableSystemMultiVectoredInt();
      /* initialisation timer 2 */
      // configure Timer 2 utilisant l'horloge interne avec prescale
      T2CON = 0x0; // stop timer and clear control register,
      T2CONSET = 0x00000070; // prescaler at 1:256, internal clock source
      TMR2 = 0x0;
      PR2 = 311; // interruption toutes les 1ms
      // configure l'interruption du timer 2
      IPC2SET = 0x0000000D; // set priority leve = 3 and sub-priority level = 1;
      IFS0CLR = 0x00000100; // clear timer interrupt status flag
      IEC0SET = 0x00000100; // enable timer interrupts  
      T2CONSET = 0x8000; // start timer
      /* fin initialisation timer 2 */
    #if 0
      /* 2eme solution d'initialisation */
      OpenTimer2(T2_ON | T2_SOURCE_INT | T2_PS_1_256, 311);
      ConfigIntTimer2(T2_INT_ON | T2_INT_PRIOR_3);
    #endif 
    
      // temporaire
      pinMode(22,OUTPUT);
      LATCCLR=0x0004;
      Serial.begin(115200);
      // fin temporaire
    }
    
    void loop(){
    }
    
    extern "C" {
      void __ISR(_TIMER_2_VECTOR, ipl3) Timer2Handler(void){
        LATCINV=0x0004;
        IFS0CLR = 0x00000100; //remise a zero du drapeau
            /* 2eme solution de remise à zéro du drapeau
            mT2ClearIntFlag();*/
      }
    }