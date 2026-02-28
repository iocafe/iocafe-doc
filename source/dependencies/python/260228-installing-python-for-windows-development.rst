Installing Python for Windows development
===================================================================

Download installer for latest stable Python version from https://www.python.org.
(python-3.13.12-amd64.exe at time of writing these notes).


Install Python
Microsoft Visual Studio with C/C++ development tools should propably be installed before
installing Python.

Check "Add python.exe to PATH" and "Use admin priviliges when installing py.exe"
Select Customize installation, make sure that every box is checked.


.. figure:: pics/260228-python-installation-advanced-options.png
   
   Select everything in advance options

You may need to modify python include and library paths to match installed python version number.

.. figure:: pics/260228-python-include-path-in-visual-studio.png

   Python include path points to headers within installed Python version.

.. figure:: pics/260228-python-library-path-in-visual-studio.png

   Python library path points to libraries within installed Python version.


    python --version


260228, last updated 27.2.2026/pekka
