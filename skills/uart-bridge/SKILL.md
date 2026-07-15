---
name: uart-bridge
description: Set up and communicate over UART serial ports on embedded Linux
---

# UART Serial Bridge

Open and communicate over UART serial ports on embedded Linux boards — configure baud rate, parity, and flow control, and send or receive data.

## When to use this skill

Use this skill when you need to:
- Open a UART port at a specific baud rate and configuration
- Send ASCII commands and read back responses
- Log serial output from a peripheral to a file
- Configure hardware (RTS/CTS) or software (XON/XOFF) flow control

## Usage

### List available serial ports

```bash
ls /dev/ttyS* /dev/ttyUSB* /dev/ttyAMA* 2>/dev/null
```

### Open a terminal session (minicom)

```bash
# 115200 baud, no flow control
minicom -D /dev/ttyS0 -b 115200
```

### Configure port with stty

```bash
# 115200 8N1, no flow control, raw mode
stty -F /dev/ttyS0 115200 cs8 -cstopb -parenb -crtscts raw
```

### Send a command and read the response

```bash
echo "AT" > /dev/ttyS0
cat /dev/ttyS0 &   # background reader
echo "AT+GMR" > /dev/ttyS0
```

### Log serial output to a file

```bash
cat /dev/ttyS0 | tee uart-log.txt
```

### Python example (pyserial)

```python
import serial

port = serial.Serial(
    port="/dev/ttyS0",
    baudrate=115200,
    bytesize=serial.EIGHTBITS,
    parity=serial.PARITY_NONE,
    stopbits=serial.STOPBITS_ONE,
    timeout=1,
)

port.write(b"AT\r\n")
response = port.readline()
print(response.decode("ascii", errors="replace"))
port.close()
```

### Enable hardware flow control (RTS/CTS)

```python
port = serial.Serial("/dev/ttyS0", 115200, rtscts=True)
```

## Notes

- On QCS6490, UART devices are typically at `/dev/ttyMSM0` or `/dev/ttyHS0`.
- Install pyserial with `pip install pyserial` if not already available.
- Ensure your user is in the `dialout` group: `sudo usermod -aG dialout $USER`.
