# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.6.0]
2013-10-17 (REL_1_6_0) Armink <armink.ztl@gmail.com>
 Notes: Added modbus master.

## [1.5.0]
2010-05-06 (REL_1_5_0) Christian Walter <cwalter@embedded-solutions.at>
 Notes: Added support for Atmel AT91SAM3S (Cortex M3) for IAR.

 Detailed notes:
  - FEATURES (ATSAM3S) : Added new port.

## [1.4.0]
2007-08-28 (REL_1_4_0) Christian Walter <wolti@sil.at>:
 Notes: Added support for HCS08. Fixed some small bugs in the documentation
  for the porting layer.

 Detailed notes:
  - FEATURES (HCS08) : Added new port.
  - BUGS (ALL)       : Fixed some small bugs in the porting guide.

## [1.3.0]
2007-07-17 (REL_1_3_0) Christian Walter <wolti@sil.at>:
 Notes: Added ARM7/AT91SAM7X port. Added Linux/TCP port from Steven Guo.

 Detailed notes:
  - FEATURES (ARM7): Added ARM7/AT91SAM7x port.
  - FEATURES (LINUX): Added Linux/TCP port from Steven Guo.
  - BUGS (ALL): Fixed bug in <eMBFuncReadInputRegister> where the high
      byte of the register count was ignored. This does not have a
	  practical impact because the actual number of registers is always
	  lower.

## [1.3.0]
2007-07-17 (REL_1_3_0) Christian Walter <wolti@sil.at>:
 Notes: Added Linux/TCP port. Fixed bug in MSP430 port.
 Detailted notes:
  - FEATURE (LINUX): Added Linux/TCP port.
  - BUGS (MSP430): Fixed bug with calculating the timer value.

## [1.2.0]
2007-04-25 (REL_1_2_0) Christian Walter <wolti@sil.at>:
 Notes: Added LPC214X ARM port with Keil compiler. Added Z8Encore port for
   Z8F6422 microcontroller.

 Detailed notes:
  - FEATURE (ARM): Added LPC214X ARM port for Keil ARM 2.41.
  - FEATURE (Z8ENCORE): Added Z8F6422 for Z8Encore using the ZDS II - Z8
      Encore! development tools.

## [1.1.2]
2007-02-18 (REL_1_1_2) Christian Walter <wolti@sil.at>:
 Notes: Fixed typo with the defined defining the supported Modbus
   functions.  Fixed bug when illegal slave address was passed to eMBInit
   where the error was not detected. Fixed typo in the holding registers
   where the frame for write multiple registers was parsed with the wrong 
   constants. The fix is not critical because the values matched. Fixed bug 
   in discrete input registers implementation where the frame was not parsed
   correctly. Added new support for a CodeWarrior Coldfire port.

 Detailed notes:
   - BUG (ALL): Modbus functions are compiled into the stack conditionally
      by changing the MB_FUNC_XXX defines to either true(1) or false(0).
      The defines for MB_FUNC_READ_HOLDING and MB_FUNC_WRITE_HOLDING
      were wrong.
  - BUG (ALL): eMBInit did not correctly check for addresses. Therefore
      is was possible to start the Modbus stack with an address of 0
      or one > 247.
  - BUG (ALL): eMBFuncWriteHoldingRegister should use 
      MB_PDU_FUNC_WRITE_MUL_ADDR_OFF and not MB_PDU_FUNC_READ_ADDR_OFF.   
  - BUG (ALL): eMBFuncReadDiscreteInputs calculated the number of discrete
      registers to read wrong.
  - FEATURE (ALL): Fixed some warnings in the code.

## [1.1.1]
2006-11-19 (REL_1_1_1) Christian Walter <wolti@sil.at>:
 Notes: Fixed bug in Read/Write Multiple Registers function where
   the registers addresses where calculated wrong.
   Fixed bug in RTU and ASCII with the resource allocation in case of
   an error. 
   Changed license to BSD style licsense.

 Detailed notes:
   - OTHER (ALL): License is now BSD for protocol stack.
   - BUG (ALL): The registers address received in a Modbus frame
      must be converted to application addresses. The code for this
      conversion was missing and therefore has lead to error when
      this function was used (Registers of by one, Start at > 1).
   - BUG (ALL): If the serial initialization within the porting fails
      a timer is still allocated in eMBRTUInit and eMBASCIIInit. This
      can lead to a memory leak depending upon the implementation of the
      porting layer.
   - FEATURE (MCF5235): Added sample shell scripts for testing.
   - FEATURE (MSP430): Added sample shell script for testing and
      changed default values to match the other ports.

## [1.1.0]
2006-10-30 (REL_1_1_0) Christian Walter <wolti@sil.at>:
 Notes: Added support for Read/Write Multiple Registers function
   (0x17). Added some tips to reduce memory requirements. 
   Added MSP430 Port for GCC and Rowley Crossworks.

 Detailed notes:
   - FEATURE (MSP430): Added new MSP430 port.
   - FEATURE (ALL): Added support for Read/Write Multiple Registers
      function (0x17). The implementation simply makes two callbacks
      to the eMBRegHoldingCB function where first the values are
      written and then the other register values are read.
   - FEATURE (ALL): Added some tips on reducing memory requirements
       with the protocol stack.

