## Raspberry Pi Audio Receiver - Fork of Project from Nicokaiser to be adapted for raspberry pi zero w

A simple, light weight audio receiver with Bluetooth (A2DP), AirPlay 1, and Spotify Connect.
Original repository can be found [here](github.com/nicokaiser/rpi-audio-receiver).

## Features

Devices like phones, tablets and computers can play audio via this receiver.

## Requirements

- A USB Bluetooth dongle (the internal Raspberry Pi Bluetooth chipset turned out as not suited for audio playback and causes all kinds of strange connectivity problems)
- Raspberry Pi OS Buster Lite (legacy)
- Internal audio, HDMI, USB or I2S Audio adapter (tested with [Adafruit USB Audio Adapter](https://www.adafruit.com/product/1475),  [pHAT DAC](https://shop.pimoroni.de/products/phat-dac), and [HifiBerry DAC+](https://www.hifiberry.com/products/dacplus/))

## Installation

The installation script asks whether to install each component.

    wget -q https://github.com/Arcadia197/rpi-audio-receiver/archive/main.zip
    unzip main.zip
    rm main.zip

    cd rpi-audio-receiver-main
    sudo ./install.sh

### Basic setup

Lets you choose the hostname and the visible device name ("pretty hostname") which is displayed as Bluetooth name, in AirPlay clients and in Spotify.

### Bluetooth

Sets up Bluetooth, adds a simple agent that accepts every connection, and enables audio playback through Alsa (via BlueAlsa). A udev script is installed that disables discoverability while connected.

### AirPlay 1

Installs [Shairport Sync](https://github.com/mikebrady/shairport-sync) AirPlay Audio Receiver.

### Spotify Connect

Installs a working version of [Raspotify](https://github.com/dtcooper/raspotify), an open source Spotify client for Raspberry Pi.

## Limitations

- Only one Bluetooth device can be connected at a time, otherwise interruptions may occur.
- The device is always open, new clients can connect at any time without authentication.
- To permanently save paired devices when using read-only mode, the Raspberry has to be switched to read-write mode until all devices have been paired once.
- You might want to use a Bluetooth USB dongle or have the script disable Wi-Fi while connected (see `bluetooth-udev`), as the BCM43438 (Raspberry Pi 3, Zero W) has severe problems with both switched on, see [raspberrypi/linux/#1402](https://github.com/raspberrypi/linux/issues/1402).
- The Pi Zero may not be powerful enough to play 192 kHz audio, you may want to change the values in `/etc/asound.conf` accordingly.

## Wiki

There are some further examples, tweaks and how-tos in the [GitHub Wiki](https://github.com/nicokaiser/rpi-audio-receiver/wiki).

## Disclaimer

These scripts are tested and work on a current Raspberry Pi OS setup on Raspberry Pi. Depending on your setup (board, configuration, sound module, Bluetooth adapter) and your preferences, you might need to adjust the scripts. They are held as simple as possible and can be used as a starting point for additional adjustments.

## Upgrading

This project does not really support upgrading to newer versions of this script. It is meant to be adjusted to your needs and run on a clean Raspberry Pi OS install. When something goes wrong, the easiest way is to just wipe the SD card and start over. Since apart from Bluetooth pairing information all parts are stateless, this should be ok.

Updating the system using `apt-get upgrade` should work however.

## Uninstallation

This project does not support uninstall at all. As stated above, it is meant to run on a dedicated device on a clean Raspberry Pi OS. If you choose to use this script along with other services on the same device, or install it on an already configured device, this can lead to unpredictable behaviour and can damage the existing installation permanently.
However, trying to recreate the state can be tried with the following code. This does however not remove residual files and is experimental:

    sudo apt-get purge raspotify shairport-sync
    sudo systemctl disable bthelper@hci0.service bt-agent@hci0.service
    sudo systemctl stop bthelper@hci0.service bt-agent@hci0.service

## Contributing

Package and configuration choices are quite opinionated but as close to the Debian defaults as possible. Customizations can be made by modifying the scripts, but the installer should stay as simple as possible, with as few choices as possible. That said, pull requests and suggestions are of course always welcome. However I might decide not to merge changes that add too much complexity.

## References

- [Shairport Sync: AirPlay Audio Receiver](https://github.com/mikebrady/shairport-sync)
- [Raspotify: Spotify Connect client for the Raspberry Pi that Just Works™](https://github.com/dtcooper/raspotify)

## License

[MIT](LICENSE)
