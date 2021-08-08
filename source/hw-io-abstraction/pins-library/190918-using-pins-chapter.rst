Using “pins” library
=====================
IO configuration for a specific hardware is written as JSON file. This JSON file is converted to C code
by script. Generated C code has static structure describing each IO pin. The application reads and writes
IO pins trough pins library function, giving pointer to static pin structure as argument.
Same application code can be used with different HW by including and linking generated C filed for the hardware.

Steps:

* Create JSON file /coderoot/pins/examples/jane/config/pins/carol/pins_io.json. Replace “jane” with
  name of your application and “carol” with name of your hardware (nick name of your board).
* Generate python script /coderoot/pins/examples/jane/scripts/config-to-c-code.py which converts
  JSON configuration to C code.
* Include generated .c and .h files in your project. Set /coderoot/pins/examples/jane/config/include/carol
  in you compiler’s include path.
* Add line #include pins_io.c in applications config.c file and #include pins_io.h in
  application’s main header file.


JSON configuration
*******************
The idea is to write application’s hardware configurations as JSON, and then generate matching C code and
header files by Python script pins-to-c.py. It is possible to write directly C code, writing JSON just
makes it easier because information needs to be written only in one place.

For example I have application “jane”, which controls “led_builtin” binary output to on a LED on IO board.
I want to test this on hardware I named “carol”, which is ESP32, Arduino libraries and the LED connected
to pin 2. I need to bind pin address 2 and other pin attributes (like pull-up), etc to name “led_builtin”.
I write this information a JSON file jane/config/pins/carol/jane-io.json. Then I run the pins-to-c.py
script to generate C code. I use same JSON file together with IOCOM signal configuration JSON to map the
IO pins to IOCOM communication signals.

If I would need to run same “jane” application on different hardware, let’s say “alice”: Separate
JSON HW configuration file would written for it.

* The led_builtin output is mapped to specific IO pin by pin number, and/or bank number and.
  We may also have some attributes for IO pin. For example SERVO PWM has frequency, resolution,
  initial value, etc.
* One pin, or more generally IO item since we can include timers etc, is configured by name,
  address and and various parameters.
* Group attribute can be used to generate groups of pins.
* Attribute values excluding group are integers.
* Some parameters can be changed while the code runs.
* IO pins can be mapped directly to IOCOM signals.

A JSON pin setup would look something like ESP32 example below:

.. code-block:: json

    {
      "io": [{
        "name": "jane",
        "title": "IO pin setup for 'jane' application on 'carol' hardware",
        "groups": [
          {
            "name": "inputs",
            "pins": [
              {"name": "gazerbeam", "addr": 39, "interrupt": 1},
              {"name": "dip_switch_3", "addr": 34, "pull-up": 1},
              {"name": "dip_switch_4", "addr": 35},
              {"name": "touch_sensor", "addr": 36, "touch": 1}
            ]
          },
          {
            "name": "outputs",
            "pins": [
              {"name": "led_builtin", "addr": 2}
            ]
          },
          {
            "name": "analog_inputs",
            "pins": [
              {"name": "potentiometer", "addr": 26, "speed": 3, "max": 4095}
            ]
          },
          {
            "name": "pwm",
            "pins": [
              {"name": "servo", "bank": 0, "addr": 22, "frequency": 50, "resolution": 12, "init": 2048, "max": 4095},
              {"name": "dimmer_led", "bank": 1, "addr": 27, "frequency": 5000, "resolution": 12, "init": 0, "max": 4095}
            ]
          },
          {
            "name": "uart",
            "pins": [
              {"name": "uart2", "addr": 2, "rx": 16, "tx": 17, "speed": 96}
            ]
          },
          {
            "name": "debugger",
            "pins": [
                {"name": "AD1_TDI", "addr": 12},
                {"name": "AD0_TCK", "addr": 13},
                {"name": "AD3_TMS", "addr": 14},
                {"name": "AD2_TDO", "addr": 15},
                {"name": "U0_TXD", "addr": 1},
                {"name": "U0_RXD", "addr": 3}
              ]
          },
          {
            "name": "devboard",
            "pins": [
                {"name": "DEV_SCK", "addr": 6},
                {"name": "DEV_SDO", "addr": 7},
                {"name": "DEV_SDI", "addr": 8},
                {"name": "DEV_SHD", "addr": 9},
                {"name": "DEV_SWP", "addr": 10},
                {"name": "DEV_CSC", "addr": 11}
              ]
          }
        ]
      }]
    }


