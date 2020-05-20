Alternative 1, copy example project
=================================================

Most example code comes with optional libraries to make iocom appear more like framework than library.
This is not perhaps typical use case, we assume that more often iocom is plainly used as communication library.
Reason for this is simple, we need to be able to run the examples on many different microcontrollers and development 
tools and they need to contain necessary supporting code.

Select name for project
***********************
Difficult to change

Choose name for your IO application, only alpha ‘a’ -’z’ and ‘A’ - ‘Z’, no underlines, special chatacters or numbers. Make it short.

Copy example project
********************
Copy gina
Delete temporary and other unnecessary files

Change application name
***********************
Use your editors "replace in files" function to change most
Replace in files, change 
* "examples/gina" -> "newname"
* "gina" -> "newname"
* "Gina" -> "Newname"
* "GINA" -> "NEWNAME"

Test JSON processor script
**************************

Try to build C code 


Remove/add optional libraries as needed
***************************************


Modify IO pin configuration
***************************

Setup signal configuration
**************************

200518, updated 19.5.2020/pekka