## [1.0.5]
2006-10-30 (REL_1_0_5) Christian Walter <wolti@sil.at>:
 Notes: eMBDisable and eMBClose can now be called multiple times 
   which makes shutdown of the protocol stack easier.
   Fixed bug in RTU state machine where we switched from the
   error state immediately to the idle state. Correct behaviour
   would be to wait till the end of frame.
   Added new STR71X GCC port which uses only freely available tools
   like GNU ARM, OpenOCD (Wiggler) and GDB. 

 Detailed notes:
   - FEATURE (STR71X): Added GCC standalone port which does not
      depend on the Rowley Crosswork tools.
   - FEATURE (ALL): eMBDisable can now be called multiple times
      and returns MB_ENOERR in case is was already disabled. 
      eMBClose also supports beeing called multiple times in 
      which pvMBFrameCloseCur( ) is called when the protocol stack
      is in state STATE_DISABLED.
   - BUG (RTU): Fixed bug in xMBRTUReceiveFSM where the error
     state is immediately left because of a missing break. Instead
     we should wait till the damaged frame is finished.

## [1.0.4]
2006-10-11 (REL_1_0_4) Christian Walter <wolti@sil.at>:
 Notes: Fixed bug when more than 255 coils are requested. Fixed bug in 
   Linux/Cygwin port when not all bytes could be written by the first 
   call to write. Added support for removing previously registered
   function handlers.
   
 Detailed notes:
   - BUG (ALL): mbfunccoils contained a bug which limited the amount
      of coils to read to 255. 
   - BUG (LINUX): prvbMBPortSerialWrite contained a bug in the loop
      which writes the RTU/ASCII frame to the serial file descriptor.
      If not all bytes where written in the first call or write was
      interrupted the sent frame is corrupted.
   - FEATURE (ALL): eMBRegisterCB now supports NULL as handler 
      argument in which case a previously registered function
	  handler is deregistered from the protocol stack.

## [1.0.3]
2006-09-27 (REL_1_0_3) Christian Walter <wolti@sil.at>:
 Notes: Added new functions to support registering of custom callback
   handlers. This makes it possible to implement new Modbus function
   codes without touching the protocol stack.
   New port for ATMega128 added. Thanks to Richard C Sandoz Jr. for
   the patches.

 Detailed notes:
   - FEATURE (ALL): Added support for registering new functions handlers
      with eMBRegisterCB.
   - FEATURE (AVR): Added patches from Richard C Sandoz Jr. for ATMega128

## [1.0.2]
2006-09-06 (REL_1_0_2) Christian Walter <wolti@sil.at>:
 Notes: Fixed bug in FreeRTOS porting layer for STR71X/lwIP target where
   memory is not freed in the sys_arch_thread_remove function. 
   Synched MCF5235TCP port with the FreeRTOS/lwIP port for the STR71X.

 Detailed notes:
   - BUG (STR71XTCP): Sys_arch_thread_remove did not free the memory from
       the TCB.
   - BUG (STR71XTCP): Unnecessary call to vTaskSuspendAll removed.
   - BUG (STR71XTCP): Bug with counting variable. The first to lwIP tasks
       got the same name (lwIP0).
   - FEATURE (MCF5235TCP): Enhanced functions from the STR71X/lwIP port
       merged into the Coldfire port.

## [1.0.1]
2006-09-04 (REL_1_0_1) Christian Walter <wolti@sil.at>:
 Notes: Fixed bug in serial driver for STR71x target when the ring buffer
   overflows. 

 Detailed notes:
   - BUG (STR71XTCP): Under high load the ring buffer in the serial driver
       functions might overflow. There was an error with counting the number
       of received characters which corrupted received frames.
       Now receiver correctly recovers in case of dropped bytes.

## [1.0.0]
2006-09-04 (REL_1_0) Christian Walter <wolti@sil.at>:
 Notes: Added support for ATmega8, ATmega16, ATmega32, ATmega169 and 
   RS485 drivers in the AVR support. Special thanks to Tran Minh Hoang 
   for his contribution.
   Added a new lwIP port for the STR71X target which uses one serial 
   interface for a PPP connection. This can be used for remote Modbus/TCP
   devices in combination with a Modem (E.g.  GPRS or Analog).

 Detailed notes:
   - FEATURES (AVR): Integrated patches from Tran Minh Hoang to support the
       ATmega8, ATmega16, ATmega32, ATmega169 controllers.
   - FEATURES (AVR): Added support for RS485 drivers in the AVR code. The 
       example supports the DS76176.
   - FEATURES (STR71XTCP): implemented function in STR71X/lwIP porting layer
       to remove running tasks.
   - FEATURES (STR71XTCP): added new thread creation function in STR71X/lwIP
       porting layer which allows specifing the stack size.
   - BUGS (STR71XTCP): pppOpen defined in ppp.c does not check the return 
       value of sys_thread_new. If task creation fails the system crashes.
   - BUGS (STR71XTCP): pppMain must not return - Instead it should remove 
       its task from the scheduler.

