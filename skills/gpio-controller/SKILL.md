---
name: gpio-controller
description: Control GPIO pins on Qualcomm embedded boards
---

# GPIO Controller

Control digital GPIO pins on Qualcomm embedded boards using the Linux sysfs or libgpiod interface.

## When to use this skill

Use this skill whenever you need to:
- Read or write a digital GPIO pin (e.g., LED, button, sensor)
- Configure pin direction (input / output) and pull resistors
- Set up edge-triggered interrupts to react to pin state changes

## Usage

### List available GPIO chips

```bash
gpiodetect
```

### Read a pin (libgpiod)

```bash
# Read the current value of line 5 on gpiochip0
gpioget gpiochip0 5
```

### Write a pin

```bash
# Drive line 5 high
gpioset gpiochip0 5=1

# Drive line 5 low
gpioset gpiochip0 5=0
```

### Monitor for edge events

```bash
# Watch for rising and falling edges on line 5
gpiomon --num-events=10 --rising-edge --falling-edge gpiochip0 5
```

### Python example (libgpiod)

```python
import gpiod

chip = gpiod.Chip("gpiochip0")
line = chip.get_line(5)

# Configure as output
line.request(consumer="my-app", type=gpiod.LINE_REQ_DIR_OUT)
line.set_value(1)  # high
line.set_value(0)  # low
line.release()
```

## Notes

- Pin numbering depends on the board; refer to the hardware reference manual for your platform.
- `libgpiod` is the modern replacement for the deprecated `/sys/class/gpio` sysfs interface.
- On RB3 Gen 2, GPIO banks are exposed as `gpiochip0`–`gpiochip3`.
