Controlled development environment
==================================
A development environment consists of operating system and multiple tools. These that are typically actively developed and updated frequently. 
For example ESP-IDF/Arduino framework, Platform IO, Visual Studio Code, OS, USB device drivers, etc. These updates sometimes break the build of 
and application for a specific hardware. This is is practically impossible for anybody to predict or test in a bulletproof manner. 
Even we mostly want to use the new tools, we do want to the let tool updates to prevent us from working on application. 

The tool and OS updates need to be taken to use in controlled manner. I need to be one to choose to when and wether do updates. 
There must be time to test that all works after updates, and option to roll back to previous tools if things do not work out right.

Virtual machines can be backed up and copied
********************************************
A virtual machine (run by VMware or VirtualBox) with development tools is . Then I disable automatic updates, on Visual Studio Code,
PlatformIO, so I can choose the time when these are done. I use Linux Mint as a host machine and Deepin in a virtual machine for 
development (Ubuntu would probably be the safest choice for VM). When I plan for tool update, I simply make a copy of the whole virtual 
machine folder as a backup, and then run the tool updates. This way tool updates never put me offline.

Downsides
*********
* Virtual machines are always less responsive than the host machine. This typically is not a big deal, but might be annoying.
* Running virtual machines requires a relatively powerful host computer. Better to have plenty of RAM and HDD space.
* Switching USB devices between host and virtual machines and selecting a network for the virtual machines may cause a bit of confusion in beginning.
* Sometimes one needs to disable (blacklist) USB drivers which communicate with a debugger or with dev board in the host system side to use these from the virtual machines.

Additional benefits
*******************
* Linux virtual machine images can be published in net and copied from developer to another. If someone prepares a good quality development image for 
  a specific purpose, this would have great value for those getting started. It could be used as-is or as a reference how to make things work.
* In a commercial setting, products often need to be supported for ten years after initial product development. Virtual machines with disabled updates
  can be used to "freeze a project for specific HW device version" and burn it into long term storage. So that new people, perhaps after years, can modify 
  the application code and build the binary for micro-controller. 

200116, updated 20.5.2020/pekka

