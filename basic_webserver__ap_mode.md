# Basic WebServer ใน AP Mode

```
#include <Arduino.h>
#include <ESP8266WiFi.h>
#include <ESP8266WebServer.h>


ESP8266WebServer _server(80);

void setup() {
  WiFi.disconnect();
  WiFi.mode(WIFI_AP_STA);
  WiFi.softAP(getId().c_str());
}

void loop() {
  _server.handleClient();
}
```