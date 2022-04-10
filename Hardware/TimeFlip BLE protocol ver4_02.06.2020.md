









**TIMEFLP2** 

**DATA TRANSFER PROTOCOL**

ver. 4.3.

rev. 20.02.2022**

All values are stored in TimeFlip2 on-board RAM memory and are reset to default when the battery is taken out or replaced.

TimeFlip2 device uses Bluetooth Low Energy (BLE) protocol. Services and characteristics are listed in the table 1:


Tab. 1 – TimeFlip2 services and characteristics:

|#|Service name/UUID|Characteristic’s name/UUID|Size, bytes|<p>Properties, </p><p>R – read</p><p>W – write</p><p>N – notify</p>|
| :-: | :-: | :-: | :-: | :- |
|1|<p>Generic Access /</p><p>0x1800</p>||||
|||<p>Device Name /</p><p>0x2A00</p>|20|R|
|2|<p>Device Information /</p><p>0x180A</p>||||
|||<p>Manufacturer Name String /</p><p>0x2A29</p>|20|R|
|||<p>Model Number String /</p><p>0x2A24</p>|20|R|
|||<p>Hardware Revision String /</p><p>0x2A27</p>|20|R|
|||<p>Firmware Revision String /</p><p>0x2A26</p>|20|R|
|||<p>System ID / </p><p>0x2A23</p>|20|R|
|3|<p>Battery Service /</p><p>0x180F</p>||||
|||<p>Battery Level / </p><p>0x2A19</p>|1|R, N|
|4|TimeFlip / <br>F1196F50-71A4-11E6-BDF4-0800200C9A66||1||
|||TimeFlip events data /<br>F1196F51-71A4-11E6-BDF4-0800200C9A66|20|R, N|
|||<p>Facets /</p><p>F1196F52-71A4-11E6-BDF4-0800200C9A66</p>|1|R, N|
|||<p>Command result output /</p><p>F1196F53-71A4-11E6-BDF4-0800200C9A66</p>|20|R|
|||<p>Command /</p><p>F1196F54-71A4-11E6-BDF4-0800200C9A66</p>|20|R, W|
|||<p>Double tap definition/</p><p>F1196F55-71A4-11E6-BDF4-0800200C9A66</p>|1|N|
|||System state /<br>F1196F56-71A4-11E6-BDF4-0800200C9A66|4|R, N|
|||<p>Password check /</p><p>F1196F57-71A4-11E6-BDF4-0800200C9A66</p>|6|W|
|||History data/<br>F1196F58-71A4-11E6-BDF4-0800200C9A66|20|R, W, N|


   

**Firmware Revision String** 

UUID:

0x2A26

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Firmware version|<p>Read</p><p></p>|6|<p>Contains stock firmware version.</p><p>0x544676332E31 = “TFv3.xx”</p>|


   

**Battery Level** 

UUID:

0x2A19

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Battery charge level|Read, Notify|1|Battery charge in percentage (1-100)|

**TimeFlip events data**

UUID:
F1196F51-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|TimeFlip events|Read, Notify|20|Notifications about events in TimeFlip, saved to events log. Sent in ASCI text format.|


**Facets characteristic**
UUID:
F1196F52-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Facet data|Read, Notify|1|<p>ID of a notified facet (1 - 12) </p><p>0 – if the facet in not defined </p><p>0 – if the password is wrong</p>|




**Command result output characteristic**

UUID:

F1196F53-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Commandoutput|Read|20|<p>Output of a command result: </p><p>0x10 – status request</p>|


**Command characteristic**

UUID:

F1196F54-71A4-11E6-BDF4-0800200C9A66

Successfully executed command is shown in characteristics as 0xXX 0x02, where 0хХХ is command message. 

