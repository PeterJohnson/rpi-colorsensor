# REV Color Sensor Raspberry Pi - NetworkTables interface

This is intended for use with the WPILibPi image, but can work with any
Raspberry Pi that has RobotPy networktables installed.

## Raspberry Pi Setup

Setting up the rPi:
- ssh in (username pi, password raspberry), then:
  - run ``rw`` to put the Pi into writable mode
  - run ``sudo systemctl enable pigpiod``
  - run ``sudo raspi-config``, Interface Options, I2C, Yes, **OR**:
    - edit /etc/modules, add ``i2c-dev`` line
    - edit /boot/config.txt:
      - uncomment ``dtparam=i2c_arm=on`` line

The default hardware I2C bus is bus 1 on GPIO 2 (Data) and GPIO 3 (Clock).
These pins include 1.8k pull-up resistors to 3.3V.

It is possible to get up to 4 additional I2C busses (3, 4, 5, 6) via other GPIO pins.
Note: you'll need to add external pull-ups to 3V3 on these GPIO pins.
Alternatively, the Pi has support for external I2C mux devices hooked up to
I2C bus 1, see i2c-mux in /boot/overlays/README.

On the rPi 3b, software I2C must be used; add the following to /boot/config.txt:
```
      dtoverlay=i2c-gpio,bus=6,i2c_gpio_sda=22,i2c_gpio_scl=23
      dtoverlay=i2c-gpio,bus=5,i2c_gpio_sda=12,i2c_gpio_scl=13
      dtoverlay=i2c-gpio,bus=4,i2c_gpio_sda=8,i2c_gpio_scl=9
      dtoverlay=i2c-gpio,bus=3,i2c_gpio_sda=4,i2c_gpio_scl=5
```

On the rPi 4, additional hardware busses are available; add to /boot/config.txt:
```
      dtoverlay=i2c3
      dtoverlay=i2c4
      dtoverlay=i2c5
      dtoverlay=i2c6
```

This program shows how to operate a single device on bus 1 and two devices
(one on bus 1 and one on bus 3). The multiple device code is commented out.

## Pinouts

For information on how the GPIO numbers map to the Raspberry Pi pins, see [I2C at Raspberry Pi GPIO Pinout](https://pinout.xyz/pinout/i2c).

The 4-pin sensor cable is GND, 3.3V, SDA, and SCL. So to wire it to the Raspberry Pi I2C bus 1, connect:
- GND (black) to Pin 6
- 3.3V (red) to Pin 1
- SDA (white) to Pin 3
- SCL (blue) to Pin 5
