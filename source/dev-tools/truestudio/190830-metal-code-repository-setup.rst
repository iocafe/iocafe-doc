STM32 specific - this is needed for Atollic True Studio builds
===============================================================

Use STM32CubeMX to get repositories for used microcontroller families, like STM32F4xx, STM32L4xx..

Create directory /coderoot/repository

Create soft link to repository folder under user's home folder for each microcontroller family

::

   ln -s /home/john/STM32Cube/Repository/STM32Cube_FW_F4_V1.24.1 /coderoot/repository/f4
   ln -s /home/john/STM32Cube/Repository/STM32Cube_FW_L4_V1.14.0 /coderoot/repository/l4
