# Fire-Alarm-System

This Project has been created in partial fulfilment of the course Microprocessors and Interfacing at BITS Pilani.

## Problem Statement

This system checks for abnormal smoke content in a room every two seconds. Under abnormal conditions it throws open two doors and two windows and opens a valve that releases the gas to put-out the fire. An Alarm is also sounded; this alarm is sounded until the smoke level in the room drops to an acceptable level. The smoke detection system is made up of two smoke sensors placed on the ceiling of the room. When the smoke level comes back below the danger level, the doors, windows and valves are closed.

## Assumptions

In order to reduce the design effort as well as to get the simplest design possible using the minimum amount of
hardware while meeting all the restrictions specified by our problem statement, we have made the following assumptions:

1. The smoke sensors used come with a built in ADC. It gives high output (1) in case smoke is present near it but gives low output (0) when smoke is not present.
2. Abnormal conditions are considered when both the smoke sensors give high output.
3. Dangerous conditions are considered when only one of the sensors gives high output.
4. Below the danger level, both the sensors give low output and the doors, windows and valve are closed.
5. We have used LEDs to simulate the 2 doors, 2 windows, 1 valve and 1 alarm.

## Design Process

#### Components Used 

| Chip Number | Quantity | Chip | Purpose |
|-------------|-----------|----------|--------|
|8086|1|Microprocessor|CPU|
|8253A|1|Programmable Interval Timer|To generate timing signals|
|8255A|1|Programmable Peripheral Interface|to interface ports as input and output for smoke sensors, openeing and closing doors, valves and windows and the sounding og the alarm|
|2732|2|EPROM|Read only erasable programmable memory to house code segment|
|6116|2|RAM|Read write memory|
|-|33|2 input OR gates|decoding logic|
|-|5|NOT gates|decoding logic|
|-|6|LEDs|For simulation of doors, windows, and valves|
|74LS373|3|8 bit bidirectional buffer|Buffering Address Bus|
|74LS245|2|8 bit bidirectional buffer|Buffering Data Bus|
|74LS244|2|Octal buffer|Creation of Vector Number (40h)|

#### Memory Organisation

**Total RAM used by system = 4KB
Total ROM used by system = 8KB**

RAM chip used = 6116.

So, size of each RAM chip = 16/8 = 2KB.
**Hence, number of 6116 RAM chips required = 4KB/2KB = 2.**

EPROM chip used = 2732
So, size of each ROM chip = 32/8 = 4KB.
**Hence, number of 2732 EPROM chips required = 8KB/4KB = 2.**

Let us consider RAM first.

RAM must house the data segment and stack segment.

Starting address of RAM = 00000 H
System has 4KB RAM. So, Ending address of RAM = 00FFF H . This is divided into even and odd banks.

**Even Bank of RAM** = 00000 H , 00002 H, 00004 H, 00006 H, . . . . . . . . . . . . . . . . . , 00FFE H.
**Odd Bank of RAM** = 00001 H , 00003 H, 00005 H, 00007 H, . . . . . . . . . . . . . . . . . , 00FFF H.

Now, let us consider ROM.

ROM must house the code segment.
Starting address of ROM = FD000 H
System has 8KB ROM. So, Ending address of ROM = FEFFF H .

**Even Bank of ROM** = FD000 H , FD002 H, FD004 H, FD006 H, . . . . . . . . . . . . . . . . . , FEFFE H.
**Odd Bank of ROM** = FD001 H , FD003 H, FD005 H, FD007 H, . . . . . . . . . . . . . . . . . , FEFFF H.

#### IO Based Memory Mapping

##### 8253 programmable Interval Timer
| |Address|A7|A6|A5|A4|A3|A2|A1|A0|
|--|------|-|-|-|-|-|-|-|-|
|Port A |30h|0|1|0|1|0|0|0|0|
|Port B |32h|0|1|0|1|0|0|1|0|
|Port C |34h|0|1|0|1|0|1|0|0|
|Control register |36h|0|1|0|1|0|1|1|0|

##### 8255 programmable Peripheral Interface
| |Address|A7|A6|A5|A4|A3|A2|A1|A0|
|--|------|-|-|-|-|-|-|-|-|
|Port A |00h|0|0|0|0|0|0|0|0|
|Port B |02h|0|0|0|0|0|0|1|0|
|Port C |04h|0|0|0|0|0|1|0|0|
|Control register |06h|0|0|0|0|0|1|1|0|

#### Sensor

MQ-2 Smoke Sensor circuit with built in Analog to digital convertor (with logic 1 as output in case of presence of
smoke else logic 0 is the output)

The 8253 is used in Mode 2 as a rate counter where an interrupt is given to the 8086 microprocessor every 2 seconds.
On the interrupt the 8086 checks for the values at the smoke detectors, that is if it is logic 1 or logic 0. On the basis of the following truth table the actions are taken :-

|Sensor 1|Sensor 2|Action Taken|
|---|---|---|
|0|0|Close door valves and windows|
|0|1|Open door valves and windows|
|1|0|Open door valves and windows|
|1|1|Open door valves and windows and alarm is sounded|

### For Circuit Diagram, Flowchart and detailed methodology, refer to the report.



 
