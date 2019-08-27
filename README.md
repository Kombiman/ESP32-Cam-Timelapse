# ESP32-Cam-Timelapse
A power-efficient and customizable program to create time-lapses or record picture sequences using deep sleep in between pictures and before starting up.

Can be uploaded to the module using the default partition scheme

# Programming
Here is the ESP32-CAM module, I added a 32GB Micro SD card in the top, however only 4GB is usable

Plug it in into a FTDI programmer board like the red one above. Remember to set the FTDI board to 3.3v (As 5v may damage the board), which is the right position on the switch when the USB port is facing up. In my programmer, I am supplying a 5v VCC supply voltage to the ESP32-CAM module so less uploading and brownout problems occur when using a less-powerful USB port or a module that has a "less-than-optimal" power regulator. When programming the board, pull GPIO 0 to ground (which is the small switch I put on the side in the left position). Remember to pull GPIO 0 to ground before plugging the USB port into your computer, or else the next step may not work and get stuck in a `Connecting....._____.....` loop and errors out.

Set the board to the ESP Wrover Module and the settings below. Press the upload button on the Arduino IDE and when the serial monitor on the bottom starts saying `Connecting......._____ `, press the button on the bottom of the ESP32-CAM Module. This resets the board and allows the code to upload to the ESP32 module. The serial monitor will hard reset and tell you when it hard resets and is done uploading.

On some of my boards, the power regulator seemed to not operate well when plugged into any of my desktop's USB ports, and was in a constant restart loop. Due to this, I had to set the board to Flash Frequency: 40MHz, instead of the usual Flash Frequency: 80MHz in order to be able to run the code, though uploading on 80MHz ran without error until it ran the code iteself. I labeled these boards as "40" to keep track of them.

# Running
If you want to test the board, unplug the VCC power connection or the USB connection to the FTDI programmer and disconnect GPIO 0 from ground (right position in my board). Plug the power back in, and open the serial monitor running at a baud rate of 115200. Press the reset button located underneath the ESP32-CAM module and it should output a bunch of text, and then some serial output.

If you want to run the board without the FTDI programmer, you need to supply the camera with 5v through the top left pin labeled as 5V (or 3.3v in the top left pin labaled 3V3), and connect the ground pin to GND. In my case, I used 3 standard 18650 3.7v Lithium Ion battery in series with a buck converter and charger. In most cases, using a lithium battery would be one of the better options, so modules stepping the voltage up to 5v and offing charge protection is quite common and cheap. You could either solder on a male USB header, connecting the red wire to 5V, black wire to GND, and optionally connecting the white and green data pins together. 

# Customizable Variables
## initDelay
This is the initial delay where the ESP32-CAM is put into deep sleep before waking up and taking pictures. By default I set it to 30 seconds, and the range can be from 0 seconds to 584,542 years (Max 64 bit integer in microseconds).

## TIME_TO_SLEEP
This is the time the camera waits before taking the next picture. By default, I set it to 3 seconds.

## config.frame_size (in the setup() function)
This is how big you want the picture to be taken. By default, I set it to UXGA (1600px by 1200px), which averaged around 180kb - 230kb per picture. Changing the frame size allows for more photos to be stored onto the SD card (limited by the 4GB).

## config.jpeg_quality (in the setup() function)
This is how good the picture should be. By default, I set it to 10, and the lower it is, the better quality it is. The range for this is 10-63, with 10 being the best quality and 63 being the worst.

## config.fb_count (in the setup() function)
This allocates the size of the (frame buffer)?. From the examples including PSRAM (which this module does), it defaults to 2, whereas without PSRAM it goes to 1. I would not recommend changing this unless there are some quality issues with the photos.

# Additional Information
The built-in flash LED is connected to one of the pins of the SD card, and I am unsure if removing it may damage the SD card operation. Whenever the SD card is being written, the flash will turn on, and is a hardware issue that can not be fixed in software (as far as I know). I would recommend taping over the light with something like electrical tape if this is an issue.



