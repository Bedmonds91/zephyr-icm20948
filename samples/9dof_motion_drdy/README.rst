9DOF Motion Data Ready Sample
#############################

Overview
********

This sample application periodically (0.5 Hz) measures the sensor
temperature, acceleration, angular velocity, and magnetic field from an
ICM-20948, displaying the values on the console along with a timestamp
since startup.

The ICM-20948 is a 9-axis motion tracking device that includes:

* 3-axis accelerometer
* 3-axis gyroscope
* 3-axis magnetometer (AK09916)
* Temperature sensor

When triggered mode is enabled the measurements are displayed at the
sensor's configured data rate.

Requirements
************

This sample is part of the ``zephyr-icm20948`` out-of-tree module. Before
building, add the module to your west workspace manifest (``west.yml``):

.. code-block:: yaml

   manifest:
     projects:
       - name: icm20948
         url: https://github.com/Bedmonds91/zephyr-icm20948
         revision: main
         path: modules/icm20948

Then run ``west update`` to fetch the module.

Wiring
******

This sample requires an ICM-20948 breakout board connected via I2C or SPI.
A devicetree overlay must be provided to identify the bus and interrupt GPIO.

An example overlay for the nRF52840 DK (I2C, Arduino header) is provided at
``boards/nrf52840dk_nrf52840.overlay``:

* SCL: P0.27
* SDA: P0.26
* INT: P0.11

Building and Running
********************

Build from the module path. Replace ``<path/to/module>`` with the location
reported by ``west list`` for the ``icm20948`` module:

.. code-block:: console

   west build -b nrf52840dk/nrf52840 <path/to/module>/samples/9dof_motion_drdy

Or using the ``ZEPHYR_EXTRA_MODULES`` environment variable without modifying
your manifest:

.. code-block:: console

   west build -b nrf52840dk/nrf52840 samples/9dof_motion_drdy \
     -DZEPHYR_EXTRA_MODULES=/path/to/zephyr-icm20948

Flash after building:

.. code-block:: console

   west flash

Sample Output
*************

.. code-block:: console

   *** Booting Zephyr OS build v3.x.0 ***
   Found ICM-20948 device icm20948@68
   [0:00:00.010]: 25.3 Cel
     accel  0.123456 -0.234567  9.812345 m/s/s
     gyro   0.001234 -0.002345  0.003456 rad/s
     magn  23.456789 -12.345678  45.678901 uT
   [0:00:02.022]: 25.4 Cel
     accel  0.124567 -0.235678  9.823456 m/s/s
     gyro   0.001345 -0.002456  0.003567 rad/s
     magn  23.567890 -12.456789  45.789012 uT

   <repeats endlessly>
