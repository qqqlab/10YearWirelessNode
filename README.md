## Goal

A wireless sensor node that runs 10 years on a single CR2032 coin cell battery, transmitting a wireless message every 2 minutes. Approximate range: outdoor 300 meter, indoor 30 meter.

## Components
 - ATTINY3216 AVR Microcontroller
 - RFM69CW 868MHz Radio Module
 - Si7021 Temperature and Humidity Sensor
 - CR2032 Battery
 
## Power Budget for 10 Years Operation

(Worst Case Scenario)

__CR2032 Initial capacity: 200 mAh__

__Minus: 10 years self discharge: 40 mAh__ (10 * 2% * 200mAh)
 
Standby Current  
AVR: 0.8uA  
RFM69: 0.1 uA  
Si7021: 0.06 uA  
Total: 1 uA  
10 years = 87600 hours (24h * 365d * 10y)   
__Minus: 10 years standby: 90 mAh__ (1uA * 87600h)

__Available Operating Capacity: 70 mAh__ (or 252As) (200 - 40 - 70)

### Operating as Transmitter
Number of Messages: 252As / 102uAs = 2.5 million   
10 years = 5 million minutes (60m * 24h * 365d * 10y)  
__Message Interval: 1 message every 2 minutes__ (5M min/yr / 2.5M msg)

### Operating as Transmitter and Receiver
Number of Messages: 252As / 282uAs = 0.9 million  
__Message Interval: 1 message every 6 minutes__ (5M min/yr / 0.9M msg)

## Operating Power Consumption

### Wireless Packet format
3 bytes preample, 2 sync, 1 length, 1 msgtype, 6 adr, 16-32 (encrypted) payload, 2-4 crc or cmac  
Bytes per packet: 31-49  
Time per packet: 1.0-1.5 ms (at max speed)

### Operating as transmitter only, encrypted:   
Sensor conversion: 3.5 uAs 
AVR Sensor processing: 20kC * 0.4uAs/kC = 8 uAs  
AVR C encrypt: 2 block payload + 2 block cmac = 20 uAs  
RFM69CW TX: 1.0-1.5ms * 45mA = 50-70 uAs 
Total: 102 uAs / msg

### Operating as transmitter/receiver, encrypted:
Sensor conversion: 3.5 uAs 
AVR Sensor processing: 20kC * 0.4uAs/kC = 8 uAs  
AVR C encrypt: 2 block payload + 2 block cmac = 20 uAs  
RFM69CW TX: 1.0-1.5ms * 45mA = 50-70 uAs  
RFM69CW RX: 10ms * 16mA = 160uAs  
AVR C decrypt: 2 block payload + 2 block cmac = 20 uAs  
Total: 282 uAs / msg

### Encryption options
AVR C encrypt 16 byte block: 12kC * 0.4uAs/kC = 5 uAs  
AVR ASM encrypt 16 byte block: 3kC * 0.4uAs/kC = 1.2 uAs  
RFM69CW encrypt 16 byte block: 0.7us * 2mA = 0.0015 uAs  


## Component Specifications

### RFM69CW
Transmit full power (13dB): 45mA  
Receive: 16 mA  
Idle: 2 mA  
Sleep: 0.1 uA  
Max transmission speed: 250kbps  

### ATTINY3216/1616
Running 3V 5MHz: 1.7mA -> power consumption per 1000 cycles (1kC): 0.4uAs/kC  
Standby with RTC: 0.71uA  

### CR2032 Coin Cell Battery
Nominal Voltage: 3.0 Volts  
Typical Capacity: 200-235 mAh (to 2.0 volts, Rated at 15K ohms at 21°C)  
Typical Weight: 3.0 grams (0.10 oz.)  
Typical Volume: 1.0 cubic centimeters (0.06 cubic inch)  
Max Rev Charge: 1 microampere  
Energy Density: 198 milliwatt hr/g, 653 milliwatt hr/cc  
Typical Li Content: 0.109 grams (0.0038 oz.)  
Operating Temp: -30C to 60C  
Self Discharge: 1-2% / year  

### Si7021 Temperature and Humidity Sensor
Operating: 1.9 - 3.6 V  
Standby: 0.06 uA  
Humidity Conversion: 12 ms * 180uA = 2.2 uAs  
Temperature Conversion: 11 ms * 120uA = 1.3 uAs  
Total Conversion: 3.5 uAs  
