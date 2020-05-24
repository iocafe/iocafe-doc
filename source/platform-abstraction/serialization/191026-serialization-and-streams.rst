Serialization and streams
=============================
Data is typically a stored in computer’s memory is almost always organized differently that what is needed to save the data to file or transfer it over network connection. For the latter we need a flat stream of bytes. This conversion of data format is called serialization, or stream writer and reader.

* Independent of computer architecture
* Extensible, we can add information and modify types without losing compatibility.
* Compresses to use minimal system resources

Serialization, as are stream implementations, are implemented as eosal extensions and can be included or excluded by compile time defines.

Streams
***************************
Files, sockets (TCP or TLS) and serial ports can be treated as streams. To be able to use any of these from serialization code, data is written to/read from stream trough stream interface. There is also stream buffer class which implements the stream interface: This allows serialization in memory.

Integer serialization
***************************
When an integer is serialized, number of bytes written to stream depends on integer value, not on integer data type -> Small numbers take less space than large ones. This is computer architecture independent, since generated data has always same byte order and size doesn’t depend on data type size.

Float serialization
***************************
Floating points are split as two integers, mantissa and exponent. These are serialized as two integers. This makes serialization format independent on internal float presentation (in practice most computers use IEEE 754, but byte and word order do vary).

JSON serialization
***************************
The JSON is used as hierarchical and extensible presentation of data. JSON data is normally plain text, and quite inefficient as such. Thus it is routinely compressed as packed binary JSON. Binary JSON is suited for internal data presentation for multiple purposes, like presenting IOCOM data structures, saving parameters, etc. Plain text JSON is good for editing and communicating with systems by other manufacturers. 

JSON utility
***************************
A JSON utility can be used to convert between plain text JSON and packed binary JSON, etc. The source code for utility is in /coderoot/eosal/util/json directory and it builds as coderoot/bin/linux/json executable. For example “json -t2b node.json node.json-bin” would convert node.json to binary format. 

191026, 22.5.2020/pekka

