---
title: Reflection
---

# Review
I was successful in getting all requirements of my subsystem working. I was able to successfully have a working Wi-Fi connection and be able to send and recieve messages over UART between team members.

# Lessons Learned
Top 10 most important things that I learned from working on this project:

 * Having a jumper to seperate the power from the ESP32 and the voltage regulator is really useful for testing purposes.
 * Adding diodes on shared power and ground pins to prevent shorting will save your PCB if your teammates mess up.
 * Extra LED's and buttons for testing and debugging is really useful.
 * I learned how to correctly read through datasheets to find power requirements for parts.
 * Taking a video of your circuit when it works is really good evidence for when it breaks later.
 * When making a circuit having capacitors on data connections is useful for making a signal clearer.
 * I learned to double check which pinouts are the correct pins for RX and TX before finalizing your design.
 * When designing a test circuit it is a good idea to have test points inbetween important parts to be able to change the layout after you find a mistake.
 * It is important to check which exact version of a microcontroller you have so you know which firmware is the correct one to install.
 * I learned that the correct version of micropython matters a lot and it needs to match the model of ESP32 you have exactly. Double check what the memory size of the ESP is before flashing the firmware and make sure they match.

# Future Recommendations

 1. Choose the ESP32 over the pic, way easier to get it working and to code.
 2. Read the datasheet for your voltage regulator to make sure you have the correct power output.
 3. Add a jumper connection between the voltage regulator and the ESP32 so you can test them individually without burning them out if something is setup incorrectly.
 4. Add extra testpoints between important parts of your circuit.
 5. Have connection points on all pins of the ESP32 as a backup.
 6. I recommend getting your PCB done early. Have a first version of your PCB layout done on your computer 1 month before the assignment to submit it for manufacturing.
