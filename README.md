# zephyr-icm20948

Out-of-tree Zephyr driver for the InvenSense ICM-20948 9-axis IMU (accelerometer, gyroscope, and AK09916 magnetometer). Supports I2C and SPI, data-ready and wake-on-motion triggers, and the standard Zephyr Sensor API.

## Features

- 3-axis accelerometer, gyroscope, and magnetometer (AK09916)
- I2C and SPI bus support
- Data-ready and wake-on-motion interrupt triggers
- Global thread and own-thread trigger modes
- Standard `sensor_read` / `sensor_trigger_set` API

## Adding to your project

Add the module to your west manifest (`west.yml`):

```yaml
manifest:
  projects:
    - name: icm20948
      url: https://github.com/Bedmonds91/zephyr-icm20948
      revision: main
      path: modules/icm20948
```

Then fetch it:

```sh
west update
```

Alternatively, pass it directly at build time without modifying your manifest:

```sh
west build -b <board> <app> -DZEPHYR_EXTRA_MODULES=/path/to/zephyr-icm20948
```

## Enabling the driver

In your project's `prj.conf`:

```
CONFIG_SENSOR=y
CONFIG_ICM20948=y
```

## Devicetree

Add an ICM-20948 node to your board's overlay. Example for I2C:

```dts
#include <zephyr/dt-bindings/sensor/icm20948.h>

&i2c0 {
    imu: icm20948@68 {
        compatible = "invensense,icm20948";
        status = "okay";
        reg = <0x68>;
        int-gpios = <&gpio0 11 GPIO_ACTIVE_HIGH>;
        mag-mode = <ICM20948_DT_MAG_MODE_100HZ>;
    };
};
```

See `dts/bindings/sensor/` for all available properties.

## Sample

A ready-to-build sample that reads all axes and prints them to the console is in [`samples/9dof_motion_drdy/`](samples/9dof_motion_drdy/).

## Zephyr compatibility

Developed against Zephyr 4.4. The driver uses only stable Sensor API and should be compatible with recent LTS releases.
