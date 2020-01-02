# eibparser

Parses EIB/KNX telegrams from hexadecimal input.

## Requirements

 * Python 3
 * Input data, for example from [knxtool](https://github.com/knxd/knxd) vbusmonitor1|vbusmonitor1time|vbusmonitor2

## Usage:

```
	eibparser [-m mappingfile [-m mappingfile...]] [-f formatstr]
	eibparser --mapping help
	eibparser --format help
```

## Format string

Define a format string using following placeholders:

    {control}     - Numeric value of header field 'control'
    {dest}        - Destination address in 0.0.0 (PA) or 0/0/0 (GA)
    {dest_name}   - Translated name of destination address from mapping
    {dest_raw}    - Numeric value of destination address
    {dest_t}      - Type of destination address (PA or GA)
    {hops}        - Value of head field 'hops'
    {parity}      - Numeric parity value
    {payload}     - Payload (decoded from mapping type, if available)
    {prio}        - Numeric value of header field 'priority'
    {repeat}      - Numeric value of header field 'repeat'
    {size}        - Numeric value of payload size
    {source}      - Source address in 0.0.0
    {source_name} - Translated name of source address from mapping
    {source_raw}  - Numeric value of source address
    {time}        - Message time. Taken from input in HH:MM:SS, if available; current time otherwise
    {type}        - Message type, on of 'read', 'write' or 'response'
    {type_raw}    - Numeric message type from header field

A special argument is 'json' which outputs all available fields as one-line JSON object.

Default is:
{time} from {source\_name} to {dest\_name} {type}: {payload}

## Mapping file
One mapping entry per line.

    Syntax of GA: 0/0/0 TYPE NAME
    Syntax of PA: 0.0.0 NAME

Lines of PA and GA can be mixed.

TYPE is one of:
    int    - Integer, encoded as big endian
    int:be - Integer, encoded as big endian
    int:le - Integer, encoded as little endian
    str    - ASCII string
    hex    - Undecoded hexadecimal output (default)

Empty lines and lines starting with '#' will be ignored.
Multiple mapping files will be merged.