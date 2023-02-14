# Protocol
Misc notes from my attempts to reverse engineer the TV.

## Headers
These must be set on ALL requests
```
Content-Type: application/atom+xml
```

## Misc Notes:
If you leave the session token blank, it bypasses all authentication needed seemingly. Still need to do more testing.

## Notes

### Make sure we can talk to TV P1

```
URL: http://10.0.0.24:8080/
METHOD: GET
```

Response:
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <HDCPError>406</HDCPError>
    <HDCPErrorDetail>Not Acceptable</HDCPErrorDetail>
</envelope>
```

### Make sure we can talk to TV P2
```
URL: http://10.0.0.24:8080/hdcp/api/data?target=version_info
METHOD: GET
```

Response:
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <HDCPError>200</HDCPError>
    <HDCPErrorDetail>OK</HDCPErrorDetail>
    <data>
        <hdcpVersion>1.0</hdcpVersion>
        <serviceVersion>dtv_wifirc1.0</serviceVersion>
    </data>
</envelope>
```

### Display pairing Key on screen
```
URL: http://10.0.0.24:8080/hdcp/api/auth
METHOD: POST
```
Body:
```xml
<?xml version="1.0" encoding="utf-8"?><auth><type>AuthKeyReq</type></auth>
```
Response:
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <HDCPError>200</HDCPError>
    <HDCPErrorDetail>OK</HDCPErrorDetail>
</envelope>
```
```
Pairing Key: FFRNTH
```

### Get Session
```
URL: http://10.0.0.24:8080/hdcp/api/auth
METHOD: POST
```
Body:
```xml
<?xml version="1.0" encoding="utf-8"?><auth><type>AuthReq</type><value>FFRNTH</value></auth>
```
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <HDCPError>200</HDCPError>
    <HDCPErrorDetail>OK</HDCPErrorDetail>
    <session>368473745</session>
</envelope>
```

### Press Button:
```
URL: http://10.0.0.24:8080/hdcp/api/dtv_wifirc
METHOD: POST
```
Body:
```xml
<?xml version=\"1.0\" encoding=\"utf-8\"?><command><session>SESSION_GOES_HERE</session><type>HandleKeyInput</type><value>KEY_VALUE_GOES_HERE</value></command>

<?xml version=\"1.0\" encoding=\"utf-8\"?><command><session>368473745</session><type>HandleKeyInput</type><value>89</value></command>
```

Response:
```xml
<?xml version="1.0" encoding="utf-8"?>
<envelope>
    <HDCPError>200</HDCPError>
    <HDCPErrorDetail>OK</HDCPErrorDetail>
    <session>368473745</session>
</envelope>
```

#### Keys
* Menu:
    * Status Bar: 35
    * Quick Menu: 69
    * Home Menu: 67
    * Premium Menu: 89
    * Installation Menu: 207
    * Factory Advanced Menu #1: 251
    * Factory Advanced Menu #2: 255
  
* Power Controls
    * Power Off: 8
    * Sleep Timer: 14
  
* Navigation
    * Left: 7
    * Right: 6
    * Up: 64
    * Down: 65
    * Select: 68
    * Back: 40
    * Exit: 91
    * Red: 114
    * Green: 113
    * Yellow: 99
    * Blue: 97

* Number Pad:
    * "0": 16
    * "1": 17
    * "2": 18
    * "3": 19
    * "4": 20
    * "5": 21
    * "6": 22
    * "7": 23
    * "8": 24
    * "9": 25
    * Underscore: 76

* Playback Controls:
    * Play: 176
    * Pause: 186
    * Fast Forward: 142
    * Rewind: 143
    * Stop: 177
    * Record: 189

* Input Controls:
    * Tv Radio: 15
    * Simplink: 126
    * Input: 11
    * Component Rgb Hdmi: 152
    * Component: 191
    * Rgb: 213
    * Hdmi: 198
    * Hdmi #1: 206
    * Hdmi #2: 204
    * Hdmi #3: 233
    * Hdmi #4: 218
    * Av #1: 90
    * Av #2: 208
    * Av #3: 209
    * Usb: 124
    * Slideshow Usb #1: 238
    * Slideshow Usb #2: 168

* TV Controls:
    * Channel Up: 0
    * Channel Down: 1
    * Channel Back: 26
    * Favorites: 30
    * Teletext: 32
    * T Opt: 33
    * Channel List: 83
    * Greyed Out Add Button?: 85
    * Guide: 169
    * Info: 170
    * Live Tv: 158

* Picture Controls:
  * Av Mode: 48
  * Picture Mode: 77
  * Ratio: 121
  * Ratio 4 3: 118
  * Ratio 16 9: 119
  * Energy Saving: 149
  * Cinema Zoom: 175
  * "3d": 220
  * Factory Picture Check: 252

* Audio Controls:
    * Volume Up: 2
    * Volume Down: 3
    * Mute: 9
    * Audio Language: 10
    * Sound Mode: 82
    * Factory Sound Check: 253
    * Subtitle Language: 57
    * Audio Description: 145