Unexecuted command is shown in characteristics as 0xXX 0x01, where 0хХХ is command message.

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Command|Write|20|<p></p><p>0x04 0x01 – Lock mode\* is on</p><p>0x04 0x02 – Lock mode is off</p><p></p><p>0x05 0xXX 0xXX - Auto-pause mode\*\* (disabled by default)</p><p>0xXX 0xXX – Delay time, in minutes, after which the tracker will be set on pause automatically. (0 – disabled). Auto-pause delay timer is reset every time the tracker facet is changed, it starts counting down from zero. </p><p></p><p>0x06 0x01 – Pause mode\*\*\* on<br>0x06 0x02 – Pause mode off</p><p></p><p>0x07 – requests the actual time in TimeFlip in utc(0) format.  Read response in “**command result output**” as 0x07 + uint64\_t –number of second from the year 1970. </p><p></p><p>0x08 – Set current time in TimeFlip in utc(0) format. Command format: 0x08 + uint64\_t – number of second from the year 1970. </p><p></p><p>0x09 – Tune LED brightness. Command format: 0x09 0xXX, where 0xXX – is brightness level in % (1-100).</p><p>0x0A – Tune LED blink period. Command format:</p><p>0x0A 0xXX, where 0xXX – is the delay between two consecutive LED flashes, in seconds (5-60).</p><p></p><p>0x10 – Status request. Read response in “**command result output**” as 0xXX  0xYY  0xZZ 0xZZ.<br>0xXX – lock mode status (0x01 – on, 0x02 – off)<br>0xYY – pause mode (0x01 – on, 0x02 – off)<br>0xZZ 0xZZ – auto-pause timer delay (in minutes)</p><p></p><p>0x11 – Set color for a facet. Command format:</p><p>0x11 0xNN 0xRR 0xRR 0xGG 0xGG 0xBB 0xBB <br>0xNN – facet number (0-24)<br>0xRR 0xRR – amount of red in the spectrum<br>0xGG 0xGG – amount of green in the spectrum<br>0xBB 0xBB – amount of blue in the spectrum</p><p>0x13 –Set task parameters for a facet. Command format:</p><p>0x13 0xNN 0xPP 0xTT 0xTT 0xTT 0xTT </p><p>0xNN – facet/task number (0-24)</p><p>0xPP – task mode (0 – simple, 1 – pomodoro and/or other modes that can applied in future)</p><p>0xTT 0xTT 0xTT 0xTT – pomodoro timer limit in seconds.</p><p></p><p>0x14 – Read command parameters for a facet. </p><p>Command format: 0x14 0xNN </p><p>Read response in “**command result output**” as: </p><p>0x14 0xNN 0xPP 0xTT 0xTT 0xTT 0xTT 0xCC 0xCC 0xCC 0xCC</p><p>0xNN – facet/task number (0-24)</p><p>0xPP – task mode (0 – simple, 1 – pomodoro)</p><p>0xTT 0xTT 0xTT 0xTT – pomodoro timer limit in seconds, if the task mode = 1</p><p>0xCC 0xCC 0xCC 0xCC – number of seconds from the moment the timer was started.</p><p></p><p>0x15 0xXX 0xZZ … 0xZZ – name record </p><p>0xXX – number of symbols in the name</p><p>0xZZ … 0xZZ - name (18 symbols MAX. ASCII coding) </p><p></p><p>0x16 – Set double-tap (click) parameters for accelerometer. Command format:</p><p>0x16 0x3A 0xTH 0x3B 0xLI 0x3C 0xLT 0x3D 0x WD<br>0x3A – CLICK\_THS register address (threshold)<br>0xTH – value to write in the register (0-255)</p><p>0x3B – LIMIT register address<br>0xLI – value to write in the register (0-255)</p><p>0x3C – LATENCY register address<br>0xLT – value to write in the register (0-255)</p><p>0x3D – WINDOW register address<br>0xWD – value to write in the register (0-255)</p><p></p><p>0x17 – Read double-tap (click) parameters for accelerometer. </p><p>Read response in “**command result output**”. Response format:</p><p>0x17 0x3A 0xTH 0x3B 0xLI 0x3C 0xLT 0x3D 0x WD<br>0x3A –CLICK\_THS register address<br>0xTH –CLICK\_THS register value</p><p>0x3B – LIMIT register address<br>0xLI – LIMIT register value</p><p>0x3C – LATENCY register address<br>0xLT – LATENCY register value</p><p>0x3D – WINDOW register address<br>0xWD – WINDOW register value</p><p></p><p>0х30 0xZZ … 0xZZ – set new password</p><p>0xZZ … 0xZZ – password to be set, length is 6 symbols</p><p></p><p>0xFE – Reset task info to default.</p><p></p><p>0xFF – Erase all data stored in flash memory. Reset the tracker to the factory settings. This command is activated by “Disconnect TimeFlip” button in the mobile app.</p><p></p>|
||Read|2|<p></p><p>0xXX, 0xYY</p><p>0xXX – number of a command</p><p>0xYY – error value (2 - OK, 1 - ERROR)</p><p></p>|

