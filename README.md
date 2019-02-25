# Attendance-Device

Once in a while when we travel to some places in a group, either it't with a bunch of friends, or it's within a travel group where people don't know about each other at all, we would always have to spend some time on finding out who had turned up and who's not after individual activities. This *Attendance-Device* is trying to tease this problem with the quite mature RFID technology which is quite popular in the IoT world, especially in the storage management and goods delivery. What if we give each of the group member an RFID tag and scan them when we need to do a head count? 

* [Hardware List](#hardware-list)
* [Hardware Connections](#hardware-connections)
* [Hardware Configurtaions](#hardware-configurations)
* [Android Client](#android-client)

# Hardware List

* **RFID Module**: [UM202 UHF-RFID Development Board](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.4a812e8dZ4PXkE&id=584916767519&_u=p1f3q4imcb7c) manufactured by Guangzhou Yuhui Technology
* **Bluetooth Module**: [HC-05 Bluetooth Module](https://item.taobao.com/item.htm?spm=a1z09.2.0.0.4a812e8dfzVVcu&id=562980764753&_u=p1f3q4im0ba7) manufactured by Guangzhou Huicheng Technology
* **Voltage Converter**:[Six in One USB to RS-232 to TTL Converter](https://detail.tmall.com/item.htm?id=41297073849&spm=a1z09.2.0.0.4a812e8dMSRica&_u=p1f3q4im2c3f) manufactured by Shenzhen Telesky
* **Power Bank**: Any usual Power Bank that could provide a 5V output would be sufficient for this device. 

I've hyperlinked the each component's store link and included all the documentations for the above hardware in the [documentation](/documentation) folder. However I do apologize all of them are written in Chinese since they are purchased on [www.taobao.com](www.taobao.com). Please use google translator (or any methods that would help you to understand them) if you wish to have a deep reading into them. 

# Hardware Connections

This device uses a typical 5V power bank as the power supply unit. Currently it uses Dupont wire as the connecting wires between different components. The detail connections is listed as below: 

* **RFID Module & Voltage Converter**

  Since the RFID Module uses RS-232 and the Bluetooth Module uses TTL, it is necessary to convert the voltage using the Voltage Converter in order to let them communicate correctly. 

  1. Connect the *232Txd* terminal of the Voltage Converter with the *232Rxd* terminal of the RFID Module
  2. Connect the *232Rxd* terminal of the Voltage Converter with the *232Txd* terminal of the RFID Module
  3. Connect one of the *GND* terminals of the Voltage Converter with the *GND* terminal of the RFID Module
  4. Connect the *5V* terminal of the Voltage Converter with the *VCC* terminal of the RFID Module

* **Bluetooth Module & Voltage Converter**

  1. Connect the *Rxd* terminal of the Bluetooth Module to the *Txd* terminal of the Converter 
  2. Connect the *Txd* terminal of the Bluetooth Module to the *Rxd* terminal of the Converter 
  3. Connect the *GND* terminal of the Bluetooth Module to the other *GND* Module of the Converter 
  4. Connect the *VCC* terminal of the Bluetooth Module to the *3.3V* of the Voltage Converter

* **Voltage Converter & Power Bank**

  Plug the Voltage Converter into the Power Bank so that it could start supplying power to the RFID Module and the Bluetooth Module simultaneously and convert the electric signal at the mean time. 

# Hardware Configurations

* **Configuring Bluetooth Module**

  By default, the RFID Module is using UART serial communication port to communicate. It uses 1 start bit, 8 data bits, zero parity bits and 1 stop bit for communication and the baud rate is 115,200. In order to hook up the Bluetooth Module with the RFID Module and let them communicate each other, we need to configure the Bluetooth Module to transmit data at 115,200 Baud rate. 

  To do so, we need to start up the Bluetooth Module by setting pin 38 into high voltage so that we could enter into AT mode. To do so on the HC-05 Module, just press and hold the black button on the chip while plug it into any suitable power source (such as the voltage adapter). Then with the proper wiring (Rxd-Txd, Txd-Rxd, Gnd-Gnd, Vcc-Vcc), we use the AT command *AT+UART=115200,1,0 and ends with an return to set the baud rate of the Bluetooth Module to 115,200 with 1 stop bit and no parity bit.

* **Configuring the Converter Module**

  Since the converter is a six-in-one module, in order for it to convert from RS-232 signals to TTL module, set both *switch 1* and *switch 2* to off and set the *black switch* to the 232-ttl side. 

# Android Client

Within the [code](/code) folder there is a basic framework of android application which is not working due to time limitation. However I've included the UHF_RFID test software in the [documentations](/documentations) folder so that you could connect the Bluetooth Module with your laptop, setup serial port on top of the Bluetooth Connections and test is with the software. 

It is certainly wise to use existing library to boost the development. However, in this case, since the RFID Module communicates in frames of bytes, most Bluetooth library are using strings, which is definitely not using ascii encoding. Hence referencing other Bluetooth library structures and build byte streams communications from scratch is necessary. 

For basic utility testing, the hex command `A5 5A 00 0A 82 27 10 BF 0D 0A` would let the RFID device looks for tags for 10,000 times. To stop it, uses the hex command `A5 5A 00 08 8C 84 0D 0A`.