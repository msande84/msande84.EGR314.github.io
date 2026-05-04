---
title: Power Budget
---


**Power Management**

* 3.3V Power rail
* 12V Input power from battery

  | Component         |	Voltage (V) |	Typical Current (mA) |	Max Current (mA)	| Typical Power (mW) |	Max Power (mW) |
  |-------------------|-------------|----------------------|--------------------|--------------------|-----------------|
  | Microcontroller	  | 3.3	        | 20                   | 40	                | 66                 | 	132            |
  | Voltage Regulator	| 3.3	        | 5	                   | 10	                | 16.5	             |  33             |
  | Total		          | -           | 25	                 | 50	                | 82.5	             |  165            |

**Reasoning**
The only component using a significant amount of power is the ESP32. The only other power consuming parts are the LED's. Based on this my subsystem uses a small fraction of the power all the subsystems will use.
