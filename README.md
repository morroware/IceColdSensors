# Raspberry Pi Temperature and Humidity Monitoring

This repository contains two Python scripts for monitoring temperature and humidity using BME280 sensors on a Raspberry Pi. The scripts send alerts via Slack and log data to Adafruit IO.

- `dual_sensor_script.py`: For monitoring two BME280 sensors.
- `single_sensor_script.py`: For monitoring a single BME280 sensor.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Initial Setup](#initial-setup)
- [Hardware Setup](#hardware-setup)
- [Software Setup](#software-setup)
- [Configuration Files](#configuration-files)
- [Adafruit IO Setup](#adafruit-io-setup)
- [Running the Scripts](#running-the-scripts)
- [Start Script on Boot](#start-script-on-boot)
- [Troubleshooting](#troubleshooting)

## Prerequisites

- Raspberry Pi (Any model with GPIO pins will work)
- BME280 sensor(s)
- Jumper wires
- Breadboard (optional)
- Internet connection
- Slack Workspace
- Adafruit IO account

## Initial Setup

### Enable SSH and WiFi

1. Place a file named `ssh` (no extension) in the root directory of the boot partition to enable SSH.
2. Create a `wpa_supplicant.conf` file in the same boot partition with the following content:

    ```
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    country=US
    update_config=1

    network={
    ssid="WIFI_SSID"
    scan_ssid=1
    psk="WIFI_PASSWORD"
    key_mgmt=WPA-PSK
    }
    ```

3. Insert the SD card back into the Raspberry Pi and boot it up. It should connect to the WiFi network, and SSH should be enabled.

## Hardware Setup

### Enabling I2C on Raspberry Pi

1. Open the Raspberry Pi configuration menu: `sudo raspi-config`
2. Navigate to `Interface Options > I2C` and enable it.
3. Reboot your Raspberry Pi.

### Detailed Hardware Setup for BME280 Sensor(s)

1. Power off the Raspberry Pi.
2. Connect the `VCC` pin of the BME280 to the `3.3V` pin on the Raspberry Pi.
3. Connect the `GND` pin of the BME280 to the `Ground` pin on the Raspberry Pi.
4. Connect the `SDA` pin of the BME280 to the `SDA` pin (GPIO 2) on the Raspberry Pi.
5. Connect the `SCL` pin of the BME280 to the `SCL` pin (GPIO 3) on the Raspberry Pi.
6. If using two sensors, connect them in parallel to the same SDA and SCL lines. Ground one sensor's `SDO` pin to change its address to `0x76`.
7. Power on the Raspberry Pi.

## Software Setup

1. Update your Raspberry Pi: `sudo apt update && sudo apt upgrade`
2. Install the required Python packages:

    ```bash
    pip install smbus2 bme280 slack_sdk Adafruit_IO
    ```

## Configuration Files

Create configuration files with appropriate settings. Samples are provided in the repository.

## Adafruit IO Setup

1. Log in to your Adafruit IO account or create one if you don't have it.
2. Navigate to `Dashboards` and create a new dashboard for your sensors.
3. Add blocks (e.g., gauge, chart) to your dashboard.
4. Go to `Feeds` and create new feeds for temperature and humidity.
5. Place these feeds into a **group**.
6. Make sure to update the `ADAFRUIT_IO_USERNAME`, `ADAFRUIT_IO_KEY`, `ADAFRUIT_IO_GROUP_NAME`, `ADAFRUIT_IO_TEMP_FEED`, and `ADAFRUIT_IO_HUMIDITY_FEED` in your configuration files to match your Adafruit IO setup.

## Running the Scripts

1. Clone this repository:

    ```bash
    git clone <repository_url>
    ```

2. Navigate into the directory:

    ```bash
    cd <repository_directory>
    ```

3. Run the script:

    - For dual sensors:

        ```bash
        python dual_sensor_script.py
        ```

    - For a single sensor:

        ```bash
        python single_sensor_script.py
        ```

## Start Script on Boot

1. Open crontab configuration:

    ```bash
    crontab -e
    ```

2. Choose `nano` as the editor if prompted.

3. Add the following line at the end of the file:

    ```bash
    @reboot python /path/to/your/script.py &
    ```

4. Save the file and exit `nano`.

## Troubleshooting

For troubleshooting, please refer to the `Troubleshooting.md` file in this repository.

---

For more help, please open an issue in this GitHub repository.