## [0.9.0]
2006-08-30 (REL_9) Christian Walter <wolti@sil.at>:
 Notes: Added lwIP port for the MCF5235 target. The lwIP part is
   generic and therefore FreeModbus now works on any target with
   lwIP support.
        
 Detailed notes:
   - FEATURES: Incoperated MCF5235 FreeRTOS/lwIP port done by the 
       author in this project.
   - FEATURES: Added lwIP port for FreeModbus
   - FEATURES: Added demo application for FreeModbus and lwIP.

## [0.82.0]
2006-08-22 (REL_0_82) Christian Walter <wolti@sil.at>
 Notes: Fixed bug with Modbus ASCII support

 Detailed notes:
   - BUG: During the last upgrade an error was introduced in the
       initialization code of Modbus ASCII and therefore ASCII
       support was broken. The bug is fixed now and was tested with
       the Win32 port.

## [0.81.0]
2006-08-22 (REL_0_81) Christian Walter <wolti@sil.at>
 Notes: Added porting guide

 Detailed notes:
   - OTHER: Added a new porting guide to the documentation.
   - OTHER: Added a empty example for new ports to the project as a 
       starting point.

## [0.8.0]
2006-08-01 (REL_0_8) Christian Walter <wolti@sil.at>
 Notes: Added Linux RTU/ASCII port.

 Detailed notes:
   - FEATURES: Added a new Linux RTU/ASCII port. The port should work
       on any Linux distribution and it should be possible to run it
       on uCLinux.

## [0.7.0]
2006-06-26 (REL_0_7) Christian Walter <wolti@sil.at>
 Notes: Changed the WIN32 serial port to better fit into the design.
 
 Detailed notes:
   - OTHER: Design of the WIN32 serial port changed. The polling function
       for the serial device are now called from the event loop.
   - OTHER: Debugging uses the same interface as the WIN32/TCP port.

2006-06-25 Christian Walter <wolti@sil.at>

 Notes: Initial work on a Modbus/TCP port is available. The port includes
   an example for a Win32 port which uses the Winsock API.
   
 Detailed notes:
   - FEATURES: added required functions to core protocol stack to support
       a Modbus/TCP implementation.
   - FEATURES: added a Win32 port for the Modbus/TCP core. The port is
       currently limited to one concurrent client.
   - OTHER: The implementation of eMBClose to shutdown the protocol stack
       was changed to unify it with the new Modbus/TCP code. 
   - 

2006-06-18 Christian Walter <wolti@sil.at>

 Detailed notes:
   - OTHER: while working on the Win32 port some line feeds got
       wrong. Also some source files used tabs instead of spaces.
   - OTHER: prototypes for xMBUtilSetBits and xMBUtilGetBits fixed.
       usNBits should be ucNBits by convention.

2006-06-17 Christian Walter <wolti@sil.at>

 Notes: Fixed various bugs with the Win32 port

 Detailed notes:
   - FEATURES: implement shutdown functionality for protocol stack.
   - FEATURES: protocol stack can be enabled and disabled during runtime.
   - FEATURES: interface functions now do more error checking. For
       example if eMBPool is called in an uninitialized state.
   - FEATURES: extended Win32 demo application to use the new features.
   - BUG: fixed bug in Win32 demo for ASCII mode.

2006-06-16 Christian Walter <wolti@sil.at>
 Notes: The new version includes a new port for the
   Win32 platform

 Detailed notes:
   - FEATURES: added Win32 platform

2006-05-14 Christian Walter <wolti@sil.at>
 Notes: The new version includes a new port for the
   Freescale MCF5235 processor.

 Detailed notes:
   - FEATURES: added new MCF5235 port.
   - OTHER: fixed some missing code headers.

2006-05-01 Christian Walter <wolti@sil.at>
 Notes: This version removes the t1.5 timers from the Modbus RTU
   implementation because no one actually uses it and the CPU
   load is very high. T
   In addition some documentation cleanups has been done and the
   ARM demo has been updated.

 Detailed notes:

  - FEATURES: the t1.5 timeout has been removed. Therefore only
     one timer is required. 
  - BUG: the ARM demo project missed some files in the project
     workspace and did not compile cleanly

2006-02-28 Christian Walter <wolti@sil.at>
 Notes: This version includes support for two new command 
   (write multiple coils, read discrete input)

 Detailed notes:
  - BUG: some function used the wrong data types
  - FEATURES: added support for write multiple coils function.
  - FEATURES: added support for read discrete input.
  - OTHER: some code cleanups with lint tool.

2006-02-28 Christian Walter <wolti@sil.at>

 Notes: The new version 0.31 adds support for reading and writing the
   coil registers and add some bug fixes.
   
 Detailed notes:
  - BUG: fixed bug with to small modbus requests being ignored.
  - FEATURES: added support for write single coil function.
  - FEATURES: added support for working with byte packed bit fields
     to support coils and discrete inputs better.
  - API: API for set slave id functions changed.

2006-02-26 Christian Walter <wolti@sil.at>

 Notes: First public release which includes an ARM and AVR port.