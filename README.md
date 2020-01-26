# wifi-kit-32
Mac OS X Setup instructions and examples for the WiFi Kit 32 Board

![](http://esp32.net/images/Heltec/WIFI-Kit-32/Heltec_WIFI-Kit-32_PhotoDisplay.jpg)

## Setup

1. Install the esptool

	```
	% pip install esptool 
	% pip install esptool --upgrade
	```
1. Install Adafruit Micropython Tool (ampy)

	```
	% pip install adafruit-ampy
	```
1. Download the Micropython ESP32 firmware from here: [https://micropython.org/download#esp32](https://micropython.org/download#esp32)

1. Connect the board to the computer.  Verify the USB connection is present:

	```
	% ls /dev/tty.SLAB*
	/dev/tty.SLAB_USBtoUART
	```

1. Test the communications:

	```
	% esptool.py --port /dev/tty.SLAB_USBtoUART flash_id
	esptool.py v2.8
	Serial port /dev/tty.SLAB_USBtoUART
	Connecting........_
	Detecting chip type... ESP32
	Chip is ESP32D0WDQ6 (revision 1)
	Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
	Crystal is 26MHz
	MAC: 2x:xx:xx:xx:xx:x8
	Uploading stub...
	Running stub...
	Stub running...
	Manufacturer: ef
	Device: 4017
	Detected flash size: 8MB
	Hard resetting via RTS pin...
	```
	
	```
	% esptool.py --port /dev/tty.SLAB_USBtoUART chip_id
	esptool.py v2.8
	Serial port /dev/tty.SLAB_USBtoUART
	Connecting........_
	Detecting chip type... ESP32
	Chip is ESP32D0WDQ6 (revision 1)
	Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
	Crystal is 26MHz
	MAC: 24:6f:28:79:10:f8
	Uploading stub...
	Running stub...
	Stub running...
	Warning: ESP32 has no Chip ID. Reading MAC instead.
	MAC: 2x:xx:xx:xx:xx:x8
	Hard resetting via RTS pin...
	```
1. Erase the flash

  ```
  % esptool.py --port /dev/tty.SLAB_USBtoUART erase_flash
  esptool.py v2.8
  Serial port /dev/tty.SLAB_USBtoUART
  Connecting........_____.
  Detecting chip type... ESP32
  Chip is ESP32D0WDQ6 (revision 1)
  Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
  Crystal is 26MHz
  MAC: 2x:xx:xx:xx:xx:x8
  Uploading stub...
  Running stub...
  Stub running...
  Erasing flash (this may take a while)...
  Chip erase completed successfully in 18.9s
  Hard resetting via RTS pin...
  ```

1. Upload the Micropython firmware

	```
	% esptool.py --port /dev/tty.SLAB_USBtoUART write_flash -z 0x1000 ~/Downloads/esp32-idf3-20200125-v1.12-87-g96716b46e.bin
	esptool.py v2.8
	Serial port /dev/tty.SLAB_USBtoUART
	Connecting........_
	Detecting chip type... ESP32
	Chip is ESP32D0WDQ6 (revision 1)
	Features: WiFi, BT, Dual Core, 240MHz, VRef calibration in efuse, Coding Scheme None
	Crystal is 26MHz
	MAC: 2x:xx:xx:xx:xx:x8
	Uploading stub...
	Running stub...
	Stub running...
	Configuring flash size...
	Auto-detected Flash size: 8MB
	Flash params set to 0x0230
	Compressed 1433664 bytes to 912886...
	Wrote 1433664 bytes (912886 compressed) at 0x00001000 in 80.8 seconds (effective 141.9 kbit/s)...
	Hash of data verified.
	
	Leaving...
	Hard resetting via RTS pin...
	```
	
1. Connect to the board and verify Micropython

	```
	screen /dev/tty.SLAB_USBtoUART 115200
	```

1. Download the OLED SSD1306 python library and unzip: [https://github.com/adafruit/micropython-adafruit-ssd1306](https://github.com/adafruit/micropython-adafruit-ssd1306)

1. Upload the ssd1306 library:

	```
	ampy --port /dev/tty.SLAB_USBtoUART put ~/Downloads/micropython-adafruit-ssd1306-master/ssd1306.py
	```

1. Test the OLED display:

	```
	import machine, ssd1306
	from machine import Pin
	
	# pull the OLED_RST pin HIGH
	oled_rst = Pin(16, Pin.OUT)
	oled_rst.value(1)
	
	# Connect to the board
	i2c = machine.I2C(scl=machine.Pin(15), sda=machine.Pin(4))
	oled = ssd1306.SSD1306_I2C(128, 64, i2c)
	
	# Display text
	oled.text('Hello World', 0, 0)
	oled.show()
	```

## Reference

[WIFI Kit 32 Pinout Diagram](http://esp32.net/images/Heltec/WIFI-Kit-32/Heltec_WIFI-Kit-32_DiagramPinout.jpg)

![WIFI Kit 32 Pinout Diagram](http://esp32.net/images/Heltec/WIFI-Kit-32/Heltec_WIFI-Kit-32_DiagramPinout.jpg)

