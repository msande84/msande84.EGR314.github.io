---
title: API
---


**API**

*ESP32*

  | Pin Name  | #  |
  |-----------|----|
  | LED       | 16 |
  | LED2      | 38 |
  | LED3      | 42 |
  | TX        | 44 |
  | RX        | 43 |

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

Message format: "AZ12messagehereYB"
1 = Who sent the message
2 = Who the message is for

  | Message Type  |	Armando ID: A | Manuel ID: C | Khalid ID: K | Lia ID: L | Vedaa ID: V | Matthew ID: M |
  |---------------|---------|---------|---------|---------|---------|---------|
  | Sensor value  | -       | -       | -       | -       | S       | R       |
  | Motor speed   | -       | -       |	R       | S       | -       | R       |
  | Arm position  | R       | -       | -       | S       | -       | R       |
  | Connected     | S/R     | S/R     |	S/R     | S/R     | S/R     | R       |
  | error message | S       | S       |	S       | S/R     | -       | S/R     |
  | reset         | R       | R       |	R       | S/R     | R       | S/R     |

  
