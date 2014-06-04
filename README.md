# Push Button Stop Motion

Make your own stop motion animation rig with a push button, using Python Picamera and GPIO.

You can use LEGO to animate a tower being built, or figures acting out a scene! Or anything else you can think of...

## Requirements

As well as a Raspberry Pi with an SD card with Raspbian, you'll also need:

### Hardware

- 1 x [Raspberry Pi Camera Module](http://www.raspberrypi.org/product/camera-module/)
- 1 x Solderless breadboard (e.g. from [Pimoroni](http://shop.pimoroni.com/products/colourful-mini-breadboard))
- 2 x male-to-female jumper leads (e.g. from [Pimoroni](http://shop.pimoroni.com/products/jumper-jerky))
- 1 x tactile button (e.g. from [RS Components](http://uk.rs-online.com/web/p/tactile-switches/7182443/))

### Software

- python-picamera
- python-rpi.gpio
- ffmpeg

Install these with the following command:

```bash
sudo apt-get install python-picamera python-rpi-gpio ffmpeg
```

Alternatively using Python 3, the following command will install the required packages:

```bash
sudo apt-get install python3-picamera python3-rpi-gpio ffmpeg
```

### Extras

- Animation subject (e.g. LEGO)
- Camera mount (optional but useful)
    - Can be home-made / makeshift (e.g. cardboard and/or blu tack)
    - Can be bought (e.g. from [Pimoroni](http://shop.pimoroni.com/products/raspberry-pi-camera-mount) or [ModMyPi](https://www.modmypi.com/flexible-camera-mount))

## Steps

0. Setup the Pi and camera board
1. Test the camera
2. Take a picture with Python
3. Connect a hardware button
4. Take a selfie
5. Stop motion animation
6. Compile the video

## Worksheet

Go to the [worksheet](worksheet.md)

## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

[![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

***Push Button Stop Motion*** by [Dave Jones](https://github.com/waveform80) and the [Raspberry Pi Foundation](http://www.raspberrypi.org) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Based on a work at https://github.com/raspberrypilearning/push-button-stop-motion