Configuring inputs
~~~~~~~~~~~~~~~~~~~~~~~~~~
Inputs are configured within “inputs” group. Pin name is can be up to 15 characters + terminating ‘\0’.
Use only ‘a’ - ‘z’, ‘A’ - ‘Z’, ‘0’ - ‘1’ and underscore ‘_’ characters. The pin name will be used in
C code as written. Address “addr” is GPIO pin address.

The attributes for inputs

• pull-up: Set 1 to enable pull-up resistor on input.
• pull-down: Set 1 to enable pull-down resistor on input.
• touch: Set 1 to define this input as touch sensor (this is set up here, even signal may be analog):
• interrupt: Set 1 to trigger HW interrupt when pin state changes, either falling edge,
  rising edge or both. Edge on which to trigger is in C code.

::

{"name": "gazerbeam", "addr": 39, "interrupt": 1},
{"name": "dip_switch_3", "addr": 34, "pull-up": 1}

Configuring outputs
~~~~~~~~~~~~~~~~~~~~~~~~~~
Configuring outputs under “outputs” group is similar to configuring inputs.

The attributes for inputs:

• pull-up: Set 1 to enable pull-up resistor on output.
• pull-down: Set 1 to enable pull-down resistor on output.

::

{"name": "led_builtin", "addr": 33}


Configuring analog inputs
~~~~~~~~~~~~~~~~~~~~~~~~~~
Analog inputs are configured within “analog_inputs” group.

The attributes for analog inputs

• addr: GPIO pin address.
• max: Set maximum value for analog input. Typically set by number AD conversion resolution,
  1023 for 10-bit ADC and 4095 for 12 bit ADC, etc. Application and IOCOM link can use this
  setting to scale value to known units, which is useful if for example newer version of
  the hardware has higher ADC resolution.

::

{"name": "potentiometer", "addr": 26, "max": 4095}


Configuring analog outputs
~~~~~~~~~~~~~~~~~~~~~~~~~~
Analog outputs are configured within “analog_outputs” group.

The attributes for analog outputs

* max: Set maximum value for analog output. Typically set by number D/A conversion resolution,
  1023 for 10-bit DAC and 4095 for 12 bit DAC, etc.

::

{"name": "myaout", "addr": 25, "max": 4095}

Configuring PWM
~~~~~~~~~~~~~~~~~~~~~~~~~~
PWM pins are configured within “pwm” group.
The attributes for PWM pin:

* bank: PWM channel, typically 0 - 7.
* addr: GPIO pin address.
* frequency: PWM frequency in Hz, max 65535.
* frequency-kHz: PWM frequency kHz. This is used instead of “frequency” to set higher frequencies than 65kHz.
* timer: Which timer to use to generate the pulse
* init: Initial duty cycle 0- 2^resolution
* hpoint: Counter value where the duty cycle 0 - 2^resolution, shifts pulse position in time.
* resolution: Number of bits to set PWM duty. If set to 12, values range from 0 to 4095.
  For example 2048 would mean that pin is on 50 % of time (50 % duty cycle). If generating clock pulse, set 1 bit.
* max: Set maximum value for analog output. Typically set by number D/A conversion resolution,
  1023 for 10-bit DAC and 4095 for 12 bit DAC, etc.
