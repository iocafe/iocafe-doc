JSON serialization
======================
The JSON is used as hierarchical and extensible presentation of data. JSON plain text is routinely compressed as packed binary JSON. 

Convert JSON as packed binary
*****************************************************
JSON as text has about 50% overhead from text format. To nip this off from data transfers and micro-controller
storage, the JSON is packed as architecture independent binary file. Binary JSON is suited for internal data 
presentation for multiple purposes, like presenting IOCOM data structures, saving parameters, etc. 
The binary file can be further turned into C code and complied with micro-controller application. 

Required defines:

::

    OSAL_SERIALIZE_SUPPORT = 1
    OSAL_JSON_TEXT_SUPPORT = 1

Function:

::

    osalStatus osal_compress_json(
        osalStream compressed,
        os_char *json_source,
        os_char *skip_tags,
        os_int flags);

Convert packed binary to plain JSON text
*****************************************************
To export data, the packed binary can be converted back to plain text JSON. Plain text JSON is good
for editing and communicating with systems by other manufacturers. 

Required defines:

::

    OSAL_SERIALIZE_SUPPORT = 1
    OSAL_JSON_TEXT_SUPPORT = 1

Function:

::

    osalStatus osal_uncompress_json(
        osalStream uncompressed,
        os_char *compressed,
        os_memsz compressed_sz,
        os_int flags);

Accessing data in packed binary JSON
*****************************************************
Once packed binary JSON is in memory, regardless if it is as “const” C code in micro-controller’s 
flash or received trough communication, it can be accessed.

Required defines:

::
    OSAL_SERIALIZE_SUPPORT = 1

Functions:

::

    /* Create indexer to access compressed data easily.
     */
    osalStatus osal_create_json_indexer(
        osalJsonIndex *jindex,
        os_char *compressed,
        os_memsz compressed_sz,
        os_int flags);
    /* Release indexer when no longer in use.
     */
    void osal_release_json_indexer(
        osalJsonIndex *jindex);
    /* Get next JSON item from the indexer.
     */
    osalStatus osal_get_json_item(
        osalJsonIndex *jindex,
        osalJsonItem *item);

JSON item data structure:
*****************************************************

::

    /** Information about single JSON item.
     */
    typedef struct osalJsonItem
    {
        /** One of: OSAL_JSON_START_BLOCK, OSAL_JSON_END_BLOCK, OSAL_JSON_VALUE_STRING,
            OSAL_JSON_VALUE_INTEGER or OSAL_JSON_VALUE_FLOAT.
         */
        osalJsonElementCode code;
        /** Tag name is name in double quotes before colon..
         */
        const os_char *tag_name;
        /** Recursion level in JSON.
         */
        os_int depth;
        /** Primitive value of the item, for codes OSAL_JSON_VALUE_STRING,
            OSAL_JSON_VALUE_INTEGER or OSAL_JSON_VALUE_FLOAT.
         */
        union
        {
            os_long l;          /* For code OSAL_JSON_VALUE_INTEGER */
            os_double d;        /* For OSAL_JSON_VALUE_FLOAT */
            const os_char *s;   /* For OSAL_JSON_VALUE_STRING */
        }
        value;
    }
    osalJsonItem;

Example, access compressed JSON data:
*****************************************************

::

    osalJsonIndex jindex;
    osalJsonItem item;
    s = osal_create_json_indexer(&jindex, ... )
    if (s) error...
    while (!(s = osal_get_json_item(&jindex, &item))
    {
        switch (item.code)
        {
            case OSAL_JSON_START_BLOCK:
                printf ("%s\n", item.tag_name);
                break;
            ...
        }
        ....
    }
    osal_release_json_indexer(&jindex);


191026, 23.5.2020/pekka

