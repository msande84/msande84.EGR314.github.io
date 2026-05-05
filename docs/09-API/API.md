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

*Message Structure*

  | **Byte** | **Variable Name** | **Type** | **Example** |  **Description** | **Message Range**
  |---:|---|---|---|----|:---:|
  | Byte 1-2 | start      | char | AZ | Start of the message | AZ-AZ |
  | Byte 3 | source_id    | char | M | Who is it from? | C,L,M,K,V,A,X |
  | Byte 4 | dest_id | char | L | Who is it for? | C,L,M,K,V,A,X |
  | Byte 5-26 | message | bytearray | f | Commands or telemetry | 1,2,3,4,5,6,7,8,9,f,r,s |
  | Byte end | end | char | YB | End of message| YB-YB|
