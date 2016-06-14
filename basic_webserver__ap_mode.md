# Basic WebServer ใน AP Mode

```
#include <Arduino.h>
#include "Constants.h"
#include <EEPROM.h>
#include <ESP8266WiFi.h>

#include <ESP8266WebServer.h>


ESP8266WebServer _server(80);
```