* init: Set initial value. for example 1024 with 12 bit resolution would mean 25% duty cycle.


Inverted servo control (directional throttle control, tested with raspberry pi/pigpio).
Servo frequency 60 Hz, input -100.0 (smin/10^digs = -10000/100 = -100) to 100.0 degrees:

::

{"name": "throttle", "addr": 1, "frequency": 60, "resolution": 12, "init": 3755, "min": 3995, "max": 3515, "smin": -10000, "smax": 10000, "digs": 2}

Servo control using pca9685 board connected to Raspberry pi with SPI. Servo frequency 60 Hz, input -90.0 to 90.0 degrees
scaled to 12 bit signal (default for pca9685) from 100 (0.41ms) to 580 (2.36 ms), neural position 340 (340÷4095×1000÷60 = 1.38ms):

::

{"name": "left_wheel", "device": "i2c.pwmboard", "addr": 0, "frequency": 60, "init": 340, "min": 100, "max": 580, "smin": -9000, "smax": 9000, "digs": 2}

LED brightness control (tested with ESP32), input directly 0 .. 4095:

::

{"name": "led_morse", "timer": 1, "bank": 5, "addr": 4, "frequency": 5000, "resolution": 12, "init": 0, "max": 4095}


800 kHz clock pulse (tested with ESP32):

::

{"name": "ccd_clock",  "bank": 0, "addr": 22, "timer": 0, "frequency-kHz": 800, "resolution": 1, "init": 1, "hpoint": 0}

ESP32 notes

* LED PWM of the ESP32 is composed of 16 independent channels (0 … 15), with configurable duty
  cycles and wave periods. The accuracy of the duty cycle can be configured up to 16 bits of
  resolution.  Channels 0 … 7 are high speed channels, and 8 … 15 are low speed channels.
* ESP32: ESP32/Arduino implementation ignores “timer” attribute.


Configuring SPI bus
~~~~~~~~~~~~~~~~~~~~~~~~~~
SPI buses are configured within “spi” group.

The attributes for SPI bus

* addr: SPI bus number to configure.
* max: Set maximum value for analog output. Typically set by number D/A conversion resolution,
  1023 for 10-bit DAC and 4095 for 12 bit DAC, etc.
* miso: Sets GPIO pin address for MISO (master in, slave out) signal.
* mosi: Sets GPIO pin address for MOSI (master out, slave in)signal.
* sclk: Sets GPIO pin address for SCLK (clock) signal.
* cs: Sets GPIO pin address for CS (chip select) signal. This enables a specific SPI device.
* dc: Needed for some devices, like some TFT displays. Sets data control GPIO pin address.

::

{"name": "tft_spi", "addr": 0, "miso": 19, "mosi": 23, "sclk": 18, "dc": 2}


Configuring timers
~~~~~~~~~~~~~~~~~~~~~~~~~~
Timers are used to trigger timed interrupts.

Timer attributes

* bank: .
* addr:  Pin address.
* timer: Which timer to use to generate the pulse 0 …
* init:
* frequency: PWM frequency in Hz, max 65535.
* frequency-kHz: PWM frequency kHz. This is used instead of “frequency” to set higher frequencies than 65kHz.

::

{"name": "igc_timer",  "bank": 0, "addr": 0, "timer": 0, "frequency-kHz": 250, "resolution": 1}


Configuring UARTS
~~~~~~~~~~~~~~~~~~~~~~~~~~
Serial port UARTS are configured same way regardless of signal levels, difference between TTL level, RS232, RS422.

UART attributes

* addr: UART number.
* rx: Receive data GPIO pin address.
* tx: Transmit data GPIO pin address.
* tc: Transmit control GPIO pin address (reserved for future).
* speed: Baud rate, divided by 100. Set for example 96 for 9600 bps.

::

{"name": "uart2", "addr": 2, "rx": 16, "tx": 17, "speed": 96}


