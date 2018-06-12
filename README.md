# emonTXethernet

This repository is a collection of files used to correctly setup and use a EmonTX Shield on a Arduino. It uses a Ethernet Shield for the communication instead of the usual radio seen on EmonTX. 

This repository has the following items:
* The firmware sketch for a Arduino to measure the EmonTX CT sensor inputs and send them with a HTTP GET request to EmonCMS.
* 3D printing files for a case that holds a Arduino Uno with a mounted standard Ethernet Shield and EmonTX Shield.
* Simplistic OpenHab instructions for correctly reading the EmonCMS MQTT messages and display them on your sitemap.
* Custom PCB Board files to make a simplified EmonTX Board soley focused on the CT input sensors and 9V AC-AC adapter. Without all components for UART or radio communication. 
* Simplified instruction steps to set this all up correctly. 

This is all based on original work by others. See the invidiual files for more credit information. Mostly OpenEnergyMonitor is responsible for all the posibilities seen here. https://openenergymonitor.org/

## Instruction Steps
* Step 1
* Step 2
* ... WIP ... 
