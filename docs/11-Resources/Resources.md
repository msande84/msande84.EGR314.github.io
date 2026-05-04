

## Video
<iframe width="560" height="315" src="https://www.youtube.com/embed/M10dVee0vPY?si=JxDzDAdwJpcqoWx_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

[![Subsystem Working](https://img.youtube.com/vi/M10dVee0vPY/0.jpg)](https://www.youtube.com/watch?v=M10dVee0vPY)


## Code
```python
import ssl
from mqtt_as.mqtt_as import MQTTClient
from mqtt_as.mqtt_local import wifi_led, blue_led, config
import uasyncio as asyncio
from machine import UART
from config import *
from machine import Pin
from time import sleep

# =========================
# USER / TEAM CONFIG
# =========================
MY_ID = b'M'  # Your ID
TEAM = [b'M', b'C', b'A', b'L', b'V', b'K']  # valid team members
MAX_MSG_LEN = 32

# =========================
# UART SETUP
# =========================
uart = UART(2, 9600, tx=Pin(44, Pin.OUT), rx=Pin(43, Pin.IN))
uart.init(9600, bits=8, parity=None, stop=1, flow=0)

led = Pin(16, Pin.OUT)
led2 = Pin(38, Pin.OUT)
led3 = Pin(42, Pin.OUT)
Button = Pin(15, Pin.IN)
led.value(1)
sleep(0.2)
led.value(0)
led2.value(1)
sleep(0.2)
led2.value(0)
led3.value(1)
sleep(0.2)
led3.value(0)
led.value(1)
# =========================
# MESSAGE PARSER
# =========================
def parse_message(msg):
    try:
        if not msg.startswith(b'AZ') or not msg.endswith(b'YB'):
            return None, "FORMAT_ERROR"

        core = msg[2:-2]  # remove AZ and BY

        if len(core) < 2:
            return None, "FORMAT_ERROR"

        src = core[0:1]
        dest = core[1:2]
        body = core[2:]

        if len(body) > MAX_MSG_LEN:
            return None, "TOO_LONG"

        if src not in TEAM:
            return None, "UNKNOWN_SENDER"

        

        return (src, dest, body), None

    except Exception:
        return None, "PARSE_FAIL"

# =========================
# UART RECEIVER TASK
# =========================
async def receiver():
    buffer = b''
    sreader = asyncio.StreamReader(uart)
    print("UART RX LIVE")
    while True:
        res = await sreader.read(1)
        led3.value(1)
        sleep(0.5)
        led3.value(0)
        print(res)

        if not res:
            continue

        buffer += res

        # Detect end of message
        if buffer.endswith(b'YB'):
            print("UART RX:", buffer)

            parsed, error = parse_message(buffer)

            if error:
                print("UART Error:", error)
                buffer = b''
                continue

            src, dest, body = parsed

            if dest == MY_ID:
                print("UART message for ME")
                print("From:", src.decode(), "Message:", body.decode())
                await client.publish(TOPIC_PUB, buffer, qos=1)
                led.value(0)
                sleep(0.5)
                led.value(1)
                
            else:
                print("Forwarding via MQTT")
                await client.publish(TOPIC_PUB, buffer, qos=1)
                #Uncomment below to switch to sending message through UART loop
                #print("Forwarding to UART ", buffer)
                #uart.write(buffer)
                #led2.value(1)
                #sleep(0.5)
                #led2.value(0)

            buffer = b''
# =========================
# MQTT SUB CALLBACK
# =========================
def sub_cb(topic, msg, retained):
    print("MQTT RX:", msg)

    parsed, error = parse_message(msg)

    if error:
        print("MQTT Error:", error)
        return

    src, dest, body = parsed
    
    if dest not in TEAM:
        print("UNKNOWN_DESTINATION")

    if dest == MY_ID:
        print("MQTT message for ME")
        print("From:", src.decode(), "Message:", body.decode())

        # Flash LED
        led.value(0)
        sleep(0.5)
        led.value(1)
        
    else:
        print("Forwarding to UART ", msg)
        uart.write(msg)
        led2.value(1)
        sleep(0.5)
        led2.value(0)

# =========================
# HEARTBEAT LED
# =========================
async def heartbeat():
    while True:
        await asyncio.sleep_ms(50)

# =========================
# WIFI HANDLER
# =========================
async def wifi_han(state):
    print('Wifi is ', 'up' if state else 'down')
    await asyncio.sleep(1)

# =========================
# MQTT CONNECT HANDLER
# =========================
async def conn_han(client):
    await client.subscribe(TOPIC_SUB, 1)

# =========================
# BUTTON TASK
# =========================
async def button_task():
    last_state = 1  # assuming pull-up (not pressed = 1)
    
    while True:
        current_state = Button.value()

        # Detect press (falling edge)
        if last_state == 1 and current_state == 0:
            print("Button Pressed!")

            # Example action: send a message
            msg = b'AZ' + MY_ID + b'A' + b'0' + b'YB'
            print("Sending:", msg)
            uart.write(msg)
            await client.publish(TOPIC_PUB, msg, qos=1)

            # Flash LED for feedback
            led.value(0)
            await asyncio.sleep(0.2)
            led.value(1)
            led2.value(1)
            await asyncio.sleep(0.2)
            led2.value(0)
            led3.value(1)
            await asyncio.sleep(0.2)
            led3.value(0)

        last_state = current_state

        await asyncio.sleep_ms(50)  # debounce + CPU friendly
        
# =========================
# MAIN LOOP
# =========================
async def main(client):
    try:
        await client.connect()
    except OSError:
        print('Connection failed.')
        return

    asyncio.create_task(receiver())
    asyncio.create_task(button_task())

    n = 0
    while True:
        await asyncio.sleep(5)
        print('publish', n)
        await client.publish(TOPIC_HB, '{} {}'.format(n, client.REPUB_COUNT), qos=1)
        n += 1

# =========================
# MQTT CONFIG
# =========================
config['server'] = MQTT_SERVER
config['ssid'] = WIFI_SSID
config['wifi_pw'] = WIFI_PASSWORD

config['ssl'] = True

with open('certs26/student_key.pem', 'rb') as f:
    key_data = f.read()
with open('certs26/student_crt.pem', 'rb') as f:
    cert_data = f.read()
with open('certs26/ca_crt.pem', 'rb') as f:
    ca_data = f.read()

ssl_params = {
    "cert": cert_data,
    "key": key_data,
    "cadata": ca_data,
    "server_hostname": MQTT_SERVER,
    "cert_reqs": ssl.CERT_REQUIRED
}

config["ssl_params"] = ssl_params
config["time_server"] = MQTT_SERVER
config["time_server_timeout"] = 10

config['subs_cb'] = sub_cb
config['wifi_coro'] = wifi_han
config['connect_coro'] = conn_han
config['clean'] = True
config['user'] = MQTT_USER
config["password"] = MQTT_PASSWORD

# =========================
# START CLIENT
# =========================
MQTTClient.DEBUG = True
client = MQTTClient(config)

asyncio.create_task(heartbeat())

try:
    asyncio.run(main(client))
finally:
    client.close()
    asyncio.new_event_loop()
```

## Resouces

The Zip folder of the code is [*here*](Code.zip).
