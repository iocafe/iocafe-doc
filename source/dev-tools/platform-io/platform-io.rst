Platform IO
==================================
Writing portable micro-controller libraries is hard. The first issue that I run into is that it is hard 
to set up a tool-chain and debugger for a specific micro-controller to run builds from the command line.
Possible, but takes a lot of reading, knowledge, and effort. When it is necessary to build the code to 
multiple micro-controllers, this becomes a major burden. Some device manufacturers, like STMicroelectronics 
(TrueStdio/STM32CubeIDE), offer good free or low-cost tools for their own processors. Then Keil is generally 
ok for ARM development but expensive. An additional downside is that both Keil and TrueStudio bundle 
application and platform code together, which leads to non-portable application. One must take a special 
effort to avoid this.

I have been lately using PlatformIO and been very impressed. It is not perfect and doesn’t solve all 
my problems. But it is good and makes my work much easier. When I want to build code to a new micro-controller,
all I need to do is add a section for it into my platformio.ini according to instructions from platform 
documentation. And like magic, it loads build tools and sets up the whole tool-chain, and documentation 
tells me how to connect debugger. Task what would have taken me perhaps days, is now minutes.
And I have GUI for debugging from the start.

Still do not expect even this to be easy and polished like PC development under Visual Studio 2019, 
QT creator, etc. It is still micro-controller development and takes some effort. Polished development 
tools simply do not exist for micro-controllers, the whole field is so diverse that this may not even 
be possible in the near future. You need to deal with many issues, and you need still rewrite 
hardware-dependent parts of code.

But if you want the build the house, use a good hammer. And this is the best I have tried.

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   200314-debugging-esp32-visual-studio-code-platform-io
   200517-linux-usb-device-not-working
   230906-platformio-cannot-find-python-3-6-interprenter-on-linux


DETAIL, NEEDS TO MOVE FROM HERE:s
The library source code files will be in subdirectory arduino or esp32 within /coderoot/lib folder. Platform IO
will use files directly from this location. 

