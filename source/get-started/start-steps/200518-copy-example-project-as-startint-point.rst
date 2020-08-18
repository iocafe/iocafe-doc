Alternative 1, copy example project
=================================================

Most example code comes with optional libraries to make iocom appear more like framework than library.
Reason for this is simple, we need to be able to run the examples on many different micro-controllers and development
tools and they need to contain necessary supporting code.

We assume that more often iocom is used as plain communication library within an application, see "alternative 2"
for discussion about.


Select name for the new application
***********************************
* Choose a good name for your IO application, it will be extra work to change it later on.
* Keep it short, so it can be displayed in user interface menus, etc. Maximum 15 characters.
* Use only alphabet's characters ‘a’ -’z’ and ‘A’ - ‘Z’, no underlines, special characters or numbers.


Copy example project
********************
* Copy example project to start from.
* In this text I "copy" gina example in /coderoot/iocom/examples/gina directory to /coderoot/myproject/myapp directory.
* I could as well /coderoot/myapp directory, using /coderoot/myproject/myapp gives extra layer in directory tree.
  This is useful if something else project includes other things besides one IO device application.
* Delete temporary and other unnecessary files and folders from /coderoot/myproject/myapp.


Change application name in files
********************************
Editor's "replace in files" function to change most stings containing project path or name.

* "iocom/examples/gina" -> "myproject/myapp"
* "iocom/examples" -> "myproject"
* "gina" -> "myapp"
* "Gina" -> "Myapp"
* "GINA" -> "MYAPP"


Test JSON processor script
**************************
Run script to convert JSON to C code and to compressed format from terminal.

.. code-block:: shell

   cd /coderoot/myproject/myapp
   python3 config_to_c_code


Name your IO board and modify IO pin configuration
**************************************************
If you will be using pins library for IO device, the IO pins are configured in pins_io.json file.
Among other files, this file is further converted to C code by config_to_c_code script.
This file is located in /coderoot/iocom/examples/gina/config/pins/<yourdevicename>/pins_io.json

Your hardware (physical IO device) needs a name. This allows building same application on different boards.
Hardware name specifies underlying IO API, etc, to use and to which purpose each IO pin is used.

If you are using platform's IO hardware directly or trough platform specific API, you should delete
all pins_io.json file and all pins library related things.


Setup signal configuration
**************************
Similarly to pins, signals passed trough IOCOM communication are defined in JSON file signals.json
(/coderoot/iocom/examples/gina/config/signals/signals.json). The file merge.json is used to include
typically device configuration signals or other common signal collections.

200518, updated 19.5.2020/pekka