Pins reserved for debugger and development board
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
These are skipped by pins-to-c.py script and cannot be accessed from C code. Reason for these to be defined here
is to avoid accidentally using a reserved pin, and in future to include these in IO documentation once we have
automatic JSON configuration to document converter written.


Generating C source and header files from JSON
*************************************************
The hardware specific IO configuration, like jane-io.json, is converted to C files by pins-to-c.py script.
This will generate jane-io.c and jane-io.h file, which can be compiled into the application.

jane-io.h

.. code-block:: c

    /* This file is gerated by pins-to-c.py script, do not modify. */
    OSAL_C_HEADER_BEGINS
    typedef struct
    {
      struct
      {
        PinGroupHdr hdr;
        Pin dip_switch_3;
        Pin dip_switch_4;
        Pin touch_sensor;
      }
      inputs;
      struct
      {
        PinGroupHdr hdr;
        Pin led_builtin;
      }
      outputs;
      struct
      {
        PinGroupHdr hdr;
        Pin potentiometer;
      }
      analog_inputs;
      struct
      {
        PinGroupHdr hdr;
        Pin servo;
        Pin dimmer_led;
      }
      pwm;
    }
    pins_t;
    extern const IoPinsHdr pins_hdr;
    extern const pins_t pins;
    OSAL_C_HEADER_ENDS

And in C file

.. code-block:: c

    /* This file is generated by pins-to-c.py script, do not modify. */
    #include "pins.h"
    static os_short pins_inputs_dip_switch_3_prm[]= {PIN_PULL_UP, 1};
    static os_short pins_inputs_touch_sensor_prm[]= {PIN_TOUCH, 1};
    static os_short pins_analog_inputs_potentiometer_prm[]= {PIN_SPEED, 3, PIN_DELAY, 11};
    static os_short pins_pwm_servo_prm[]= {PIN_RESOLUTION, 12, PIN_FREQENCY, 50, PIN_INIT, 2048};
    static os_short pins_pwm_dimmer_led_prm[]= {PIN_RESOLUTION, 12, PIN_FREQENCY, 5000, PIN_INIT, 0};
    const pins_t pins =
    {
      {{3, &pins.inputs.dip_switch_3},
        {"dip_switch_3", PIN_INPUT, 0, 34, pins_inputs_dip_switch_3_prm, sizeof(pins_inputs_dip_switch_3_prm)/sizeof(os_short), OS_NULL},
        {"dip_switch_4", PIN_INPUT, 0, 35, OS_NULL, 0, OS_NULL},
        {"touch_sensor", PIN_INPUT, 0, 4, pins_inputs_touch_sensor_prm, sizeof(pins_inputs_touch_sensor_prm)/sizeof(os_short), OS_NULL}
      },
      {{1, &pins.outputs.led_builtin},
        {"led_builtin", PIN_OUTPUT, 0, 2, OS_NULL, 0, OS_NULL}
      },
      {{1, &pins.analog_inputs.potentiometer},
        {"potentiometer", PIN_ANALOG_INPUT, 0, 25, pins_analog_inputs_potentiometer_prm, sizeof(pins_analog_inputs_potentiometer_prm)/sizeof(os_short), OS_NULL}
      },
      {{2, &pins.pwm.servo},
        {"servo", PIN_PWM, 0, 32, pins_pwm_servo_prm, sizeof(pins_pwm_servo_prm)/sizeof(os_short), OS_NULL},
        {"dimmer_led", PIN_PWM, 1, 33, pins_pwm_dimmer_led_prm, sizeof(pins_pwm_dimmer_led_prm)/sizeof(os_short), OS_NULL}
      }
    };
    static const PinGroupHdr *pins_group_list[] =
    {
      &pins.inputs.hdr,
      &pins.outputs.hdr,
      &pins.analog_inputs.hdr,
      &pins.pwm.hdr
    };
    const IoPinsHdr pins_hdr = {pins_group_list, sizeof(pins_group_list)/sizeof(PinGroupHdr*)};