\* Lock – this mode “freezes” TimeFlip to count time on the last active facet and blocks the device from switching facets when TimeFlip is flipped.

\*\* autopause – puts tracker in idle after pre-set timer expires.

\*\*\* pause – time count is set on pause, but the facets continue to be notified (user can flip the tracker and assign new tasks to facets), An interval will be added to the history for the facet with Side + 128.

**Double tap characteristic.**

UUID:

F1196F55-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Double tap|Notify|1|<p>When double-tap is detected (recognized) by the device, it sets tracking on pause and sends notification to the mobile app. The message will encode the facet ID and the pause mode.  </p><p>If the received value is less than 128, then it is the facet ID and pause is “off”. Otherwise, 128 shall be subtracted from the received value to yield actual facet ID with the pause “on”.  </p>|

**System state characteristics**

UUID:

F1196F56-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
||<p>Read,</p><p>Notify</p>|4|<p>System state is calibration characteristics. The first two bytes are responsible for the status:</p><p></p><p>0х00 0х00 – All OK</p><p>0х01 0х00 – The device has been reset to the factory      settings.</p><p>0х02 0х01 – Time synchronization required</p><p>0х02 0х02 – Facet color synchronization required</p><p>0х02 0х03 – LED brightness synchronization required</p><p>0х02 0х04 – Blink interval synchronization required</p><p>0х02 0х05 – Task parameters synchronization required</p><p>0х02 0х06 – Auto-pause synchronization required</p><p></p><p>The second two bytes are responsible for hardware issues: </p><p>0хХХ 0хХХ 0х00 0х00 – Peripherals initialized, all OK</p><p>0хХХ 0хХХ 0х02 0х01 – Accelerometer error</p><p>0хХХ 0хХХ 0х02 0х02 – Flash memory error</p><p>0хХХ 0хХХ 0х02 0х03 – Accelerometer and flash memory error</p>|

This characteristic is used to check if the device is working properly or not. If the facet calibration in the device matches the calibration in the app. A check is performed by comparing a value read in the characteristic with the value stored in the app. When TIMEFIP facets are assigned, an arbitrary value is written to the characteristic and the same value is saved to the app. 


**Password characteristic**

UUID:

F1196F57-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Password check|Write|6|<p>Requires password to be written in the characteristic to allow TIMEFLIP functioning.</p><p>The characteristic is reset every time TIMEFLIP is disconnected from the app. Requests password at every re-connect attempt. </p>|

If the password is not provided or incorrect, TIMEFLIP will not execute commands from the app (be blocked). Default password is ASCII "000000" or (0x30, 0x30, 0x30, 0x30, 0x30, 0x30)

*The response for the password entered can be read in the “command result output characteristic”.*

The result of the password check will be written to the command result output characteristic in the first (high) byte of the massive: 0x01 means the password is correct, 0x02- the password is wrong. 

**History characteristic**

UUID:

F1196F58-71A4-11E6-BDF4-0800200C9A66

