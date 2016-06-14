# Basic WebServer ใน AP Mode

```
#include <Arduino.h>
#include "Constants.h"
#include <EEPROM.h>
#include <ESP8266WiFi.h>
#include <ArduinoJson.h>
#include "CMMC_Interval.hpp"
#include "CMMC_NTP.hpp"
#include <CMMCEasy.h>
#include "FS.h"
#include <functional>
#include <ESP8266WebServer.h>

ADC_MODE(ADC_VCC);


CMMCEasy easy;
float full_batt = 3122;
CMMC_Interval interval;
ESP8266WebServer _server(80);

`