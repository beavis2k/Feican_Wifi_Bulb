# Feican_Wifi_Bulb
Controls a Wifi Bulb made by FeiCan http://www.feicanled.com/articleshow.asp?id=807
it uses the DreamColor App https://play.google.com/store/apps/details?id=com.feicanled.dreamcolor&hl=en
Does NOT work with the Bluetooth version, only the Wifi.  Should also work with the LED Strip.
This uses a familiar ESP chip for wifi.

I purchased the light bulb and tried to use it with the app, it crashed on my phone.  I used an old android phone that worked, got it to connect to my wifi and it worked fine (when it didn't crash on old phone). I took it as a simple project to figure out how to make it work without the app.  I have Android x86 in a Virtual Machine, and WinShark to listened to the app and bulb communicate.
With winshark I found that it seemed to operate with UDP protocol in port 5000.
I got some values to turn it on and off, then next to set it to various colors.
Thanks to other projects that work in a similar way but different codes, I was able to make it work with a Raspberry Pi Zero
I can turn the bulb on, off and set a specific RGB color, but not the fancy blink or other modes.  Turning it on, off and set the color was more than enough for me.

First use the App to connect to your wifi, for best results use an old android phone 4.x 
Second find your bulb's IP address (ex. 192.168.1.62) Mac is 5c cf 7f ?? ?? ??, or Expressif 
Install the linux utility xxd, it converts a string to hex.
send the code with

echo "code" | xxd -r -p \> /dev/udp/ip-address/5000

Key Code:
Turn the Bulb on send "7e 04 04 01 ff ff ff 00 ef" to its IP
Turn the Bulb off send "7e 04 04 00 ff ff ff 00 ef"
Set the Bulb cool white send "7e 07 05 03 ff ff ff 00 ef"
Set the bulb any RGB color
             RR GG BB
"7e 07 05 03 RR GG BB 00 ef"
change the corresponding value of RR, GG or BB in hex

to convert "warm white" 255, 172, 68 -\> FF, AC, 44 -\> "7e 07 05 03 ff ac 44 00 ef"
On:
	echo "7e 04 04 01 ff ff ff 00 ef" | xxd -r -p \> /dev/udp/192.168.1.62/5000
Off:
	echo "7e 04 04 00 ff ff ff 00 ef" | xxd -r -p \> /dev/udp/192.168.1.62/5000
Change Color Warm White:
	echo "7e 07 05 03 ff ac 44 00 ef" | xxd -r -p \> /dev/udp/192.168.1.62/5000

if you get an error about folder not found, use "bash", not "sh" as sh does not have remote udp access.

Use code at own risk.
