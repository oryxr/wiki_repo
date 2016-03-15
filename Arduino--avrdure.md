Programming arduino with avrdude
================================

With AVR-ISP-MKII of olimex
---------------------------

For reading information:

    avrdude.exe -p atmega16 -P usb -c avrispmkii -B5

For flash writing :

    avrdude.exe -p atmega16 -P usb -c avrispmkii -B5 -Uflash:w:[project_path]\fileCompiled.hex:i
