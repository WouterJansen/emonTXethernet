# Arduino Firmware
This firmware sketch is meant for a Arduino with Ethernet Shield and EmonTX Shield with a plugged in 9V AC-AC adapter. It measures all 3 CT sensors (disable the lines of those you don't use) and sends the data(RealPower,SupplyVoltage and Irms) with a HTTP GET request to the EmonCMS server. Make sure to include your IP address for your EmonCMS Server, your EmonCMS Write API Key and the MAC and IP you choose for your Ethernet Shield. 

It also prints out the information on the serial connection for debugging purposes. 

Based on the EmonTX Shield Serial Only example:
https://github.com/openenergymonitor/emontx-shield/blob/master/firmware/Shield_CT1234_Voltage_SerialOnly/Shield_CT1234_Voltage_SerialOnly.ino
and the Ethernet Client example.

