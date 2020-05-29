Float serialization
====================
Floating points can be split as two integers, mantissa and exponent. Computer’s internal floating point
presentation doesn’t effect to values of mantissa and exponent. 

Required defines:

::

    OSAL_SERIALIZE_SUPPORT = 1

Splitting float or double into mantissa and exponent
*****************************************************
To make serialization independent on computer’s floating point presentation, we break a C floating 
point type into two integers, mantissa and exponent. Then we serialize those. 

::

    void osal_float2ints(
        os_float x,
        os_long *m,
        os_short *e);

    void osal_double2ints(
        os_double x,
        os_long *m,
        os_short *e);

Merging mantissa and exponent into float or double
*****************************************************
A C floating point type can be constructed from mantissa and exponent:

::

    os_boolean osal_ints2float(
        os_float *x,
        os_long m,
        os_short e);

    os_boolean osal_ints2double(
        os_double *x,
        os_long m,
        os_short e);

191026, 23.5.2020/pekka
