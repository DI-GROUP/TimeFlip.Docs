
# DATA TRANSFER PROTOCOL 

All values are stored in TimeFlip on-board  RAM memory and are reset to default when the battery is taken out or replaced. 

TimeFlip device uses Bluetooth Low Energy (BLE) protocol. Services and specifications are listed in the table:

| Service name / UUID            | Description                             |
|:----------------------------|:-------------------------------------|
|Device Information / 0x180A  | Contains device specific info    |
|Battery Service / 0x180F     | Battery charge                        |
|TimeFlip / F1196F50-71A4-11E6-BDF4-0800200C9A66 | Time and facets |

#### Device Information Service / 0x180A
| Characteristic's name | Size, bytes | Properties, R/W/N |
| :----------------- |:------------:|:---------------:|
| Firmware Revision String / 0x2A26| 6 | R |
R – reading W – writing N – notification

#### Battery Service / 0x180F
| Characteristic's name | Size, bytes | Properties, R/W/N |
| :----------------- |:------------:|:---------------:|
| Battery Level / 0x2A19| 1 | R,N |
R – reading W – writing N – notification

#### TimeFlip Service /  F1196F50-71A4-11E6-BDF4-0800200C9A66
| Characteristic's name | Size, bytes | Properties, R/W/N |
| :----------------- |:------------:|:---------------:|
| Accelerometer data / F1196F51-71A4-11E6-BDF4-0800200C9A66 | 6 | R |
| Facets / F1196F52-71A4-11E6-BDF4-0800200C9A66 | 1 | R, N |
| Command result output / F1196F53-71A4-11E6-BDF4-0800200C9A66 | 21 | R |
| Command / F1196F54-71A4-11E6-BDF4-0800200C9A66 | 21 | R, W |
| Double tap definition / F1196F55-71A4-11E6-BDF4-0800200C9A66 | 1 | N |
| Calibration version / F1196F56-71A4-11E6-BDF4-0800200C9A66 | 4 | R, W |
| Password / F1196F57-71A4-11E6-BDF4-0800200C9A66 | 6 | W |
R – reading W – writing N – notification

### Firmware Revision String / 0x2A26
Contains stock firmware version.
0x544676332E31 = “TFv3.1”

### Battery Level  / 0x2A19
Battery charge

### Accelerometer values characteristic / F1196F51-71A4-11E6-BDF4-0800200C9A66
_big-endian_ 0xXXYYZZ  - (x,y,z) acceleration vector.

### Facets characteristic / F1196F52-71A4-11E6-BDF4-0800200C9A66
ID value of notified facet (0..47)

### Command result output characteristic / F1196F53-71A4-11E6-BDF4-0800200C9A66
Output of command result, for example history read request "0x01" returns result in "Command result output characteristic"

History is read out in packages of 21 bytes.
History block contains 3 bytes. Example:

| Byte number | 0 |   |   |   |   |   |   |   | 1 |   |    |    |    |    |    |    |  2 |    |    |    |    |    |    |    |
|:-----------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
|  Bit number | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 |
|    Time    | X | X | X | X | X | X | X | X | X | X |  X |  X |  X |  X |  X |  X |  X |  X |    |    |    |    |    |    |
|   Facet   |   |   |   |   |   |   |   |   |   |   |    |    |    |    |    |    |    |    |  X |  X |  X |  X |  X |  X |

Maximum amount of time stored in history – 262144 seconds or ~ 3,03 days. New interval created on exceed.
Maximum facets number – 48
One package contains 7 history blocks.

#### History read-out protocol
After history read-out request is sent, the first package of 7 history blocks will be written in the characteristics. History blocks will be updated along with history reading-out until the very last one. Example:

| Byte number  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|--------------|---|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| History block | 0 | 0 | 0 | 1 | 1 | 1 | 2 | 2 | 2 | 3 |  3 |  3 |  4 |  4 |  4 |  5 |  5 |  5 |  6 |  6 |  6 |

