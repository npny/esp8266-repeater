Instructions
============

- Get one of [these](https://www.amazon.co.uk/WIFI-Internet-Development-Board-Module/dp/B01N5D3MV8/) boards
	
	Actually get 10 of them. They're great. Extra documentation can be found at:
	
	- https://www.espressif.com/en/support/download/documents
	- https://github.com/nodemcu/nodemcu-firmware
	- https://nodemcu.readthedocs.io/
	- https://github.com/nodemcu/nodemcu-devkit-v1.0

- Connect it to your computer over microUSB.

	- The dev board converts USB 5V power lines to a 3.3v supply for the ESP8266, and the USB data lines to UART RX/TX lines connected to the ESP8266.  
	- This USB/Serial adapter shows up as an extra tty device on your computer and allows you to program the chip very easily without any extra hardware.  
	- On boot, the LED on the board should flash briefly a couple of times then go off. Don't worry, the board is still on.

- If running OSX, you need to download [drivers](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers) for the USB/Serial adapter first

- Install esptool

		pip install esptool

- Check that you can talk to the board

		esptool.py chip_id

- Get NodeMCU sources

		git clone https://github.com/nodemcu/nodemcu-firmware.git

- Enable desired modules

		nano nodemcu-firmware/app/include/user_modules.h

- Compile

		docker run --rm -it -v nodemcu-firmware:/opt/nodemcu-firmware marcelstoer/nodemcu-build

- Flash binary

		esptool.py erase_flash
		esptool.py write_flash 0x00000 nodemcu_integer*.bin

- Check that the board still works

		esptool.py chip_id

- Create filesystem

		npm install nodemcu-tool
		node_modules/nodemcu-tool/bin/nodemcu-tool.js mkfs
		node_modules/nodemcu-tool/bin/nodemcu-tool.js fsinfo

- Create barebone file to start hosting an AP

		echo <<EOF > ap.lua
			wifi.setmode(wifi.SOFTAP)
			wifi.ap.config {ssid="ap", pwd="password", auth=wifi.WPA2_PSK}
		EOF

- Upload and run it

		node_modules/nodemcu-tool/bin/nodemcu-tool.js upload ap.lua
		node_modules/nodemcu-tool/bin/nodemcu-tool.js run ap.lua

- Check that an AP is indeed being hosted