|Type|Access|Size, bytes|Description|
| :-: | :-: | :-: | :-: |
|Command output|<p>Read,</p><p>Write,</p><p>Notify</p>|20|<p>Output of command result, for example history read request: 0x01 – for a single element of history + 4 bytes for the number of the element in history. (read response in “**History data**”)</p><p></p><p>Command format: 0x01 0xXX 0xXX 0xXX 0xXX, </p><p>0xXX 0xXX 0xXX 0xXX – the number of an event saved in history. The number equal to 0xFF FF FF FF – returns the number of the last event in history.</p><p></p><p>0x02 – full history read request + 4 bytes for the event number. (read response as data flow with notification in “**History data**”)</p><p> </p><p>Command format: 0x02 0xXX 0xXX 0xXX 0xXX, </p><p>0xXX 0xXX 0xXX 0xXX – the number of event starting from which all history is requested. The history will be sent with all intervals that lasted for at least 5 sec.</p>|

History read-out protocol:

History read request for a single event

After history read-out request is sent (0x01), the number of the event in history will be written to the the characteristic. Example: 

<table>
    <thead width>
        <tr>
           <th>Byte number</th>
           <th>0</th>
           <th>1</th>
           <th>2</th>
           <th>3</th>
           <th>4</th>
           <th>5</th>
           <th>6</th>
           <th>7</th>
           <th>8</th>
           <th>9</th>
           <th>10</th>
           <th>11</th>
           <th>12</th>
           <th>13</th>
           <th>14</th>
           <th>15</th>
           <th>16</th>
        </tr>
    </thead>
    <tbody>
        <tr>
           <td>History block</td>
           <td colspan=4; align = "center">N event</td>
           <td>Side</td>
           <td colspan=8; align = "center">Moment flip</td>
           <td colspan=4; align = "center">Duration side</td>
        </tr>
    </tbody>
</table>

If Side = 66, the side is not defined, accelerometer error.

If Side is >127, the event is “pause” for the task number: Side – 128.

The last package contains zeros, it thus communicates the end of history data transmission. Example:

|**Byte number**|**0**|**1**|**2**|**3**|**4**|**5**|**6**|**7**|**8**|**9**|**10**|**11**|**12**|**13**|**14**|**15**|**16**|**17**|**18**|**19**|
| :- | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
|History block|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|0|

After the request (0x02) is sent, all intervals starting from the current and that lasted for at least 5 seconds will be written to the characteristic. Example:

<table>
    <thead width>
        <tr>
           <th>Byte number</th>
           <th>0</th>
           <th>1</th>
           <th>2</th>
           <th>3</th>
           <th>4</th>
           <th>5</th>
           <th>6</th>
           <th>7</th>
           <th>8</th>
           <th>9</th>
           <th>10</th>
           <th>11</th>
           <th>12</th>
           <th>13</th>
           <th>14</th>
           <th>15</th>
           <th>16</th>
           <th>17</th>
           <th>18</th>
           <th>19</th>
        </tr>
    </thead>
    <tbody>
        <tr>
           <td>History block</td>
           <td colspan=4; align = "center">N event</td>
           <td>Side</td>
           <td colspan=8; align = "center">Moment flip</td>
           <td colspan=4; align = "center">Duration side</td>
           <td colspan=3; align = "center">N previous event</td>
        </tr>
    </tbody>
</table>

The last package contains zeros and the number of the previous event in the sequence. This package communicates the end of history data transmission. Example:

<table>
    <thead width>
        <tr>
           <th>Byte number</th>
           <th>0</th>
           <th>1</th>
           <th>2</th>
           <th>3</th>
           <th>4</th>
           <th>5</th>
           <th>6</th>
           <th>7</th>
           <th>8</th>
           <th>9</th>
           <th>10</th>
           <th>11</th>
           <th>12</th>
           <th>13</th>
           <th>14</th>
           <th>15</th>
           <th>16</th>
           <th>17</th>
           <th>18</th>
           <th>19</th>
        </tr>
    </thead>
    <tbody>
        <tr>
           <td>History block</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td>0</td>
           <td colspan=3; align = "center">N previous event</td>
        </tr>
    </tbody>
</table>












PAGE10