If the last history package is not full, missing values will be filled with zeros.
Penultimate package will contain the information on the number of sent history packages. Example:

| Byte number  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|:--------------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| History block count | X | X | 0 | 0 | 0 | 0 | 0 | 0 | 0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | 0 |

The last package contains zeros, it thus communicates the end of history data transmission.

| Byte number  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|:--------------:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| History block | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 |  0 | 0 |

### Command characteristic / F1196F54-71A4-11E6-BDF4-0800200C9A66

Executed command is shown in characteristics as 0xXX 0x02 where 0хХХ – is command message.
Unexecuted command is shown in characteristics as 0xXX 0x01 where 0хХХ – is command message.

|Type      | Access | Size, bytes | Description |
|---------|--------|----|----------|
| Command | Write | 21 | 0xXXYY..YY, where XX - command, YY - data. response is in  “**command result output**” |
|         | Read | 2  | 0xXXYY, where XX - command YY - error code (2 - OK, 1 - ERROR)|

#### Commands: 

0x01 – history read out request  
0x02 – delete history

0x03 – calibration reset.
Resets values for TimeFlip  facets and resets characteristics Calibration version (UUID: F1196F56-71A4-11E6-BDF4-0800200C9A66) to zero.

0x04 0x01 – lock function* on
0x04 0x02 – lock function off

0x05 0xXX 0xXX – auto-pause** (off by default).
0xXX 0xXX – timer value in minutes after which the auto-pause is activated (0 = auto-pause off)
Auto-pause timer is automatically reset when TimeFlip is flipped to another facet or on successful password write.

0x06 0x01 - pause*** function on
0x06 0x02 – pause function off

0x10 – status request (response in  “command result outup” shown as 0xXXYYZZZZ)
0xXX – lock function (0x01 – on, 0x02 – off)
0xYY - pause (0x01 – on, 0x02 – off)
0xZZ 0xZZ – auto-pause timer value (in minutes)

0x15 0xXX 0xZZ … 0xZZ - write name
0xXX – number of symbols in name	
0xZZ … 0xZZ - name (19 symbols MAX. ASCII coding) 

0х30 0xZZ … 0xZZ – set new password 
0xZZ … 0xZZ – password set, length is 6 symbols

0x50 0xAA – Delete current firmware and reboot to firmware loader.

\* lock function  – locks TimeFlip to count time on current active facet and blocks the device from switching facets when TimeFlip is turned or flipped. 

\*\* Autopause function – automatically sets time count on pause after pre-set period of time (timer value).

\*\*\* Pause – time count is set on pause, but the facets continue to be notified (user can turn/flip TimeFlip and assign new tasks to facets). This appear in history as facet with ID 63 (0b111111).

### Double tap characteristic / F1196F55-71A4-11E6-BDF4-0800200C9A66
Reserved for future use

### Calibration version characteristic / F1196F56-71A4-11E6-BDF4-0800200C9A66

| Type | Access         | Size, byte | Description                                                                                                                                                 |
|-----|----------------|--------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
|  Command   | Write, Read | 4            | Saves the value recorded between connections. The value is reset to ”0” when the battery is pulled out or “reset calibration” command is executed (0x03)”. |

This characteristics is used to check whether TimeFlip facets calibration corresponds to facets calibration in the mobile app. The check is performed by comparing value read out from this characteristics and the one stored in the mobile app. When the facets are first time assigned in TimeFlip, an arbitrary number is written to present characteristics and in the same time in the mobile app. 

### Password characteristic / F1196F57-71A4-11E6-BDF4-0800200C9A66

| Type    | Access | Size, byte | Description |
|--------|--------|--------------|---|
| Password | Write | 6            | Requires password to be written in it to allow TimeFlip operation. |

If the password is not provided, or provided incorrect, TimeFlip service's characteristics will return nothing on reading. Note that: 
- TimeFlip requires password input after re-connect to authorize connected device
- password is reset to default every time the battery is taken out or replaced
- default password is ASCII "000000" or {0x30, 0x30, 0x30, 0x30, 0x30, 0x30}
