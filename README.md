# Due Debug
My notes on debugging a arduino Due in ubuntu 16.04, for future reference, and to help whoever needs to do the same.

## Material:
1. Arduino Due.
2. stm32 arduino board (~2$) with stlink v2 programmer (~2$) from aliexpress.
3. Micro USB cables.
4. Connectors.

## Steps:

### STM32 Firmware
Get Mitek's wonderfull stm32 CMSIS-DAP firmware:
[https://github.com/x893/CMSIS-DAP](https://github.com/x893/CMSIS-DAP)

Download the .hex file in "CMSIS-DAP/Firmware/STM32/hex/CMSIS-DAP-V1-F103.hex"

### Convert to a bin file using:
$ arm-none-eabi-objcopy -I ihex --output-target=binary CMSIS-DAP-V1-F103.hex firmware.bin

### Upload to smt32 
1. Install stlink: [https://github.com/texane/stlink](https://github.com/texane/stlink)
2. $ make release
3. (for  system wide install)
   $ cd build/Release; sudo make install 
4. upload using stlink or platformio
   $ cd stm32
   $ sh upload.sh
   
### Upload Blink with debug symbols (-g) to due
$ cd ..
$ cd due
$ sh upload.sh

Use edbg to test programming and verifying the firmware:
[https://github.com/ataradov/edbg](https://github.com/ataradov/edbg)

### Download the repo:
$ make all

### Program:
$ ./edbg -p -t atmel_cm3 -f myduefirmware.bin 

#### Verify:
$ ./edbg -v -t atmel_cm3

## Debug with OpenOCD
$ apt install openocd

$ openocd -c "interface cmsis-dap" -f due.cfg 

## Using gdb interface
$ arm-none-eabi-gdb example.elf
(gdb) target remote localhost:3333
Remote debugging using localhost:3333

## Using Clion interface





