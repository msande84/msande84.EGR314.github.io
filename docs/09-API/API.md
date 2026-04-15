---
title: API
---


**API**

*Connectors*

  | Pin Name          | # | # | Pin Name |
  |-------------------|---|---|----------|
  | 12V Battery Power | 1 | 2 | UART     |
  | N/A               | 3 | 4 | N/A      |
  | N/A               | 5 | 6 | N/A      |
  | N/A               | 7 | 8 | Ground   |

*UART Info*

  | Parameter    | Value     |
  |--------------|-----------|
  | Speed        | 9600 Baud |
  | Num Bytes    | 8         |
  | Parity       | None      |
  | Stop Bits    | 1         |
  | Flow control | None      |

*Data Stream*

  |               |	Byte 1  | Byte 2    | Byte 3  | Byte 4  | Byte 5  |
  |---------------|---------|-----------|---------|---------|---------|
  | Variable Name | speed   | direction | arm_x   | arm_y   | arm_z   |
  | Variable Type | char    | char      |	char    | char    | char    |
  | Min Value     | -100    | 0         |	-100    | -100    | -50     |
  | Max Value     | 100     | 1         |	100     | 100     | 100     |

  
