Espressif ESP32 recommendations
======================================

`Espressif recommends ESP32 chips <https://www.espressif.com/en/products/hardware/modules>`_ 
listed below. There are many versions on the market, and for production use it is important to check exact chip
type to avoid potential for “difficult to solve” problems on delivered products.

* ESP32-WROOM-32E integrates ESP32-D0WD-V3, with higher stability and safety performance.
* ESP32-WROOM-32UE integrates ESP32-D0WD-V3, with higher stability and safety performance.
* ESP32-WROVER-E integrates ESP32-D0WD-V3, with higher stability and safety performance.
* ESP32-WROVER-IE integrates ESP32-D0WD-V3, with higher stability and safety performance.

The difference is that WROVER integrates directly with sofware debugger, just plug USB cable in and you should be ready to test it. WROOM required external debugger, ESP prog. Wiring the external debugger requires a bit of precision and is not doable on field. On the other hand, WROVER is more expensive.

I would start with WROVER. In beginnig it is likely that some bugs need to be worked out, and it is easier with this one. Later on, when product is mature, 
it can be switched to WROOM.