Pins library types in C
***********************
In C code, an IO pin, a SPI bus, timer or UART is referred by static “Pin” structure.
Static structure for each “pin” is initialized in C code generated by script from JSON configuration.
The “Pin” structure is the same, regardless what this “pin” actually is, “pinType type” member of
the structure has the pin type:

Pins library main header file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Use #include “pinsx.h” to include pins library headers for use with IOCOM.
Enumeration of pin types

.. code-block:: c

    typedef enum
    {
        PIN_INPUT,
        PIN_OUTPUT,
        PIN_ANALOG_INPUT,
        PIN_ANALOG_OUTPUT,
        PIN_PWM,
        PIN_SPI,
        PIN_TIMER,
        PIN_UART
    }
    pinType;

Enumeration of pin attributes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Each “pin” has specific set of attributes, see JSON configuration notes to which attributes can
be applied for a specific pin type.  Notice that numeric enumeration values can change.

.. code-block:: c

    typedef enum
    {
        PIN_PULL_UP,
        PIN_TOUCH,
        PIN_FREQENCY,
        PIN_FREQENCY_KHZ,
        PIN_RESOLUTION,
        PIN_INIT,
        PIN_INTERRUPT,
        PIN_TIMER_SELECT,
        PIN_MISO,
        PIN_MOSI,
        PIN_SCLK,
        PIN_CS,
        PIN_DC,
        PIN_RX,
        PIN_TX,
        PIN_MIN,
        PIN_MAX
    }
    pinPrm;

Pin definition structure
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Structure that holds static information about one IO pin. Pin structure for each IO “pin”
is initialized in C code generated from JSON configuration.

.. code-block:: c

    typedef struct Pin
    {
        /** Pint type, like PIN_INPUT, PIN_OUTPUT... See pinType enumeration.
         */
        os_char type;

        /** Hardware bank number for the pin, if applies.
         */
        os_short bank;

        /** Hardware address for the pin.
         */
        os_short addr;

        /** Pointer to parameter array, two first os_shorts are reserved for storing value
            as os_int.
         */
        os_short *prm;

        /** Number of items in parameter array. Number of set parameters is this divided by
            two, since each 16 bit number is parameter number amd parameter value.
         */
        os_char prm_n;

        /** Next pin in linked list of pins belonging to same group as this one. OS_NULL
            to indicate that this pin is end of list of not in group.
         */
        const struct Pin *next;

        /** Pointer to IO signal, if this pin is mapped to one.
         */
        const struct iocSignal *signal;
    }
    Pin;

There is group_next and board_next. Often is handy to loop trough all pins, like when making
memory map for IO com. Or reading group of inputs with one command. To facilitate this we can
group pins together. Pins with same PIN_GROUP number set go generate linked list and all pins
of IO board a second linked list.

All pins of a device can be referred using “extern const IoPinsHdr pins_hdr” in script generated
pins_io.h. We use pins_setup(&pins_hdr, 0) this to set up all the IO pins, or
“pins_read_all(&pins_hdr, PINS_DEFAULT)” in main loop to read all pins.


Pins library functions
***********************

.. code-block:: c

    /* Set up IO hardware.
     */
    void pins_setup(
        const IoPinsHdr *pins_hdr,
        os_int flags);
    Set and get pin state

    /* Set IO pin state.
     */
    void pin_set(
        const Pin *pin,
        os_int state);

    /* Get pin state.
     */
    os_int pin_get(
        const Pin *pin);
    Access pin parameters

    /* Modify IO pin parameter.
     */
    void pin_set_prm(
        const Pin *pin,
        pinPrm prm,
        os_int value);

    /* Get value of IO pin parmeter.
     */
    os_int pin_get_prm(
        const Pin *pin,
        pinPrm prm);



190918, updated 21.5.2020/pekka
